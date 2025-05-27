## ## ActiveDirectoryの手動の列挙
- 従来のWindowsアプリケーションを使用してADを列挙する。
- Powershellと.NETを使用して追加のAD列挙を実行する。

## ## ADの列挙の初めはユーザとグループの列挙から行う。
1. ドメインユーザを調べる。
```Powershell
net user /domain
```
↑の実行結果から注意してみるべきところは○○adminとなっているユーザである。
管理者権限が付与されている可能性が高いため。

2. netコマンドと/domainオプションを使用して、特定のユーザの検査を実施する。
```powershell
net user jeffadmin /admin
```
どのグループに属しているかを確認する。
Domain Adminsグループに属していればラッキー！
このアカウントを侵害できれば実質的にドメイン管理者に昇格したことになる。

3. ドメイン内のグループを列挙する。
```
net group /domain
```
↑の実行結果から、カスタムグループ(管理者によって作成されたグループ)を列挙する。
```powershell
net group "Sales Department" /domain
```
↑このコマンドの実行結果からこのグループにどのメンバーが属しているかがわかる。

## ## Powershellと.Netを使用した列挙

1. LDAPクエリを送るのにADと通信する必要があるが、最新の情報を保持してるDC(PDC)を特定したい。
```powershell
[System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()
```
実行結果から「PdcRoleOwner」の行に記載されている値がPDCの情報を持っている

2. DNを取得する。
```powershell
([adsi]'').distinguishedName
```

``` pdc_autoenum.ps1

# Store the domain object in the $domainObj variable

$domainObj = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()

# Store the PdcRoleOwner name to the $PDC variable

$PDC = $domainObj.PdcRoleOwner.Name

# Print the $PDC variable

$PDC

# Store the Distinguished Name variable into the $DN variable

$DN = ([adsi]'').distinguishedName

# Print the $DN variable

$DN

$LDAP = "LDAP://$PDC/$DN"

$LDAP

$direntry = New-Object System.DirectoryServices.DirectoryEntry($LDAP)

$dirsearcher = New-Object System.DirectoryServices.DirectorySearcher($direntry)

$dirsearcher.filter="samAccountType=805306368"

$result = $dirsearcher.FindAll()

Foreach($obj in $result)

{

Foreach($prop in $obj.Properties)

{

$prop

}

Write-Host "-------------------------------"

}

```

LDAP://DC1.corp.com/DC=corp,DC=com

---
# # PowerView
- PowerViewマニュアルリンク
	[https://powersploit.readthedocs.io/en/latest/Recon/](https://powersploit.readthedocs.io/en/latest/Recon/)
- ツールダウンロードリンク:https://github.com/PowerShellMafia/PowerSploit/

## ## PowerViewのインポート
```powershell
PS C:\Tools> Import-Module .\PowerView.ps1
```
---
## ## ドメインの基本情報を取得する。
```powershell
PS C:\Tools> Get-NetDomain
```
---
## ## ドメイン内のすべてのユーザリストを取得する。
このコマンドはすべてのユーザオブジェクトの属性を自動的に列挙できる。
```powershell
PS C:\Tools> Get-NetUser # 全てのユーザ情報が出力される。

PS C:\Tools> Get-NetUser | select cn # grepと同じコマンドを使用して特定の属性のみ出力する。

```

- ユーザ情報とどこのドメイングループに属しているかを出力する。
```powershell
PS$ Get-DomainUser -Properties name, MemberOf | fl
```
---
## ## パスワードが変更長い間変更されていない可能性がある。
それを確認するには以下のコマンド使用する。※パスワード自体は列挙できない。
```powershell
PS C:\Tools> Get-NetUser | select cn,pwdlastset,lastlogon
```
![file://C:/Users/alien/AppData/Local/Temp/.B6HJ62/1.png](file://C:/Users/alien/AppData/Local/Temp/.B6HJ62/1.png)

---
## ## グループの列挙
```powershell
PS C:\Tools> Get-NetGroup | select cn
```
![file://C:/Users/alien/AppData/Local/Temp/.B6HJ62/2.png](file://C:/Users/alien/AppData/Local/Temp/.B6HJ62/2.png)

---
## ## 特定のグループを列挙する。
※ネストされたグループは解明されないことに注意
```powershell
PS C:\Tools> Get-NetGroup "Sales Department" | select member
```

![file://C:/Users/alien/AppData/Local/Temp/.B6HJ62/3.png](file://C:/Users/alien/AppData/Local/Temp/.B6HJ62/3.png)
