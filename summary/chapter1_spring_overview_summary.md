# 第1章まとめ：SpringとSpring Boot

## ■ 概要
- アプリケーション開発では、ビジネスの成長に伴い機能が増加し、  
  データベース・セキュリティ・クラウド対応など多くの要素が必要になる
- これらをすべてゼロから開発するのは非効率

→ そこで使われるのが **Javaの代表的フレームワーク「Spring」**

---

## ■ Springが使われる理由

### 1. 開発効率が高い
- 共通の仕組みが用意されているため、  
  簡単かつスピーディに開発できる

### 2. 標準でセキュア
- セキュリティが最初から考慮されており、  
  安全なアプリを構築しやすい

### 3. 機能が豊富
- よく使う機能が揃っており、  
  小規模〜大規模まで幅広いプロジェクトに対応可能

### 4. 常に進化している
- 継続的に開発されており、  
  クラウドなど最新技術にも対応

---

## ■ 補足
- Springは多数の機能を持ち、それぞれが  
  **プロジェクト単位で分割・管理されている**

---

# Spring Framework（基盤）

## ■ 概要
- Springの中核となる機能群
- アプリケーション開発の「土台」を提供する

---

## ■ 主な機能

### 1. 依存性の注入（DI: Dependency Injection）
- プログラムの部品（オブジェクト）を外部から渡す仕組み
- 部品同士の結びつきを弱くし、交換しやすくする

### 2. Spring MVC
- Webアプリケーション開発の基本フレームワーク

### 3. トランザクション管理
- データベース処理の失敗を防ぎ、一貫性を保つ

---

## ■ 補足（DIの重要性）
- DIはSpringの中核技術
- 柔軟性・保守性・テスト容易性を大きく向上させる

---

## ■ 依存性の注入（DI）の具体例

### ● イメージ
- DIなし：自分で部品を作る（固定される）
- DIあり：外から部品をもらう（交換できる）

---

### ● コード例

#### ❌ DIなし
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

#### ⭕ DIあり
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

### ● メリット
- 機能の差し替えが容易（Email → SMSなど）
- テストがしやすい
- 大規模開発に適した設計になる

---

# Spring Data（データベース操作）

## ■ 概要
- データベースごとに操作方法が異なると開発が複雑になる
- Spring Dataはそれを統一する仕組み

👉 異なるDBでも「同じ書き方」で操作可能

---

## ■ CRUD操作
- Create：登録  
- Read：取得  
- Update：更新  
- Delete：削除  

---

## ■ コード例①（基本CRUD）

    public interface EmployeeRepository extends CrudRepository<Employee, String> {

    }

### ● ポイント
- インターフェース定義だけでCRUDが利用可能
- SQLをほぼ書かずに済む

---

## ■ コード例②（検索）

    public interface EmployeeRepository extends CrudRepository<Employee, String> {

        public List<Employee> findByLastName(String lastName);

    }

### ● 解説
- メソッド名からクエリが自動生成される

    SELECT * FROM employee WHERE last_name = ?

---

## ■ メリット
- DBに依存しない
- コードがシンプル
- 開発スピード向上

---

# Spring Security / Batch / Cloud

## ■ Spring Security
- 認証（ログイン）と認可（権限制御）を提供
- アプリケーションの安全性を担保

---

## ■ Spring Batch
- 大量データ処理をまとめて実行
- 例：バックアップ、集計処理

---

## ■ Spring Cloud
- AWSやAzureなどクラウドサービスとの連携を簡素化
- クラウド前提の開発を効率化

---

# Spring Boot

## ■ 概要
- Springは高機能だが、初期設定や環境構築が複雑
- それを解決するのがSpring Boot

👉 「すぐ開発できる状態」を作るツール

---

## ■ 解決する課題
- 設定の複雑さ（XMLなど）
- サーバー準備の手間（Tomcatなど）

---

## ■ 「設定より規約」

### ● デフォルト設定
- よく使われる設定を自動適用

👉 例  
- Webアプリ選択でTomcatが自動組み込み

---

### ● 処理の自動生成
- 規約に従えばコードを自動生成

👉 例  
- findByLastName → 検索処理生成

---

## ■ メリット
- 初期設定を大幅削減
- すぐ開発開始できる
- ビジネスロジックに集中可能

---

# ■ 最終まとめ
- フレームワーク：開発の土台
- Spring：高機能なJavaフレームワーク
- Spring Boot：その複雑さを解消し、開発を加速させる仕組み

👉 結論  
Spring Bootにより、Springの機能を「実用的な速度」で扱えるようになる
