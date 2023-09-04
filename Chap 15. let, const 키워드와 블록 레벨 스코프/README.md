## 15.1 var 키워드로 선언한 변수의 문제점

### 15.1.1 변수 중복 선언 허용

```jsx
var x = 1;
var y = 1;

// var 키워드로 선언된 변수는 같은 스코프 내에서 **중복 선언을 허용**한다.
// 초기화문이 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작한다.
var x = 100;
// 초기화문이 없는 변수 선언문은 무시된다.
var y;

console.log(x); // 100
console.log(y); // 1
```

- 초기화문이 있는 변수 선언문은 재선언 가능
- 초기화문이 없는 변수 선언문은 무시된다.

### 15.1.2 함수 레벨 스코프

- var 키워드로 선언한 변수는 오로지 **함수의 코드 블록만을 지역 스코프로 인정**한다.
- 따라서 var 키워드로 선언한 변수는 **코드 블록 내에서 선언해도 모두 전역 변수**가 된다.

```jsx
var x = 1;

if (true) {
  // x는 전역 변수다. 이미 선언된 전역 변수 x가 있으므로 x 변수는 중복 선언된다.
  // 이는 의도치 않게 변수값이 변경되는 부작용을 발생시킨다.
  var x = 10;
}

console.log(x); // 10
```

```jsx
var i = 10;

// for문에서 선언한 i는 전역 변수이다. 이미 선언된 전역 변수 i가 있으므로 중복 선언된다.
for (var i = 0; i < 5; i++) {
  console.log(i); // 0 1 2 3 4
}

// 의도치 않게 i 변수의 값이 변경되었다.
console.log(i); // 5
```

→ for 문의 변수 선언 문에서 var 키워드로 선언한 변수도 전역 변수가 된다. 

### 15.1.3 변수 호이스팅

- var 키워드로 변수를 선언하면 **변수 호이스팅에 의해 변수 선언문이 선두로 끌어 올려진 것처럼 동작**한다.
- 변수 호이스팅에 의해 var 키워드로 선언한 변수는 변수 선언문 이전에 참조할 수 있다.

```jsx
// 이 시점에는 변수 호이스팅에 의해 이미 foo 변수가 선언되었다(1. 선언 단계)
// 변수 foo는 undefined로 초기화된다. (2. 초기화 단계)
console.log(foo); // undefined

// 변수에 값을 할당(3. 할당 단계)
foo = 123;

console.log(foo); // 123

// 변수 선언은 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 실행된다.
var foo;
```

## 15.2 let 키워드

### 15.2.1 변수 중복 선언 금지

- let 키워드로 이름이 같은 변수를 중복 선언하면 문법 에러가 발생한다.

```jsx
var foo = 123;
// var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용한다.
// 아래 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작한다.
var foo = 456;

let bar = 123;
// let이나 const 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용하지 않는다.
let bar = 456; // SyntaxError: Identifier 'bar' has already been declared
```

### 15.2.2 블록레벨 스코프

- let 키워드로 선언한 변수는 **모든 코드블록 ( if, for, while, try/catch문 ) 을 지역 스코프로 인정하는** **블록 레벨 스코프**를 따른다.
- 블록 레벨 스코프는 중첩될 수 있다.

```jsx
let foo = 1; // 전역 변수

{
  let foo = 2; // 지역 변수
  let bar = 3; // 지역 변수
}

console.log(foo); // 1
console.log(bar); // ReferenceError: bar is not defined
```

### 15.2.3 변수 호이스팅

- **let 키워드**로 선언한 변수는 **변수 호이스팅이 발생하지 않는 것 처럼 동작**한다.
- let 키워드로 선언한 변수는 **“선언 단계”와 “초기화 단계”가 분리되어 진행되기 때문.**
    
    ⇒ 선언은 런타임 이전에 먼저 실행되지만 초기화는 변수 선언문에 도달했을 때 실행.
    

? 선언은 되어있다면 referenceError 가 아니라 쓰레기 값을 출력해야할 것 같은데..

스코프의 시작 지점 ~ 초기화 전까지 변수를 참조할 수 없는 구간 ⇒ 일시적 사각지대

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/88978fda-4e46-4399-9c60-47b1fe95bcab/Untitled.png)

```jsx
// 런타임 이전에 **선언 단계가 실행**된다. 아직 변수가 초기화되지 않았다.
// 초기화 이전의 일시적 사각 지대에서는 변수를 참조할 수 없다.
console.log(foo); // ReferenceError: foo is not defined

let foo; // 변수 선언문에서 초기화 단계가 실행된다.
console.log(foo); // undefined

foo = 1; // 할당문에서 할당 단계가 실행된다.
console.log(foo); // 1
```

```jsx
let foo = 1; // 전역 변수

{
  console.log(foo); // ReferenceError: Cannot access 'foo' before initialization
  let foo = 2; // 지역 변수
}
```

- 변수 호이스팅이 발생하지 않는다면 위 예제는 전역변수 foo의 값을 출력해야한다.
- 하지만 지역변수 foo 의 호이스팅이 발생했기 때문에 참조에러가 발생한다.

