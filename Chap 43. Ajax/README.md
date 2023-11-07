# Ajax 란?

JS 를 사용하여 **브라우저가 서버에게 비동기 방식으로 데이터를 요청**하고, 
**서버가 응답한 데이터를 수신하여 웹페이지를 동적으로 갱신**하는 프로그래밍 기법 

- Ajax는 브라우저에서 제공하는 Web API 인 **XMLHttpRequest 객체**를 기반으로 동작한다.
    
    ⇒ HTTP 비동기 통신을 위한 메서드와 프로퍼티 제공
    

**☸️ 전통적인 웹페이지의 생명주기** 

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/64cce8a5-7d45-4993-a055-683c24813104/Untitled.png)

- html 태그로 시작해서 html 태그로 끝나는 완전한 HTML 을 서버로부터 전송받아 웹페이지 전체를 처음부터 다시 렌더링하는 방식으로 동작
- 화면이 전환되면 서버로부터 새로운  HTML 을 전송받아 웹페이지 전체를 처음부터 다시 렌더링했다.
- 그러나 전통적인 방식은 다음과 같은 단점이 있다.
    - **변경할 필요가 없는 부분까지 포함된 완전한 HTML 을 서버로부터 매번 다시 전송**받기에 **불필요한 데이터 통신**이 발생한다.
    - 변경할 필요가 없는 부분까지 **처음부터 다시 렌더링**한다. 이로 인해 화면 전환이 일어나면 화면이 순간적으로 **깜박이는 현상**이 발생한다.
    - 클라이언트와 서버와의 통신이 **동기 방식**으로 동작하기 때문에,  서버로부터 응답이 있을 때까지 다음 처리는 **블로킹**된다.

⇒ Ajax 의 등장은 이전의 전통적인 패러다임을 획기적으로 전환했다. 

⇒ 즉, 서버로부터 **웹페이지의 변경에 필요한 데이터만 비동기 방식으로 전송**받아 웹페이지를 변경할 필요가 없는 부분은 다시 렌더링 하지 않고,

**변경할 필요가 있는 부분만 한정적으로 렌더링**하는 방식이 가능해진 것이다. 

![Ajax](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/e2d3eadb-3e83-44d4-9bd4-2a8d9a4de1ae/Untitled.png)

Ajax

- Ajax 는 전통적인 방식과 비교해서 다음과 같은 장점이 있다.
    - 변경할 부분을 갱신하는 데 필요한 데이터만 서버로부터 전송받기 때문에 불필요한 데이터 통신이 발생하지 않는다.
    - 변경할 필요가 없는 부분은 다시 렌더링하지 않는다. 따라서 화면이 순간적으로 깜박이는 현상이 발생하지 않는다.
    - 클라이언트와 서버와의 통신이 비동기 방식으로 동작하기 때문에 서버로 요청을 보낸 이후, 블로킹이 발생하지 않는다.

## JSON

- 클라이언트와 서버 간의  HTTP 통신을 위한 텍스트 데이터 포맷
- 자바스크립트에 종속되지 않는 언어 독립형 데이터 포맷으로, 대부분의 프로그래밍 언어에서 사용할 수 있다.

### JSON 표기 방식

- 객체 리터럴과 유사하게 키와 값으로 구성된 순수한 텍스트
- **키(Key)는 반드시 큰따옴표**로 묶어야한다.

```jsx
{
	"name" : "Sam",
    "age" : 20,
    "moto" : ["hustlin","grindin"]
}
```

### JSON.stringfy

> JSON.stringify(변환할 대상,replacer함수, 들여쓰기 칸수)
> 

- 객체를 JSON 포맷의 문자열로 변환한다.
- 클라이언트가 서버로 객체를 전송하려면 객체를 문자열화해야 하는데 이를 **직렬화**라고 한다.
- 객체 뿐 아니라, 배열도 JSON 포캣의 문자열로 변환한다.

