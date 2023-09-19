## 26.1 함수의 구분

- ES6 이전의 함수는 동일한 함수라도 다양한 형태(일반함수, 생성자함수, 메서드) 로 호출할 수 있다.

```tsx
var foo = function () {
  return 1;
};

// 일반적인 함수로서 호출
foo(); // -> 1

// 생성자 함수로서 호출
new foo(); // -> foo {}

// 메서드로서 호출
var obj = { foo: foo };
obj.foo(); // -> 1
```

- 객체에 바인딩된 함수도 일반 함수로서 호출할 수 있고, 생성자 함수로서 호출할 수도 있다.

```tsx
// 프로퍼티 f에 바인딩된 함수는 callable이며 constructor다.
var obj = {
  x: 10,
  f: function () { return this.x; }
};

// 프로퍼티 f에 바인딩된 함수를 메서드로서 호출
console.log(obj.f()); // 10

// 프로퍼티 f에 바인딩된 함수를 일반 함수로서 호출
var bar = obj.f;
console.log(bar()); // undefined

// 프로퍼티 f에 바인딩된 함수를 생성자 함수로서 호출
console.log(new obj.f()); // f {}
```

- 객체에 바인딩된 함수가 constructor 라는 것은 객체에 바인딩된 함수가 prototype 프로퍼티를 가지며, 프로토타입 객체도 생성한다는 것을 의미하기 때문이다.
- 콜백함수도 constructor 이기 때문에 불필요한 프로토타입 객체를 생성한다.

⇒ 호출방식에 특별한 제약이 없고 생성자 함수로 호출되지 않아도 프로토타입 객체를 생성한다.

⇒ 이러한 문제를 해결하기 위해 함수를 사용목적에 따라 세가지 종류로 명확히 구분한다.

| ES6 함수의 구분 | constructor | prototype | super | arguments |
| --- | --- | --- | --- | --- |
| 일반 함수 | O | O | X | O |
| 메서드 | X | X | O | O |
| 화살표 함수 | X | X | X | X |

## 26.2 메서드

- ES6 사양에서 **메서드**는 **메서드 축약 표현으로 정의된 함수**만을 의미한다.
- ES6 사양에서 정의한 메서드는 인스턴스를 생성할 수 없는 non-constructor 이다. 따라서 ES6 메서드는 생성자 함수로서 호출할 수 없다.
- ES6 메서드는 인스턴스를 생성할 수 없으므로 prototype 프로퍼티가 없고 프로토타입도 생성하지 않는다.

```tsx
const obj = {
  x: 1,
  // 메서드 축약 표현으로 정의된 foo는 메서드이다.
  foo() { return this.x; },
  // bar에 바인딩된 함수는 메서드가 아닌 **일반 함수**이다.
  bar: function() { return this.x; }
};

console.log(obj.foo()); // 1
console.log(obj.bar()); // 1

new obj.foo(); // -> TypeError: obj.foo is not a constructor
new obj.bar(); // -> bar {}
```

```tsx
// obj.foo는 constructor가 아닌 ES6 메서드이므로 prototype 프로퍼티가 없다.
obj.foo.hasOwnProperty('prototype'); // -> false

// obj.bar는 constructor인 일반 함수이므로 prototype 프로퍼티가 있다.
obj.bar.hasOwnProperty('prototype'); // -> true
```

- ES6 메서드는 자신을 바인딩한 객체를 가리키는 내부 슬롯 [[HomeObject]] 를 갖는다.
- super 참조는 내부 슬롯 [[HomeObject]] 를 사용하여 수퍼클래스의 메서드를 참조하므로 내부슬롯 [[HomeObject]] 를 갖는 메서드는 super 키워드를 사용할 수 있다.
    
    ```tsx
    const base = {
      name: 'Lee',
      sayHi() {
        return `Hi! ${this.name}`;
      }
    };
    
    const derived = {
      __proto__: base,
      // sayHi는 ES6 메서드다. ES6 메서드는 [[HomeObject]]를 갖는다.
      // sayHi의 [[HomeObject]]는 sayHi가 바인딩된 객체인 derived를 가리키고
      // super는 sayHi의 [[HomeObject]]의 프로토타입인 base를 가리킨다.
      sayHi() {
        return `${super.sayHi()}. how are you doing?`;
      }
    };
    
    console.log(derived.sayHi()); // Hi! Lee. how are you doing?
    ```
    
