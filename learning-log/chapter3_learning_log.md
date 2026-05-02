# 学習ログ：第3章 Webアプリケーション開発・はじめの一歩

## ■ 学び・気づき

- Webアプリケーションは  
  「画面（HTML）」「処理（Controller）」「データ（Model）」に分離して構築される（MVCモデル）

- URLにアクセスすると、そのURLに対応するControllerが呼ばれ、最終的にHTMLが返される仕組みになっている :contentReference[oaicite:0]{index=0}

- return "hello" は単なる文字列ではなく  
  ビュー名（HTMLのあだ名）であり、ViewResolverによってtemplates配下のHTMLに変換される

- localhost:8080 は外部サーバーではなく  
  自分のPC上で動いているTomcatサーバーを指している

- GETとPOSTは役割が異なる  
  GET：画面取得  
  POST：データ送信

- formタグは  
  「どこに（action）」「どの方法で（method）」「何を（input）」送るかを定義している

- inputタグのname属性は  
  サーバー側でデータを識別するキーになる

---

## ■ つまずいたポイント①（HTMLの理解不足）

- inputタグやsubmitボタンの役割が分からなかった
- HTMLが単なる表示ではなく、データ送信の役割を持つことを理解できていなかった

理解後：
- type="text" → 入力欄  
- name → データの名前  
- submit → 送信ボタン  

画面操作とサーバー通信が結びついた

---

## ■ つまずいたポイント②（POST処理の流れ）

- formと@PostMappingの関係が分からなかった

理解後：
- form送信 → POSTリクエスト  
- SpringがURLを見てControllerを特定  
- 対応するメソッドが実行される  

HTMLとJavaの接続が理解できた

---

## ■ つまずいたポイント③（@RequestParamの理解）

- なぜこれで値が取れるのか不明だった

理解後：
- inputのname属性と一致するキーで値が送られる  
- @RequestParamはそのキーを指定して値を取得する  

例：
text1=Spring → text1に代入

---

## ■ つまずいたポイント④（Modelの正体）

- Modelが何なのか分からなかった

理解後：
- ModelはControllerからHTMLへデータを渡すための箱  
- 内部はキーと値のペアで管理されている  

例：
response = Spring

HTMLはControllerの変数を直接参照できないため、Modelを介する必要がある

---

## ■ つまずいたポイント⑤（名前を付ける理由）

- なぜresponseという名前が必要か疑問だった

理解後：
- HTMLがデータを取り出すためにはキーが必要  
- addAttributeで名前付きで保存する必要がある  

---

## ■ つまずいたポイント⑥（Thymeleafの範囲）

- どこまでがThymeleafなのか分からなかった

理解後：
- th:が付いている部分だけがThymeleaf  
- それ以外は通常のHTML  

例：

<p th:text="${response}"></p>

<p> → HTML  
th:text → Thymeleaf  
${response} → EL式  

---

## ■ つまずいたポイント⑦（EL式の理解）

- ${}の意味が分からなかった

理解後：
- EL式は値を取り出すための記法  
- ${response} は response の値を取得する  

---

## ■ つまずいたポイント⑧（JSPとの違い）

- 現在のコードがJSPなのか混乱した

理解後：
- JSPはHTMLとJavaが混在する古い技術  

例：
<%= response %>

- 現在はThymeleafで分離されている  

例：
<p th:text="${response}"></p>

教科書のJSP説明は歴史的背景である

---

## ■ つまずいたポイント⑨（Modelが見えない理由）

- HTMLにModelが書かれていないのに使える理由が不明だった

理解後：
- ThymeleafはModelを前提として自動参照する  
- ${response} はModelの中から値を探す  

---

## ■ つまずいたポイント⑩（テンプレートエンジン）

- Thymeleafが何なのか曖昧だった

理解後：
- HTMLとデータを結合して完成HTMLを生成する仕組み  
- Javaのライブラリとして実装されている  

---

## ■ 最終理解（全体像）

ブラウザ → リクエスト（GET/POST）  
→ Controller  
→ Modelにデータ保存  
→ ThymeleafがHTMLを処理  
→ 完成HTML生成  
→ ブラウザ表示  

---

## ■ 自分の理解の変化

最初：
- HTMLとJavaの関係が不明
- Model・Thymeleaf・EL式の違いが分からない

途中：
- POSTの流れは理解できたが内部が曖昧

最終：
- MVCの役割を理解
- データの流れを理解
- Thymeleafの処理範囲を理解
- JSPとの違いを整理できた
