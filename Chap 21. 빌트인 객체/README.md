## 21.1 자바스크립트 객체의 분류

- 표준 빌트인 객체 : ECMAScript 사양에 정의된 객체
    - 애플리케이션 전역의 공통 기능을 제공
    - 자바스크립트 실행 환경(브라우저 or Node.js환경)과 관계없이 언제나 사용할 수 있다.
- 호스트 객체 : 자바스크립트 실행환경(브라우저 or Node.js환경)에서 추가로 제공하는 객체
    - 브라우저 환경 → DOM, BOM, Canvas, XMLHttpRequest, fetch, requestAnimationFrame, SVG, Web Storage, Web Component, Web Worker와 같은 클라이언트 사이드 Web API
    - Node.js 환경 → Node.js 고유의 API를 호스트 객체로 제공
- 사용자 정의 객체 : 사용자가 직접 정의한 객체

## 21.2 표준 빌트인 객체

- 자바스크립트는 Object, String, Number, Boolean, Symbol, Date 등 40여개의 표준 빌트인 객체를 제공
- 생성자 함수인 표준 빌트인 객체가 생성한 인스턴스의 프로토타입은 표준 빌트인 객체의 protytype 프로퍼티에 바인딩된 객체이다.
    
    ```jsx
    // String 생성자 함수에 의한 String 객체 생성
    const strObj = new String('Lee'); // String {"Lee"}
    
    // String 생성자 함수를 통해 생성한 strObj 객체의 프로토타입은 String.prototype이다.
    console.log(Object.getPrototypeOf(strObj) === String.prototype); // true
    ```
    
- 표준 빌트인 객체는 인스턴스 없이도 호출이 가능한 빌트인 정적 메서드를 제공한다.

```jsx
// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(1.5); // Number {1.5}

// toFixed는 Number.prototype의 프로토타입 메서드다.
// Number.prototype.toFixed는 소수점 자리를 반올림하여 문자열로 반환한다.
console.log(numObj.toFixed()); // 2

// isInteger는 Number의 정적 메서드다.
// Number.isInteger는 인수가 정수(integer)인지 검사하여 그 결과를 Boolean으로 반환한다.
console.log(Number.isInteger(0.5)); // false
```

## 21.3 원시값과 래퍼 객체

- 원시 값은 객체가 아니므로 프토퍼티나 메서드를 가질 수 없는데 원시값인 문자열이 마치 객체처럼 동작한다.
    
    → 원시값인 문자열, 숫자, 불리언 값의 경우 이들 원시값에 대해 마치 객체처럼 마침표 표기법(또는 대괄호 표기법)으로 접근할때마다
     **자바스크립트 엔진이 일시적으로 원시값을 연관된 객체로 변환**해 주기 때문이다.
    
- 문자열, 숫자, 불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체 ⇒ **래퍼 객체**

![문자열 래퍼 객체의 프로토타입 체인](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/9736295e-26ef-4404-ace8-b3faaf69359e/Untitled.png)

문자열 래퍼 객체의 프로토타입 체인

- 래퍼 객체의 처리가 종료되면 식별자가 원시값을 갖도록 되돌리고 래퍼 객체는 가비지 컬렉션의 대상이 된다.
    
    ```jsx
    // ① 식별자 str은 문자열을 값으로 가지고 있다.
    const str = 'hello';
    
    // ② 식별자 str은 암묵적으로 생성된 래퍼 객체를 가리킨다.
    // 식별자 str의 값 'hello'는 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된다.
    // 래퍼 객체에 name 프로퍼티가 동적 추가된다.
    str.name = 'Lee';
    
    // ③ 식별자 str은 다시 원래의 문자열, 즉 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된 원시값을 갖는다.
    // 이때 ②에서 생성된 래퍼 객체는 아무도 참조하지 않는 상태이므로 가비지 컬렉션의 대상이 된다.
    
    // ④ 식별자 str은 새롭게 암묵적으로 생성된(②에서 생성된 래퍼 객체와는 다른) 또 다른 래퍼 객체를 가리킨다.
    // **새롭게 생성된 래퍼 객체에는 name 프로퍼티가 존재하지 않는다.**
    console.log(str.name); // undefined
    
    // ⑤ 식별자 str은 다시 원래의 문자열, 즉 래퍼 객체의 [[StringData]] 내부 슬롯에 할당된 원시값을 갖는다.
    // 이때 ④에서 생성된 래퍼 객체는 아무도 참조하지 않는 상태이므로 가비지 컬렉션의 대상이 된다.
    console.log(typeof str, str); // string hello
    ```
    

⇒ 암묵적으로 생성되는 래퍼객체가 있으므로 굳이 **new 연산자로 인스턴스를 생성할 필요가 없고 권장하지 않는다.**

! null과 undefined 값은 래퍼객체를 생성하지 않음에 유의!

## 21.4 전역 객체

