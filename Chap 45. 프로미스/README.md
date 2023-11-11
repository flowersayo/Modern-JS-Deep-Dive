- 비동기 처리를 위한 하나의 패턴으로 콜백 함수 사용 → 가독성 나쁘고 병렬처리에 한계
- ES6 에서는 비동기 처리를 위한 또 다른 패턴인 프로미스 도입

## 비동기 처리를 위한 콜백 패턴의 단점

### 콜백 헬

- 비동기 함수 : 함수 내부에 비동기로 동작하는 코드를 포함한 함수
- 비동기 함수를 호출하면 함수 내부의 비동기로 동작하는 코드가 완료되지 않았다 해도 기다리지 않고 즉시 종료된다.
- 즉, **비동기 함수 내부의 비동기로 동작하는 코드는 비동기 함수가 종료된 이후에 완료**된다.
- 비동기 함수 내부의 비동기로 동작하는 코드에서 처리 결과를 외부로 반환하거나 상위 스코프의 변수에 할당하면 기대한 대로 동작하지 않는다.

```jsx
let g = 0

setTimeout(() => {
  g = 100
}, 0)
console.log(g) // 0
```

- setTimeout 을 호출하면, 콜백 함수를 호출스케줄링해서 타이머 id 를 반환하고 즉시 종료한다.

- GET 요청을 전송하고 서버의 응답을 전달받는 get 함수도 비동기 함수이다.
    
    ```
    //GET 요청을 위한 비동기 함수
    const get = url => {
    	const xhr = new XHMLHttpRequest();
        xhr.open('GET', url_;
        xhr.send();
    
        xhr.onload =()=>{
        	if(xhr.status ===200){
            	// 1. 서버의 응답을 반환한다.
            	return JSON.parse(xhr.response);
            }else{
            	console.error(`${xhr.status} ${xhr.statusText}`);
            }
        };
    };
    
    // id가 1인 post를 취득
    const response= get('https://jsonplaceholder.typicode.com/posts/1';
    console.log(response); //undefined
    ```
    
    - get 함수 내부의 onload 이벤트 핸들러는 비동기로 동작한다.
    - GET 요청을 전송하고, onload 이벤트 핸들러를 등록한 다음 undefined 를 반환하고 즉시 종료된다.
        - 함수 코드를 평가하는 과정에서 get 함수의 실행 컨텍스트가 생성되고 실행 컨텍스트 스택에 푸시된다.
        - 이후 함수 코드 실행 과정에서 xhr.onload 이벤트 핸들러 프로퍼티에 이벤트 핸들러가 바인딩된다.
    - 즉, 비동기 함수인 get 함수 내부의 이벤트 핸들러는 get 함수가 종료된 이후 ( = console.log 까지 종료한 이후 ) 에 실행된다.
    - 따라서 get 함수의 이벤트 핸들러에서 서버의 응답 결과를 반환하거나 상위 스코프의 변수에 할당하면 기대한대로 동작하지 않는다.
    - xhr.load 핸들러 프로퍼티에 바인딩한 이벤트 핸들러는 즉시 실행되는 것이 아니다. **load 이벤트가 발생하면 일단 태스크 큐에 저장되어 대기하다가, 콜스택이 비면(= 할 일을 모두 끝내면) 이벤트 루프에 의해 콜 스택으로 푸시되어 실행된다.**

- 비동기 함수는 비동기 처리 결과를 외부에 반환할 수 없고, 상위 스코프의 변수에 할당할 수도 없다.

⇒ 따라서 **비동기 함수의 처리 결과(서버의 응답)에 대한 후속 처리는 비동기 함수 내부에서 수행**해야 한다.

- 이 때, 비동기 함수를 범용적으로 사용하기 위해 **비동기 함수에 비동기 처리 결과에 대한 후속 처리를 수행하는 콜백 함수를 전달하는 것이 일반적**이다.
- 필요에 따라 비동기 처리가 성공하면 호출될 콜백 함수와 비동기 처리가 실패하면 호출될 콜백 함수를 전달할 수 있다.

콜백 함수를 통해 비동기 처리 결과에 대한 후속 처리를 수행하는 **비동기 함수가 비동기 처리 결과를 가지고 
또다시 비동기 함수를 호출해야 한다면** 콜백 함수 호출이 중첩되어 복잡도가 높아지는 현상이 발생 ⇒ **콜백 헬**

- 콜백헬 예시

