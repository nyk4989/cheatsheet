## ## リストの要素を変更する
リストの変数名の後ろに変更し太陽のインデックスを指定し、その要素に持たせる新しい値を指定する。
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
使い方は`.append()`