- ES6 메서드가 아닌 함수는 내부 슬롯 [[HomeObject]]을 갖지 않기에 super 키워드를 사용할 수 없다.
    
    ```tsx
    const derived = {
      __proto__: base,
      // sayHi는 ES6 메서드가 아니다.
      // 따라서 sayHi는 [[HomeObject]]를 갖지 않으므로 super 키워드를 사용할 수 없다.
      sayHi: function () {
        // SyntaxError: 'super' keyword unexpected here
        return `${super.sayHi()}. how are you doing?`;
      }
    };
    ```
    

⇒ 프로퍼티 값으로 익명 함수 표현식을 할당하는 ES6 이전의 방식은 사용하지 않는 것이 좋다.

## 26.3 화살표 함수

**함수 정의**

: 함수 표현식으로만 정의해야한다.

```jsx
const multiply = (x, y) => x * y;
multiply(2, 3); // -> 6
```

**매개변수 선언**

```jsx
const arrow = (x, y) => { ... };
const arrow = x => { ... };
const arrow = () => { ... };
```

**함수 몸체 정의**

- 이때 함수 몸체 내부의 값이 값으로 평가될 수 있는 문이라면 암묵적으로 반환된다.
- 함수 몸체가 하나의 문이라면 중괄호를 생략할 수 있다.
    
    ```jsx
    // concise body
    const power = x => x ** 2;
    power(2); // -> 4
    
    // 위 표현은 다음과 동일하다.
    // block body
    const power = x => { return x ** 2; };
    ```
    
- 객체 리터럴을 반환하는 경우는 소괄호 `(` `)` 로 감싸주어야 한다. 그렇지 않으면 함수 몸체를 감싸는 중괄호로 잘못 해석한다.
    
    ```jsx
    const create = (id, content) => ({ id, content });
    create(1, 'JavaScript'); // -> {id: 1, content: "JavaScript"}
    
    // 위 표현은 다음과 동일하다.
    const create = (id, content) => { return { id, content }; };
    
    // { id, content }를 함수 몸체 내의 쉼표 연산자문으로 해석한다.
    const create = (id, content) => { id, content };
    create(1, 'JavaScript'); // -> undefined
    ```
    
- 화살표 함수도 즉시실행함수로 사용할 수 있다.
    
    ```jsx
    const person = (name => ({
      sayHi() { return `Hi? My name is ${name}.`; }
    }))('Lee');
    
    console.log(person.sayHi()); // Hi? My name is Lee.
    ```
    
- 화살표 함수도 일급 객체이므로 고차 함수에 인수로 전달할 수 있다. ⇒ 가독성 GOOD!
    
    ```jsx
    // ES5
    [1, 2, 3].map(function (v) {
      return v * 2;
    });
    
    // ES6
    [1, 2, 3].map(v => v * 2); // -> [ 2, 4, 6 ]
    ```
    

### 26.3.2 화살표 함수와 일반 함수의 차이

1. 화살표 함수는 인스턴스를 생성할 수 없는 **non-consturctor** 이다.
    
    ```jsx
    const Foo = () => {};
    // 화살표 함수는 생성자 함수로서 호출할 수 없다.
    new Foo(); // TypeError: Foo is not a constructor
    ```
    
    ```jsx
    const Foo = () => {};
    // 화살표 함수는 prototype 프로퍼티가 없다.
    Foo.hasOwnProperty('prototype'); // -> false
    ```
    

1. 중복된 매개변수 이름을 선언할 수 없다.
    
    ```jsx
    function normal(a, a) { return a + a; }
    console.log(normal(1, 2)); // 4
    ```
    
    ```jsx
    'use strict'; // 단 strict mode 에서는 에러가 발생한다.
    
    function normal(a, a) { return a + a; }
    // SyntaxError: Duplicate parameter name not allowed in this context
    ```
    
    ```jsx
    const arrow = (a, a) => a + a;
    // SyntaxError: Duplicate parameter name not allowed in this context
    ```
    
