## 9.1 타입변환

- `명시적 타입변환 / 타입 캐스팅` : 개발자가 의도적으로 값의 타입을 변환하는 것
- `암묵적 타입변환 / 타입 강제변환` : 개발자의 의도와 상관없이 표현식을 평가하는 도중에 JS엔진에 의해 암묵적으로 타입이 자동 변환되는 것
- **둘 다 기존 원시 값을 직접 변경하는 것은 아니다.**
- 타입 변환이란 기존 원시 값을 사용해 **다른 타입의 새로운 원시 값을 생성하는 것**이다.

## 9.2 암묵적 타입 변환

- 자바스크립트 엔진은 코드의 문맥을 고려해 암묵적으로 데이터 타입을 강제 변환할때가 있다.

### 9.2.1 문자열 타입으로 변환

- 문자열 연결 연산자 + 가 활용된 표현식을 평가하기 위해서 피연산자를 모두 문자열로 암묵적 타입변환한다.
- 피연산자만이 암묵적 타입변환의 대상이 되는 것은 아니고, 표현식을 평가할 때 코드 문맥에 부합하도록 암묵적 타입 변환을 실행한다.
- 예를 들어 템플릿 리터럴의 표현식 삽입은 표현식의 평가 결과를 문자열 타입으로 암묵적 타입 변환한다.

```jsx
`1 + 1 = ${1 + 1}` 
```

```
// 숫자 타입
0 + ''              // "0"
  -0 + ''             // "0"
1+ ''               // "1"
  -1 + ''             // "-1"
NaN + ''            // "NaN"
Infinity + ''       // "Infinity"
  -Infinity + ''      // "-Infinity"

// boolean 타입
true + ''           // "true"
false + ''          // "false"

// null 타입
null + ''           // "null"

// undefined 타입
undefined + ''      // "undefined"

// 심벌 타입
(Symbol()) + ''     // "TypeError"

// 객체 타입
({}) + ''           // "[object Object]"
Math + ''           // "[object Math]"
[] + ''             // ""
[10, 20] + ''       // "10, 20"
(function(){}) + '' // "function()"
Array + ''          // "function Array() {[native code]}"
```

→ 문자열로 형변환하고 싶을때 +’ ‘ 를 활용하면 유용할 듯?!

### 9.2.2 숫자 타입으로 변환

- 산술 연산자의 모든 피연산자는 코드 문맥상 모두 숫자 타입이어야 한다.
- 산술 연산자 표현식을 평가하기 위해 산술 연산자의 피연산자 중에서 숫자 타입이 아닌 피연산자를 숫자 타입으로 암묵적 타입 변환한다.
- 피연산자를 숫자 타입으로 변환할 수 없는 경우에는 표현식의 평가 결과는 NaN이 된다.
- 비교 연산자 또한 마찬가지이다.

```
// 숫자 타입
+''               // 0
+'0'              // 0
+'1'              // 1
+'string'         //  NaN

// boolean 타입
+true             //  1
+false            //  0

// null 타입
+null             //  0

// undefined 타입
+undefined        //  NaN

// 심벌 타입
+Symbol()         //  TypeError

// 객체 타입
+{}               //  NaN
+[]               //  0
+[10, 20]         //  NaN
+(function(){})   //  NaN
```

- + 단항 연산자는 피연산자가 숫자 타입의 값이 아니면 숫자 타입의 값으로 암묵적 타입 변환을 수행한다.
- **객체와 빈 배열이 아닌 배열, undefined는 변환되지 않아 NaN이 된다는 것에 주의**하자.

### 9.2.3 불리언 타입으로 변환

- if문이나 for문과 같은 제어문 또는 삼항 조건 연산자의 조건식은 불리언 값, 즉 논리적 참/거짓으로 평가되어야 하는 표현식이다.
- 자바스크립트 엔진은 **조건식의 평가 결과를 불리언 타입으로 암묵적 타입 변환**한다.

```jsx
if(' ') console.log('1'); //빈문자열은 Falsy한 값
if(true) console.log('2');
if(0) console.log('3');		//0은 Falsy한 값
if('str') console.log('4'); //문자열은 Truthy한 값
if(null) console.log('5');	//null은 Falsy한 값
```

<aside>
💡 **false로 평가되는  Falsy 값**

