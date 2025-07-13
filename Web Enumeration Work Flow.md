# # SQLi [CheatSheet](obsidian://open?vault=CheatSheet&file=ChatSheet%2F%E8%84%86%E5%BC%B1%E6%80%A7%E5%88%A5%2FSQL%20Injection)
## ## 1. URLパラ、POSTパラ、JSONボディ
- 怪しい値(id / user / page)などのDBとやり取りしてそうな値がある場合にSQLiを試す。

```例
https://0afc009d047790c78112d44600d300c9.web-security-academy.net/filter?category=Gifts
```

- **Check List**
[1] クォーテーション投入  
- [ ] `'` シングルクォート  
- [ ] `"` ダブルクォート  
→ **以下を確認**
- [ ] レスポンス変化あり  
- [ ] SQL構文エラー表示あり  
[2] どれか確認できたら実行  
- [ ] SQLmap投入（PoC / 自動化確認）

---
## ## 2. ログイン機能
- ログイン機能があった場合に、Bypassができないかを試みる。
	![[Pasted image 20250714001725.png]]

- **Check List**
**[1] クォーテーション投入**
- [ ] `'` シングルクォート
- [ ] `"` ダブルクォート
→ **以下を確認**
 - [ ] レスポンス変化（エラー / 挙動差）
 - [ ] SQL構文エラー表示あり
**[2] 差異・エラーが確認できた場合**
- [ ] OR-based SQLi（`OR 1=1 --`）確認
- [ ] UNION SELECT 検証
---
# # XSS [CheatSheet](obsidian://open?vault=CheatSheet&file=ChatSheet%2F%E8%84%86%E5%BC%B1%E6%80%A7%E5%88%A5%2FXSS)