```jsx
// GET 요청을 위한 비동기 함수
const get = (url, callback) => {
    const xhr = new XMLHttpRequest();
    xhr.open('GET', url);
    xhr.send();

    xhr.onload = () => {
        if (xhr.status === 200) {
            // 서버의 응답을 콜백 함수에 인수로 전달하면서 호출하여 응답에 대한 후속처리를 한다.
            callback(JSON.parse(xhr.response));
        } else {

            console.error(`${xhr.status} ${xhr.statusText}`);
        }
    };
};

const url = 'https://jsonplaceholder.typicode.com/posts/1';

// id가 1인 post의 userId를 취득
get(`{$url}/posts/1`, ({userId}) => {
    console.log(userId); // 1
    // post의 userId를 사용하여 user 정보를 취득
    get(`${url}/users/${userId}`, userInfo => {
        console.log(userInfo);
    });
});
```

```jsx
get('/step1', a => {
   get(`/step2/${a}`, b => {
      get(`/step3/${b}`, c => {
         get(`/step4/${c}`, d => {
             console.log(d);
			});
		});
   });
});
```

### 에러 처리의 한계

- 비동기 처리를 위한 콜백 패턴의 문제점 중 가장 심각한 것은 에러처리가 곤란하다는 것

```jsx
 try{
 	setTimeout(()=> { throw new Error('Error:'); }, 1000);
 } catch (e){
 	//에러를 캐치하지 못한다.
    console.error('캐치한 에러', e);
```

→ setTimeout 내부에서 발생한 에러는 catch 코드 블럭에서 캐치되지 않는다.

→ **에러는 호출자 방향으로 전파된다.**

→ 그러나 setTimeout 함수의 콜백 함수를 호출한 것이 setTimeout 함수가 아니다. ( setTimeout 의 콜백 함수가 실행될 때 setTimeout 함수는 이미 콜 스택에서 제거된 상태이다. ) 

⇒ setTimeout 함수의 콜백 함수가 발생시킨 에러는 catch 블럭에서 캐치되지 않는다.

<aside>
☝ **try .. catch .. finally 문**

try 블럭에서 에러가 발생하면, 해당 에러는 catch 문의 err 변수에 전달되고 catch 코드 블럭이 실행된다.
finally 코드 블록은 에러 발생과 상관 없이 반드시 한 번 실행된다.
**try .. catch .. finally 문 으로 에러를 처리하면 프로그램이 강제 종료되지 않는다.**

</aside>

## 프로미스의 생성

- Promise 생성자 함수를 new 연산자와 함께 호출하면 프로미스 객체를 생성한다.
- ES6 에서 도입된 Promise 는 호스트 객체가 아닌, ECMAScript 사양에 정의된 **표준 빌트인 객체**이다.

```jsx
// 프로미스 생성
const promise = new Promise((resolve, reject) => {
  // Promise 함수의 콜백 함수 내부에서 비동기 처리를 수행한다.
  if(비동기 처리 성공){
    resolve('result')
  } else {
    reject('failure reason')
  }
})
```

- 비동기 처리가 성공하면 resolve 함수를 호출하고, 실패하면 reject 함수를 호출한다.

- 앞서 살펴본 비동기 함수 get 을 프로미스를 사용하여 다시 구현한 것이다.

```jsx
// GET 요청을 위한 비동기 함수
const promiseGet = url => {
    return new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest();
        xhr.open('GET', url);
        xhr.send();

        xhr.onload = () => {
            if (xhr.status === 200) {
                // 성공적으로 응답을 전달받으면 resolve 함수를 호출한다.
                resolve(JSON.parse(xhr.response));
            } else {
                // 에러 처리를 위해 reject 함수를 호출한다.
                reject(new Error(xhr.status));
            }
        };
    });
};

promiseGet('https://jsonplaceholder.typicode.com/posts/1');
```

- 프로미스는 다음과 같이 현재 비동기 처리가 어떻게 진행되고 있는지를 나타내는 상태 정보를 갖는다.
    - Pending(대기 상태) : 비동기 처리가 아직 수행되지 않은 상태 | 프로미스 생성 직후
    - Fulfilled(성공) : 비동기 처리가 수행된 상태(성공)
    - Rejected(실패) : 비동기 처리가 수행된 상태(실패)
- 비동기 처리 성공 : resolve 함수 호출 → fulfilled 로 상태변경
- 비동기 처리 실패 : reject 함수 호출 → Rejected 상태변경
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/4620d4ae-74f3-4498-b371-988ba43ec327/Untitled.png)
    

