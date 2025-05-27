## ## バナー出力
```zsh
nc -nv IP 25
```
↓ユーザが存在しない場合以下の文言が返される。
[50 5.1.1 : Recipient address rejected: User unknown in local recipient table]

---
## ## 一般的な列挙
```
# nmap -p25 --script smtp-commands 10.10.10.10

# nmap -p25 --script smtp-open-relay 10.10.10.10 -v
```
---
## ## ユーザ列挙
### ### [smtp-user-enum](https://github.com/cytopia/smtp-user-enum)
```zsh
smtp-user-enum -M VRFY -U user.txt -t IP
```
---

