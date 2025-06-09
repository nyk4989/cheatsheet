## ## 別の観点でのポート
- 5985
	よくNmapを掛けると上がってくるが、リモート接続用のポートでプロトコルをHTTPにを利用しているにすぎないのでWebと同じ列挙は不要。

# # 基本列挙
## ## Directory Bruteforce
- 注意点：CMS系のマシンの場合、拡張子を指定したものでないと見つけられないディレクトリがあったりするのでちゃんと列挙すること。
### ### Tools
- Gobuster
```sh
# basic
gobuster -u https://10.10.10.60/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -k

# 遷移先が全て30x系で遷移する場合は以下のオプションを使用するのがいい。
gobuster dir -k -u http://exfiltrated.offsec/ -w /opt/work/WordLists/directory-list-2.3-medium.txt -r -o gobuster.txt

# ヘッダーを指定するやり方
gobuster dir -H "Authorization: Basic b2Zmc2VjOmVsaXRl" -u [http://192.168.154.46:242/](http://192.168.154.46:242/) -w /opt/work/WordLists/directory-list-2.3-medium.txt -o gobuster.log
```

- Ferxbuster
```sh
# basic
feroxbuster -u IP -w wordlist -o ferox.txt

# 俺的
feroxbuster -C 404,400 -x php,txt -u http://blaze.offsec/ -w /opt/work/WordLists/directory-list-2.3-medium.txt -o feroxbuster_80.txt
```

- Diresearch
```sh
dirseach -u http://<IP>:<Port>/
```

- FFuF
```sh 
ffuf -w /path/to/wordlist -u https://target/FUZZ
​
	- 有用だと思われるオプション
	    -recuest : リクエストをファイルから読み取れる
	    -replay_proxy 
	    -ic : ワードリスト内のコメントアウトを無視する 
	    -of : 出力ファイル形式を出力する。json, ejson, html, md, csv, ecsv 
	    -timeout 
	    -rate 
	    --recursive : 再帰的スキャン 
	    -H : "user-Agent: hacker-one"
```

- DirBuster
```sh
dirbuster -u https://10.10.10.60 -t 20 -l /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -r sense-10.10.10.60/dirbuster_dir-med -e php,txt,html
```

#### #### 拡張子について
使用している製品に含まれる拡張子。(confm,cgi)
開発言語の拡張子(php,pl...)

---
## ## リクエストパラメータの列挙
http://192.168.56.134/manage.phpに対して、http://192.168.56.134/manage.php?の後ろにパラメータを順々にここなっていく。

```sh
# パラメータの列挙
ffuf -w burp-parameter-names.txt -u http://dc9.local/welcome.php?FUZZ=test_value -b "PHPSESSID=811tqnan26gfrdvq17hqpqfmpr" -fs 4242 -replay-proxy http://127.0.0.1:8080/

# パラメータ値の列挙
## wfuzzの場合
wfuzz -c -w /opt/work/WordLists/SecLists-master/Discovery/Web-Content/burp-parameter-names.txt --hc 404,302 --hh 963 -b "Cookie: PHPSESSID=811tqnan26gfrdvq17hqpqfmpr" http://dc9.local/welcome.php?FUZZ=../../../../../../etc/passwd

## ffufの場合
ffuf -w /path/to/paramnames.txt -u https://target/script.php?FUZZ=test_value -fs 4242
```
- wordlistの場所
```sh
SecLists/Discovery/Web-Content/burp-parameter-names.txt
```
---
## ## vhost
注意：二宮個人的にはGobusterだと正しく列挙できないことが多いが、念のため2つ回している。
- FFUF
```sh
gobuster vhosts -u hostname -w wordlists -t 150
```

- Gobuster
```sh
ffuf -w /path/to/vhost/wordlist -u https://target -H "Host: FUZZ" -fs 4242
```
---
### ## herfリンクなどのクロール
### ### hakrawler
爆速＆軽量。`<a>, <script>`など静的リンク探索に最適。JSレンダなしでもOK。初動でサクッと使える。
```sh
hakrawler -url http://target.htb -depth 2 -plain
```

### ### LinkFinder
JavaScript内にある隠しAPIやパスを抽出できる。ログイン周り・API呼び出しを狙うときに強い。
```
python3 linkfinder.py -i http://target.htb/assets/app.js -o cli
```