⇒**프로미스의 상태는 resolve 또는 reject 함수를 호출하는 것으로 결정된다.**

- settled 상태 : fulfilled 또는 rejected 상태와 상관없이 pending이 아닌 상태로 비동기 처리가 수행된 상태
- 프로미스는 비동기 처리 결과도 상태로 갖는다. ( 위 그림의 result )

⇒ **프로미스는 비동기 처리 상태와 처리 결과를 관리하는 객체이다.**

## 프로미스의 후속 처리 메서드

- 비동기 처리가 수행된 이후 fulfilled, rejected 로 변화하면 그 처리결과를 가지고 무엇인가를 해야한다.
- 이를 위해 후속 메서드 then, catch, finally 를 제공한다.
- 프로미스의 비동기 처리 상태가 변화하면 후속 처리 메서드에 인수로 전달한 콜백 함수가 선택적으로 호출된다.
- 후속 처리 메서드의 콜백 함수에는 프로미스의 처리 결과가 인수로 전달된다.
- 모든 후속 처리 메서드는 프로미스를 반환하며, 비동기로 동작한다.

### Promise.prototype.then

- then 메서드는 두 개의 콜백 함수를 인수로 전달받는다. (fulfilled, reject)
- 첫 번째 콜백 함수는 비동기 처리가 성공했을 때 호출되는 성공 처리 콜백 함수이며, 두 번째 콜백 함수는 비동기 처리가 실패했을 때 호출되는 실패 처리 콜백 함수다.
- then 메서드는 언제나 Promise 를 반환한다.
    - 콜백 함수가 프로미스를 반환하면 당연히 그 프로미스를 그대로 반환
    - 프로미스가 아니어도 그 값을 암묵적으로 resolve 또는 reject 하여 프로미스를 생성해 반환한다.

```jsx
// fulfilled
new Promise(resolve => resolve('fulfilled'))
    .then(v => console.log(v), e => console.log(e)); // fulfilled

// rejected
new Promise((_, reject) => reject(new Error('rejected')))
    .then(v => console.log(v), e => console.error(e)); // Error: rejected
```

### Promise.prototype.catch

- catch 메서드를 호출하면 내부적으로 then(undefined, onRejected)을 호출한다.
- 프로미스가 rejected 상태인 경우에만 호출
- 언제나 Promise 를 반환한다.

```jsx
// rejected
new Promise((_, reject) => reject(new Error('rejected')))
  .catch(e => console.log(e)); // Error: rejected
```

### Promise.prototype.finally

- 성공 또는 실패 여부와 상관 없이 무조건 한번 호출
- finally 메서드는 프로미스의 상태와 상관 없이 공통적으로 수행해야 할 처리 내용이 있을 경우 유용

```jsx
// rejected
new Promise(() => {})
  .finally(() => console.log('finally')); // finally
```

## 프로미스의 에러 처리

- then 메서드의 두 번째 콜백 함수는 첫 번째 콜백 함수에서 발생한 에러를 캐치하지 못한다.

```jsx
// 부적절한 URL이 지정되었기 때문에 에러가 발생한다.
promiseGet(wrongUrl).then(
  res => console.xxx (res),
  err => console.error(err)
); // success callback 에서의 에러는 캐치하지 못한다. 
```

- **catch 메서드를** 모든 then 메서드를 호출한 이후에 호출하면 비동기 처리에서 발생한 에러뿐만 아니라 **then 메서드 내부에서 발생한 에러까지 모두 캐치할 수 있다.**

```jsx
const wrongUrl = 'https://jsonplaceholder.typicode.com/XXX/1';

promiseGet(wrongUrl)
  .then(res => console.log(res))
  **.catch(err => console.error(err));**
```

→ 에러 처리는 catch 에서 할 것을 권장

## 프로미스 체이닝

- 후속 처리 메서드는 언제나 프로미스를 반환하므로 연속적으로 호출할 수 있다.

```jsx
const url = 'https://jsonplaceholder.typicode.com';

// id가 1인 post의 userId를 취득
promiseGet(`${url}/posts/1`)
  // 취득한 post의 userId로 user 정보를 취득
  .then(({userId}) => promiseGet(`${url}/users/${userId}`))
  .then(userInfo => console.log(userInfo))
  .catch(err => console.error(err));
```

- 위 코드 후속 처리 메서드의 콜백 함수는 다음과 같이 인수를 전달받으면서 호출된다.

