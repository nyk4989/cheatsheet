## ## tree

| コマンド例           | 説明                                       |
| --------------- | ---------------------------------------- |
| `tree -L 2`     | 深さ2まで表示（サブディレクトリが多い時に便利）                 |
| `tree -a`       | **隠しファイル（.dotfiles）も含めて表示**              |
| `tree -f`       | **フルパスで表示**（後でコピペしやすい）                   |
| `tree -pug`     | **パーミッション、所有者、グループ**付きで表示（権限確認に便利）       |
| `tree -h`       | **ファイルサイズを人間に読みやすく表示**（KB/MB/GB）         |
| `tree -I '*.log | *.tmp'`                                  |
| `tree -s`       | **サイズ順に並べる**（-Dと併用すると日時も表示可能）            |
| `tree -C`       | **色付き出力**（デフォで有効な場合も）                    |
| `tree -Q`       | **ファイル名をダブルクォートで囲む**（スペース入りファイル名がわかりやすい） |
|                 |                                          |
### ### 情報収集向き
```sh 
tree -a -f -pug -h -L 3
```

---

## ## 解凍コマンド
### ### 7z
```sh
7z x file.7z
```

### ### tar.gz
```sh
tar -zxvf filename.tar.gz
```

### ### zip
- 圧縮
```
zip -r FILENAME.zip FILE_DIR
```

# Enumeration
## Nmap
- サーバに負荷をかけないコマンド
-max-rate 50 --min-rate 10 --max-retries 2 --scan-delay 500ms

## Web

---

### vhosts
- ffuf
```
ffuf -w /opt/work/WordLists/SecLists-master/Discovery/DNS/subdomains-top1million-110000.txt -u http://spectra.htb/ -H "Host: FUZZ.spectra.htb" -ic -fw 22
```
### Wordpress,WP,WPSCAN
#### WPScan
apitoken:9q9BSbdaRwKT1mQZ6t1YmSU6RZW44oxgicqa42TvtBU

- basic usage
wpscan --url "target" --verbose --api-token xxxxxxxxxxxxxxxxxxxxxxxx
​
- enumerate vulnerable plugins, users, vulrenable themes, timthumbs
wpscan --url "target" --enumerate vp,u,vt,tt --verbose --api-token xxxxxxxxxxxxxxxxxxxxxxxx

- WPでプラグインをアップロードしたあと以下のディレクトリでアクセスができる。
[WP root]/wp-content/plugins/[plugin name]/[filename]

# MySQL コマンド
mysql -u root -h 127.0.0.1 -e 'show databases;'

# Vuln Enumeration
## Path Travarsal
- ffufを使ったTravarsalの列挙
ffuf -H "Cookie: PHPSESSID=jvjrluq9mao70i7j4upomht27v" -u http://nocturnal.htb/view.php -data "username=hoge&file=FUZZetc/passwd" -w /opt/work/WordLists/PayloadsAllTheThings/File\ Inclusion/Intruders/dot-slash-PathTraversal_and_LFI_pairing.txt:FUZZ -fw 1170 -x http://127.0.0.1:8080