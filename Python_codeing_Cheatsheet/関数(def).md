## ## 関数とは
- 関数は特定の処理を書いたコードブロックのこと。
- 特定の処理を実行する際には関数名を呼び出す必要がある。

## ## 関数を定義する
```python
def greet_user(): # defを使用することで、関数を定義することを伝える。
	"""シンプルな挨拶メッセージを出力する""" # Docstringsと呼ばれるもので関数の動作に関わる説明を入れる。
	print("こんにちは!") # この関数の処理の部分となる。
	
greet_user() # 関数の呼び出し
```

---
## ## 関数に追加の情報を渡す
```python
def greet_user(username):
	"""シンプルな挨拶メッセージを出力する"""
	print(f"こんにちは!{username.title()}")
	
greet_user('yuki') # こうすることにより、printに必要な情報が渡される。
```
---
## ## 位置引数
- 位置引数とは
	- 位置引数は変数名と実引数が紐づくようになっている。
```python
def describe_pet(animal_type,pet_name): # ここと
	"""ペットの情報についての情報を出力する。"""
	print(f"\n 私は{animal_type}を飼ってます。")
	print(f"\n {animal_type}の名前は{pet_name}です。")

describe_pet('ネコ','ミケ') # ここが一致している。
# animel_typeにはネコが入り、pet_nameにはミケが入る。
```

### ### 複数回関数を呼び出すとき
```python
def describe_pet(animal_type,pet_name):
	"""ペットの情報についての情報を出力する。"""
	print(f"\n 私は{animal_type}を飼ってます。")
	print(f"\n {animal_type}の名前は{pet_name}です。")

describe_pet('ネコ','ミケ')
describe_pet('イヌ','チョコ')
```
---
## ## キーワード引数
- キーワード引数とは
	- 名前と値のペアを関数に渡す。
```python
def describe_pet(animal_type,pet_name):
	"""ペットの情報についての情報を出力する。"""
	print(f"\n 私は{animal_type}を飼ってます。")
	print(f"\n {animal_type}の名前は{pet_name}です。")

# 位置引数の場合
# describe_pet('ネコ','ミケ')
# describe_pet('イヌ','チョコ')

# キーワード引数の場合
describe_pet(animal_type='ネコ',pet_name='ミケランジェロ')
```

---
## ## デフォルト値
- 各仮引数にデフォルト値として値を設定することができる。
	- 実引数に値を入力されれば、そちらが優先され、何も値がなければ、デフォルト値が参照される。
```python
def http_port(port='80') # <-仮引数であるこれは実引数に何も入力されなかったときにこいつがそのまま値として入る。
・・・
http_port(port='8080') # <-これが優先される。
```
- デフォルト値を設定する場合、持たせたい関数の値を一番後ろにする必要がある。
- 空の値を持たせるとデフォルト値が消え空になってしまう。

## ## 戻り値を設定する(return)
```python
def get_formatted_name(first_name,last_name): # この関数は、借り引数としてfirst_nameとlast_nameを受け取る。
	"""フォーマットされたフルネーム"""
	full_name=f"{first_name} {last_name}" # 関数は、名と性の間にスペースを挟んで文字連結したものをfull_name変数に代入する。
	return full_name.title() # full_nameの値をタイトルケースに変更して関数を呼び出した行に返す。
	
musician=get_formatted_name('alexi','lyho') # 関数を呼び出したときに値を返ってくるようにするには、戻り値を代入する変数を指定する必要がある。
print(musician) # 指定した性と名を元にフォーマットされたフルネームが出力される。
```

## ## オプション引数を作成する。
これを使うかとにより、通常の関数では入力値を含めないと駄目だが、空の文字をデフォルト値として入力して起き、必要な時だけ使用することができる。
```python

```