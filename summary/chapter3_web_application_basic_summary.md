# 第3章まとめ：Webアプリケーション開発・はじめの一歩

## ■ 概要
- Spring Bootで実際に動くWebアプリを作成し、MVCモデル（画面・処理・データ分離）の基本を理解する

---

## ■ 3-1 シンプルなWebアプリケーション

### ■ 手順

1. プロジェクト構成
- Controllerクラス作成
- templatesフォルダにHTML配置

2. HTML作成（hello.html）
- Thymeleaf宣言を記述
- Hello World表示

3. Controller作成

    @GetMapping("/hello")
    public String getHello() {
        return "hello";
    }

4. アプリ起動・確認
- http://localhost:8080/hello にアクセス

---

### ■ 処理の流れ（GET）

- ブラウザ → GET /hello  
- Spring → @GetMapping("/hello") を探す  
- Controller実行 → return "hello"  
- ViewResolver → templates/hello.html に変換  
- HTMLを返して表示  

---

### ■ ポイント

- @Controller → コントローラー定義  
- @GetMapping → URLと処理の紐付け  
- return → 表示するHTML（templatesからの相対パス）  

---

## ■ 3-2 画面から別画面に値を渡す

### ■ 手順

1. フォーム追加（hello.html）

    <form method="post" action="/hello">
        <input type="text" name="text1" />
        <input type="submit" value="クリック" />
    </form>

2. Controller修正

    @PostMapping("/hello")
    public String postHello(@RequestParam("text1") String text1, Model model) {
        model.addAttribute("response", text1);
        return "hello/response";
    }

3. 表示用HTML作成（response.html）

    <p th:text="${response}"></p>

---

### ■ 処理の流れ（POST）

① GET /hello → 画面表示  
↓  
② 入力 → text1=Spring  
↓  
③ POST /hello  
↓  
④ @PostMapping 実行  
↓  
⑤ @RequestParam → text1取得  
↓  
⑥ Model → responseとして保存  
↓  
⑦ return "hello/response"  
↓  
⑧ response.html表示  
↓  
⑨ th:text → 値を埋め込み表示  

---

### ■ ポイント

- method="post" → POST送信  
- action="/hello" → Controllerへ送信  
- @PostMapping → POST受信  
- @RequestParam → 入力値取得  
- Model → データ受け渡し  
- addAttribute → 名前＋値で保存  

---

### ■ Thymeleaf

    <p th:text="${response}"></p>

- ${} → 値を取り出す（EL式）  
- th:text → 表示する  
- Thymeleaf → HTMLを書き換える  

---

## ■ GETとPOSTの違い

| 項目 | GET | POST |
|------|-----|------|
| 用途 | 取得 | 送信 |
| 送信方法 | URL | ボディ |

---

## ■ 最終まとめ

- GET → 画面表示  
- POST → データ送信  
- Controller → 処理  
- Model → データ渡す  
- Thymeleaf → 表示  

👉 入力 → 受け取り → 渡す → 表示
