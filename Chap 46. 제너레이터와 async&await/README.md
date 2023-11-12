## 제너레이터란?

> 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수
> 

<aside>
📌 **제너레이터와 일반 함수의 차이

1. 제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다.**
일반 함수를 호출하면 제어권이 함수에게 넘어가고 함수 호출자는 함수를 호출한 이후 실행을 제어할 수 없다.
함수 호출자가 함수 실행을 일시 중지시키거나 재개시킬 수 있다. 
⇒ 함수의 제어권을 함수가 독점하는 것이 아니라 함수 호출자에게 양도할 수 있다는 것을 의미한다.

**2. 제너레이터 함수는 함수 호출자와 함수의 상태를 주고받을 수 있다.**
함수가 실행되고 있는 동안에는 함수 외부에서 함수 내부로 값을 전달하여 함수의 상태를 변경할 수 없다.
함수 호출자와 양방향으로 상태를 주고 받을 수 있다. 
⇒ 제너레이터 함수는 함수 호출자에게 상태를 전달할 수 있고 함수 호출자로부터 상태를 전달받을 수도 있다.

**3. 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.**
일반 함수를 호출하면 코드를 일괄 실행하고 값을 반환한다. 그러나, 제너레이터 함수를 호출하면 
함수 코드를 실행하는 것이 아니라 이터러블이면서 동시에 이터레이터인 제너레이터 객체를 반환한다.

</aside>

## 제너레이터 함수의 정의

- `function*` 키워드로 선언
    - `*` 의 위치는 function 키워드와 함수 이름 사이라면 어디든지 괜찮다.
    - 그러나 일관성을 위해 function 키워드 바로 뒤에 붙이는 것을 권장
- 하나 이상의 yield 표현식을 포함
    
    ```jsx
    // 제너레이터 함수 선언문
    function* genDecFunc() {
      yield 1;
    }
    
    // 제너레이터 함수 표현식
    const genExpFunc = function* () {
      yield 1;
    }
    
    // 제너레이터 메서드
    const obj = {
      * genObjMethod() {
        yield 1;
      }
    }
    
    // 제너레이터 클래스 메서드
    class MyClass {
      * genClsMethod() {
        yield 1;
      }
    }
    ```
    
- 화살표 함수로 정의할 수 있다.

```jsx
const genArrowFunc = * () => {
  yield 1;
}// syntaxError: Unexpected token '*'
```

- new 연산자와 함께 생성자 함수로 호출할 수 없다.

```jsx
function* genFunc() {
  yield 1;
}

new genFunc(); // TypeError: genFunc is not a constructor
```

## 제너레이터 객체

- 일반 함수처럼 함수 코드 블럭을 실행하는 것이 아니라 제너레이터 객체를 생성해 반환한다.
- 제너레이터 객체는 이터러블이면서 동시에 이터레이터이다.
    - Symbol.iterator 를 상속받는 이터러블
    - value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환하는 next 메서드를 소유하는 이터레이터
- 따라서 별도로 이터레이터를 생성할 필요가 없다.

```jsx
// 제너레이터 함수
function* genFunc() {
  yield 1;
  yield 2;
  yield 3;
}

// 제너레이터 함수를 호출하면 제너레이터 객체를 반환
const generator = genFunc();

// 제너레이터 객체는 이터러블이면서 동시에 이터레이터
// 이터러블은 Symbol.iterator 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체다.
console.log(Symbol.iterator in generator); // true
// 이터레이터는 next 메서드를 갖는다.
console.log('next' in generator) // true
```

- 이터레이터에는 없는 return, throw 메서드를 갖는다.
    - **next 메서드**를 호출하면 제너레이터 함수의 **yield 표현식까지 코드 블록을 실행하고 yield된 값을 value 프로퍼티 값으로, false를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환**한다.
    - **return 메서드**를 호출하면 **인수로 전달받은 value 값을 프로퍼티 값으로, true 를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환**한다.
    - **throw 메서드**를 호출하면 인수로 **전달받은 에러를 발생**시키고 **undefined를 value 프로퍼티 값으로, true 를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환**한다.
    
    ```jsx
    // 46-06
    function genFunc() {
      try {
        yield 1;
        yield 2;
        yield 3;
      } catch (e) {
        console.error(e);
      }
    }
    
    const generator = genFunc();
    
    console.log(generator.next()); // {value: 1, done: false}
    console.log(generator.return('End!')); // {value: 'End!', done: true}
    console.log(generator.throw('Error')); // {value: undefined, done: true}
    ```
    

