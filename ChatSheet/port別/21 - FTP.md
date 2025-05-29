## ## 基本情報
アクティブ接続とパッシブ接続
・アクティブFTP
 	アクティブFTPでは、まずFTPクライアントはポートNからFTPサーバのコマンドポート21への制御接続を開始する。
 	次にクライアントは、ポートN+1をリッスンし、ポートN+1をFTPサーバに送信する。
	最後にFTPサーバは、そのポートMからFTPクライアントのポートN+1へのデータ接続を開始する。
	
・パッシブ接続
	クライアントはまず制御接続を確立する。
	その後クライアントはpassvコマンドを発行する。
	次にサーバはクライアントにポート番号Mの1つを送信する。
	クライアントはポートPからFTPからFTPサーバのポートMへのデータ接続を開始する。

## ## Enumeration
### ### バナーの表示
```zsh
nc -vn <IP> 21
```

### ### 自動列挙
AnonymousログインおよびバウンスFTPのチャックを行ってくれる。-sCなどでもできる。
```zsh
nmap --script ftp-* -p 21 <IP>
```

### ### FTP Bruteforceリスト
[https://github.com/danielmiessler/SecLists/blob/master/Passwords/Default-Credentials/ftp-betterdefaultpasslist.txt](https://github.com/danielmiessler/SecLists/blob/master/Passwords/Default-Credentials/ftp-betterdefaultpasslist.txt)

---
## ## FTP接続後
### ### ファイルのダウンロード
- 単一ファイルのダウンロード
```zsh
GET <File Name>
```

- 複数ファイルのダウンロード
```zsh
wget -m [ftp://anonymous:anonymous@10.10.10.98](ftp://anonymous:anonymous@10.10.10.98) #Donwload all

wget -m --no-passive [ftp://anonymous:anonymous@10.10.10.98](ftp://anonymous:anonymous@10.10.10.98) #Download all
```

---
## ## ASCIIモードとBinaryモード
1. ASCIIモード
	- テキストファイル用
	- 改行コードをOSごとに自動変換してくれる。
2. Binaryモード
	- 実行ファイルや画像、圧縮ファイルなどのバイナリ用
	- ファイルを一切変更せずに転送。<- これが推奨モードとされている。
### ### 切り替え方法
```
ftp target_ip
ftp> binary     ← バイナリモードに切り替える
ftp> get file.exe
```