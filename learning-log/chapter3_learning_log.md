# 学習ログ：第3章 Webアプリケーション開発・はじめの一歩（詳細版）

---

# ■ この章の特徴と学習方針

この章はこれまでのJava単体の学習とは異なり、

- HTML
- HTTP（GET / POST）
- Spring（Controller, Model）
- テンプレートエンジン（Thymeleaf）

といった複数の概念が同時に登場する。

そのため、流れを追おうとしても理解が追いつかず、

「何が分からないのか分からない状態」

に陥った。

そこで今回は、

- まず分からないワード・文法・仕組みを全て洗い出す
- それぞれを単体で理解する
- その後で全体の流れに戻る

という順序で復習を行った。

---

# ■ 事前知識の整理（用語・仕組みの具体理解）

---

## ◆ HTTPのGETメソッド

GETはデータを「取得する」ためのリクエストである。

主な用途：
- 商品一覧を見る
- ページを表示する

特徴：
- サーバーの状態を変えない
- URLにデータが表示される

例：
https://example.com/search?q=apple

このときの

q=apple は

- q → キー（データの名前）
- apple → 値（データ本体）

つまり、

URLの一部として「キーと値のデータ」がそのまま表示されている。

---

## ◆ HTTPのPOSTメソッド

POSTはデータを「送る」ためのリクエストである。

主な用途：
- ログイン
- 会員登録
- フォーム送信

特徴：
- サーバーの状態が変わる
- URLにデータが表示されない

疑問：
URLに出ないならデータはどこにあるのか？

理解：
データはリクエストの「body（見えない部分）」に含まれている。

---

## ◆ テンプレートエンジン

HTMLの空欄にデータを埋めて完成させる仕組み。

従来：
<p>Hello Naoki</p>（固定）

テンプレート：
<p>Hello ○○</p>（後から変わる）

例：

<p th:text="${name}"></p>  
model.addAttribute("name", "Naoki");

結果：

<p>Naoki</p>

つまり、

HTMLは最初から完成しているのではなく、  
後からデータを埋め込んで完成することがある。

---

## ◆ Thymeleaf

Spring Bootで使われるテンプレートエンジン。

重要ポイント：

<p th:text="${name}"></p>

- th:text → このタグの中身を書き換える
- ${name} → nameというデータを取り出す

組み合わせると、

「nameの値をここに表示する」

という意味になる。

---

## ◆ Model

ControllerからHTMLにデータを渡すための箱。

例：

model.addAttribute("name", "Naoki");

意味：

- "name" → キー（ラベル）
- "Naoki" → 値

Modelの中では

name → "Naoki"

という形で保持される。

重要：

これは変数ではなく、

「キーと値のセット」

である。

---

## ◆ コントローラー

ブラウザからのリクエストを受け取り、どの画面を返すか決める役。

例：

@GetMapping("/hello")  
public String getHello() {  
    return "hello";  
}

意味：

- /hello にアクセスが来たらこのメソッドを実行
- return "hello" → templates/hello.html を表示

---

## 【疑問】

### ① @GetMappingは何か

結論：

Springが提供しているアノテーションであり、

「/hello へのGETリクエストはこのメソッドで処理する」

という意味を持つ。

Java単体では意味を持たない。

---

### ② なぜ return "hello" でHTMLが表示されるのか

Springのルール：

- returnの文字列は「ビュー名」
- templatesフォルダから対応するHTMLを探す

これを行っているのがViewResolver

---

### ③ Controllerの構成

- 1つにまとめることも可能
- 複数に分けることも可能（実務では分割）

---

### ④ ViewResolverの注意点

同名ファイルが複数ある場合：

return "hello" では特定できない

→ 相対パスが必要

例：
return "test/hello"

---

### ⑤ URLは自分で決める

@GetMappingで定義したものだけが有効

---

### ⑥ @Controllerの役割

Springにクラスを認識させる

これがないとURLアクセスしても反応しない

---

## ◆ formタグ

入力データをまとめてサーバーに送るための箱。

段階的理解：

1. <form></form> → 空の箱
2. input追加 → 入力だけ可能
3. button追加 → 送信可能
4. action / method追加 → 完全

完成形：

<form action="/submit" method="post">
    <input type="text" name="fruit">
    <button type="submit">送信</button>
</form>

意味：

入力データをfruitという名前で、/submitにPOSTで送る

---

## ◆ inputタグ

<input type="text" name="fruit">

- type → 入力の種類（文字）
- name → データのラベル

重要：

nameは変数ではなく、

「送信時のキー」

---

## ◆ buttonタグ

<button type="submit">送信</button>

押すことでPOSTリクエストが発生する

---

## ◆ @PostMapping

@PostMapping("/submit")

意味：

「/submit に対してPOSTリクエストが来たときに、このメソッドを実行する」という指定である。

ここで重要なのは「URLだけでなく、HTTPメソッド（GETかPOSTか）も含めて判断している」という点。

例えば同じ /submit でも：

- GET /submit → @GetMapping が処理
- POST /submit → @PostMapping が処理

というように、同じURLでも別の処理として扱われる。

実際の流れ：

<form action="/submit" method="post">
<input type="text" name="fruit">
<button type="submit">送信</button>
</form>

ユーザーが「apple」と入力して送信すると、

POST /submit  
fruit=apple  

というリクエストがサーバーに送られる。

Springはこのリクエストを受け取り、

1. URLが /submit
2. メソッドが POST

であることを確認し、

@PostMapping("/submit")

に対応するメソッドを呼び出す。

つまり@PostMappingは、