## 제너레이터의 일시 중지와 재개

- yield 키워드와 next 메서드를 통해 실행을 일시 중지했다가 필요한 시점에 다시 재개할 수 있다.
- 제너레이터는 함수 호출자에게 제어권을 양도하여 필요한 시점에 함수 실행을 재개할 수 있다.
- **yield 키워드는 제너레이터 함수의 실행을 일시 중지 시키거나 표현식의 평가 결과를 함수 호출자에게 반환한다.**
    - next 메서드를 호출하면  yield 표현식까지 실행되고 일시 중지된다.
    - 함수의 제어권이 호출자로 양도된다.

```jsx
function* genFunc() {
  const x = yield 1;

  const y = yield (x + 10);

  // 일반적으로 제너레이터의 반환값은 의미가 없다.
  // 따라서 제너레이터에서는 값을 반환할 필요가 없고 return은 종료의 의미로만 사용해야 한다.
  return x + y;
}

const generator = genFunc(0);
let res = generator.next();
console.log(res); // {value: 1, done: false}

res = generator.next(10);
console.log(res); // {value: 20, done: true}

res = generator.next(20);
console.log(res); // {value: 30, done: true}

```

⇒ next() 를 반복 호출하면 yield 표현식까지 실행과 일시 중지를 반복하다가 제너레이터 함수가 끝까지 실행되면 { value : 반환 값, done : true } 가 할당된다.

<aside>
📌 generator.next() -> yield -> generator.next() -> yield -> ...generator.next() -> return

</aside>

- 제너레이터의 **next() 메서드에 전달한 인수는 제너레이터 함수의 yield 표현식을 할당받는 변수에 할당**된다.

```jsx
function* genFunc() {
  // 처음 next 메서드를 호출하면 첫 번째 yield 표현식까지 실행되고 일시 중지된다.
  // 이때 yield된 값 1은 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에 할당된다.
  // x 변수에는 아직 아무것도 할당되지 않았다. x 변수의 값은 next 메서드가 두 번째 호출될 때 결정된다.
  const x = yield 1;

  // 두 번째 next 메서드를 호출할 때 전달한 인수 10은 첫 번째 yield 표현식을 할당받는 x 변수에 할당된다.
  // 즉, const x = yield 1;은 두 번째 next 메서드를 호출했을 때 완료된다.
  // 두 번째 next 메서드를 호출하면 두 번째 yield 표현식까지 실행되고 일시 중지된다.
  // 이때 yield된 값 x + 10은 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에 할당된다.
  const y = yield (x + 10);

  // 세 번째 next 메서드를 호출할 때 전달한 인수 20은 두 번째 yield 표현식을 할당받는 y 변수에 할당된다.
  // 즉, const y = yield (x + 10);는 세 번째 next 메서드를 호출했을 때 완료된다.
  // 세 번째 next 메서드를 호출하면 함수 끝까지 실행된다.
  // 이때 제너레이터 함수의 반환값 x + y는 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에 할당된다.
  // 일반적으로 제너레이터의 반환값은 의미가 없다.
  // 따라서 제너레이터에서는 값을 반환할 필요가 없고 return은 종료의 의미로만 사용해야 한다.
  return x + y;
}

// 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
// 이터러블이며 동시에 이터레이터인 제너레이터 객체는 next 메서드를 갖는다.
const generator = genFunc(0);

// 처음 호출하는 next 메서드에는 인수를 전달하지 않는다.
// 만약 처음 호출하는 next 메서드에 인수를 전달하면 무시된다.
// next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에는 첫 번째 yield된 값 1이 할당된다.
let res = generator.next();
console.log(res); // {value: 1, done: false}

// next 메서드에 인수로 전달한 10은 genFunc 함수의 x 변수에 할당된다.
// next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에는 두 번째 yield된 값 20이 할당된다.
res = generator.next(10);
console.log(res); // {value: 20, done: false}

// next 메서드에 인수로 전달한 20은 genFunc 함수의 y 변수에 할당된다.
// next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에는 제너레이터 함수의 반환값 30이 할당된다.
res = generator.next(20);
console.log(res); // {value: 30, done: true}
```