- 코드가 실행되기 이전 단계에 자바스크립트 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수한 객체이며, 어떤 객체에도 속하지 않은 최상위 객체이다.
- 자바스크립트 환경에 따라 지칭하는 이름이 제각각이다.
    - 브라우저에서는 window, self, this, frames
    - Node.js 환경에서는 global 이 전역 객체를 가리킨다.

<aside>
✍️ **globalThis**
브라우저 환경과 Node.js 환경의 다양한 식별자를 통일한 식별자이다.

</aside>

- **전역 객체는 표준 빌트인 객체와 환경에 따른 호스트 객체, 그리고 var 키워드로 선언한 전역 변수와 전역 함수를 프로퍼티로 갖는다.**
- 전역 객체는 계층적 구조상 어떤 객체에도 속하지 않은 모든 빌트인 객체의 최상위 객체이다.
    - 프로토타입 상속 관계상에서 최상위라는 뜻은 아니다.
    - 전역 객체 자신이 어떤 객체의 프로퍼티도 아니며, 객체의 계층적 구조상 표준 빌트인 객체와 호스트 객체를 프로퍼티로 소유한다는 것을 말한다.
- 전역 객체의 특징
    - 개발자가 의도적으로 생성할 수 없다.
    - 전역 객체의 프로퍼티를 참조할 때 **window(global)을 생략할 수 있다.**
    
    ```jsx
    // 문자열 'F'를 16진수로 해석하여 10진수로 변환하여 반환한다.
    window.parseInt('F', 16); // -> 15
    // window.parseInt는 parseInt로 호출할 수 있다.
    // window 생략가능
    parseInt('F', 16); // -> 15
    
    window.parseInt === parseInt; // -> true
    ```
    
    - Object, RegExp, Date, Promise 등의 모든 표준 빌트인 객체를 프로퍼티로 가지고 있다.
    - 자바스크립트 실행 환경에 따라 추가적으로 프로퍼티와 메서드를 갖는다.
        - 브라우저 환경 → DOM, BOM, Canvas, XMLHttpRequest, fetch, requestAnimationFrame, SVG, Web Storage, Web Component, Web Worker와 같은 클라이언트 사이드 Web API
        - Node.js 환경 → Node.js 고유의 API를 호스트 객체로 제공
    - var 키워드로 선언한 전역 변수와 선언하지 않은 변수에 값을 할당한 암묵적 전역, 그리고 전역 함수는 전역 객체의 프로퍼티가 된다.
    
    ```jsx
    // var 키워드로 선언한 전역 변수
    var foo = 1;
    console.log(window.foo); // 1
    
    // 선언하지 않은 변수에 값을 암묵적 전역. bar는 전역 변수가 아니라 전역 객체의 프로퍼티다.
    bar = 2; // window.bar = 2
    console.log(window.bar); // 2
    
    // 전역 함수
    function baz() { return 3; }
    console.log(window.baz()); // 3
    ```
    
    - let이나 const 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다.즉, window.foo와 같이 접근할 수 없다. let이나 const로 선언한 전역변수는 **보이지 않는 개념적인 블록(전역 렉시컬 환경의 선언적 환경 레코드)내에 존재**하게 된다.
    
    ```jsx
    let foo = 123;
    console.log(window.foo); // undefined
    ```
    
    - 브라우저 환경의 모든 자바스크립트 코드는 하나의 전역 객체 window 를 공유한다.

### 21.4.1 빌트인 전역 프로퍼티

: 전역 객체의 프로퍼티

**`Infinity`**: 무한대를 나타내는 숫자 값 

`**NaN**` : 숫자가 아닌 값,  typeof NaN은 number이다.

**`undefined`**

### 21.4.2 빌트인 전역 함수

`**eval**`: 전달 받은 문자열 코드가 **표현식이라면 런타임에 평가하여 값을 생성하고, 표현식이 아닌 문이라면 문자열 코드를 런타임에 실행**한다.

```jsx
// 표현식인 문
eval('1 + 2;'); // 3
// 표현식이 아닌 문
eval('var x = 5;'); // undefined

// eval 함수에 의해 런타임에 변수 선언문이 실행되어 x 변수가 선언되었다.
console.log(x); // 5

// 객체 리터럴은 반드시 괄호로 둘러싼다.
const o = eval('( {a: 1} );');
console.log(o); // {a: 1}

// 함수 리터럴은 반드시 괄호로 둘러싼다.
const f = eval('( function() {return 1;} )');
console.log(f()); // 1

// 전달 받은 문자열이 여러개의 문으로 구성된 경우 마지막 결과값을 반환한다.
eval('1 + 2; 3 + 4;'); // 7
```

- 자신이 호출된 위치에 해당하는 기존의 스코프를 런타임에 동적으로 수정한다.

```jsx
const x = 1;

function foo(){
  eval('var x = 2;');
  console.log(x);
}

foo();
console.log(x);
```

