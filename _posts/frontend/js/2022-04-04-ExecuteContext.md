---
title: "실행 컨텍스트"

categories:
  - "js"
tags:
  - [js]

toc: true
toc_sticky: true

date: 2022-04-04
last_modified_at: 2022-04-04
---

# Execute context

category: Javascript

global execute context에서 실행될 때

| a = 1       | Global |
| ----------- | ------ |
| var a = 1   | Global |
| let a = 1   | Script |
| const a = 1 | Script |

function execute context에서 실행될 때

| a = 1       | Global |
| ----------- | ------ |
| var a = 1   | Local  |
| let a = 1   | Local  |
| const a = 1 | Local  |

### 코드

<img style="margin-left:20px;"  width="500" alt="test" src="/assets/img/js/exeCon1.jpeg">
<br />

### 코드 도식화

<img style="margin-left:20px;"  width="1100" alt="test" src="/assets/img/js/exeCon2.jpeg">

<br />

- 참고

[JavaScript - Execute context](https://www.youtube.com/watch?v=QtOF0uMBy7k&t=98s)
