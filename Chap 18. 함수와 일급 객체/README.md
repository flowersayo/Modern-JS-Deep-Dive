## 18.1 일급 객체

1. 무명의 **리터럴**로 생성할 수 있다. 즉, 런타임에 생성할 수 있다.
2. 변수나 자료구조(객체, 배열) 등에 **저장**할 수 있다.
3. 함수의 **매개변수에 전달**할 수 있다.
4. 함수의 **반환값**으로 사용할 수 있다.

⇒ 자바스크립트의 함수는 위의 조건을 모두 만족하는 일급 객체다.

```jsx
// 1. 무명의 리터럴로 생성할 수 있다.
// 2. 변수에 저장할 수 있다.
// 런타임에 함수 리터럴이 평가되어 함수 객체가 생성되고 변수에 할당된다.
const increase = function (num) {
  return ++num;
};
const decrease = function (num) {
  return --num;
};
// 2. 함수는 객체에 저장할 수 있다.
const auxs = { increase, decrease };
// 3. 함수의 매개변수에 전달할 수 있다.
// 4. 함수의 반환값으로 사용할 수 있다.
function makeCounter(aux) {
  let num = 0;

  return function () {
    num = aux(num);
    return num;
  };
}

// 3. 함수는 매개변수에게 함수를 전달할 수 있다.
const increaser = makeCounter(auxs.increase);
console.log(increaser());	// 1
console.log(increaser());	// 2
const decreaser = makeCounter(auxs.decrease);
console.log(decreaser());	// -1
```

- 함수를 객체와 동일하게 사용할 수 있다. → 값으로 취급 가능
- 런타임에 함수 객체로 평가된다.

## 18.2 함수 객체의 프로퍼티

- 함수도 객체이므로 프로퍼티를 가질 수 있다.

```jsx
function square(number) {
  return number * number;
}

console.dir(square);
```

![함수 객체의 프로퍼티](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/9f6ae474-de85-48e5-a673-e3d9653d18b2/Untitled.png)

함수 객체의 프로퍼티

![모든 프로퍼티 어트리뷰트](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/fd223852-bace-4ae1-9112-9e1cb0110ab3/Untitled.png)

모든 프로퍼티 어트리뷰트

```jsx
// __proto__는 square 함수의 프로퍼티가 아니다.
console.log(Object.getOwnPropertyDescriptor(square, '__proto__')); // undefined

// __proto__는 Object.prototype 객체의 접근자 프로퍼티다.
// square 함수는 Object.prototype 객체로부터 __proto__ 접근자 프로퍼티를 상속받는다.
console.log(Object.getOwnPropertyDescriptor(Object.prototype, '__proto__'));
```

- `__proto__`는 접근자 프로퍼티이며, 함수 객체 고유의 프로퍼티가 아니라 `Object.prototoype` 객체의 프로퍼티를 상속 받은 것을 알 수 있다.
- `Object.prototoype` 객체의 프로퍼티는 모든 객체가 상속받아 사용할 수 있다. → 즉, `Object.prototoype` 객체의 `__proto__`는 모든 객체가 사용할 수 있다.

### 18.2.1 arguments 프로퍼티

- arguments 객체는 함수 호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한 유사 배열 객체이며, 함수 내부에서 지역 변수처럼 사용된다.
- 함수가 호출되면 함수 몸체 내에서 암묵적으로 매개변수가 선언되고 undefined 로 초기화된 이후 인수가 할당된다.
- 초과된 모든 인수는 암묵적으로 arguments 객체의 프로퍼티로 보관된다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/1903cd0e-0fac-4511-84cc-33433c198e1b/Untitled.png)
    
    - callee : arguments 객체를 생성한 함수, 즉 함수 자신을 가리킨다.
    - length : 인수의 개수를 나타낸다.
- arguments 객체의 **Symbol(Symbol.iterator)** 프로퍼티는 arguments 객체를 **순회 가능한 자료구조인 이터러블로** 만들기 위한 프로퍼티다.
    
    ```jsx
    function multiply(x, y) {
      // 이터레이터
      const iterator = arguments[Symbol.iterator]();
    
      // 이터레이터의 next 메서드를 호출하여 이터러블 객체 arguments를 순회
      console.log(iterator.next()); // {value: 1, done: false}
      console.log(iterator.next()); // {value: 2, done: false}
      console.log(iterator.next()); // {value: 3, done: false}
      console.log(iterator.next()); // {value: undefined, done: true}
    
      return x * y;
    }
    multiply(1, 2, 3);
    ```
    
