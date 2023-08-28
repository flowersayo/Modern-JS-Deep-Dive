## 10.1 객체란?

- 자바스크립트를 구성하는 거의 "모든 것"이 객체다.
- 원시 값을 제외한 나머지 값(함수, 배열, 정규표현식)은 모두 객체다.
- 객체 타입은 다양한 타입의 값을 하나의 단위로 구성한 복합적인 자료구조이다.
- 객체는 변경 가능한 값이다.
- 객체는 0개 이상의 프로퍼티로 구성된 집합이며, 프로퍼티는 키와 값으로 구성된다.
    - 프로퍼티 : 객체의 상태를 나타내는 값
    - 메서드 : 프로퍼티를 참조하고 조작할 수 있는 동작
- 자바스크립트에서 사용할 수 있는 모든 값은 프로퍼티 값이 될 수 있다.
    - **함수도 일급 객체이므로** 값으로 취급할 수 있고 프로퍼티 값으로 사용할 수 있다.
    - 프로퍼티 값이 함수일 경우, 일반 함수와 구분하기 위해 메서드라고 부른다.
    

⇒ 상태와 동작을 하나의 단위로 구조화할 수 있어 유용하다.

## 10.2 객체 리터럴에 의한 객체 생성

- 객체지향 언어에서는 클래스를 사전에 정의하고 필요한 시점에 생성자를 호출하여 인스턴스를 생성
    - 인스턴스란 클래스에 의해 생성되어 메모리에 저장된 실체
    - 객체 = 클래스 + 인스턴스
- 다양한 객체 생성 방법
    - 객체 리터럴 ⇒ 가장 일반적!
    
    ```jsx
     var person = {
    
    		name :'kimseoyeon',
    		sayHello : function(){
    		}
    	};
    ```
    
    - Object 생성자 함수
    - 생성자 함수
    - Object.create 메서드
    - 클래스(ES6)

## 10.3 프로퍼티

객체는 프로퍼티의 집합이며, 프로퍼티는 키와 값으로 구분된다.

- 프로퍼티 키 : 빈 문자열을 포함하는 모든 **문자열** 또는 심벌 값
- 프로퍼티 값 : 자바스크립트에서 사용할 수 있는 모든 값

- 식별자 네이밍 규칙을 준수하는 이름, 자바스크립트에서 사용 가능한 유효한 이름인 경우 따옴표를 생략할 수 있다.
- 반대로 말하면 식별자 네이밍 규칙을 따르지 않는 이름에는 반드시 따옴표를 사용해야 한다.

```jsx
var person = {
      firstname:'Ung-mo',
      //식별자 네이밍 규칙을 준순한 프로퍼티 키 -> 따옴표 생략!!!
      'last-name' : 'lee'
      //식별자 네이밍 규칙을 준수하지 않은 프로퍼티 키 -> 따옴표 생략 불가!!!
      //(-)는 네이밍 규칙에 어긋난다.
    };
    console.log(person);
```

- 문자열 또는 문자열로 평가할 수 있는 표현식을 사용해 프로퍼티 키를 동적으로 생성할 수 있는데, 이 경우에는 표현식을 대괄호 [] 로 묶어주어야한다.

```jsx
      var obj ={};
      var key = 'hello';
      //Es5 프로퍼티 키 동적 생성
      obj[key] = 'world';
      //변수 key의 값인 hello가 프로퍼티 키가 되고 'world'가 그 값으로 들어가게 된다.
      //ES6 : 계산된 프로퍼티 이름
      //var obj = {[key]: 'world'};
      console.log(obj); //{hello:'wolrd'}
```

- 빈 문자열을 프로퍼티 키로 사용해도 문제가 발생하지 않는다. 하지만 키로서의 의미를 갖지 못하므로 권장하지 않는다.
- 프로퍼티 키에 문자열이나 심벌 값 외의 값을 사용하면 **암묵적 타입 변환을 통해 문자열이 된다.**
- 이미 존재하는 프로퍼티 키를 중복 선언하면 나중에 선언한 프로퍼티가 먼저 선언한 프로퍼티를 덮어쓴다.

## 10.4 메서드

- 객체에 묶여있는 함수

```jsx
 var circle = {
  radius : 5,
  //원의 지름
  getDiameter : function(){ //메서드
  	return 2 * this.radius; //this는 circle을 가리킨다.
    }
   };
   console.log(circle.getKIameter()); //10
```

## 10.5 프로퍼티 접근

- 마침표 프로퍼티 접근 연산자 `.` 를 사용하는 마침표 표기법
- 대괄호 프로퍼티 접근 연산자 `[]` 를 사용하는 대괄호 표기법
    - 프로퍼티 접근 연산자 내부에 지정하는 프로퍼티 키는 반드시 따옴표로 감싼 문자열이어야 한다.
    - 따옴표로 감싸지 않으면 식별자로 해석한다.

- 식별자 네이밍 규칙을 따르면 둘다 이용할 수 있고, 그렇지 않으면 대괄호 표기법만 이용할 수 있다.
- **객체에 존재하지 않는 프로퍼티에 접근하면 error가 아닌 undefined를 반환한다.**

```jsx
var person = {
  'last-name' : 'lee',
  1:10
};
person.'last-name'; //SyntaxError : Unexpected string
person.last-name; //브라우저 환경 : NaN
				  // Node.js환경 : ReferenceError : name is not defined
person[last-name]; //ReffernceErro : last is not defined
person['last-name']; //lee

```

