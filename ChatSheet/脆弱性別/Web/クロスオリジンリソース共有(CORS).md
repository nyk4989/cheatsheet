## 前提知識
### クロスオリジンリソース共有とは？
- 特定のドメインのリソースにアクセスさせるための機能。
- 同一ドメインポリシーの拡張機能的役割。

## 攻撃の説明
1. **クライアントが指定したOriginヘッダーからサーバが生成したACAOヘッダー**
	リクエストからOriginヘッダーを読取り、レスポンスが許可されていることを示すレスポンスヘッダーを含める。
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
	Access-Control-Allow-Origin: https://malicious-website.com
	Access-Control-Allow-Credentials: true
	...
	```
	
