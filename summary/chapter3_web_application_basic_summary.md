# 第3章まとめ：Webアプリケーション開発・はじめの一歩

## ■ 概要
- Spring Bootで実際に動くWebアプリを作成し、MVCモデル（画面・処理・データ分離）の基本を理解する

---

## ■ 3-1 シンプルなWebアプリケーション

### ■ 使用コード

#### ● Controller（Java）

    package com.example.demo.hello;

    import org.springframework.stereotype.Controller;
    import org.springframework.web.bind.annotation.GetMapping;

    @Controller
    public class HelloController {

        @GetMapping("/hello")
        public String getHello() {
            return "hello";
        }
    }

---

#### ● HTML（hello.html）

    <!DOCTYPE html>
    <html xmlns:th="http://www.thymeleaf.org">
    <head>
        <meta charset="UTF-8">
        <title>Hello World</title>
    </head>
    <body>
        <h1>Hello World</h1>
    </body>
    </html>

---

### ■ 何をしているか

- ブラウザで `/hello` にアクセスする
- Controllerの `@GetMapping("/hello")` が実行される
- `return "hello"` により templates/hello.html が返される
- 「Hello World」が表示される

---

### ■ 処理の流れ（GET）

ブラウザ → GET /hello  
↓  
Controller実行  
↓  
return "hello"  
↓  
templates/hello.html  
↓  
ブラウザ表示  

---

## ■ 3-2 画面から別画面に値を渡す

### ■ 使用コード

#### ● HTML（hello.html）

    <!DOCTYPE html>
    <html xmlns:th="http://www.thymeleaf.org">
    <head>
        <meta charset="UTF-8">
        <title>Hello World</title>
    </head>
    <body>
        <h1>Hello World</h1>

        <form method="post" action="/hello">
            <input type="text" name="text1" />
            <input type="submit" value="クリック" />
        </form>

    </body>
    </html>

---

#### ● Controller（Java）

    package com.example.demo.hello;

    import org.springframework.stereotype.Controller;
    import org.springframework.ui.Model;
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.PostMapping;
    import org.springframework.web.bind.annotation.RequestParam;

    @Controller
    public class HelloController {

        @GetMapping("/hello")
        public String getHello() {
            return "hello";
        }

        @PostMapping("/hello")
        public String postHello(@RequestParam("text1") String text1, Model model) {
            model.addAttribute("response", text1);
            return "hello/response";
        }
    }

---

#### ● HTML（response.html）

    <!DOCTYPE html>
    <html xmlns:th="http://www.thymeleaf.org">
    <head>
        <meta charset="UTF-8">
        <title>Hello Response</title>
    </head>
    <body>
        <h1>Hello Response</h1>
        <p th:text="${response}"></p>
    </body>
    </html>

---

### ■ 何をしているか

- 入力欄に文字を入力して送信する
- POST /hello が送られる
- @PostMapping が実行される
- 入力値が text1 に入る
- Modelに response として保存する
- response.html に遷移する
- Thymeleafが値を埋め込む
- 入力した文字が画面に表示される

---

### ■ 処理の流れ（POST）

GET /hello  
↓  
画面表示  
↓  
入力（例：Spring）  
↓  
POST /hello  
↓  
@PostMapping 実行  
↓  
@RequestParam → text1 = Spring  
↓  
Model → response = Spring  
↓  
return "hello/response"  
↓  
response.html  
↓  
th:text により表示  
↓  
Spring 表示  

---

## ■ 重要ポイント

- form → データ送信の定義
- input → 入力値を保持
- @PostMapping → POST受信
- @RequestParam → 入力値取得
- Model → データ受け渡し
- Thymeleaf → HTML書き換え

---

## ■ GETとPOSTの違い

| 項目 | GET | POST |
|------|-----|------|
| 用途 | 画面表示 | データ送信 |
| データ位置 | URL | ボディ |

---

## ■ 最終まとめ

- GET → 画面表示  
- POST → データ送信  
- Controller → 処理  
- Model → データ渡す  
- Thymeleaf → 表示  

入力 → 受け取り → 渡す → 表示
