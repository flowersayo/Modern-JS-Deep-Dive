## 17.1  Object 생성자 함수

```jsx
// 빈 객체의 생성
const person = new Object();
// 프로퍼티 추가
person.name = 'Kozel';
person.sayHi = function () {
  console.log('Hi! My name is' + this.name);
};
```

- new 연산자와 함께 Object 생성자 함수를 호출하면 빈 객체를 생성하여 반환한다.
- 생성자 함수란 new 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수를 말한다.
- 생성자 함수에 의해 생성된 객체를 **인스턴스**라고 한다.
- 자바스크립트는 Object 생성자 함수 이외에도 String, Number, Boolean, Function, Array, Date, RegExp, Promise 등의 빌트인 생성자 함수를 제공한다.

```jsx
//String 생성자 함수에 의한 String 객체 생성
const strObj = new String('Kozel');
console.log(typeof strObj); // object
console.log(strObj);		  // String {"Lee"}
```

→ { } 객체 리터럴을 사용하는게 더 편한데.. 굳이 생성자 함수로?

## 17.2 생성자 함수

### 17.2.1 객체 리터럴에 의한 객체 생성 방식의 문제점

- 객체리터럴 → 직관적이고 간편하지만 단 하나의 객체만 생성한다.
- 동일한 프로퍼티를 갖는 객체를 여러개 생성해야하는 경우 같은 코드를 여러번 써야한다.

```jsx
const circle1 = {
  radius: 5,
  getDiameter() {
    return 2 * this.radius;
  }
};
const circle2 = {
  radius: 10,
  getDiameter() {
    return 2 * this.radius;
  }
};
// 이런 형식의 객체가 수십개가 필요하다면?
```

### 17.2.2 생성자 함수에 의한 객체 생성 방식의 장점

- 생성자 함수로 객체를 생성하면 템플릿(클래스) 를 활용해 프로퍼티 구조가 동일한 여러개 객체를 간편하게 생성할 수 있다.

```jsx
// 생성자 함수
function Circle(radius){
  this.radius = radius;
  this.getDiameter = function(){
    return 2 * this.radius;
  };
}
// 인스턴스 생성
const circle1 = new Circle(5);
console.log(circle1.getDiameter()); // 10
// 여러개를 쉽게 생성 가능하다.
```

<aside>
🗨️ **this**

this는 객체 자신의 프로퍼티나 메서드를 참조하기 위한 **자기 참조 변수(self-referencing variable)**다.
 **this가 가리키는 값은 함수 호출 방식에 따라 동적으로 결정된다.**

| 함수 호출 방식 | this가 가리키는 값(this 바인딩) |
| --- | --- |
| 일반 함수로서 호출 | 전역 객체 |
| 메서드로서 호출 | 메서드를 호출한 객체(마침표 앞의 객체) |
| 생성자 함수로서 호출 | 생성자 함수가 (미래에) 생성할 인스턴스 |

```jsx
 // 함수는 다양한 방식으로 호출될 수 있다.

function foo(){
  console.log(this);
}

// 일반적인 함수로서 호출

foo(); // window

const obj = { foo }; // ES6 프로퍼티 축약 표현

obj.foo(); //obj

const inst = new foo(); // inst
```

</aside>

- 자바처럼 생성자 함수의 형식이 정해져있는 것이 아니라, **일반 함수와 동일한 방법으로 생성자 함수를 정의**
- new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작한다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/f5db055c-09f8-42c0-8b60-c6a6ceb09872/Untitled.png)

### 17.2.3 생성자 함수의 인스턴스 생성 과정

- 생성자 함수의 역할은 프로퍼티 구조가 동일한 인스턴스를 생성하기 위한 템플릿으로 동작하여
인스턴스를 생성하고 생성된 인스턴스를 초기화하는것이다.
- 인스턴스를 생성하고 반환하는 코드는 보이지 않지만 암묵적인 처리를 통해 생성, 초기화 후 반환한다.

```jsx
// 생성자 함수
function Circle(radius){

// 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.

// 2. this에 바인딩되어 있는 인스턴스를 초기화한다.

  this.radius = radius;
  this.getDiameter = function(){
    return 2 * this.radius;
  };
// 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
}
// 인스턴스 생성. Circle 생성자 함수는 암묵적으로 this를 반환한다.
const circle1 = new Circle(5); // 반지름이 5인 Circle 객체를 생성

```

1. **인스턴스 생성과 this 바인딩** 

- 암묵적으로 빈 객체가 생성된다.
- 이 빈 객체가 바로 (아직 완성되지는 않았지만) 생성자 함수가 생성한 인스턴스이다.
- 암묵적으로 생성된 빈 객체, 즉 인스턴스는 this 에 바인딩된다. → 함수 몸체 실행되는 런타임 이전에 실행

<aside>
❓ **바인딩**

