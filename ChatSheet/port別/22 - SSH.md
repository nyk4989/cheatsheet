## ## sshキーの作成手順
1. 攻撃端末にて以下のコマンドでキー生成
```zsh
ssh-keygen -t rsa
```
▼以下のキーが作成される。
	・id_rsa (秘密鍵)
	・id_rsa.pub (公開鍵)

2. ターゲット端末に転送する。authorized_keysの作成。
	上記で生成したid_rsa.pubをauthorized_keysに直す。
```zsh
cat id_rsa.pub >> authorized_keys
or
mv id_rsa.pub **authorized_keys**
```

3. permissionの変更
```zsh
chmod 600 authorized_keys
```

4. ターゲットマシンに転送
```zsh
/home/<username>/.ssh/authorized_keys
```

5. 秘密鍵のパーミッション変更
```zsh
chmod 600 id_rsa
```
6. 接続
```zsh
ssh -l [ユーザ名] -i [秘密鍵のパス] [サーバBのホスト名]
```
<参考文献>
- [https://mqt.gitbook.io/oscp-notes/ssh-keys?source=post_page-----9c7f5b963559--------------------------------](https://mqt.gitbook.io/oscp-notes/ssh-keys?source=post_page-----9c7f5b963559--------------------------------)
- [https://qiita.com/HamaTech/items/21bb9761f326c4d4aa65](https://qiita.com/HamaTech/items/21bb9761f326c4d4aa65)

---
## ## PortForward
