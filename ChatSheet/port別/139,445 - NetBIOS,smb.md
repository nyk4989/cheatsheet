## ## Enumeration
### ### Nmap
```zsh
nmap -v -p 139,445 IP
nmap --script smb* IP
- smb-vuln-ms08-067のような特定のスクリプトを実施する場合
nmap --script smb-vuln-ms08-067 IP
# 脆弱であれば「State: VULNERABLE」と出力される。
```
---
### ### NetBIOS over tcp
```zsh
sudo nbtscan -r 10.11.1.0/24
```
---
### ### SMB2の列挙
```zsh
sudo apt install libsmbclient-dev
git clone https://github.com/vanhauser-thc/thc-hydra.git
cd thc-hydra
./configur
make
sudo make install
./hydra -V -l karel -P ~/pass.txt 10.10.10.1 smb2
```

---
## ## 自動列挙
### ### enum4linux or ng
```zsh
enum4linux -a -oY enum4linux.log 10.10.10.3
enum4linux-ng.py -As <target> -oY out
enum4linux-ng -As 192.168.56.136 -u 'qiu' -p 'password' # ユーザ、パスワード指定した方法
```
---
### ### smbclinet
```zsh
smbclient --no-pass -L //10.10.10.3/ # 資格情報なしの接続
smbclient --no-pass //<IP>/<Folder>  # 接続
smbclient -U qiu //192.168.56.136/qiu # ユーザ名指定
```
- ファイルダウンロード&アップロード
```zsh
get target_file local_filename # ダウンロード
put local_file target_file # アップロード
```
---
### ### 共有されているフォルダーの列挙
```zsh
smbmap -H <IP> 
nmap --script smb-enum-shares.nse -p 445
```
---
### ### User Enumeration
```
GetNPUsers.py htb.htb/ -dc-ip 10.10.10.100
```
![[Pasted image 20250522232924.png]]
- ハッシュ要求
```
GetNPUsers.py htb.local/ -dc-ip 10.10.10.161 -request # ハッシュ要求
```
![[Pasted image 20250522232937.png]]