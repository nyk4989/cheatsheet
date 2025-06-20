## ## ZIP File
1. **zip2john**
```sh
zip2john credentials.zip > hash.txt
```
2. **hashcrack**
```sh
john-the-ripper --incremental=ASCII credentials.zip.hash
```