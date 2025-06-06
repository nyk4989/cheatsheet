## ## nmapのオプション
```
--min-rate <number>
	与えられたレートと同党もしくはそれ以上のスピードでスキャンをしてくる。
	例えば--min-rate 300を指定すると毎秒300パケット以上に維持してくる。
	HTBでは、10000に指定していた。
	※nmapを使用した時にfilteredと返ってくるポートがある場合は-sSも試してみるのも効果的
```

- 負荷を気にしたときのスキャン
```sh
-max-rate 50 --min-rate 10 --max-retries 2 --scan-delay 500ms
```
---
## ## ステルススキャン
送受信するパケット数が少ないから、速度は上がる。
```zsh
sudo nmap -sS 10.11.1.220
```

---
## ## network sweeping
```zsh
nmap -sn 10.11.1.1-254
```

---
## ## トップポート指定する。
上位20ポートがどうやって決まるかは以下のファイルパスに記載されているポートで決まる。
/usr/share/nmap/nmap-services
```zsh
nmap --top-ports=20 IP
```
---
## ## NSEスクリプトの使い方の説明は以下の様に行う。
```zsh
# サンプルコード
nmap --script-help dns-zone-transfer
```
---
## ## よくわからんポートを見つけたとき
以下のコマンドを試してみる。
```zsh
telnet
nc
curl
```
---
## ## ネットワークグループのスキャン
```zsh
sudo nmap -Pn -o nmap_ip_all.log 172.16.244.0/24 -vv
```
---
# # NSEスクリプト
NSEスクリプトとは、Nessusに代わる脆弱性スキャナーであり、ものによっては悪用までも行ってくれる。
**※vulnとexploitで区別できるもののvulnと記載があるものの中には悪用までやってしまうものもあるので中が必要。**

## ## vulnカテゴリーのスクリプトを実行する方法
```
sudo nmap --script vuln 10.11.1.10
```