- false
- undefined
- null
- 0, -0
- NaN
- ‘ ‘ (빈 문자열)

</aside>

⇒ Falsy 값 외의 모든 값은 모두 true로 평가되는 Truthy 값이다.

⇒ ‘0’, {} , []  은 falsy 가 아니라 true 값임에 유의!

## 9.3 명시적 타입 변환

- 표준 빌트인 생성자 함수를 new 연산자 없이 호출하는 방법
- 빌트인 메서드를 사용하는 방법
- 암묵적 타입 변환을 이용하는 방법

### 9.3.1 문자열 타입으로 변환

1. String 생성자 함수를 new 연산자 없이 호출하는 방법
2. Object.prototype.toString 메서드를 사용하는 방법
3. 문자열 연결 연산자를 이용하는 방법

1. String 생성자 함수를 new연산자 없이 호출

```jsx
//숫자->문자열타입
String(1);		//-> '1'
String(NaN);		//-> 'NaN'

//불리언 타입 -> 문자열타입
String(true) 		//->'true'
String(false)		//->'false'
```

1. Object.prototype.toString 메서드 사용 -> `toString()`

```jsx
  //숫자->문자열타입
  (1).toString();	//-> '1'
  (NaN).toString();	//-> 'NaN'

  //불리언 -> 문자열타입
  (true).toString();	//-> 'true'
  (false).toString();	//-> 'false'
```

1. `+`(문자열 연결 연산자) 이용

```jsx
  //숫자 -> 문자열 타입
  1 + ''; 	//-> '1'
  //불리언 -> 문자열 타입
  true + '' //'true'
```

### 9.3.2 숫자 타입으로 변환

1. Number 생성자 함수를 new 연산자 없이 호출하는 방법
2. parseInt, parseFloat 함수를 사용하는 방법(문자열만 숫자 타입으로 변환 가능)
3. +단항 산술 연산자를 이용하는 방법
4. *산술 연산자를 이용하는 방법

> 
> 
> 1. `Number 생성자 함수`를 new연산자 없이 호출하는 방법
> 
> ```jsx
>   //문자열 -> 숫자타입
>   Number('0');	//0
>   Number('10.53');	//10.53
> 
> //불리언 -> 숫자타입
> Number(true);		//1
> Number(false);	//0
> ```
> 
> 1. `PareInt` `ParseFloat`함수를 사용하는 방법(문자열->숫자타입으로만 가능)
> 
> ```jsx
>   //문자열 -> 숫자타입
> ParseInt('-1');		//-1
> ParseFloat('10.53');	//10.53
> ```
> 
> 1. `+단항연산자` 이용
> 
> ```jsx
> //문자열 -> 숫자
>   +'0';		//0
>   +'10.53'	//10.53
> 
> //불리언 -> 숫자
>   +true;	//1
>   +false;	//0
> ```
> 
> 1. `* 산술연산자` 이용
> 
> ```jsx
>   //문자 -> 숫자타입
>   '-1' * 1;		//1
>   '3' * 0;		//0
> 
>   //불리언 -> 숫자타입
>   true * 1; 	//1
>   false * 1;	//0
>   true * 30;	//30
> ```
> 

### 9.3.3 불리언 타입으로 변환

1. boolean 생성자 함수를 new연산자 없이 호출하는 방법
2. ! 부정 논리 연산자를 두 번 사용하는 방법

> 
> 
> 1. `Boolean 생성자 함수`를 new 연산자 없이 호출하는 방법
> 
> ```jsx
>   //문자열 -> 불리언
>   Boolean('x');			//true
>   Boolean(' ');			//false
>   Boolean('false');		//true
> ```
> 
> ```jsx
> //숫자 -> 불리언
> Boolean(0);			//false
> Boolean(1);			//true
> Boolean(NaN);			//false
> Boolean(Infinity);	//true
> ```
> 
> ```jsx
> //null타입 -> 불리언
> Boolean(null);		//false
> ```
> 
> ```jsx
> //undefined -> 불리언
> Boolean(undefined);	//false
> ```
> 
> ```jsx
> //객체 타입 -> 불리언
> Boolean({});			//true
> Boolean([]);			//true
> ```
> 
> 1. `! 부정 논리 연산자`를 두번 사용하는 벙법
> 
> ```jsx
>   //문자열 -> 불리언
>   !!'x';			//true
>   !!' ';			//false
> ```
> 
> ```jsx
>   //숫자 -> 불리언
>   !!0;				//false
>   !!1;				//true
>   !!NaN;			//false
>   !!Infinity		//true
> ```
> 
> ```jsx
>   //null타입 -> 불리언
>   !!null;			//false
> ```
> 
> ```jsx
>   //undefined타입 -> 불리언
>   !!undefined;		//false
> ```
> 
> ```jsx
>   //객체타입 -> 불리언
>   !!{};				//true
>   !![];				//true
> ```
> 

