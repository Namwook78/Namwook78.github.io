---
title: "[nodejs][프로그래머스] 내적"

categories:
  - "programers"
tags:
  - [nodejs, algorithm]

toc: true
toc_sticky: true

date: 2022-03-31
last_modified_at: 2022-03-31
---

## ❓문제

길이가 같은 두 1차원 정수 배열 a, b가 매개변수로 주어집니다. a와 b의 내적을 return 하도록 solution 함수를 완성해주세요. <br />
이때, a와 b의 내적은 `a[0]*b[0] + a[1]*b[1] + ... + a[n-1]\*b[n-1]` 입니다. (n은 a, b의 길이)

## ⁉️ 제한 조건

- a, b의 길이는 1 이상 1,000 이하입니다.
- a, b의 모든 수는 -1,000 이상 1,000 이하입니다.

## 📥 입력 📤 출력

<img style="margin-left:20px;"  width="300" alt="dotProduct" src="/assets/img/algoritm/programers/dotProduct.png">

### 입출력 예 #1

- a와 b의 내적은 `1*(-3) + 2*(-1) + 3*0 + 4*2 = 3` 입니다.

### 입출력 예 #2

- a와 b의 내적은 `(-1)*1 + 0*0 + 1\*(-1) = -2` 입니다.

## 📝 풀이 코드

```js
function solution(a, b) {
  var answer = 0;

  for (let i = 0; i < a.length; i++) {
    answer += a[i] * b[i];
    // a배열과 b배열의 항목을 더해서 answer 변수에 저장 answer = answer + a[i] * b[i]
  }
  return answer;
}
```

## ❗️ 정리

- 입출력 예를 보고 순서대로 문제를 확인 후 해결하였다.
