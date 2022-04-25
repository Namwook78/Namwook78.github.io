---
title: "[nodejs][프로그래머스] 완주하지 못한 선수"

categories:
  - "programers"
tags:
  - [nodejs, algorithm]

toc: true
toc_sticky: true

date: 2022-03-23
last_modified_at: 2022-03-23
---

## ❓문제

수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.

마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.

## ⁉️ 제한 조건

- 마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.
- completion의 길이는 participant의 길이보다 1작습니다.
- 참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.
- 참가자 중에는 동명이인이 있을 수 있습니다.

## 📥 입력 📤 출력

<img style="margin-left:20px;"  width="600" alt="test" src="/assets/img/algoritm/programers/nocomplete.png">

### 예제 #1

"leo"는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.

### 예제 #2

"vinko"는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.

### 예제 #3

"mislav"는 참여자 명단에는 두 명이 있지만, 완주자 명단에는 한 명밖에 없기 때문에 한명은 완주하지 못했습니다.

## 📝 풀이 코드

### for문을 이용한 풀이

```js
function solution(participant, completion) {
  participant.sort(); // 알파벳으로 정렬
  completion.sort(); // 알파벳으로 정렬
  for (let i = 0; i < participant.length; i++) {
    if (participant[i] !== completion[i]) {
      // participant요소와 completion 요소가 같지 않을때(participant가 완주 못했을때)
      // 같은 경우는 participant가 완주 할 경우
      return participant[i];
      // 조건에 맞는 index i의 요소를 출력한다.
    }
  }
}
```

### 다른분의 해시 맵 풀이

```js
function solution(participant, completion) {
  const hashMap = new Map();
  participant.forEach((p) => {
    const isExist = hashMap.get(p); // 처음에 undefined
    if (!isExist) {
      hashMap.set(p, 1);
      // isExist가 undefined이면 participant 요소를 key로, value를 1으로 Map을 생성
      return;
    }
    hashMap.set(p, isExist + 1);
    // isExist의 value가 있으면 그 value에 1을 더함
  });

  console.log("hashMap1 : ", hashMap);
  // Map(3) { 'leo' => 1, 'kiki' => 1, 'eden' => 1 }

  for (const c of completion) {
    const pNum = hashMap.get(c); // eden, kiki의 value 1을 반환
    hashMap.set(c, pNum - 1);
    // hashMap에 completion요소(eden, kiki)에 해당하는 key의 value에 pNum을 빼서 value를 수정한다.
  }

  console.log("hashMap2 :", hashMap);
  // Map(3) { 'leo' => 1, 'kiki' => 0, 'eden' => 0 }

  for (const [key, value] of hashMap) {
    // hashMap에 value가 0이 아니면 key를 출력
    if (value !== 0) {
      return key;
    }
  }
}

console.log("Result :", solution(["leo", "kiki", "eden"], ["eden", "kiki"]));
```

## ❗️ 정리

- 해시 알고리즘을 몰라서 for문을 이용하여 문제를 해결 하였다.