- arguments 객체는 매개변수 개수를 확정할 수 없는 **가변 인자 함수** 를 구현할 때 유용하다.
    
    ```jsx
    function sum() {
      let res = 0;
      for (let i = 0; i < arguments.length; i++){
        res += arguments[i];
      }
      return res;
    }
    
    console.log(sum(1, 2));	// 3
    ```
    
    - arguments 객체는 배열이 아닌 유사 배열 객체이다. → length 프로퍼티를 가진 객체로 for문으로 순회할 수 있는 객체
        - 따라서 배열 메서드를 사용할 경우 에러
        - 배열 메서드를 사용하려면 Function.prototype.call, Function.prototype.apply를 사용해 간접 호출해야 하는 번거로움이 있다.
        
        ```jsx
        function sum1() {
          // arguments 객체를 배열로 변환
          const array = Array.prototype.slice.call(arguments);
          return array.reduce(function (pre, cur) {
            return pre + cur;
          }, 0);
        }
        
        console.log(sum1(1, 2, 3, 4));	// 10
        
        // 이러한 번거로움을 해결하기 위해 ES6에서는 Rest 파라미터를 도입했다.
        function sum2(...args) {
          return args.reduce((pre, cur) => pre + cur, 0);
        }
        
        console.log(sum2(1, 2, 3));	// 6
        // Rest 파라미터의 도움으로 arguments 객체의 중요성이 떨어졌지만
        // 언제나 ES6만 사용하지는 않을 수 있기 때문에 알아두자.
        ```
        
        - 이러한 번거로움을 해결하기 위해 ES6에서는 **Rest 파라미터**를 도입하였다.
    
    <aside>
    ✍️ **유사 배열 객체와 이터러블**
    
    ES6에서 도입된 이터레이션 프로토콜을 준수하면 순회 가능한 자료구조인 이터러블이 된다. 이터러블의 개념이 없었던 ES5에서 
    ****arguments 객체는 유사 배열 객체로 구분되었다. 하지만 이터러블이 도입된 **ES6부터 arguments 객체는 유사 배열 객체이면서 이터러블이다.**
    
    </aside>
    

### 18.2.2 caller 프로퍼티

- ECMAScript 사양에 포함되지 않은 비표준 프로퍼티다.
- 표준화될 예정도 없기에 참고로만 알아두자.

```jsx
function foo(func) {
  return func();
}
function bar() {
  return 'caller : ' + bar.caller;
}

// 브라우저에서 실행한 결과
console.log(foo(bar));	// caller : function foo(func) {...}
console.log(bar());		// caller : null
```

### 18.2.3 length 프로퍼티

- 함수를 **정의할 때 선언한** 매개변수의 개수
- 실제로 인자로 들어오는 arguments.length와는 값이 다를 수 있다. 인자 ≠ 매개변수 일 수 있기 때문

```jsx
function foo() {}
console.log(foo.length);	// 0

function baz(x, y){
  return x * y;
}
console.log(baz.length);	// 2
```

### 18.2.4  name 프로퍼티

- 함수 이름
- ES5 ES6에서 동작을 달리하므로 주의!
    - 익명 함수 표현식의 경우 ES5에서 name 은 빈 문자열을 값으로 가진다.
    - 그러나 ES6에서는 함수 객체를 가리키는 식별자를 값으로 갖는다.
    - 함수 이름과 함수 객체를 가리키는 식별자는 의미가 다르다는 것을 주의!
    
    ```jsx
    // 기명 함수 표현식
    var namedFunc = function foo() {};
    console.log(namedFunc.name);	// foo
    
    // 익명 함수 표현식
    var anonymousFunc = function() {};
    // ES5에서 name 프로퍼티는 빈 문자열을 값으로 갖는다.
    console.log(anonymousFunc.name);	// anonymousFunc
    
    // 함수 선언문
    function bar() {}
    console.log(bar.name);	// bar
    ```
    

### 18.2.5 proto 접근자 프로퍼티

- **모든 객체는 [[Prototype]]이라는 내부 슬롯을 갖는다.** [[Prototype]] 내부 슬롯은 객체지향 **프로그래밍의 상속을 구현하는 프로토타입 객체**를 가리킨다.
- __proto__ 프로퍼티는 [[Prototype]] 내부 슬롯과 [[Prototype]] 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 **접근자 프로퍼티**다.

```jsx
const obj = { a: 1 };

// 객체 리터럴 방식으로 생성한 객체의 프로토타입 객체는 Object.prototype이다.
console.log(obj.__proto__ === Object.prototype);	// true

// 객체 리터럴 방식으로 생성한 객체는 프로토타입 객체인 Object.prototype의 프로퍼티를 상속받는다.
// hasOwnProperty 메서드는 Object.prototype의 메서드다.

// obj.hasOwnProperty : 인수로 전달받은 프로퍼티 키가 객체 고유의 프로퍼티 키인 경우에만 true를 반환하고 상속받은 프로토타입의 프로퍼티 키인 경우 false를 반환한다.

console.log(obj.hasOwnProperty('a'));			// true
console.log(obj.hasOwnProperty('__proto__'));	// false
```

### 18.2.6 Prototype 프로퍼티

- Prototype 프로퍼티는 생성자 함수로 호출할 수 있는 함수 객체, 즉 **constructor만이 소유하는 프로퍼티**이다.
- 일반 객체와 생성자 함수로 호출할 수 없는  non-constructor에는  prototype 프로퍼티가 없다.
- Prototype 프로퍼티는 함수가 객체를 생성하는 생성자 함수로 호출될 때 생성자 함수가 생성할 인스턴스의 프로토타입 객체를 가리킨다.