- next() 메서드와 yield 표현식을 통해 함수 호출자와 함수의 상태를 주고받을 수 있다.
    - 함수 호출자는 next 메서드를 통해 yield 표현식까지 함수를 실행시켜 제너레이터 객체가 관리하는 상태를 꺼내올 수 있고,
    - next 메서드에 인수를 전달해서 제너레이터 객체에 상태(yield 표현식을 할당받는 변수)를 밀어넣을 수 있다.

⇒ 이러한 제너레이터의 특성을 활용하여 비동기 처리를 동기 처리처럼 구현할 수 있다.

## 제너레이터의 활용

### 이터러블의 구현

: 이터레이션 프로토콜을 준수하여 이터러블을 생성하는 방식보다 간단하게 이터러블을 구현할 수 있다.

- 이터레이션 프로토콜을 준수

```jsx
// 무한 이터러블을 생성하는 함수
const infiniteFibonacci = (function () {
  let [pre, cur] = [0, 1];
  return {
    [Symbol.iterator]() {return this;},
    next() {
      [pre, cur] = [cur, pre + cur];
      // 무한 이터러블이므로 done 프로퍼티를 생략한다.
      return {value: cur};
    }
  };
})();

for (const num of infiniteFibonacci) {
  if (num > 10000) break;
  console.log(num);
}
```

- 제너레이터 사용

```jsx
// 무한 이터러블을 생성하는 제너레이터 함수
const infiniteFibonacci = (function* () {
  let [pre, cur] = [0, 1];
  while (true) {
    [pre, cur] = [cur, pre + cur];
    yield cur;
  }
})();

for (const num of infiniteFibonacci) {
  if (num > 10000) break;
  console.log(num);
}
```

### 비동기 처리

- 상태를 주고 받을 수 있는 특성을 활용하여 프로미스를 사용한 비동기 처리를 동기 처리처럼 구현할 수 있다.
- 프로미스의 후속 처리 메서드 then/catch/finally 없이 비동기 처리 결과를 반환하도록 구현할 수 있다.

```jsx
// node-fetch는 node.js 환경에서 window.fetch 함수를 사용하기 위한 패키지다.
// 브라우저 환경에서 이 예제를 실행한다면 아래 코드는 필요 없다.
// https://github.com/node-fetch/node-fetch
const fetch = require('node-fetch');

// 제너레이터 실행기
const async = generatorFunc => {
  const generator = generatorFunc(); // ②

  const onResolved = arg => {
    const result = generator.next(arg); // ⑤

    return result.done
      ? result.value // ⑨
      : result.value.then(res => onResolved(res)); // ⑦
  };

  return onResolved; // ③
};

(async(function* fetchTodo() { // ①
  const url = 'https://jsonplaceholder.typicode.com/todos/1';

  const response = yield fetch(url); // ⑥
  const todo = yield response.json(); // ⑧
  console.log(todo);
  // {userId: 1, id: 1, title: 'delectus aut autem', completed: false}
})()); // ④
```

<aside>
❓ **동작 순서**

1. async 함수가 호출되면 인수로 전달받은 제너레이터 함수를 호출하여 제너레이터 객체를 생성 (②) 하고 onResolved 함수를 (③) 반환한다. onResolved 는 상위 스코프의 generator 변수를 기억하는 클로저이다. async 함수가 반환한 onResolved 를 즉시 실행 (④) 하여 제너레이터 객체의 next 메서드를 처음 호출(⑤)한다. 

2. next 메서드가 처음 호출되고, 첫 번째 yield 문까지 실행된다. 
result 변수에 제너레이터 결과값이 할당되고, fetch 함수가 반환한 Response 객체를 onResolved 함수에 인수로 전달하면서 재귀호출(⑦) 한다.

3.  next 메서드가 두번째 호출되고, 두 번째 yield 문까지 실행된다. 
인수로 전달한 Response 객체는 제너레이터 함수 내부의 response 변수에 할당된다. 
두번째 yield 된 response.json 메서드가 반환한 프로미스가 resolve 한 todo객체를
onResolved 함수에 인수로 전달하면서 재귀호출(⑦) 한다.