2. 화살표 함수는 함수 자체의  this, arguments, super, [new.target](http://new.target) 바인딩을 갖지 않는다.
    
    → 스코프체인을 통해 상위 스코프의 this, arguments, super, [new.target](http://new.target) 를 참조한다. 
    

### 26.3.3 this

- **화살표 함수의 this 는 일반 함수의 this 와 다르게 동작한다.**
- this 바인딩은 함수 호출 방식, 즉 **함수가 어떻게 호출되었는지**에 따라 동적으로 결정된다.
    - 일반 함수로서 호출되는 모든 함수 내부의 this 는 전역 객체를 가리킨다.
    - strict mode에서 일반함수로 호출된 모든 함수 내부에는 전역객체가 아니라  undefined 가 바인딩된다.
- [Array.prototype.map](http://Array.prototype.map) 메서드가 콜백 함수를 일반함수로서 호출하기 때문에 this 바인딩→ undefined
    
    ```jsx
    class Prefixer {
      constructor(prefix) { this.prefix = prefix }
    
      add(arr) {
        //	여기서 this는 메서드를 호출한 객체 prefixer
        arr.map(function (item) {
          return this.prefix + item;
          // TypeError: Cannot read property 'prefix' of undefined
          // 여기서 this는 undefined
        });
      }
    }
    
    const prefixer = new Prefixer('aa');
    console.log(prefixer.add(['apple', 'airplane']));
    ```
    

> **ES6 이전에 콜백 함수 내부의  this 문제를 해결했던 방법**
> 
> 
> 
> 1. **this 킵해두기**
> 
> : add 메서드를 호출한 prefixer 객체를 가리키는 this를 일단 회피시킨 후에 콜백 함수 내부에서 사용한다.
> 
> ```jsx
> add(arr) {
>   const that = this;
>   return arr.map(function (item) {
>     return that.prefix + item;
>   });
> }
> ```
> 
> 1. **this 직접 전달하기**
> 
> : [Array.prototype.map](http://Array.prototype.map) 의 두번째 인수로 add 메서드를 호출한 prefixer 객체를 가리키는 this 를 전달한다.
> 
> ```jsx
> add(arr) {
>   return arr.map(function (item) {
>     return this.prefix + item;
>   }, this);
> }
> ```
> 
> 1. **Function.prototype.bind 메서드 사용**
> 
> : add 메서드를 호출한 prefixer 객체를 가리키는 this 를 바인딩한다.
> 
> ```jsx
> add(arr) {
>   return arr.map(function (item) {
>     return this.prefix + item;
>   }.bind(this));
> }
> ```
> 

⇒ ES6 에서는 화살표 함수를 사용하여 “콜백 함수 내부의 this 문제” 를 해결할 수 있다.

```jsx
add(arr) {
  return arr.map(item => this.prefix + item);
}
```

- 화살표 함수는 함수 자체의 this 바인딩을 갖지 않는다.
- 렉시컬 스코프와 같이 화살표 함수가 정의된 위치에 의해 this 가 결정된다.
- 따라서 화살표 함수 내부에서 this 를 참조하면 스코프체인을 통해 상위 스코프의 this 를 그대로 참조한다. ⇒ lexical this
    
    ```jsx
    (function(){
    const foo = () => console.log(this);
    foo();
    }).call({a:1}); //{a:1} call 메서드는 함수를 호출할 때 사용되며, 첫 번째 인자로는 함수 내에서의 this 값을 설정할 객체를 받는다.
    
    (function(){
    const bar = () => () => console.log(this);
    bar()();
    }).call({a:1}); //{a:1} 
    
    // 화살표함수가 전역함수라면 this 는 전역 객체를 가리킨다. 전역 함수의 상위 스코프는 전역이기 때문이다.
    const foo = () => console.log(this);
    foo(); //window
    ```
    
- 프로퍼티에 할당된 화살표 함수도 마찬가지이다.
    
    ```jsx
    const counter = {
    
    num: 1,
    increase : () => ++this.num
    };
    
    console.log(counter.increase()); // NaN
    ```
    
- 화살표 함수 내부의 this를 교체할 수 없다.
    
    ```jsx
    window.x = 1;
    
    const normal = function () {
      return this.x;
    };
    const arrow = () => this.x;
    
    console.log(normal.call({ x: 10 })); // 10
    console.log(arrow.call({ x: 10 })); // 1
    ```
    
- 메서드를 화살표 함수로 정의하는 것을 피해야한다.
    
    ```jsx
    // 메서드를 호출한 객체를 가리키지 않고 상위 스코프의 this를 가리킴 -> 즉, 전역객체 
    
    const person = {
      name: "Lee",
      sayHi: () => console.log(`Hi, ${this.name}`),
    };
    
    person.sayHi(); // Hi
    ```
    
    ⇒ 메서드를 정의할 때는 ES6 축약 표현으로 정의한  ES6 메서드를 사용하는 것이 좋다.
    
    ```jsx
    const person = {
      name: "Lee",
      sayHi(){
    			console.log(`Hi, ${this.name}`); //this 가 person 객체에 바인딩된다.
    	}
    };
    
    person.sayHi(); // Hi Lee
    ```
    
- 프로토타입 객체의 프로퍼티에 화살표 함수를 할당하는 경우도 동일한 문제가 발생한다.
    
    ⇒ 프로퍼티를 동적 추가할 때에는 ES6 메서드 정의를 사용할 수 없으니 일반 함수를 할당한다.
    
    ES6 메서드를 동적 추가 하고 싶다면 객체 리터럴을 바인딩하고 프로토타입의 constructor 프로퍼티와 생성자 함수간의 연결을 재설정한다.
    
- 클래스 필드 정의 제안을 사용하여 클래스 필드에 화살표 함수를 할당할 수도 있다.
    - 클래스 안에 할당된 화살표 함수의 상위 스코프는 무엇일까? ⇒ 사실 클래스 외부이다.
    - 하지만 this 는 클래스 외부의 this 를 참조하지 않고 클래스가 생성할 인스턴스를 참조한다.
    
    ```jsx
    class Person {
      name = "Lee";
      sayHi = () => console.log(`Hi ${this.name}`);
    
    }
    
    const person = new Person();
    person.sayHi(); // Hi Lee
    ```
    
    ```jsx
    class Person {
    
    	constructor() {
    		 this.name = "Lee";
    		 this.sayHi = () => console.log(`Hi ${this.name}`);
    	 }
    }
    
    const person = new Person();
    person.sayHi(); // Hi Lee
    ```
    
    ⇒ 그러나 클래스 필드에 할당한 화살표 함수는 인스턴스 메서드가 되므로 **ES6 메서드 축약 표현으로 정의한 메서드를 사용하는 것이 좋다.**
    

### 26.3.4 super

- 화살표 함수는 함수 자체의 super 바인딩을 갖지 않는다.
- 화살표 함수 내부에서 super 를 참조하면 this와 마찬가지로 상위 스코프의 super를 참조한다.
    
    ```jsx
    //화살표함수 내부에서 super를 참조하면 constructor 내부의 super 바인딩 참조
    
    class Base {
      constructor(name) {
        this.name = name;
      }
    
      sayHi() {
        return `Hi! ${this.name}`;
      }
    }
    
    class Derived extends Base {
      sayHi = () => `${super.sayHi()} how are you doing?`;
    }
    
    const derived = new Derived("Lee");
    console.log(derived.sayHi()); // Hi! Lee how are you doing?
    ```
    

### 26.3.5 arguments

- 화살표 함수는 함수 자체의 arguments 바인딩을 갖지 않는다.
- 화살표 함수 내부에서 arguments 를 참조하면 this와 마찬가지로 상위 스코프의 arguments 를 참조한다.
    
    ```jsx
    (function () {
      const foo = () => console.log(arguments); // [Arguments] { '0': 1, '1': 2 }
      foo(3, 4);
    })(1, 2);
    
    const foo = () => console.log(arguements);
    foo(1,2); // referenceError 
    ```
    
    ⇒ 화살표 함수로 가변 인자 함수를 구현해야 할 때는 반드시  Rest 파라미터를 사용해야 한다.
    

## 26.4 Rest 파라미터

### 26.4.1 기본 문법

- 매개변수 이름 앞에 세개의 점 … 을 붙여서 정의한 매개변수
- **Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받는다.**

```jsx
function foo(...rest) {
  console.log(rest); // [1, 2, 3, 4, 5]
}

foo(1, 2, 3, 4, 5);
```

- Rest 파라미터는 반드시 마지막 파라미터이어야 한다.
- Rest 파라미터는 단 하나만 선언할 수 있다.
- Rest 파라미터는 함수 정의 시 선언한 매개변수를 나타내는 함수 객체의 length 프로퍼티에 영향을 주지 않는다.

### 26.4.2 Rest 파라미터와  arguments 객체

- ES6 에서는 rest 파라미터를 사용하여 가변 인자 함수의 인수 목록을 배열로 직접 전달받을 수 있다.

```jsx
function sum(...args) {
	
	// rest 파라미터 args에는 배열 [1,2,3,4,5]가 할당된다.
  return args.reduce((pre,cur) => pre + cur ,0);
}

console.log(sum(1, 2, 3, 4, 5)); //15
```

## 26.5 매개변수 기본 값

- 인수가 전달되지 않은 매개변수의 기본 값은 undefined 이다.
- 따라서 인수가 제대로 전달되지 않은 경우 매개변수에 기본 값을 할당할 필요가 있다.
- ES6 에서 도입된 매개변수 기본값을 사용하면 인수 체크 및 초기화를 간소화할 수 있다.

```jsx
function sum( x = 0, y = 0){
		return x + y;
}

console.log(sum(1,2)); // 3
```