## 9.4 단축 평가

### 9.4.1 논리 연산자를 사용한 단축 평가

```jsx
'cat' && 'dog' // "dog"
```

- 논리곱 연산자는 두개의 피연산자가 모두 true로 평가될 때 true를 반환한다.
- 논리 곱 연산자는 좌항 → 우항으로 평가가 진행된다.
- 두 번째 피연산자가 논리곱 연산자 표현식의 평가 결과를 결정한다. 이때 논리곱 연산자는 논리 연산의 결과를 결정하는 두번째 피연산자를 그대로 반환한다.

```jsx
 "cat" || "dog" // "cat"
```

- 논리 합 연산자 ( || ) 도 마찬가지이다.
- 두 개의 피연산자 중 하나만 true로 평가되어도 true를 반환한다.
- 논리연산의 결과를 결정한 첫번째 피연산자를 그대로 반환한다.

논리 연산의 결과를 결정하는 피연산자를 타입 변환하지 않고 그대로 반환한다. 

**⇒ 단축평가 ( 표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것 )** 

| 단축 평가 표현식 | 평가 결과 |
| --- | --- |
| true || anything | true |
| false || anything | anything |
| true && anything | anything |
| false && anything | false |
- 단축 평가를 잘 사용하면 if 문을 대체할 수 있다.
- 어떤 조건이 Truthy 값 ( 참으로 평가되는 값 ) 일 때 무언가를 해야한다면 논리곱(&&) 연산자 표현식으로 if문을 대체할 수 있다.
- 어떤 조건이 Falsy 값 ( 거짓으로 평가되는 값 ) 일 때 무언가를 해야한다면 논리합(||) 연산자 표현식으로 if문을 대체할 수 있다.

단축평가 활용하기!

- 객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때

```jsx
var elem = null; 

var value = elem && elem.value; // elem 이 존재할 경우에만 
```

- 함수 매개변수에 기본값을 설정할 때

```jsx
function f(str){

str = str || ' ';

}

function f(str = ''){

}
```

### 9.4.2 옵셔널 체이닝 연산자

- `?` 는 좌항의 피연산자가 null 또는 undefined인 경우 undefined를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.

```jsx
var elem = null;

var value = elem?.value;
```

- 옵셔널 체이닝 연산자는 객체를 가리치기를 기대하는 변수가 null 또는 undefined인지 확인하고 프로퍼티를 참조할 때 유용하다.

```jsx
var str =' ';

var length = str && str.length; // ' ' ->  문자열의 길이를 참조하지 못한다.

var str =' ';

var length = str?.length; // 0
```

- 0이나 ‘ ‘ 은 객체로 평가될 때도 있다.
- 하지만 옵셔널 체이닝 연산자는 좌항 피연산자가 false로 평가되는 Falsy값 이라도 null 또는 undefined가 아니면 우항의 프로퍼티 참조를 이어간다.

### 9.4.3 null 병합 연산자

- **피연산자가 null 또는 undefined 일 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환**한다.

```jsx
var foo = null ?? 'default string';
```

- 변수에 기본값을 설정할 때 유용하다.
    - 이전에는 || 를 사용했으나 Falsy 값인 0이나 ‘’도 기본값으로도 유효하다면 예기치 않은 동작이 발생할 수 있다.
    
    ```jsx
    var foo = ‘ ‘|| default string’ // ‘default string’ 반환
    ```
    
    - 하지만 ?? 는 좌항이 false로 평가되는 Falsy값이라도 null또는 undefined가 아니면 좌항의 피연산자를 그대로 반환한다.
    
    ```jsx
    var foo = '' ?? 'default string';  // ' ' 반환
    ```
