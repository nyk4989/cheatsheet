# # SQLi
## ## 1. URLパラ、POSTパラ、JSONボディ
- 怪しい値(id / user / page)などのDBとやり取りしてそうな値がある場合にSQLiを試す。

```例
https://0afc009d047790c78112d44600d300c9.web-security-academy.net/filter?category=Gifts
```

- **Check List**
	- [ ] `'`シングルクォーテーションを入力。
		- [ ] レスポンスに差異がある。
		- [ ] SQLの構文エラーがある。
	- [ ] `"`ダブルクォーテーションを入力。
		- [ ] レスポンスに差異がある。
		- [ ] SQLの構文エラーがある。
	- 上記でどれか一つが、当てはまっていれば以下を実行。
		- [ ] SQLmapを実行。[SQLmapの使い方](obsidian://open?vault=CheatSheet&file=ChatSheet%2F%E8%84%86%E5%BC%B1%E6%80%A7%E5%88%A5%2FSQL%20Injection)

---
## ## 2. ログイン機能
- ログイン機能があった場合に、Bypassができないかを試みる。
	![[Pasted image 20250714001725.png]]

- **Check List**
	- [ ] `'`シングルクォーテーションを入力。
		- [ ] レスポンスに差異がある。
		- [ ] SQLの構文エラーがある。
	- [ ] `"`ダブルクォーテーションを入力。
		- [ ] レスポンスに差異がある。
		- [ ] SQLの構文エラーがある。
	- 上記のどれかが当てはまっていれば、以下を実施。
		- [ ] OR-Base SQLiを実施
	- Union Select
---