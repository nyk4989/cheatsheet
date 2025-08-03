## ## 前提
- Web Enumeration Work Flowでの列挙は完了していること。

## ## MongoDBの場合
### ### Null Strings
- MongoDBでは、ヌル文字以降の文字列がすべて無視される場合がある。つまり、意図的に後ろの条件を無効化できる可能性がある。
```
# 例えば以下のようなクエリがあったとする。
this.category == 'fizzy' && this.released == 1

# 次にfizzyの後ろに'%00を追加した場合、次に続く' && this.released == 1が無視される。
```