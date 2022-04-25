---
title: "webpack 설정하기"

categories:
  - "webpack"
tags:
  - [react, webpack]

toc: true
toc_sticky: true

date: 2021-10-20
last_modified_at: 2021-10-20
---

## webpack ❓

최신문법을 구버전의 브라우저에서 실행 할수 있게 도와 주고 여러가지 js파일들을 하나의 js파일로 변환(babel적용, 중복제거)해주는 자바스크립트 모듈 번들러

## webpack 구조

### 첫부분

```javascript
 name: "wordrelay-setting",
  mode: "development", //실서비스는 production으로 바꾸면 됨 -> mode : "production"
  devtool: "eval", //개발용은 eval, 실제배포용은 hidden-source-map
  resolve: {
    extensions: [".js", ".jsx"], //파일명 뒤의 확장자를 자동적으로 찾아줌.
  },
```

### entry

- webpack에 넣을 파일들을 설정 (입력)

```javascript
entry: {
    app: ["./client1",'./client2'],
    //resolve의 extensions을 설정해서 뒤의 확장자는 안붙여도 실행 됨
    //배열로 설정하면 여러 파일 추가
  },
```

### module(Loaders)

- entry에 있는 파일을 읽고 ,파일에서 요구하는 모듈을 적용, 아웃픗으로 이동시킴

```javascript
  module: {
    rules: [
      //여러개의 규칙들을 적용, 배열형태
      {
        test: /\.jsx?/, //규칙 적용할 파일들(js파일과 jsx파일을 규칙을 적용)
        loader: "babel-loader", //babel 규칙을 적용
        options: {
          //babel 옵션
          presets: [ //presets는 plugin의 모임
            [
              "@babel/preset-env",
              {
                //@babel/preset-env의 설정을 적용할때 배열형태로씀
                targets: {
                   // @babel/preset-env의 옵션
                   // 자동으로 최신 코드를 browsers에서 설정한 브라우저에 맞게 코드를 변환,
                   // 원하는 브라우저에서만 지원 가능
                  browsers: ["> 5% in KR", "last 2 chrome versions"], // 자동으로 어떤 browser에 적용해야 할지 설정
                },
              },
            ],
            "@babel/preset-react",
          ],
          plugins: [
            "@babel/plugin-proposal-class-properties",
            "react-refresh/babel",
          ],
        },
      },
    ],
  },
```

### plugins

- 확장 프로그램, 추가 적인 작업

```javascript
 plugins: [
    //여기에 사용할 플러그인을 나열한다.
    new webpack.LoaderOptionsPlugin({ debug: true }), //로더 옵션을 debug로 모두 설정
    new RefreshWebpackPlugin(),
  ],
```

### output

- entry에서 설정된 파일을 webpack으로 통해 통합된 파일 출력 (출력)

```javascript
  output: {
    path: path.join(__dirname, "dist"), //__dirname : 현재폴더,
    filename: "app.js",
    publicPath: "/dist/",
  }, //출력
```

## 전체 webpack.config.js 구조

```javascript
const path = require("path");
const webpack = require("webpack");
const RefreshWebpackPlugin = require("@pmmmwh/react-refresh-webpack-plugin");

module.exports = {
  name: "wordrelay-setting",
  mode: "development",
  devtool: "eval",
  resolve: {
    extensions: [".js", ".jsx"],
  },

  entry: {
    app: ["./client"],
  },

  module: {
    rules: [
      {
        test: /\.jsx?/,
        loader: "babel-loader",
        options: {
          presets: [
            [
              "@babel/preset-env",
              {
                targets: {
                  browsers: ["> 5% in KR", "last 2 chrome versions"],
                },
              },
            ],
            "@babel/preset-react",
          ],
          plugins: [
            "@babel/plugin-proposal-class-properties",
            "react-refresh/babel",
          ],
        },
      },
    ],
  },

  plugins: [
    new webpack.LoaderOptionsPlugin({ debug: true }),
    new RefreshWebpackPlugin(),
  ],

  output: {
    path: path.join(__dirname, "dist"),
    filename: "app.js",
    publicPath: "/dist/",
  },

  devServer: {
    publicPath: "/dist/",
    hot: true,
  },
};
```

---

## 참조

- [webpack 공식 문서](https://webpack.js.org/)
- [제로초 react 강좌](https://www.youtube.com/playlist?list=PLcqDmjxt30RtqbStQqk-eYMK8N-1SYIFn)