**식별자와 값을 연결하는 과정**을 의미. 예를 들어, 변수 선언은 변수 이름(식별자)와 확보된 메모리 공간의 주소를 바인딩 하는 것.
this 바인딩은 this 와 this가 가리킬 객체를 바인딩하는 것이다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/8b7f4ed5-08a5-4b75-922e-fa2dd1239e4e/Untitled.png)

</aside>

1. **인스턴스 초기화** 

- 생성자 함수에 기술되어 있는 코드가 한 줄씩 실행되어 **this에 바인딩되어 있는 인스턴스를 초기화**한다.
- 즉, this에 바인딩되어 있는 인스턴스에 프로퍼티나 메서드를 추가하고 생성자 함수가 인수로 전달받은 초기값을 인스턴스 프로퍼티에 할당하여 초기화하거나 고정값을 할당한다.

1. **인스턴스 반환**

- 생성자 함수 내부의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
- 만약 this가 아닌 **객체를 명시적으로 반환하면** return 문에 명시한 객체가 반환된다.
- 그러나 **명시적으로 원시 값을 반환하면 무시**되고 암묵적으로 this 가 반환된다.

⇒ **생성자 함수 내부에서 return 문을 반드시 생략**해야 한다.

### 17.2.4 내부 메서드 [[Call]] 과 [[Construct]]

- 함수 선언문 또는 함수 표현식으로 정의한 함수는 일반적인 함수로서 호출할 수 있는 것은 물론 생성자 함수로서 호출할 수 있다.
- 함수 객체는 일반 객체가 가지고 있는 내부 슬롯과 내부 메서드를 모두 가지고 있다.
    
    ```jsx
    function foo() {}
    // 함수는 객체이므로 프로퍼티를 소유할 수 있다.
    foo.prop = 10;
    // 메서드를 소유할 수 있다.
    foo.method = function () {
      console.log(this.prop);
    };
    foo.method(); // 10
    ```
    
- 일반 객체와 다른 점은 **함수는 호출할 수 있다는 것이다.**
- 함수로서 동작하기 위해 함수 객체만을 위한 [[Environment]], [[FormalParameters]] 등의 내부 슬롯과 [[Call]], [[Construct]] 같은 내부 메서드를 추가로 가지고 있다.
- 함수가 일반 함수로서 호출되면 함수 객체 내부 메서드 [[Call]]이 호출되고 new 연산자와 함께 생성자 함수로서 호출되면 내부 메서드 [[Construct]]가 호출된다.
    
    ```jsx
    function foo() {}
    // 일반적인 함수로서 호출: [[Call]]이 호출된다.
    foo();
    // 생성자 함수로서 호출: [[Construct]]가 호출된다.
    new foo();
    ```
    
- 내부 메서드 [[Call]] 을 갖는 함수 객체를 `callable` 이라고 하고, 내부 메서드 [[Construct]] 를 갖는 함수 객체를 constructor, [[Construct]]를 갖지 않는 함수 객체를 non-constructor라고 부른다.
- callable → 호출할 수 있는 객체, 즉 함수
- constructor → 생성자 함수로서 호출할 수 있는 함수
- 모든 함수 객체는 [[Call]] 을 갖지만 모든 함수 객체가 [[Construct]] 를 갖는 것은 아니다.

### 17.2.5 constructor와 non-constructor 의 구분

- 자바스크립트 엔진은 함수 정의를 평가하여 함수 객체를 생성할 때 함수 정의 방식에 따라 constructor와 non-constructor로 구분한다.
    - constructor : 함수 선언문, 함수 표현식, 클래스(클래스도 함수이다.)
    - non-constructor : 메서드(ES6 메서드 축약 표현), 화살표 함수

! 이때 주의할 것은 ECMAScript 사양에서 메서드로 인정하는 범위가 일반적인 의미의 메서드보다 좁다는 것이다.

```jsx
// constructor
// 함수 선언문
function foo() {}
// 함수 표현식
const bar = function () {};
**// 프로퍼티 x의 값으로 할당된 일반 함수. 메서드로 인정하지 않는다. ( = 함수로 인정된다. )** 
const baz = {
  x: function () {}
};

new foo();	// -> foo {}
new bar();	// -> bar {}
new baz.x();	// -> x {}

// non-constructor
// 화살표 함수
const arrow = () => {};
// **메서드 정의: ES6의 메서드 축약 표현만 메서드로 인정한다.**
const obj = {
  x() {}
};

new arrow();	// TypeError: arrow is not a constructor
new obj.x();	// TypeError: obj.x is not a constructor
```

- 함수를 프로퍼티 값으로 사용하면 일반적으로 메서드로 통칭한다.
- 하지만  ECMAScript 사양에서 메서드란 ES6의 메서드 축약 표현만을 의미한다.
- 함수가 어디에 할당되어 있는지 x, **함수 정의 방식에 따라 constructor 여부가 결정**된다.
- 주의할 것은 생성자 함수로서 호출될 것을 기대하고 정의하지 않은 일반 함수(callable 이면서 constructor)에 
new 연산자를 붙여 호출하면 생성자 함수처럼 동작할 수 있다는 것이다.