- 저 상태의 렉시컬 환경?
    1. 전역 렉시컬 환경:
        - **`foo`** 변수를 가지고 있고, 값은 1입니다.
    2. 블록 스코프 내부의 렉시컬 환경:
        - 블록 내부에서 선언된 변수 **`foo`**를 저장하기 위한 새로운 렉시컬 환경이 생성됩니다.
        - 이 렉시컬 환경은 전역 렉시컬 환경과 스코프 체인으로 연결됩니다.
    
    블록 내부에서 **`console.log(foo)`**를 실행하려고 할 때, JavaScript 엔진은 스코프 체인을 통해 변수 **`foo`**를 찾으려고 합니다. 그러나 스코프 체인은 먼저 현재 블록 스코프의 렉시컬 환경을 검색하며, 이 때 **`foo` 변수는 이미 선언되었지만 초기화되지 않았기 때문에 "Cannot access 'foo' before initialization" 오류가 발생**합니다
    

⇒ 모든 선언 ( var, let, const, function )을 호이스팅

⇒ 단, let, const, class 를 사용한 선언문은 호이스팅이 발생하지 않는 것 처럼 동작 = 초기화되지 않고 선언만 

### 15.2.4 전역 객체와 let

- var 키워드로 선언한 전역 변수, 전역 함수, 선언하지 않은 변수에 값을 할당한 암묵적 전역은 window객체의 프로퍼티가 된다.
- 그러나 let 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다.

⇒ let 전역 변수는 보이지 않는 개인적인 블록(전역 렉시컬 환경의 선언적 환경 레코드)  내에 존재하게 된다.

```jsx
// 이 예제는 **브라우저 환경**에서 실행해야 한다.

// 전역 변수
var x = 1;
// 암묵적 전역
y = 2;
// 전역 함수
function foo() {}

// var 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티다.
console.log(window.x); // 1
// 전역 객체 window의 프로퍼티는 전역 변수처럼 사용할 수 있다.
console.log(x); // 1

// 암묵적 전역은 전역 객체 window의 프로퍼티다.
console.log(window.y); // 2
console.log(y); // 2

// 함수 선언문으로 정의한 전역 함수는 전역 객체 window의 프로퍼티다.
console.log(window.foo); // ƒ foo() {}
// 전역 객체 window의 프로퍼티는 전역 변수처럼 사용할 수 있다.
console.log(foo); // ƒ foo() {}

let  x = 1;
console.log(window.x); // undefined
```

## 15.3 const 키워드

- 상수를 선언하기 위해 사용

### 15.3.1 선언과 초기화

- const 키워드로 선언한 변수는 반드시 **선언과 동시에 초기화**해야 한다.

```jsx
const foo = 1;

-------

const foo; // SyntaxError: Missing initializer in const declaration

-------

{
  // 변수 호이스팅이 발생하지 않는 것처럼 동작한다
  console.log(foo); // ReferenceError: Cannot access 'foo' before initialization
  const foo = 1;
  console.log(foo); // 1
}

// 블록 레벨 스코프를 갖는다.
console.log(foo); // ReferenceError: foo is not defined
```

### 15.3.2 재할당 금지

- const 키워드로 선언한 변수는 재할당이 금지된다.

```jsx
const foo = 1;
foo = 2;
```

### 15.3.3 상수

- 상수 = 재할당이 금지된 변수
- const 키워드로 선언된 변수에 원시 값을 할당한 경우, 원시 값은 변경할 수 없는 값이 되고 재할당도 금지되므로 할당된 값을 변경할 수 있는 방법은 없다.

```jsx
// 세전 가격
let preTaxPrice = 100;

// 세후 가격
// 0.1의 의미를 명확히 알기 어렵기 때문에 가독성이 좋지 않다.
let afterTaxPrice = preTaxPrice + (preTaxPrice * 0.1);

console.log(afterTaxPrice); // 110
```

- 일반적인 상수의 이름은 대문자로 선언. 단어로 이루어진 경우 언더스코어로 구분하여 스네이크 케이스 사용

```jsx
// 세율을 의미하는 0.1은 변경할 수 없는 상수로서 사용될 값이다.
// 변수 이름을 대문자로 선언해 상수임을 명확히 나타낸다.
const TAX_RATE = 0.1;

// 세전 가격
let preTaxPrice = 100;

// 세후 가격
let afterTaxPrice = preTaxPrice + (preTaxPrice * TAX_RATE);

console.log(afterTaxPrice); // 110
```

### 15.3.4 const 키워드와 객체

- const 키워드로 선언된 변수에 객체를 할당한 경우 값을 변경할 수 있다.
    - 재할당 없이도 객체 직접 변경이 가능하기 때문이다.
    - 객체가 변경되더라도 변수에 할당된 참조 값은 변경되지 않는다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b6ea853e-8948-4bc2-ad57-a74510f3abd5/Untitled.png)

⇒ const 키워드는 재할당을 금지할 뿐 “불변” 을 의미하지는 않는다.

## 15.4 var vs. let vs. const

<aside>
🗨️ **권장 가이드라인**

- ES6 를 사용한다면 var 키워드는 사용하지 않는다.
- 재할당이 필요한 경우에 한정해 let 키워드를 사용한다. 이때 변수의 스코프는 최대한 좁게 만든다.
- 변경이 발생하지 않고 읽기 전용으로 사용하는(재할당이 필요 없는 상수)원시 값과 객체에는 const 키워드를 사용한다. 
const 키워드는 재할당을 금지하므로 var, let 키워드보다 안전하다.
</aside>

⇒ 변수를 선언할 때에는 일단 const 키워드를 사용하자!