```jsx
const person = {name:'kang', age:27};
JSON.stringify(person); // '{"name":"kang","age":27}'
JSON.stringify(person,null,2); // '{\n  "name": "kang",\n  "age": 27\n}'

const filter = (key,value) => {
    return typeof value === 'number'
    ? undefined
    : value;
}

JSON.stringify(person,filter); //  '{"name":"kang"}'
```

### JSON.parse

- 객체를 JSON 포맷의 문자열을 객체로 변환한다. ⇒ **역직렬화**
- 배열의 요소가 객체인 경우 배열의 요소까지 객체로 변환한다.

## **XMLHttpRequest**

- 자바스크립트를 사용하여 HTTP 요청을 전송하려면 **`XMLHttpRequest`** 객체를 사용한다.
- Web API 인 XMLHttpRequest 객체는 **HTTP 전송과 수신을 위한 다양한 메서드와 프로퍼티를 제공**한다.

### **`XMLHttpRequest` 객체 생성**

```jsx
const xhr = new XMLHttpRequest();
```

- **`XMLHttpRequest`** 객체는 **`XMLHttpRequest`** 생성자 함수를 호출하여 생성한다.
- 브라우저에서 제공하는 Web API 이므로 브라우저 환경에서만 정상동작한다.

### **`XMLHttpRequest`  객체의 프로퍼티와 메서드**

| 프로토타입 프로퍼티 | 설명 |
| --- | --- |
| readyState | HTTP 요청의 현재 상태를 나타내는 정수 |
| status | HTTP 요청에 대한 응답 상태(HTTP 상태 코드)를 나타내는 정수 |
| statusText | HTTP 요청에 대한 응답 메세지를 나타내는 문자열 |
| responseType | HTTP 응답 타입 |
| response | HTTP 요청에 대한 응답 몸체(response body), responseType에 따라 타입이 다름 |
| responseText | 서버가 전송한 HTTP 요청에 대한 응답 문자열 |

### **`XMLHttpRequest`  객체의 이벤트 핸들러 프로퍼티**

| 이벤트 핸들러 프로퍼티 | 설명 |
| --- | --- |
| onreadystatechange | readyState 프로퍼티 값이 변경된 경우 |
| onloadstart | HTTP 요청에 대한 응답을 받기 시작한 경우 |
| onprogress | HTTP 요청에 대한 응답을 받는 도중 주기적으로 발생 |
| onabort | abort 메서드에 의해 HTTP 요청이 중단된 경우 |
| onerror | HTTP 요청에 에러가 발생한 경우 |
| onload | HTTP 요청이 성공적으로 완료한 경우 |
| ontimeout | HTTP 요청 시간이 초과한 경우 |
| onloadend | HTTP 요청이 완료한 경우, HTTP 요청이 성공 또는 실패하면 발생 |

### **`XMLHttpRequest`  객체의 메서드**

| 메서드 | 설명 |
| --- | --- |
| open | HTTP 요청 초기화 |
| send | HTTP 요청 전송 |
| abort | 이미 전송된 HTTP 요청 중단 |
| setRequestHeader | 특정 HTTP 요청 헤더의 값을 설정 |
| getResponseHeader | 특정 HTTP 요청 헤더의 값을 문자열로 반환 |

### **`XMLHttpRequest`  객체의 정적 프로퍼티**

| 정적 프로퍼티 | 설명 |
| --- | --- |
| UNSENT | 값 0, open 메서드 호출 이전 |
| OPENED | 값 1, open 메서드 호출 이후 |
| HEADERS_RECEIVED | 값 2, send 메서드 호출 이후 |
| LOADING | 값 3, 서버 응답 중(응답 데이터 미완성 상태) |
| DONE | 값 4, 서버 응답 완료 |

### HTTP 요청 전송

```jsx
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open('GET', '/users');

// HTTP 요청 헤더 설정
// 클라이언트가 서버로 전송할 데이터의 MIME 타입 지정 : json
xhr.setRequestHeader('content-type', 'application/json');

// HTTP 요청 전송
xhr.send();
```

