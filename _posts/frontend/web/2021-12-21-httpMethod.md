---
title: "HTTP Method"

categories:
  - web
tags:
  - [http]

toc: true
toc_sticky: true

date: 2021-12-21
last_modified_at: 2021-12-22
---

<div style="margin-bottom:41px"></div>

# HTTP Method

- 주어진 리소스에 수행하길 원하는 행동을 나타냄
- CRUD에 해당하는 메서드에는 POST, GET, PUT, DELETE이다.

## POST

- POST API를 사용하여 새 하위 resource를 생성하는데 사용
- 새 resource를 생성할때 부모애개 POST를 수행하고 서비스는 새 resource를 부모와 연결하고 ID를 할당하는 등의 작업을 처리
- **멱등성이 아님**, 요청은 HTML 양식을 통해 서버에 전송하며 서버에 변경사항을 만듦
  > 멱등성이란? 동일한 요청을 한 번 보내는 것과 여러번 보내는 것이 같은 효과를 지니고 있음
- resource가 성공적으로 생성되면 HTTP 응답코드인 201(CREATED)이고, 새 리소스에 대한 링크와 Location 헤더를 반환

```
POST http://localhost:5000/users
POST http://localhost:500/users/:id
```

## GET

- GET API를 사용하여 resource를 읽거나 검색하는데 사용
- XML 또는 JSON으로 표현과 200(OK)의 HTTP 응답 코드를 반환, 실패 시 404(NOT FOUND)또는 400(BAD REQUEST)의 응답 코드를 반환
- 먹등성, 여러 개 동일한 요청을 수행하면 단일 요청과 동일한 결과를 얻음

```
GET http://localhost:5000/users
GET http://localhost:500/users/:id
```

## PUT

- PUT API를 사용하여 원래의 resource를 업데이트 하는데 사용
- 대상 resource를 나타내는 데이터가 없으면, PUT 요청이 성공적으로 resource 하나를 생성할 경우 서버는 응답코드 201(CREATED)를 반환한다.
- 대상 resource를 나타내는 데이터가 있고, 요청에 대하여 성공적으로 수정이 완료 되었으면 서버는 응답코드 200(OK), 204(No CONTENT)를 반환한다.
- 멱등성, resource를 생성하거나, 업데이트한 다음 동일한 호출을 다시 수행하면 resource가 여전히 존재

```
PUT http://localhost:5000/users
PUT http://localhost:500/users/:id
```

## DELETE

- DELETE API를 사용하여 resource의 URI request 식별 기준으로 삭제
- 삭제 요청에 성공적으로 삭제 되었으면 응답코드 200(OK) 반환
- 삭제 요청을 내렸지만 아직 작업이 수행되기 전 상태이면 응답코드 202(ACCEPTED) 반환
- 작업이 수행되고 응답 본문이 없는 상태이면 응답코드 204(NO CONTENT) 반환
- 만일 삭제요청을 하고 나서 삭제를 재요청 시 이미 제거 되었기 떄문에 404(NOT FOUND) 반환
- DELETE API를 사용하여 마지막 resource를 제거하는 기능을 구현하지 않으면 멱등성에 해당한다.

```
DELETE http://localhost:500/users/:id
DELETE http://localhost:5000/users/00001
```

## 참조

- [REST API Tutorial](https://www.restapitutorial.com/lessons/httpmethods.html)
- [HTTP Methods](https://restfulapi.net/http-methods/)
- [HTTP 요청 메서드 MDN](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods)
- [POST MDN](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/POST)
- [PUT MDN](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/PUT)
- [멱등성 MDN](https://developer.mozilla.org/ko/docs/Glossary/Idempotent)