- person.last-name 은 `person.last` - `name` 으로 해석되는데, `person.last` 는 undefined 값을 반환하고
- node.js 환경에서는 name 이라는 식별자를 찾을 수 없어 reference error가 발생하지만, 브라우저 환경에서는  전역객체 window의 프로퍼티로서 
name이라는 식별자가 암묵적으로 존재하므로 브라우저에서는 undefined - ‘’  = NaN 로 평가된다.

```jsx
//프로퍼티 키가 숫자로 이루어진 문자열인 경우 따옴표를 생략 할 수 있다.
person.1; //SyntaxError : Unexpected number;
person.'1'; //SyntaxErro : Unexpected string
person[1]; //10 : person[1] -> person['1']
person['1']; //10
```

## 10.6 프로퍼티 값 갱신

- 이미 존재하는 프로퍼티에 값을 할당하면 프로퍼티 값이 갱신된다.

```jsx
var person - {
  name : 'lee'
};
//person 객체에 name프로퍼티가 존재하므로 name 프로퍼티의 값이 갱신된다.
person.name = 'Kim';
console.log(person); //{name : 'kim'}
```

## 10.7 프로퍼티 동적 생성

- 존재하지 않는 프로퍼티에 값을 할당하면 프로퍼티가 동적으로 생성되어 추가되고 프로퍼티 값이 할당된다.

```jsx
var person = {
  name : 'lee'
};
//person 객체에는 age 프로퍼티가 존재하지 않는다.
//따라서 person 객체에 age 프로퍼티가 동적으로 생성되고 값이 할당된다.
person.age = 20;
console.log(person);
//{name: 'lee', age: 20}
```

## 10.8 프로퍼티 삭제

- delete 연산자는 객체의 프로퍼티를 삭제한다.
- delete 연산자의 피연산자는 프로퍼티 값에 접근할 수 있는 표현식이어야한다.

```jsx
var person = {
  name: 'lee'
};
//프로퍼티 동적 생성
person.age = 20;
//person 객체에 age 프로퍼티가 존재한다.
//따라서 delete연산자로 age 프로퍼티를 삭제할 수 있다.
delete person.age;
//person 객체에 address프로퍼티가 존재하지 않는다.
//따라서 delete연산자로 address프로퍼티를 삭제할 수 없다. 이때 에러가 발생하지 않는다.
delete person.address;
console.log(person); {name: 'lee'}
```

## 10.9 ES6에서 추가된 객체 리터럴의 확장 기능

### 10.9.1 프로퍼티 축약 표현

- 프로퍼티 값으로 변수를 사용하는 경우 변수 이름과 프로퍼티 키가 동일한 이름일 때 프로퍼티 키를 생략할 수 있다.
- 프로퍼티 키는 변수 이름으로 자동 생성된다.

```jsx
//ES6
let x= 1, y=2;
//프로퍼티 축약표현
const obj{x,y};
console.log(obj); //{x: 1, y:2}
```

### 10.9.2 계산된 프로퍼티 이름

- 문자열 또는 문자열로 타입 변환할 수 있는 값으로 평가되는 표현식을 사용해 프로퍼티 키를 동적으로 생성할 수도 있다.
- 단, 프로퍼티 키로 사용할 표현식을 대괄호 `[]` 로 묶어야 한다. 이를 계산된 프로퍼티 이름이라고 한다.
- ES5에서는 계산된 프로퍼티 이름으로 동적 생성하려면 객체 리터럴 외부에서 `[]` 표기법을 사용해야 한다.
- 그러나 ES6에서는 객체 내부에서도 계산된 프로퍼티 이름으로 프로퍼티 키를 생성 할 수 있다.

```jsx
//ES5
var prefix = 'prop';
var i = 0;
var obj ={};
//계산된 프로퍼티 이름으로 프로퍼티 키 동적 생성
obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;
console.log(obj); //{prop-1: 1, prop-2:2, prop-3:3}
```

```jsx
//ES6
const prefix = 'prop';
let i = 0;
//객체 리터럴 내부에서 계산된 프로퍼티 이름으로 프로퍼티 키를 동적 생성
const obj = {
[ `${prefix}=${++i}`]: i,
[ `${prefix}=${++i}`]: i,
[ `${prefix}=${++i}`]: i
	};
    console.log(obj); //{prop-1: 1, prop-2: 2, prop-3:3}
```

### 10.9.3 메서드 축약 표현

- ES5에서 메서드를 정의하려면 프로퍼티 값으로 함수를 할당한다.

```jsx
var obj = {
  name : 'lee'
  sayHi : function(){
    console.log('Hi!'+this.name);
  }
};
obj.sayHi(); //Hi! lee
```

- ES6에서는 메서드를 정의할때 **function키워드를 생략한 축약 표현을 사용 할 수 있다.**

```jsx
const obj = {
  name : 'lee',
  //메서드 축약 표현
  **sayHi(){
    console.log('Hi!'+ this.name);
  }**
};
obj.sayhi(): //Hi! lee
```

- ES6의 메서드 축약 표현으로 정의한 메서드는 프로퍼티에 할당한 함수와 다르게 동작한다.
  