### 17.2.6 new 연산자

- 일반 함수와 생성자 함수에 특별한 형식적 차이는 없다.
- 다만 **new 연산자와 함께 함수를 호출하면 해당 함수는 생성자 함수로 동작**한다. →  [Construct]]  가 호출된다.

```jsx
// 생성자 함수로서 정의하지 않은 일반 함수
function add(x,y){

 return x+y;

}

// 생성자 함수로서 정의하지 않은 일반 함수를 new 연산자와 함께 호출
let inst = new add(); 

// 함수가 객체를 반환하지 않았으므로 반환문이 무시된다. 따라서 빈 객체가 생성되어 반환된다.
console.log(inst) // { } 빈 객체가 생성되어 반환

// 객체를 반환하는 일반 함수

function createUser(name,role){
	return { name, role };
}

// 일반 함수를 new 연산자와 함께 호출

inst = new createUser('lee','admin');

// 함수가 생성한 객체를 반환한다.
console.log(inst); // { name : 'lee', role : 'admin' }
```

```jsx
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// new 연산자 없이 생성자 함수 호출하면 일반 함수로서 호출된다.
const circle = Circle(5);

console.log(circle);	// undefined -> 반환값이 없기 때문.
// 일반 함수 내부의 this는 전역 객체 window를 가리킨다.
console.log(radius);	// 5
console.log(getDiameter());	// 10

circle.getDiameter();	// TypeError: Cannot read property 'getDiameter' of undefined
```

- Circle 을 일반 함수로서 호출하면 함수 내부의 this 는 전역 객체 window를 가리킨다.
- 따라서 radius 와 getDiameter 메서드는 전역 객체의 프로퍼티와 메서드가 된다.

⇒ 생성자 함수는 첫 문자를 대문자로 하는 파스칼 케이스로 명명하여 일반 함수와 구분한다.

### 17.2.7 new. target

- 생성자 함수가 new 연산자 없이 호출되는 것을 방지하기 위해서 파스칼 케이스 컨벤션을 사용한다하더라도 실수는 언제나 발생할 수 있다.
- ES6에서는 [new.target](http://new.target)을 지원한다. → 암묵적인 지역 변수와 같이 사용 ( = 메타 프로퍼티 )
- 함수 내부에서  [new.target](http://new.target) 을 사용하면 **new 연산자와 함께 생성자 함수로서 호출되었는지를 확인**할 수 있다.
- new 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 [new.target](http://new.target) 은 함수 자신을 가리킨다. new 연산자 없이 일반 함수로서 호출된 함수 내부의  new.target 은 undefined 이다.

```jsx
function Circle(radius) {
  // 이 함수가 new 연산자와 함께 호출되지 않았다면 new.target은 undefined다.
  if(!new.target){ // new 연산자를 쓰지 않아도 
    // new 연산자와 함께 생성자 함수를 재귀 호출하여 생성된 인스턴스를 반환한다.
    return new Circle(radius);
  }

  // 생략
}

// new 연산자 없이 생성자 함수를 호출하여도 new.target을 통해 생성자 함수로서 호출된다.
const circle = Circle(5);
```

<aside>
❓ **스코프 세이프 생성자 패턴

IE같이 new.target을 사용할 수 없는 상황이라면 스코프 세이프 생성자 패턴을 사용할 수 있다.**

```jsx
function Circle(radius) {
// this와 Circle이 프로토타입에 의해 연결되었는지 확인한다.
 if(!(this instanceof Circle)){
   return new Circle(radius);
 }
 //생략//
}
//new 연산자 없이 생성자 함수를 호출하여도 생성자 함수로서 호출된다.
const circle = Circle(5);
```

</aside>

- 참고로 대부분의 빌트인 생성자 함수는 new 연산자와 함께 호출되었는지를 확인한 후 적절한 값을 반환한다.
- 예를 들어 **Object와 Function 생성자 함수는 new 연산자 없이 호출해도 new 연산자와 함께 호출했을 때와 동일하게 동작**한다.
- 하지만 **String, Number, Boolean 생성자 함수는 new 연산자 없이 호출하면 문자열, 숫자, 불리언 값을 반환**한다. 이를 통해 **데이터 타입 변환**을 하기도 한다.

```jsx
// const obj = new Object();
const obj = Object();
console.log(obj);	// {}

// const f = new Function('x', 'return x ** x');
const f = Function('x', 'return x ** x');
console.log(f);	// f anonymous(x) { return x ** x }

const str = String(123);
console.log(str, typeof str);	// 123 string

const num = Number('123');
console.log(num, typeof num);	// 123 number

const bool = Boolean('true');
console.log(bool, typeof bool);	// true boolean
```
