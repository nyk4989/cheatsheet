## ## 暗号方式の確認
```sh
7z l -slt ./uploaded-file-3422.zip 

---確認観点
Method = ZipCrypto Deflate # こいつを確認すればいい
```
## ## 古い暗号化を用いたzipファイルであればパスワードクラックをしなくても復元が可能である。
```sh
unzip -v evil.zip
```
- https://medium.com/@whickey000/how-i-cracked-conti-ransomware-groups-leaked-source-code-zip-file-e15d54663a8