| 후속 처리 메서드 | 콜백 함수의 인수 | 후속 처리 메서드의 반환값 |
| --- | --- | --- |
| then | promiseGet 함수가 반환한 프로미스가 resolve한 값(id가 1인 post) | 콜백 함수가 반환한 프로미스 |
| then | 첫 번째 then 메서드가 반환한 프로미스가 resolve한 값(post의 userId로 취득한 user 정보) | 콜백 함수가 반환한 값(undefined)을 resolve한 프로미스 |
| catch | promiseGet 함수 또는 앞선 후속 처리 메서드가 반환한 프로미스가 reject한 값 | 콜백 함수가 반환한 값(undefined)을 resolve한 프로미스 |

## 프로미스의 정적 메서드

- 프로미스도 객체이므로 메서드를 가질 수 있다.
- 5가지 정적 메서드를 제공한다.

### Promise.resolve / Promise.reject

- 이미 존재하는 값을 래핑하여 프로미스를 사용하기 위해 사용한다.
    - Promise.resolve 메서드는 인수로 전달받은 값을 resolve하는 프로미스를 생성한다.
    
    ```jsx
    // 배열을 resolve하는 프로미스를 생성
    const resolvePromise = Promise.resolve([1, 2, 3]);
    // 위 코드가 아래와 같다.
    const resolvePromise = new Promise(resolve => resolve([1, 2, 3]));
    resolvePromise.then(console.log); // [1, 2, 3]
    ```
    
    - Promise.reject 메서드는 인수로 전달받은 값을 reject하는 프로미스를 생성한다.
    
    ```jsx
    // 배열을 resolve하는 프로미스를 생성
    const rejectedPromise = Promise.reject(new Error('Error!'));
    // 위 코드가 아래와 같다.
    const rejectedPromise = new Promise((_, reject) => reject(new Error('Error!')));
    rejectedPromise.catch(console.log); // Error: Error!
    ```
    

### Promise.all

- 여러개의 비동기 처리를 병렬 처리할 때 사용한다.

```jsx
const requestData1 = () =>  
  new Promise(resolve => setTimeout(() => resolve(1), 3000));  
const requestData2 = () =>  
  new Promise(resolve => setTimeout(() => resolve(2), 2000));  
const requestData3 = () =>  
  new Promise(resolve => setTimeout(() => resolve(3), 1000));  
  
// 세 개의 비동기 처리를 순차적으로 처리  
Promise.all([requestData1(), requestData2(), requestData3()])  
  .then(console.log) // [1, 2, 3] => 약 3초 소요  
  .catch(console.error);
```

- 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받는다.
- 그리고 전달받은 모든 프로미스가 모두 fulfilled 상태가 되면 **모든 처리 결과를 배열에 저장**해 **새로운 프로미스를 반환**한다.
- 전달받은 프로미스 중 **하나라도 rejected 상태가 되면** 나머지 프로미스가 fulfilled 상태가 되는 것을 기다리지 않고 **즉시 종료**한다.
- 인수로 전달받은 이터러블의 요소가 프로미스가 아닌 경우 Promise.resolve 메서드를 통해 프로미스로 래핑한다.

### Promise.race

- 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받는다.
- **가장 먼저 fulfilled 상태가 된 프로미스의 처리 결과**를 resolve 하는 새로운 프로미스를 반환한다.
- 전달받은 프로미스 중 **하나라도 rejected 상태가 되면** 나머지 프로미스가 fulfilled 상태가 되는 것을 기다리지 않고 **즉시 종료**한다.

```jsx
Promise.race([
  new Promise(resolve => setTimeout(() => resolve(1), 3000)), // 1
  new Promise(resolve => setTimeout(() => resolve(2), 2000)), // 2
  new Promise(resolve => setTimeout(() => resolve(3), 1000))  // 3
])
  .then(console.log) // 3
  .catch(console.log);
```

### Promise.allSettled

- 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받는다.
- 전달 받은 프로미스가 **모두 settled 상태 ( 비동기 처리가 수행된 상태 ) 가 되면 처리 결과를 배열로 반환**한다.
    - fulfilled: status 프로퍼티와 처리 결과를 나타내는 value 프로퍼티를 갖는다.
    - rejected: status 프로퍼티와 에러를 나타내는 reason 프로퍼티를 갖는다.

<aside>
❓ **Promise.all VS Promise.allSettled**

Promise.all 은 모든 프로미스가 fullfilled(성공) 해야하지만, Promise.allSettled는 성공여부에 관계가 없다.

</aside>

