## ## リストの要素を変更する
リストの変数名の後ろに変更し特定のインデックスを指定し、その要素に持たせる新しい値を指定する。
```Python
motocls=['honda','suzuki','kawasaki']
print(motocls)

motocls[0]='KTM' # hondaをKTMに入れ替える。
print(motocls)
```
↓出力結果
```sh
['honda', 'suzuki', 'kawasaki']
['KTM', 'suzuki', 'kawasaki']
```

---
## ## リストに要素を追加する
.appendを使用して、リストの最後に追加することができる。
使い方は`.append('追加したい要素の名前')`
```Python
motocls=['honda','suzuki','kawasaki']
print(motocls)

motocls.append('KTM') # リストの最後にKTMが追加される。
print(motocls)
```
↓出力結果
```sh
['honda', 'suzuki', 'kawasaki']
['honda', 'suzuki', 'kawasaki', 'KTM']
```

### ### 用途
空のリストを作成して、追加ができる。
Pentestならオープンポートのみをリストに追加していける感じかな。
```Python
open_port[]

poen_port.append('21')
poen_port.append('22')
poen_port.append('80')
poen_port.append('443')
```

---
## ## リスト要素を削除する
del文を使用する。
使い方`del 要素の名前['インデックス番号']`
```Python

```