## ## 基本列挙
```zsh
dig version.bind CHAOS TXT @DNS
```
---
## ## すべてのレコードを取得
```zsh
dig any version.bind @DNS
```
---
## ## ゾーン転送
```zsh
dig axfr @<DNS_IP> #Try zone transfer without domain
dig axfr @<DNS_IP> <DOMAIN> #Try zone transfer guessing the domain
fierce --domain <DOMAIN> --dns-servers <DNS_IP> #Will try toperform a zone transfer against every authoritative name server and if this doesn'twork, will launch a dictionary attack
```
---
## ## より詳しい情報
```zsh
dig ANY @<DNS_IP> <DOMAIN> #Any information
dig A @<DNS_IP> <DOMAIN> #Regular DNS request
dig AAAA @<DNS_IP> <DOMAIN> #IPv6 DNS request
dig TXT @<DNS_IP> <DOMAIN> #Information
dig MX @<DNS_IP> <DOMAIN> #Emails related
dig NS @<DNS_IP> <DOMAIN> #DNS that resolves that name
dig -x 192.168.0.2 @<DNS_IP> #Reverse lookup
dig -x 2a00:1450:400c:c06::93 @<DNS_IP> #reverse IPv6 lookup
#Use [-p PORT] or -6 (to use ivp6 address of dns)
```
---
## ## サブドメインの列挙
```zsh
dnsenum --dnsserver <DNS_IP> --enum -p 0 -s 0 -o subdomains.txt -f <WORDLIST> <DOMAIN>
gobuster dns
```
---
## ## nmap script
```zsh
nmap -n --script "(default and *dns*) or fcrdns or dns-srv-enum or dns-random-txid or dns-random-srcport" <IP>
```
---
## ## Subdomain Wordlist
```
/opt/work/WordLists/SecLists-master/Discovery/DNS/subdomains-top1million-110000.txt
```
