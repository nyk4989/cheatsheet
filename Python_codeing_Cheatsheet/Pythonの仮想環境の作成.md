## ## Python3
### ### 仮想環境の作成
- 仮想環境を作成するには、仮想環境を置くディレクトリをきめて、そのディレクトリのパスを指定して、`venv`をスクリプトとして実行する。
```sh
python -m venv tutorial-env
```
- tutorial-envディレクトリがなければ作成してくれる。その中にPythonインタプリタ、その他関連するファイルのコピーも含まれる。

---
### ### 仮想環境の有効化&無効化
- Windowsの場合
```powershell
# 有効化
tutorial-env\Scripts\activate

# 無効化
deactivate
```
- Lunux
```sh
# 有効化
source tutorial-env/bin/activate

# 無効化
deactivate
```

---
### ### pipを使ったパッケージ管理
- Install
```sh
python -m pip install <パッケージ名>
```


---
### ### ドキュメント
- [公式ドキュメント](https://docs.python.org/ja/3.13/tutorial/venv.html)
---
## pyenvを組み合わせた仮想環境の作成
```sh
pyenv virtualenv 3.10.13 tfenv
```