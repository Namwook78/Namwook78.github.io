---
title: "React Children"

categories:
  - "react"
tags:
  - [react]

toc: true
toc_sticky: true

date: 2022-01-10
last_modified_at: 2022-01-10
---

## React Children props

- 상위 Component에서 하위 컴포넌트 선언
- 하위 Component에서 children props 설정
  <br />

App.js

```jsx
import Hello from "./hello";
export default function App() {
  return (
    <>
      <Hello>
        <h1>Hello</h1>
      </Hello>
    </>
  );
}
```

Hello.js

```js
export default function Hello({ children }) {
  return <div>{children}</div>;
  //    h1태그로 Hello 출력,  <h1>Hello</h1>
}
```
