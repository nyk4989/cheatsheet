## Basic Enumeration
- **Directory Brute force (WordList)**
	- [ ] SecLists-master/Discovery/Web-Content/common.txt
	- [ ] /opt/wordlists/SecLists-master/Discovery/Web-Content/raft-large-directories-lowercase.txt
	- [ ] 拡張子:`.bak,.old,.orig,.save,.swp,.tmp,.backup,.log,.conf,.ini,.env,.json,.xml,.zip,.tar.gz,.tgz,.sql,.sql.gz,.db,.sqlite,.pem,.key`

- **vhost enumeration**
	- [ ] subdomains-top1million-110000.txt

---
## GraphQL  Enumeration
- **Directory Brute Force**
	- [ ] 

- **Headerの確認。**
	- [ ] application/json
	- [ ] application/graphql

---
## オープンリダイレクト
- **概要**
	Webサイトが意図していないURLを送信する。これにより悪意のあるサイトへの誘導、フィッシング攻撃、トークン等の値を盗むことが可能。
- **エントリポイント**
	1. URLパラメータを探す。
	2. HTML`<meta>`タグ。
	3. DOMを通じたウィンドウ`location`を確認する。
- **チェックポイント**
	- **ベーシック列挙**
		- [ ] `?redirect=http://C2.com`
		- [ ] `?redirect=//evil.com`
	- **スキームバリエーション**
		- [ ] `?redirect=http%3A%2F%2Fevil.com`
		- [ ] `?redirect=%5Cevil.com`
	- **相対パス**
		- [ ] `?redirect=../../evil.com`
		- [ ] `?redirect=/%2E%2E/evil.com`
		- [ ] `?redirect=/\evil.com`
	- **ホワイトリスト回避**
		- [ ] `?redirect=https://trusted.com.evil.com`
		- [ ] `?redirect=https://evil.com@trusted.com`
		- [ ] `?redirect=https://trusted.com#@evil.com`
	- **スキームミックス**
		- [ ] `?redirect=//trusted.com.evil.com`
		- [ ] `?redirect=https:/evil.com`
	- **二重エンコード / Unicode**
		- [ ] `?redirect=%252F%252Fevil.com`
		- [ ] `?redirect=%u2216evil.com`
	- **チェック時のポイント**
		- [ ] リダイレクトヘッダ(`Location:)
		- [ ] jメタリフレッシュ/JS経由でリダイレクトされないか

---
## HTTPパラメータの汚染(HPP)
- **概要**
	- 同じ名前のパラメータを複数回リクエストに含めた際に、サーバ側でのパラメータ解釈の違いを悪用する。
	- 各言語やフレームワーク、ミドルウェアで「最後の値を採用する」「最初の値を採用する」「配列として扱う」などの動作が異なるため、検証を怠ると意図しない挙動やセキュリティ上の欠陥を誘発する。
- **エントリポイント**

---
## クリックジャッキング

- **概要**
	- 攻撃者が偽サイトを作りその上に、iframeで正規のページのボタンを自分のサイトに埋め込み透明にする。さらに自作したボタンをその下に置く。こうすることで、正規の方法でリクエストが送られるかつ、CSRFをこちら側が入手することなく、意図しない操作が可能となる。
- **チェックポイント**
	- [ ] レスポンスヘッダにX-Frame-Optionsヘッダが存在すること
	- [ ] レスポンスヘッダまたはレスポンスボディにContent-Security-Policyヘッダ相当の要素が存在すること
- **手順等**
	- [obsidian](obsidian://open?vault=CheatSheet&file=ChatSheet%2F%E8%84%86%E5%BC%B1%E6%80%A7%E5%88%A5%2FWeb%2FClickJacking)
	- [PostSwigger](https://portswigger.net/web-security/clickjacking#what-is-clickjacking)
---
## クロスオリジンリソース共有(CORS)の設定不備
- **概要**
	- サーバ側でCORSに関するヘッダーの設定を謝ると**クロスオリジン攻撃**を受ける可能性がある。
	- この攻撃は、過度に制限を緩くしてしまったりすることで生じる脆弱性。
- **チェックポイント**
	- `Origin: https://hoge/`を入力するして、`Access-Control-Allow-Origin`と`Access-Control-Allow-Credentials: true`がレスポンスに含まれるか。
		- [ ] 含まれる。
		- [ ] 含まれない。