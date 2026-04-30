# 学習ログ：1章 SpringとSpring Bootの関係

## ■ 学び・気づき

- アプリケーション開発は単なるプログラム作成ではなく、  
  データベース・セキュリティ・クラウドなど複数の要素の組み合わせで成り立っている :contentReference[oaicite:0]{index=0}

- フレームワークは「便利なライブラリ」ではなく、  
  **開発の土台（設計の枠組み）そのもの**である

- Springは単一の機能ではなく、  
  **複数のプロジェクトの集合体で構成されている**

- Spring Frameworkはその中核であり、  
  **Web・DB・トランザクションなどの基盤機能を提供する**

- Spring Dataの特徴は、  
  **データベースごとの差異を吸収し、同じ書き方で操作できる点にある**

- CRUDという概念は単なる操作ではなく、  
  **データベース操作の基本パターンである**

- メソッド名からクエリを自動生成する仕組みは、  
  **コード記述量削減だけでなく、設計の一貫性にも寄与する**

- Spring Security / Batch / Cloudはそれぞれ  
  **セキュリティ・バッチ処理・クラウド対応という役割分担が明確に分かれている**

- Spring Bootは新しいフレームワークではなく、  
  **Springを使いやすくするための仕組み（設定・起動の簡略化）である**

- 「設定より規約」という考え方は、  
  **自由度を制限する代わりに開発速度と統一性を高める設計思想である**

---

# ■ つまずいたポイント
- 教科書の「DIなし / DIあり」の比較を見ても意味が分からなかった
- コードの違いは分かるが、**何が本質的に変わっているのか理解できなかった**
- 「newを外に出すだけ」に見え、なぜ重要なのか納得できなかった

---

## ■ 教科書の原文コード

## ❌ DIなし
UserServiceがEmailServiceを直接生成

class UserService {
    private EmailService emailService = new EmailService();

    public void send() {
        emailService.sendEmail();
    }
}

問題：
- 実装が固定され、差し替えが困難

---

## ⭕ DIあり
外部から依存を受け取る

class UserService {
    private EmailService emailService;

    public UserService(EmailService emailService) {
        this.emailService = emailService;
    }

    public void send() {
        emailService.sendEmail();
    }
}

使い方：
EmailService emailService = new EmailService();
UserService userService = new UserService(emailService);

---

## ■ DIなしコードの検証

private EmailService emailService = new EmailService();

この1行には以下の意味がある：
- 型（EmailService）を指定
- 変数（emailService）を作成
- インスタンスを生成して代入

---

## ■ 気づいたこと

このコードでは以下がすべてUserServiceの中で決まっている：

- 使うクラス → EmailService
- 作り方 → new EmailService()
- タイミング → UserService生成時

👉 つまり  
**依存関係が完全にクラス内部に固定されている**

---

## ■ 問題の具体例

SMSに変更したい場合：

class SmsService {
    public void send() {
        System.out.println("SMS送信");
    }
}

👉 しかし現在は new EmailService() 固定

→ UserServiceを書き換える必要がある

---

## ■ DIありコードの検証

class UserService {
    private EmailService emailService;

    public UserService(EmailService emailService) {
        this.emailService = emailService;
    }
}

---

## ■ 気づいたこと

- UserServiceはEmailServiceを「使うだけ」
- どの実装を使うかは外部で決まる

---

## ■ 外側での制御

EmailService emailService = new EmailService();
UserService userService = new UserService(emailService);

---

## ■ ここでの変化

👉 「作る場所」が変わった

- DIなし → クラス内部で生成
- DIあり → 外部で生成して渡す

---

## ■ 差し替えの例

EmailService emailService = new SmsService();

※ SmsServiceがEmailServiceと同じ型として扱える場合
（継承 or インターフェース実装）

👉 UserServiceは変更不要

---

## ■ 一番シンプルな理解

- DIなし → new を自分で書く
- DIあり → new を外に追い出す

---

## ■ 「newを外に追い出す」の意味

### ● 責任の分離

