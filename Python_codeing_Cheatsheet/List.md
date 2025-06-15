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
### ### del文を使用する
使い方`del 要素の名前[インデックス番号]`
```Python
motocls=['honda','suzuki','kawasaki']
print(motocls)

del motocls[0] # 削除
print(motocls)
```
↓出力結果
```sh
['honda', 'suzuki', 'kawasaki']
['suzuki', 'kawasaki']
```

### ### .pop()メソッドを使用する
**一時的に削除したい場合に有効**

#### #### 最後の要素を削除する方法
```Python
motocls=['honda','suzuki','kawasaki']
print(motocls)

popped_motocls=motocls.pop() # 一時的に削除
print(motocls)
print(poppend_motocls)
```

#### #### リストの中の任意の位置から要素を削除する
```Python
motocls=['honda','kawasaki','KTM']

first_pop=motocls.pop(0)
 print(f" インデックス番号0である {first_pop.title()} が削除される。")
```

---
## ## 