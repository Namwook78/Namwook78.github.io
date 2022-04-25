---
title: "[nodejs][프로그래머스] 숫자 문자열과 영단어"

categories:
  - "programers"
tags:
  - [nodejs, algorithm]

toc: true
toc_sticky: true

date: 2022-04-02
last_modified_at: 2022-04-02
---

## ❓문제

<img style="margin-left:20px;"  width="500" alt="numberWord" src="/assets/img/algoritm/programers/numberWord.png">

네오와 프로도가 숫자놀이를 하고 있습니다. <br />
네오가 프로도에게 숫자를 건넬 때 일부 자릿수를 영단어로 바꾼 카드를 건네주면 프로도는 원래 숫자를 찾는 게임입니다.

다음은 숫자의 일부 자릿수를 영단어로 바꾸는 예시입니다.

- 1478 → "one4seveneight"
- 234567 → "23four5six7"
- 10203 → "1zerotwozero3"

이렇게 숫자의 일부 자릿수가 영단어로 바뀌어졌거나, 혹은 바뀌지 않고 그대로인 문자열 `s`가 매개변수로 주어집니다. <br />
`s`가 의미하는 원래 숫자를 return 하도록 solution 함수를 완성해주세요.

참고로 각 숫자에 대응되는 영단어는 다음 표와 같습니다.

<img style="margin-left:20px;"  width="200" alt="numberWord" src="/assets/img/algoritm/programers/numberWord2.png">

## ⁉️ 제한 조건

- 1 ≤ `s`의 길이 ≤ 50
- `s`가 "zero" 또는 "0"으로 시작하는 경우는 주어지지 않습니다.
- return 값이 1 이상 2,000,000,000 이하의 정수가 되는 올바른 입력만 `s`로 주어집니다.

## 📥 입력 📤 출력

<img style="margin-left:20px;"  width="300" alt="numberWord" src="/assets/img/algoritm/programers/numberWord1.png">

### 입출력 예 #1

- 문제 예시와 같습니다.

### 입출력 예 #2

- 문제 예시와 같습니다.

### 입출력 예 #3

- "three"는 3, "siz"는 6, "seven"은 7에 대응하기 떄문에 정답은 입출력 예 #2와 같은 234567이 됩니다.
- 입출력 예 #2와 #3과 같이 같은 정답을 가리키는 문자열이 여러 가지가 나올 수 있습니다.

### 입출력 예 #4

- `s`에는 영단어로 바뀐 부분이 없습니다.

## 📝 풀이 코드

```js
function solution(s) {
  const numbers = [
    "zero",
    "one",
    "two",
    "three",
    "four",
    "five",
    "six",
    "seven",
    "eight",
    "nine",
  ];

  for (let i = 0; i < numbers.length; i++) {
    var regexp = new RegExp(numbers[i], "gi"); // 정규표현식 선언
    s = s.replace(regexp, i); // 정규표현식을 이용하여 문자를 나누고 그 문자는 숫자로 변환
  }
  return Number(s);
}
```

## ❗️ 정리

- 문자열을 구분할 수 있는 기준이 없기 때문에 정규표현식으로 문자열을 나눔,
- replace() 함수 : str.replace("찾을 문자열", "변경할 문자열"),
- 정규표현식 str.replace(/찾을 문자열/gi, "변경할 문자열"),
  - g는 전체 모든 문자열 변경(global),
  - i는 영문 대문자를 무시, 모두 일치하는 패턴 검색(ignore)
