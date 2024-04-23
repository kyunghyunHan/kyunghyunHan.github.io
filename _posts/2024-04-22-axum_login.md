---
layout: post
title: "Axum과 Diesel,postgresSQL을 사용하여 Login 구현 (1)"
subtitle: "Axum"
date: 2024-04-22 11:15:00
author: "Hebi"
header-img: "img/axum.png"
header-mask: 0.3
catalog: true
tags:
 - Axum
 - rust
 - postgresSQL
 - diesel
---
> Hello Axum


## What is Axum?
- Axum은 Rust를 사용하여 서버측 웹 애플리케이션을 구축하기 위한 프레임워크입니다 .
<img src="/img/axum.png" />

## 설치

- Axum프로젝트를 위해서 먼저 Rust프로젝트를 만들어줍니다.
- rust가 설치되어 있지 않다면 Rust를 먼저 설치해줍니다.

<a src="https://www.rust-lang.org/tools/install">Rust install</a>

- 그다음 axum과 tokio를 설치해줍니다.


```toml
[dependencies]
tokio = { version = "1.36.0", features = ["full"] }
axum = "0.7.5"
```


## Hello Axum

- 디렉토리에는 main.rs와 lib.rs가 있어야합니다.

<img src="/img/axum2.png" />


```rust
//main.rs
use axum_login::run;


#[tokio::main]
async fn main(){
    run().await;
}

```
- lib.rs에 run 함수를 작성해줍니다.
- Axum에 Router을 작성하며 "/"는 기본 페이지입니다.
- axum::serve()를 통해 127.0.0.1:3000에 서버를 시작합니다.

```rust
//lib.rs
pub async fn run() {
    let app = Router::new().route("/", get(index));

    let listener = tokio::net::TcpListener::bind("127.0.0.1:3000")
        .await
        .unwrap();
    println!("listening on {}", listener.local_addr().unwrap());
    axum::serve(listener, app).await.unwrap();
}
```


- index.html을 작성해줍니다.메인페이지가 될것입니다.


```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>Hello World!</h1>
</body>
</html>
```

```
$cargo run
```

을 입력하게 되면 


```
listening on 127.0.0.1:3000
```

이 뜨게 되며 "http://localhost:3000/" 에 접속하게 되면 Hello world가 맞이할것입니다.

<img src="/img/axum3.png" />



## 전체코드

```rust

pub mod db;
pub mod model;
pub mod router;
use axum::{response::Html, routing::get, Router};
use tokio;

pub async fn run() {
    let app = Router::new().route("/", get(index));

    let listener = tokio::net::TcpListener::bind("127.0.0.1:3000")
        .await
        .unwrap();
    println!("listening on {}", listener.local_addr().unwrap());
    axum::serve(listener, app).await.unwrap();
}

async fn index() -> Html<&'static str> {
    let html_content = include_str!("../index.html");
    Html(html_content)
}


```