4. next 메서드가 세번째 호출되고, todo 객체는 todo 변수에 할당된다. 제너레이터 함수가 끝까지 실행된다.반환된 제너레이터 리절트 객체의 done 프로퍼티값이 true 로 바뀌면 fetchTodo 의 반환값인  undefined를 그대로 반환하고 처리를 종료한다.

</aside>

! 제너레이터 함수는 직접 구현하기 보다 co 라이브러리를 사용하자.

## async/await

- 제너레이터보다 가독성 있게 비동기 처리를 동기 처리처럼 동작하도록 구현하는 방법
- async/await 는 프로미스를 기반으로 동작한다.

```jsx
const fetch = require('node-fetch');

async function fetchTodo() {
  const url = 'https://jsonplaceholder.typicode.com/todos/1';

  const response = await fetch(url);
  const todo = await response.json();
  console.log(todo);
  // {userId: 1, id: 1, title: 'delectus aut autem', completed: false}
}

fetchTodo();
```

### async 함수

- await 키워드는 반드시 async 함수 내부에서 사용해야 한다.
- async 함수는 await 키워드를 사용해 정의하며 언제나 프로미스를 반환한다.
- 명시적으로 프로미스를 반환하지 않더라도 암묵적으로 반환값을 resolve 하는 프로미스를 반환한다.

```jsx
// async 함수 선언문
async function foo(n) { return n; }
foo(1).then(v => console.log(v)); // 1

// async 함수 표현식
const bar = async function (n) { return n; };
bar(2).then(v => console.log(v)); // 2

// async 화살표 함수
const baz = async n => n;
baz(3).then(v => console.log(v)); // 3

// async 메서드
const obj = {
  async foo(n) { return n; }
};
obj.foo(4).then(v => console.log(v)); // 4

// async 클래스 메서드
class MyClass {
  async bar(n) { return n; }
}
const myClass = new MyClass();
myClass.bar(5).then(v => console.log(v)); // 5

class MyClass {
  async constructor() { }
  // SyntaxError: Class constructor may not be an async method
}

const myClass = new MyClass();
```

### await 키워드

- 프로미스가 settled 상태가 될 때까지 대기하다가 settled 상태가 되면 프로미스가 resolve한 처리 결과를 반환한다.
- await 키워드는 반드시 프로미스 앞에서 사용해야 한다.

```jsx
const fetch = require('node-fetch');

async function fetchTodo() {
  const url = 'https://jsonplaceholder.typicode.com/todos/1';

  const response = await fetch(url); // promise가 settled 상태가 되면 프로미스가 resolve한 처리 결과가 res 변수에 할당된다. 
  const todo = await response.json();
  console.log(todo);
  // {userId: 1, id: 1, title: 'delectus aut autem', completed: false}
}

fetchTodo();
```

- 프로미스는 비동기 처리가 완료될 때까지 대기해서 **순차적으로 처리**한다.

### 에러처리

- try .. catch 를 사용해 에러를 처리할 수 있다.
- 콜백 함수를 인수로 전달받는 비동기 함수와는 다르게 프로미스를 반환하는 비동기 함수는 명시적으로 호출할 수 있기 때문에 ( = fetch 함수를 호출한 것은 foo 함수이기에 ) 호출자가 명확하다.

```jsx
try {
  setTimeout(() => { throw new Error('Error!'); }, 1000);
} catch (e) {
  // 비동기 함수의 콜백 함수를 호출한 것은 setTimeout 함수가 아니기 때문에 에러를 캐치하지 못한다
  console.error('캐치한 에러', e);
}

```

```jsx
const fetch = require('node-fetch');

const foo = async () => {
  try {
    const wrongUrl = 'https://wrong.url';

    const response = await fetch(wrongUrl);
    const data = await response.json();
    console.log(data);
  } catch (err) {
    console.error(err); // TypeError: Failed to fetch
  }
};

foo();
```

- async 함수 내에서 catch 문을 사용해서 에러 처리를 하지 않으면 async 함수는 발생한 에러를 reject 하는 프로미스를 반환한다.

⇒ async 함수를 호출하고 .catch 후속처리를 통해 에러를 캐치할 수도 있다.

```jsx
const fetch = require('node-fetch');

const foo = async () => {
  const wrongUrl = 'https://wrong.url';

  const response = await fetch(wrongUrl);
  const data = await response.json();
  return data;
};

foo()
  .then(console.log)
  .catch(console.error); // TypeError: Failed to fetch
```
