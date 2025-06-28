## ## pyenvのコマンド集
### ### インストールできるバージョンを調べる。
```sh
pyenv install --list
```

### ### 特定のPythonのバージョンをインストール
```sh
pyenv install 3.10.13
```

### ### 現在有効なバージョンを確認
```sh
pyenv version
```

### ### バージョン切り替え
```sh
# システム全体へ適用する。
pyenv global 3.10..13

# 特定のディレクトリだけに適用する。
pyenv local 3.10.13
```

### ### 今のPythonがどこのPathから実行されているか確認する。
```sh
pyenv which python
```