### ### katana
本気で深堀りしたいとき用。遅めだが、API探索や再帰クローリングが必要な場合に最強。
```
katana -u http://target.htb -d 2 -o out.txt
```

---
# # 機能別
## ## File Upload
### ### 拡張子を変える。
```
PHP: .php, .php2, .php3, .php4, .php5, .php6, .php7, .phps, .phps, .pht, .phtm, .phtml, .pgif, .shtml, .htaccess, .phar, .inc, .hphp, .ctp, .module
---
Working in PHPv8: .php, .php4, .php5, .phtml, .module, .inc, .hphp, .ctp
---
ASP: .asp, .aspx, .config, .ashx, .asmx, .aspq, .axd, .cshtm, .cshtml, .rem, .soap, .vbhtm, .vbhtml, .asa, .cer, .shtml
---
Jsp: .jsp, .jspx, .jsw, .jsv, .jspf, .wss, .do, .action
---
Coldfusion: .cfm, .cfml, .cfc, .dbm
---
Flash: .swf
---
Perl: .pl, .cgi
---
Erlang Yaws Web Server: .yaws
---
```

- kaliでWebshellが格納されているディレクトリ
```sh
/usr/share/webshel​​ls/
```

- fuzzing list
	[https://github.com/fuzzdb-project/fuzzdb/blob/master/attack/file-upload/alt-extensions-php.txt](https://github.com/fuzzdb-project/fuzzdb/blob/master/attack/file-upload/alt-extensions-php.txt)

▼参考サイト
[https://book.hacktricks.xyz/pentesting-web/file-upload](https://book.hacktricks.xyz/pentesting-web/file-upload)

---
## ## .gitがあった場合
開発中に含まれた機密情報などを探してく。
### ### gitのディレクトリを自分の端末にダウンロードする。
- **wget**
```sh
wget --mirror -I .git http://target/.git
​
オプションの説明:
 -mirror : gitからすべてをダウンロードするように指示
 -I : ダウンロードされたすべてのファイルを作成する
```

- **GitDumper**
```sh
git-dumper http://10.10.11.58/.git/ ./dog_git_repo
```

### ### Gitコマンド
- コミット履歴の確認。どんな変更が過去にあったかを調べる。
```sh
git log
```

- ほかにどんなブランチが存在するか。(ブランチはそれぞれのバージョンみたいなもので、例えば本番用。開発用みたいな感じ)
```sh
git show-branch
git branch -a
```

- ブランチの切り替え
```sh
git switch <ブランチ名>
git checkout <ブランチ名>
```

- 複数のブランチがある場合、それの差分を見る方法
```sh
git diff <commit1> <commit2>
```

- 特定の過去のコミット状態に一時的に切り替えてファイルの中身を確認するために使用
```sh
git checkout <commit-hash>
```
---
# # 脆弱性別
## ## SQL Injection
[SQL Injection](obsidian**://open?vault=CheatSheet&file=ChatSheet%2F%E8%84%86%E5%BC%B1%E6%80%A7%E5%88%A5%2FSQL%20Injection)

## ## File Inclusion
[File Inclusion](obsidian://open?vault=CheatSheet&file=ChatSheet%2F%E8%84%86%E5%BC%B1%E6%80%A7%E5%88%A5%2FFile%20Inclusion)

## ## OS Command
[OS Command](obsidian://open?vault=CheatSheet&file=ChatSheet%2F%E8%84%86%E5%BC%B1%E6%80%A7%E5%88%A5%2FOS%20Command%20Injection)

## ## SSTI
[SSTI](obsidian://open?vault=CheatSheet&file=ChatSheet%2F%E8%84%86%E5%BC%B1%E6%80%A7%E5%88%A5%2FSSTI(Server%20Side%20Templet%20Injection))

---
# # 違和感
## ## CMSの特徴
以下のようなディレクトリがあった場合はCMSの可能性も考慮してもいいかも。
- /wp-content
- /blog
- /cms

### ### ツールでCMSを特定する方法
- [CMSeek](https://github.com/Tuhinshubhra/CMSeek) <- このツールは有名どころはいいがすべてのCMSをカバーしているわけではないことに注意。
```sh
cmseek -u http://10.10.10.180/ -v
```