「どのリクエストを、どのJavaメソッドで処理するかを決めるルーティングの役割」

を持っている。


---

## ◆ @RequestParam

最大の疑問ポイント。

理由：

Javaは

「fruitがどこから来たか分からない」

ため、

@RequestParam String fruit

と書くことで、

「fruitという名前のデータをこの引数に入れる」

と明示する。

---

## ◆ 処理の流れ（具体）

<form action="/submit" method="post">
<input type="text" name="fruit">
<button type="submit">送信</button>
</form>

ユーザー入力：apple

↓

POST /submit  
fruit=apple

↓

@RequestParamで取得

fruit = "apple"

↓

メソッド実行

return "result"

---

## ◆ Modelの必要性

疑問：

@PostMappingで

public String submitForm(@RequestParam String fruit)

のようにfruitを受け取っているなら、そのままHTMLで使えるのではないか？

結論：

そのままではHTMLには渡らない。

理由：

Java（Controller）とHTML（Thymeleaf）は、直接同じ変数を共有しているわけではないため。

より具体的に説明すると、

Controllerの中で

fruit = "apple"

という状態になっていても、

その値はあくまで「Javaのメソッド内のローカル変数」であり、

そのままではHTML側には一切見えない。

HTML側（Thymeleaf）は、

Controllerの変数を直接参照することはできず、

参照できるのは「Modelに登録されたデータだけ」である。

つまり、

Controller（Java）側：
fruit = "apple"（ローカル変数）

HTML側：
この変数は存在しないため、何も表示できない

ここで必要になるのがModelである。

model.addAttribute("fruit", fruit);

とすることで、

- "fruit" → データの名前（キー）
- fruit → 実際の値（"apple"）

をModelに登録する。

すると内部的には、

Modelの中に

fruit → "apple"

というデータが保持される。

その状態で

return "result";

とすると、

Springは

- result.html に遷移する
- 同時にModelの中身もHTMLに渡す

という処理を行う。

その結果、HTML側で

<p th:text="${fruit}"></p>

と書くと、

Modelの中からfruitを探し、

"apple" を取り出して表示することができる。

つまりModelの役割は、

「Javaの変数を、そのまま渡すのではなく、HTMLから参照可能な形に変換して橋渡しすること」

である。

---

## ◆ Model + addAttribute

model.addAttribute("fruit", fruit);

意味：

この1行は、

「Javaの変数で持っている値を、HTML側から参照できる形に登録する処理」

である。

より具体的には、

- "fruit" → HTMLから取り出すときの名前（キー）
- fruit → Javaの変数（例："apple"）

という対応関係を作っている。

例えば、

@RequestParam String fruit

によって

fruit = "apple"

という状態になっていた場合、

model.addAttribute("fruit", fruit);

を実行すると、Modelの中は次のようになる：

fruit → "apple"

ここで重要なのは、

これは「変数を渡している」のではなく、

「キーと値のセットとして登録している」という点である。

Javaのローカル変数はそのままではHTMLに渡らないため、

Modelという入れ物に入れることで、

初めてHTML側から参照できる状態になる。

---

## ◆ HTML側（Thymeleaf）

<p th:text="${fruit}"></p>

意味：

このコードは、

「Modelの中からfruitという名前のデータを取り出して、このタグの中身として表示する」

という処理を行う。

流れとしては：

1. Controller側で  
   fruit → "apple" をModelに登録

2. return "result" によって result.html に遷移

3. ThymeleafがHTMLを処理する際に  
   ${fruit} を評価する

4. Modelの中から  
   fruit → "apple" を取得

5. 最終的にHTMLは

<p>apple</p>

としてブラウザに送られる

ここで重要なのは、

th:textは「HTMLの表示内容を書き換える」役割を持つという点である。

---

## ◆ EL式

${...} の形で記述されるものをEL式（Expression Language）という。

役割：

「Modelに登録されたデータを取り出すための記法」

基本動作：

${fruit}

と書くと、

Modelの中から

fruit → "apple"

というデータを探し、

その値である "apple" を取得する。

---

### ■ 具体例

#### ① 単純な値の取得
${fruit}

→ "apple"

---

#### ② オブジェクトのプロパティ取得
${user.name}

→ Modelに

user → { name: "Naoki" }

のようなデータが入っている場合、

userの中のnameを取り出す

---

#### ③ 簡単な計算
${age + 1}

→ Modelに

age → 20

があれば、

21として表示される

---

# ■ 学び・気づき

- HTMLは表示だけでなく通信にも関与する
- データは直接渡せずModelを介する
- キー（名前）がすべてを接続する
- Java単体の理解では不十分

---

# ■ 苦戦した理由

この章が難しかった原因は明確で、

- HTTP
- HTML
- Java
- Spring

が同時に出てきたためである。

それぞれ単体なら理解可能だが、

組み合わさることで急激に難易度が上がった。

---

# ■ 最終理解（全体の流れ）

1. 入力
2. form送信
3. POST発生
4. Controller受信
5. @RequestParamで取得
6. Modelに登録
7. HTMLへ渡す
8. Thymeleafで埋め込み
9. 表示

---

# ■ 自分の理解の変化

最初：
全体像が見えず、個々の意味も曖昧

途中：
流れは分かるが仕組みが不明

最終：
全体の流れと各要素の役割が接続された

---

# ■ 結論

この章は文法ではなく、

「Webアプリケーションの構造そのもの」

を理解する章である。

また、難しく感じた理由は、

Javaの知識不足ではなく、

複数の仕組みを同時に理解する必要があったためである。
