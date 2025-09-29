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
	2. HTML`<meta>`タグ
	3. 