- Before：UserService = 作る + 使う
- After：UserService = 使うだけ

---

## ■ Qiita記事からの理解 :contentReference[oaicite:0]{index=0}

## ■ 問題の本質
- クラス内で依存オブジェクトを生成すると
- 外部環境に強く依存してしまう

例：
- ネットワーク
- 時刻
- ランダム

---

## ■ 問題
👉 テストできない

- 結果が毎回変わる
- 外部環境に依存
- 正しさを検証できない

---

## ■ DIの本質

> 依存関係を外から注入することで  
> **動作をコントロール可能にすること**

---

## ■ 結論

> DIの目的は  
> **テスト可能性を確保すること**

---

## ■ Springとの関係

## ■ DIはJavaだけでもできる
- 手動でも実装可能

---

## ■ しかし実務では破綻する

依存関係が増えると：

UserController  
 → UserService  
   → UserRepository  
     → DataSource  

さらに：
- SecurityService
- LoggingService
- PaymentService

---

## ■ 手動DIの問題

- 配線が爆発する
- ライフサイクル管理できない
- 設定が分散する
- 差し替えが困難

---

## ■ Springの役割

> **依存関係の生成と配線を自動化する**

---

## ■ 違いの整理

### ● 手動DI
- newを書く
- 配線する
- 管理する

### ● Spring
- newを書かない
- 配線しない
- 管理しない

👉 フレームワークがすべて担当

---

## ■ 最終理解

DIとは：

> **依存関係の生成を外部に任せることで、  
> 動作をコントロール可能にし、テスト可能にする設計**

---

## ■ 自分の理解の変化

最初：
- 「ただnewの位置が違うだけでは？」

途中：
- 「依存が固定されるかどうかの違い」

最終：
- 「テスト可能性と依存制御のための設計」
- 「Springはそれを自動化するための仕組み」

---

# ■ つまずいたポイント②（Spring Bootと各Springの関係）

## ■ 疑問
- Spring Bootの中にSpring FrameworkやSpring Dataが入っているのか？
- それとも全部別物なのか？
- Spring Bootを使うと何が起きているのかが曖昧だった

---

## ■ 最初の誤解
- Spring Boot = Springの本体
- 他のSpring機能が「内包されている」ように感じていた

👉 構造が分からず、全体像が曖昧だった

---

## ■ 調べて分かったこと :contentReference[oaicite:0]{index=0}

### ● 各Springは独立している

- Spring Framework（DIやMVCなどの基盤）
- Spring Data（DB操作）
- Spring Security（認証・認可）

👉 すべて別々のプロジェクト

---

### ● Spring Bootの役割

- 設定を自動化する
- 依存関係をまとめる
- 起動までを簡単にする

👉 「中に入っている」ではなく  
👉 **「まとめて使いやすくする仕組み」**

---

## ■ 実務的な理解

Spring Bootを使うと：

- DI（Spring Framework） → 自動で有効
- Spring Data → 依存追加だけで使用可能
- Spring Security → 同様にすぐ使える

👉 **結果として全部同時に使える**

---

## ■ 気づいたこと

- Spring Bootは「機能そのもの」ではない
- 「使い方を簡単にする仕組み」である

---

## ■ DIとのつながり

- DIはJava単体でも実現可能
- しかし規模が大きくなると管理できない

👉 Spring Boot + Spring Frameworkが  
👉 **DIの生成・配線・管理を自動化している**

---

# ■ 最終理解（全体像）

## ● DI
- 依存関係の生成を外部に任せる設計
- テスト可能性と柔軟性を確保する

---

## ● Spring Framework
- DIなどの基盤機能を提供

---

## ● Spring Boot
- それらをまとめて簡単に使えるようにする

---

## ■ 自分の理解の変化

最初：
- DIもSpringもよく分からない

途中：
- DIはnewの位置の違い？

最終：
- DIは依存関係をコントロールするための設計
- Springはそれを実用レベルで自動化する仕組み
- Spring Bootはそれをさらに簡単に使えるようにしたもの



---
