- **REST** :  HTTP 를 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍처
- **REST API** : REST 를 기반으로 서비스 API 를 구성한 것

## REST API 의 구성

- 자원, 행위, 표현의 3가지 요소로 구성

| 구성 요소 | 내용 | 표현 방법 |
| --- | --- | --- |
| 자원 | 자원 | URI(엔드포인트) |
| 행위 | 자원에 대한 행위 | HTTP 요청 메서드 |
| 표현 | 자원에 대한 행위의 구체적 내용 | 페이로드(전송의 근본적인 목적이 되는 데이터) |

## REST API 설계 원칙

- URI 는 리소스를 표현하는 데 집중
- 행위에 대한 정의는 HTTP 요청 메서드를 통해 하는 것

1. URI 는 **리소스를 표현**해야 한다.

- 동사보다는 명사
- 이름에 get 같은 행위가 들어가서는 안된다.

```jsx
# bad
GET /getTodos/1
GET /todos/show/1

# good
GET /todos/1
```

1. 리소스에 대한 행위는 HTTP 요청 메서드로 표현한다.
- 서버에게 요청의 종류와 목적을 알리는 방법
- 행위는 HTTP 요청 메서드를 통해 표현하며 URI 에 표현하지 않는다.

| HTTP 요청 메서드 | 종류 | 목적 | 페이로드 |
| --- | --- | --- | --- |
| GET | index/retrieve | 모든/특정 리소스 취득 | X |
| POST | create | 리소스 생성 | O |
| PUT | replace | 리소스의 전체 교체 | O |
| PATCH | modify | 리소스의 일부 수정 | O |
| DELETE | delete | 모든/특정 리소스 삭제 | X |

```jsx
# bad
GET /todos/delete/1

#good
DELETE /todos/1
```

## JSON Server를 이용한 REST API 실습

### JSON Server 설치

JSON Server는 json 파일을 사용하여 가상 REST API 서버를 구축할 수 있는 툴.

### db.json 파일 생성

db.json 파일은 리소스를 제공하는 데이터베이스 역할을 한다.

### JSON Server 실행

### GET 요청

```jsx
const xhr = new XMLHttpRequest();
xhr.open('GET', '/todos');

xhr.send();

xhr.onload = () => {
	if(xhr.status === 200) {
      document.querySelector('pre').textContext = xhr.response;
    } else {
      console.error('Error', xhr.status, xhr.statusText);
    }
};
```

### POST 요청

POST 요청 시에는 setRequestHeader 메서드를 사용하여 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정해야 함.

```jsx
xhr.open('POST', '/todos');

// 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입 지정
xhr.setRequestHeader('content-type', 'application/json');

xhr.send(JSON.stringify({id : 4, content : 'Angular');

// 생략
```

### PUT 요청

특정 리소스 전체를 교체할 때 사용

```jsx
xhr.open('PUT', '/todos/4');

// 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입 지정
xhr.setRequestHeader('content-type', 'application/json');
```

### PATCH 요청

특정 리소스의 일부를 수정할 때 사용

```jsx
xhr.open('PATCH', '/todos/4');

// 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입 지정
xhr.setRequestHeader('content-type', 'application/json');

// 리소스를 수정하기 위해 페이로드를 서버에 전송해야 함
xhr.send(JSON.stringify({completed : false}));
```

### DELETE 요청

```jsx
xhr.open('DELETE', '/todos/4');

xhr.send();
```