```jsx
Promise.allSettled([
  new Promise(resolve => setTimeout(() => resolve(1), 2000)),
  new Promise((_, reject) => setTimeout(() => reject(new Error('Error!')), 1000))
]).then(console.log);
```

## 마이크로 태스크 큐

**Q. 다음 코드가 어떤 순서대로 출력될까?**

```jsx
setTimeout(() => console.log(1), 0);

Promise.resolve()
  .then(() => console.log(2))
  .then(() => console.log(3));
```

- 프로미스의 후속 처리 메서드도 비동기로 동작하므로 1→2→3 의 순으로 출력될 것처럼 보인다.
- 그러나 2→3→1의 순서로 출력된다.
- 그 이유는 프로미스의 후속 처리 메서드의 콜백함수는 태스크 큐가 아니라 **마이크로태스크 큐**에 저장되기 때문이다.
- 마이크로태스크 큐는 태스크 큐보다 우선순위가 높다.
- 즉, 이벤트 루프는 콜 스택이 비면 먼저 마이크로 태스크 큐에서 대기하고 있는 함수를 가져와 실행하고, 그 다음 태스크 큐를 본다.

## fetch

> **const promise = fecth(url [, options)**
> 

- HTTP 요청 전송 기능을 제공하는 클라이언트 사이드 Web API
- HTTP 응답을 나타내는 **Response 객체를 래핑한 Promise 객체를 반환**한다. → then 을 통해 프로미스가 resolve 한 Response 객체를 전달 받을 수 있다.

```jsx
fetch('https://jsonplaceholder.typicode.com/todos/1')
  // response는 HTTP 응답을 나타내는 Response 객체다.
  // json 메서드를 사용하여 Response 객체에서 HTTP 응답 몸체를 취득하여 역직렬화한다.
  .then(response => response.json())
  // json은 역직렬화 HTTP 응답 몸체다.
  .then(json => console.log(json));
```

- fetch 함수를 활용할 때에는 **에러처리에 유의**해야한다.
    
    ```jsx
    fetch(wrongUrl)
      // response는 HTTP 응답을 나타내는 Response 객체다.
      // json 메서드를 사용하여 Response 객체에서 HTTP 응답 몸체를 취득하여 역직렬화한다.
      .then(() => console.log('ok'))
      // json은 역직렬화 HTTP 응답 몸체다.
      .catch(() => console.log('err'));
    ```
    
    - 부적절한 URL 이 지정되었기 때문에 catch 에 걸릴 것 같지만 ok가 출력된다.
    
    ? **fetch 함수가 반환하는 프로미스는 기본적으로 404 Not Found 나 500 Internal Server Error 와 같은 HTTP 에러가 발생해도 에러를 reject 하지 않고 불리언 타입의 ok 상태를 false 로 설정한 Response 객체를 resolve 한다. 오프라인 등의 네트워크 장애나 CORS 에 의해 요청이 완료되지 못한 경우에만 프로미스를 reject 한다.**
    
    ⇒ 따라서 fetch 를 사용할 때에는 **fetch 함수가 반환한 프로미스가 resolve한 불리언 타입의 ok 상태**를 확인해 **명시적으로 에러를 처리**할 필요가 있다.
    
    ```jsx
    fetch(url)
      .then(res => {
      if(!res.ok) throw new Error(res.statusText)
      return res.json()
    })
    ```
    

반면, axios 는 모든 HTTP 에러를 reject 하는 프로미스를 반환한다. 따라서 모든 에러를 catch 에서 처리할 수 있어서 편리하다.

****1. GET 요청****

```jsx
request.get('https://jsonplaceholder.typicode.com/todos/1')
  .then(response => response.json())
  .then(todos => console.log(todos))
  .catch(err => console.error(err));
```

****2. POST 요청****

```jsx
request.post('https://jsonplaceholder.typicode.com/todos', {
  userId: 1,
  title: 'Javascript',
  completed: false
}).then(response => response.json())
  .then(todos => console.log(todos))
  .catch(err => console.error(err));
```

****3. PATCH 요청****

```jsx
request.patch('https://jsonplaceholder.typicode.com/todos', {
    completed: true
}).then(response => response.json())
  .then(todos => console.log(todos))
  .catch(err => console.error(err));
```

****4. DELETE 요청****

```jsx
request.delete('https://jsonplaceholder.typicode.com/todos/1')
  .then(response => response.json())
  .then(todos => console.log(todos))
  .catch(err => console.error(err));
```
