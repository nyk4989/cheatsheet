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
	- `Access-Control-Allow-Origin`と`Access-Control-Allow-Credentials: true`が返ってくる。
		- ヘッダーに`Origin: https://hoge.com/`を追加
			- [ ] 含まれる。
			- [ ] 含まれない。(脅威無し)
		- `hoge.com`を`fuga-hoge.com`にしてOriginヘッダーへ
			- [ ] 含まれる。
			- [ ] 含まれない。(脅威無し)
		- `hoge.com`を`hoge.com.fuga.com`にしてOriginヘッダーへ
			- [ ] 含まれる。
			- [ ] 含まれない。(脅威無し)
		- Originヘッダーに`null`を追加してどうなるか。
			- [ ] 含まれる。
			- [ ] 含まれない。(脅威無し)
- **手順等**
	- [obsidian](obsidian://open?vault=CheatSheet&file=ChatSheet%2F%E8%84%86%E5%BC%B1%E6%80%A7%E5%88%A5%2FWeb%2F%E3%82%AF%E3%83%AD%E3%82%B9%E3%82%AA%E3%83%AA%E3%82%B8%E3%83%B3%E3%83%AA%E3%82%BD%E3%83%BC%E3%82%B9%E5%85%B1%E6%9C%89(CORS))
---
## Authentication vulnerabilities(認証の脆弱性)
- **概要**
	- 認証の脆弱性には、以下の3点に大きく分かれる。
		1. ブルートフォース
		2. 多要素認証
		3. その他のメカニズムの脆弱性
		4. OAuth認証
- **チェックポイント**
	- **ブルートフォース**
		- ユーザ列挙
			- 存在するユーザと存在しないユーザを入力した際にレスポンスに差異があるか。
				- [ ] ある(脆弱性あり)
				- [ ] ない
			- 存在するユーザ名と存在しないユーザ名を入力したときに応答時間は変わるか。
				- [ ] 変わる(脆弱性あり)
				- [ ] 変わらない
	- **多要素認証**
		- 2要素認証バイパス
			- 通常ログインをして、二要素認証の画面が表示された後、コードを入力せずにログイン後画面のURLを入力する。
				- [ ] 遷移できる(脆弱)
				- [ ] 遷移できない
	- **パスワードリセット**
		- パスワードリセットがメールでリンクが届く場合、10分以上経過してもリンクに飛べるか。
			- [ ] できる。(脆弱)
			- [ ] できない。
		- パスワードリセットページにアクセスするときに推測できるトークンもしくはユーザ名が含まれていないか。
			- [ ] 含まれている。(脆弱)
			- [ ] 含まれない。
- **悪用手順**
	- [obsidian](obsidian://open?vault=CheatSheet&file=ChatSheet%2F%E8%84%86%E5%BC%B1%E6%80%A7%E5%88%A5%2FWeb%2FAuthentication%20vulnerabilities(%E8%AA%8D%E8%A8%BC%E3%81%AE%E8%84%86%E5%BC%B1%E6%80%A7))
---
## SQL Injection
- **概要**
	バックエンドで実行されるSQL文に渡される個所に、ペイロードを文字連結がされるとSQL Injectionが発生する。

- **チェックポイント**
	- シングルクォートやダブルクォートを入力し、レスポンスに変化があるか。
		- [ ] 変わる。(脆弱性がある可能性がある。)
		- [ ] 変わらない。
	- 数値型の場合演算を行ってレスポンスが変わるか。
		- [ ] 変わる。
		- [ ] 変わらない。
	- sleepを使用して、レスポンスに遅延が発生するか。
		- [ ] 発生する。
		- [ ] 発生しない。

- **悪用手順等**
	- [obsidian](obsidian://open?vault=CheatSheet&file=ChatSheet%2F%E8%84%86%E5%BC%B1%E6%80%A7%E5%88%A5%2FWeb%2FSQL%20Injection)

---
## XSS
- **概要**
	- XSSには、3つの種類が存在する。
		- 反射型
		- 保存型
		- DOM-Base
- **チェックリスト**
	- 入力値がレスポンスに折り返ってくるか。
		- [ ] 折り返る。(脆弱な可能性)
		- [ ] 折り返らない。
	- 入力値が保存され、それを確認できる。
		- [ ] できる。(脆弱な可能性)
		- [ ] できない。
- **悪用手順**
	- [obsidian](obsidian://open?vault=CheatSheet&file=ChatSheet%2F%E8%84%86%E5%BC%B1%E6%80%A7%E5%88%A5%2FWeb%2FXSS)
---
## 