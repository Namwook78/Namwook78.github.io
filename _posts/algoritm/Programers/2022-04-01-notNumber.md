---
title: "[nodejs][프로그래머스] 없는 숫자 더하기"

categories:
  - "programers"
tags:
  - [nodejs, algorithm]

toc: true
toc_sticky: true

date: 2022-04-01
last_modified_at: 2022-04-01
---

## ❓문제

0부터 9까지의 숫자 중 일부가 들어있는 정수 배열 `numbers`가 매개변수로 주어집니다. `numbers`에서 찾을 수 없는 0부터 9까지의 숫자를 모두 찾아 더한 수를 return 하도록 solution 함수를 완성해주세요.

## ⁉️ 제한 조건

- 1 ≤ numbers의 길이 ≤ 9
  - 0 ≤ numbers의 모든 원소 ≤ 9
  - numbers의 모든 원소는 서로 다릅니다.

## 📥 입력 📤 출력

<img style="margin-left:20px;"  width="200" alt="test" src="/assets/img/algoritm/programers/notNumber.png">

### 입출력 예 #1

- 5, 9가 `numbers`에 없으므로, 5 + 9 = 14를 return 해야 합니다.

### 입출력 예 #2

- 1, 2, 3이 `numbers`에 없으므로, 1 + 2 + 3 = 6을 return 해야 합니다.

## 📝 풀이 코드

```js
function solution(numbers) {
  arr = [];
  for (let i = 0; i <= 9; i++) {
    arr.push(i); // arr 배열에 0부터 9까지 요소를 저장
  }
  numbers.sort((a, b) => a - b); // numbers의 배열이 정렬이 되지 않아 정렬
  return arr.filter((x) => !numbers.includes(x)).reduce((a, b) => a + b);
  //  numbers에 arr의 요소가 포함되어 있지 않은 것을 출력하여 더함
}
```

## ❗️ 정리

- 0 부터 9까지 배열을 만들고 나서 solution으로 들어오는 numbers 배열과 비교
- 같지 않은 것을 모아 더한 값을 리턴
