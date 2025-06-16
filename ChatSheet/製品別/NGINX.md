## ## NGINXの構成
### ### インストール時に作られる以下のファイル
/etc/nginx/sites-available/ # 全サイトの設定ファイルを置く場所（まだ有効化されていない）
/etc/nginx/sites-enabled/ # 実際に nginx が読み込む「有効化済み」の設定（available から ln -s でリンクされる）

### ### モジュールの格納先
/usr/share/nginx/modules/

## ## 危険なモジュール
- [NginxExecute](https://github.com/limithit/NginxExecute)