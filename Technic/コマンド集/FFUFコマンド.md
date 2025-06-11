## ## Recuest.txtを作成し、Fuzzingを行う方法
```sh
ffuf -request ./req.txt -w wordlist.txt
```
- この際のリクエストファイルの中に列挙したい箇所をFUZZとして置く必要がある。
```sh
POST /forgot/ HTTP/1.1
Host: 10.10.11.113:8080
Content-Length: 25
Cache-Control: max-age=0
Accept-Language: en-US,en;q=0.9
Origin: http://10.10.11.113:8080
Content-Type: application/x-www-form-urlencoded
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/133.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://10.10.11.113:8080/forgot/
Accept-Encoding: gzip, deflate, br
Connection: keep-alive

email=FUZZ
```

## ## Proxyを指定する方法