1. **`XMLHttpRequest.prototype.open`**메서드로 HTTP 요청을 초기화한다.
2. 필요에 따라 **`XMLHttpRequest.prototype.setRequestHeader`**메서드로 헤더 값을 설정한다.
3. **`XMLHttpRequest.prototype.send`**메서드로 HTTP 요청을 전송한다.

**`XMLHttpRequest.prototype.open`**

> xhr.open(method,url[, async])
> 

- method : HTTP 요청 메서드 ( 클라이언트가 서버에게 요청의 종류와 목적을 알리는 방법 )
- url : HTTP 요청을 전송할 URL
- async : 비동기 요청 여부. 옵션으로 기본값은 true.

**`XMLHttpRequest.prototype.send`**

> xhr.send(JSON.stringify({ id : 1, content : 'HTML', completed : false }));
> 

- GET 요청 메서드의 경우 **데이터를 URL의 일부분인 쿼리 문자열(query string)로 서버에 전송**
- POST 요청 메서드의 경우 **데이터(페이로드 payload)를 요청 몸체(request body)에 담아 전송**

**! HTTP 요청 메서드가 GET 인 경우 send 메서드에 페이로드로 전달한 인수는 무시되고 요청 몸체는 null로 설정된다.**

**`XMLHttpRequest.prototype.setRequestHeader`**

- 반드시 open 이후에 호출 해야한다.
- Content-type은 **요청 몸체에 담아 전송할 데이터의 MIME 타입의 정보**를 표현한다.
    - text (MIME 타입) : (서브 타입) text/plain, text/html, text/css, text/javascript
    - application : application/json, applicaiton/x-www-form-urlencode
    - multipart : multipart/formed-data
- HTTP 클라이언트가 서버에 요청할 때 서버가 응답할 데이터의 MIME 타입을 Accept로 지정할 수 있다.
    
    ! 만일 설정하지 않으면 `*/*` 으로 전송 
    

```jsx
// 클라이언트가 서버로 전송할 데이터의 MIME 타입 지정 : json
xhr.setRequestHeader('content-type', 'application/json');
```

```jsx
// 클라이언트가 서버로 전송할 데이터의 MIME 타입 지정 : json
xhr.setRequestHeader('accept', 'application/json');
```

### HTTP 응답 처리

- 서버가 전송한 응답을 처리하려면 **XMLHttpRequest 객체가 발생시키는 이벤트를 캐치**해야 한다.
- HTTP 요청의 현재 상태를 나타내는 **readyState 프로퍼티 값이 변경된 경우 발생하는 readystatechange 이벤트를 캐치하여 HTTP 응답을 처리**할 수 있다.

```jsx
xhr.onreadystatechange = () => {
  // readyState 프로퍼티는 HTTP 요청의 현재 상태 나타냄.
  // readyState 프로퍼티 값이 4(DONE)가 아니면 서버 응답이 완료되지 않은 상태
  // 만약 서버 응답 완료되지 않았다면 아무런 처리 하지 않음
  if (xhr.readyState !== XMLHttpRequest.DONE) return;

  if(xhr.status === 200) {
    console.log(JSON.parse(xhr.response)); // 서버가 응답해준 데이터를 parse
  } else {
    console.error('Error', xhr.status, xhr.statusText);
  }
};
```

! 위 코드는 서버단이 아닌 클라이언트단의 코드임

- 서버의 응답이 완료되면 HTTP 요청에 대한 응답 상태인 xhr.status 가 200인지를 확인하여 정상 처리와 에러처리를 구분한다.
- 대신 load 이벤트(HTTP 요청이 성공적으로 완료된 경우)를 캐치해도 좋다.

```jsx
xhr.onload = () => {
// 성공한 경우에만 발생되기에 xhr.readyState !== XMLHttpRequest.DONE 인지 확인할 필요 X
  if(xhr.status === 200){
    consol.log(JSON.parse(xhr.response));
  } else {
        console.error('Error', xhr.status, xhr.statusText);
  }
};
```