- eval 함수를 통해 사용자로부터 입력받은 콘텐츠를 실행하는 것은 보안에 매우 취약하다.

⇒ **eval 함수의 사용을 금지해야한다.** 

`**isFinite**`: 정상적인 유한수이면  true, 무한수이면 false를 반환. 숫자 타입이 아니라면 자동으로 숫자 타입으로 변환한 후 검사를 수행한다.

`**isNaN**` : 전달 받은 인수가 NaN인지 검사하여 그 결과를 불리언 타입으로 반환. 자동 타입 변환 수행

`**parseFloat**`: 전달 받은 인수를 실수로 해석하여 반환.

**`parseInt`**: 전달 받은 인수를  정수로 해석하여 반환. 두번째 인수로 진법을 나타내는 기수를 전달할 수 있다. 반환값은 언제나 10진수이다.

- 기수를 지정하여 10진수 숫자를 해당 기수 문자열로 변환하고 싶을때는 **Number.prototype.toString** 메서드를 사용한다.

```jsx
const x = 15;

x.toString(2); // '1111'
parseInt(x.toString(2), 2) // 15

x.toString(8); // '17'
parseInt(x.toString(8), 8) // '17' 을 8진수로 해석한 결과 -> 15 (10)

x.toString(16); // 'f'
parseInt(x.toString(16), 16) // 15

// 숫자열을 문자열로 변환한다.
x.toString(); // '15'
parseInt(x.toString(), 2) // 15
```

- 두번째 인수로 기수를 전달하지 않아도 첫 번째 인수로 전달된 문자열이 0x, 0X 등의 16진수 리터럴이라면 그렇게 해석한다.
- 하지만 2진수 리터럴(0b)과 8진수 리터럴(0o)은 제대로 해석하지 못한다.
- ES6 부터는 0으로 시작하는 문자를 8진수로 해석하지 않고 10진수로 해석한다.

`**encodeURI / decodeURI**`

: 완전한 URI 를 문자열로 전달받아 이스케이프 처리를 위해 인코딩한다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/aac924b5-c1b2-4a31-af98-223aba0b46ac/Untitled.png)

- 이스케이프 처리는 네트워크를 통해 정보를 공유할 때 **어떤 시스템에서도 읽을 수 있는 아스키 문자 셋으로 변환**하는 것
- URL 은 아스키 문자 셋으로만 구성되어야 하며 한글을 포함한 대부분의 외국어나 아스키 문자 셋에 정의되지 않은 특수 문자의 경우 포함될 수 없다.
- 단 알파벳, 0~9의 숫자, -_.!~*'() 문자는 이스케이프 처리에서 제외된다.

```jsx
const uri = 'http://example.com?name=이웅모&job=programmer&teacher';
const enc = encodeURI(uri);
// http://example.com?name=%EC%9D%B4%EC%9B%85%EB%AA%A8&job=programmer&teacher
```

- `decodeURI` ****는 인코딩된 URI를 인수로 전달받아 이스케이프 처리 이전으로 디코딩한다.

`**encodeURIComponent / decodeURIComponent**`

: URI 구성 요소를 인수로 전달받아 인코딩(디코딩)한다. 

- 인수로 전달된 문자열을 URI 의 구성요소인 쿼리 스트링의 일부로 간주한다. 따라서 쿼리스트링 구분자로 사용되는 =, ?, & 까지 인코딩한다.

```jsx
const uriComp = 'name=이웅모&job=programmer&teacher';

let enc = encodeURIComponent(uriComp);
console.log(enc);
// name%3D%EC%9D%B4%EC%9B%85%EB%AA%A8%26job%3Dprogrammer%26teacher

let dec = decodeURIComponent(enc);
console.log(dec);
// name=이웅모&job=programmer&teacher

enc = encodeURI(uriComp);
console.log(enc);
// name=%EC%9D%B4%EC%9B%85%EB%AA%A8&job=programmer&teacher

dec = decodeURI(enc);
console.log(dec);
//name=이웅모&job=programmer&teacher
```

### 21.4.3 암묵적 전역

```jsx
var x = 10;
console.log(y); // ReferenceError : y is not defined -> 호이스팅이 발생하지 않음 

function foo(){
  y = 20; // window.y = 20;
}
foo();

// 선언하지 않은 식별자 y를 전역에서 참조할 수 있다.
console.log(x+y); // 30

delete y; 
```

- 선언하지 않은 식별자 y는 마치 선언된 전역변수처럼 동작한다. 선언하지 않은 식별자에 값을 할당하면 전역 객체의 프로퍼티가 되기 때문이다. ( window 생략 )
- y는 변수 선언 없이 단지 전역 객체의 프로퍼티로 추가되었을 뿐이므로 변수가 아니고 변수 호이스팅이 발생하지 않는다.
- 삭제하려면 delete 연산자를 사용한다.
