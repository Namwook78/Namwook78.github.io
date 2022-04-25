---
title: "webpack환경에서 React 설치하기"

categories:
  - "webpack"
tags:
  - [react, webpack]

toc: true
toc_sticky: true

date: 2021-10-21
last_modified_at: 2021-10-21
---

- 현재 nodejs가 설치 되어 있는지 확인해야 한다.
- nodejs가 없다면 옆의 링크를 통해 다운 후 설치 [nodejs](https://nodejs.org/ko/download/)

## npm 초기화

npm을 실행하기 위해 아래와 깉이 입력

```
npm init
```

설정을 완료 하면 파일 목록에 pakage.json이란 파일이 생성

## npm 설치

리액트를 사용하려면 리액트 관련된 npm과 webpack을 설치해야 한다.

### 1. react 설치

```
npm i react react-dom // react와 react-dom
```

### 2. webpack 설치

```
npm i -D webpack webpack-cli
```

### 3. babel 설치

- 최신 js문법을 구 js 문법으로 변환

```
npm i -D @babel/core @babel/preset-env @babel/preset-react babel-loader
```

## 파일 추가 및 설정

  <img width="200" alt="" src="https://user-images.githubusercontent.com/44824320/138216107-ba723061-ff39-48c7-b231-6ff9fc5d3bb8.png">

### 1. index.html

- 상위 폴더에서 index.html 생성 후 아래와 같이 내용 입력

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>example</title>
  </head>
  <body>
    <div id="root"></div>
    <script src="./dist/app.js"></script>
  </body>
</html>
```

### 2. webpack.config.js

- 상위 폴더에서 webpack.config.js 생성(webpack 설정 파일)

```javascript
const path = require("path");
module.exports = {
  name: "gugudan-setting",
  mode: "development",
  devtool: "eval",
  resolve: {
    extensions: [".jsx", ".js"],
  },

  entry: { app: ["./client"] },

  module: {
    rules: [
      {
        test: /\.jsx?$/,
        loader: "babel-loader",
        options: {
          presets: ["@babel/preset-env", "@babel/preset-react"],
          plugins: ["@babel/plugin-proposal-class-properties"],
        },
      },
    ],
  },

  output: {
    path: path.join(__dirname, "dist"),
    filename: "app.js",
  },
};
```

### 3. client.jsx

- 리액트 최상위 파일

```javascript
const React = require("react");
const ReactDOM = require("react-dom");
const WebExample = require("./example");

ReactDOM.render(<WebExample />, document.getElementById("root"));
```

### 4. example.jsx

- 실질적으로 실행될 리액트

```javascript
const React = require("react");
const { Component } = React;
class WebExample extends Component {
  state = {
    text: "Hello webpack",
  };
  render() {
    return <h1>{this.state.text}</h1>;
  }
}
module.exports = WebExample;
```

<br />

## 실행하기

- 콘솔창에 아래와 같이 입력하면 dist폴더와 아래에 app.js가 생성된다.

  ```
  npx webpack
  ```

  <img width="130" alt="스크린샷 2021-10-21 오후 2 13 34" src="https://user-images.githubusercontent.com/44824320/138215875-02243b3a-1caa-464a-a580-ed999ca22d82.png">

<br />

<p>index.html 실행하면 리액트로 만들었다는 표시가 뜸.</p>

<img width="500" alt="스크린샷 2021-10-21 오후 2 29 32" src="https://user-images.githubusercontent.com/44824320/138217116-50c1c2d6-1400-4771-8824-70b42545ad67.png">
<br />

## Hot reloading

- 리액트 코드를 수정하고나서 npx webpack을 꼭 해 줘야 변경이 완료
- 만일 코드 수정 후 npx webpack을 실행 해주지 않으면 코드변경이 되지 않아 에러 발생율이 높아짐
- 그렇기 떄문에 코드 저장 후 자동적으로 webpack이 실행 되도록 설정 되어야 한다.

### 1. 설치

```
npm i react-refresh @pmmmwh/react-refresh-webpack-plugin -D
```

```
npm i -D webpack-dev-server
```

### 2. 설정

- package.json파일에서 수정

  ```json
  "script":{
    "dev": "webpack serve --env development"
  }
  ```

- webpack.config.js 수정

  ```javascript
  const path = require("path");
  const RefreshWebpackPlugins = require("@pmmmwh/react-refresh-webpack-plugin"); // 변수 선언
  module.exports = {
    ...
    ...
    module: {
      rules: [
        {
          test: /\.jsx?$/,
          loader: "babel-loader",
          options: {
            presets: [["@babel/preset-env", {targets: {browsers: ["> 1% in KR"]}, debug: true}], "@babel/preset-react"],
            plugins: ["@babel/plugin-proposal-class-properties", "react-refresh/babel"], // react-refresh/babel 추가
          },
        },
      ],
    },
    plugins: [
      new RefreshWebpackPlugins() //  new RefreshWebpackPlugins() 추가
    ],
    ...
    ...
    devServer: {
       devMiddleware: { publicPath: "/dist/" }, // webpack이 생성해주는 경로
       static: { directory: path.resolve(__dirname)} // 실제로 존재하는 정적 파일 경로
    }, // devServer 추가
    ...
  }
  ```

### 3. 실행

```
npm run dev
```

- 터미널에서 아래 명령어 입력 하면 밑의 사진 처럼 log 정보가 표시되면서 서버가 실행 된다.
- log 정보 중에서 loopback URL을 브라우저에 복사 붙여넣기 하면 리액트로 만든 사이트가 나타난다.

<br />

<div align="center">
<img width="700" alt="스크린샷 2021-10-21 오후 3 30 14" src="https://user-images.githubusercontent.com/44824320/138223617-f22f17c9-e27d-4b1c-b324-d03b3be3b74e.png">
<p>[devServer log]</p>
<br />
<img width="600" alt="" src="https://user-images.githubusercontent.com/44824320/138226493-cad7891a-8e39-427e-ba3c-a969645ba113.gif">
<p>[Hot reloading 적용]</p>
</div>

---

## 참조

- [webpack 공식 문서](https://webpack.js.org/)
- [제로초 react 강좌](https://www.youtube.com/playlist?list=PLcqDmjxt30RtqbStQqk-eYMK8N-1SYIFn)
