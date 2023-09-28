## 심벌이란?

- 변경 불가능한 원시 타입의 값
- 주로 **이름의 충돌 위험이 없는 유일한 프로퍼티 키를 만들기 위해** 사용
- **다른 값과 절대 중복되지 않는 유일무이한 값**

## 심벌 값의 생성

### Symbol 함수

- Symbol 함수를 호출하여 생성

```jsx
const mySymbol = Symbol(); // 절대 중복되지 않는 유일무이한 값을 생성
console.log(typeof mySymbol); // symbol

// 심벌 값은 외부로 노출되지 않아 확인할 수 없다.
console.log(mySymbol); // Symbol()

new Symbol(); // TypeError new 연산자와 함께 생성하지 않는다.
```

- Symbol 함수에는 선택적으로 문자열을 인수로 전달할 수 있는데, 이는 생성된 심벌 값에 대한 설명 정도로만 사용.

```jsx
// 심벌 값에 대한 설명이 같더라도 중복되지 않는 심벌 값을 생성
const mySymbol1 = Symbol('mySymbol');
const mySymbol2 = Symbol('mySymbol');
console.log(mySymbol1 === mySymbol2); // false
```

- 심벌 값도 객체처럼 접근하면 암묵적으로 래퍼 객체를 생성

```jsx
const mySymbol = Symbol('mySymbol');

// 심벌도 래퍼 객체를 생성한다.
console.log(mySymbol.description); // mySymbol
console.log(mySymbol.toString()); // Symbol(mySymbol)
```

- 심벌 값은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않지만 불리언 타입으로는 변환된다.

```jsx
const mySymbol = Symbol();

console.log(mySymbol + ''); // TypeError 문자열 변환 X
console.log(+mySymbol); // TypeError 숫자 타입 변환 X
console.log(!!mySymbol); // true 불리언 타입으로 암묵적 변환
// if문 등에서 존재 확인이 가능하다.
if(mySymbol) console.log('mySymbol is not empty');
```

### Symbol.for / Symbol.keyFor 메서드

- Symbol.for 메서드는 인수로 전달받은 문자열을 키로 사용하여 **해당 키와 일치하는 심벌 값을 검색**

⇒ 전역 심볼 레지스트리 등록 

```jsx
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 **없으면 새로 생성**
const s1 = Symbol.for('mySymbol');
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 **있으면 해당 값을 반환**
const s2 = Symbol.for('mySymbol');

console.log(s1 === s2); // true
```

- Symbol.keyFor 메서드를 사용하면 전역 심벌 레지스트리에 저장된 **심벌 값의 키를 추출**

⇒ 전역 심볼 레지스트리 검색

```jsx
// 전역 심벌 레지스트리에 mySymbol 키로 저장된 심벌 값이 없으면 새로 생성
const s1 = Symbol.for('mySymbol');
// 심벌 값의 키를 추출
Symbol.keyFor(s1); // mySymbol

// 전역 심벌 레지스트리에 등록되어 관리되지 않음
const s2 = Symbol('foo');
Symbol.keyFor(s2); // undefined
```

## 심벌과 상수

- 값에는 특별한 의미가 없고 상수 이름 자체에 의미가 있는 경우 변경/중복될 가능성이 없는 심벌 값을 사용할 수 있다.

```jsx
// 위, 아래, 왼쪽, 오른쪽을 나타내는 상수를 정의한다.
// 중복될 가능성이 없는 심벌 값으로 상수 값을 생성한다.
const Direction = {
  UP: Symbol('up'),
  DOWN: Symbol('down'),
  LEFT: Symbol('left'),
  RIGHT: Symbol('right')
};

const myDirection = Direction.UP;

if(myDirection === Direction.UP) {
  console.log('you are going up');
}
```

<aside>
💬 **enum**

명명된 숫자 상수의 집합 = 열거형
( 타입스크립트에서는 지원 ) 
자바스크립트에서는 Object.freeze() 로 객체 동결해서 enum처럼 사용 가능

</aside>

## 심벌과 프로퍼티 키

- 객체의 프로퍼티 키를 동적으로 생성할 수도 있다.
- 유일무이한 값이므로 다른 프로퍼티 키와 절대 충돌하지 않는다.

```jsx
// 심벌 값을 프로퍼티 키로 사용하려면 대괄호를 사용해야 한다.
// 다른 프로퍼티 키와 절대 충돌하지 않는다.
const obj = {
  [Symbol.for('mySymbol')]: 1
};

// 프로퍼티에 접근할 때도 마찬가지로 대괄호를 사용해야 한다.
obj[Symbol.for('mySymbol')]; // 1
```

## 심벌과 프로퍼티 은닉

- 심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티는 for ...in문이나 Object.keys, Object.getOwnPropertyNames 메서드로 찾을 수 없다.
    
    ⇒ 외부에 노출할 필요가 없는 **프로퍼티를 은닉**할 수 있다.
    

```jsx
const obj = {
  // 심벌 값으로 프로퍼티 키를 생성
  [Symbol('mySymbol')]: 1
};

for(const key in obj) {
  console.log(key); // 아무것도 출력되지 않음
}

console.log(Object.keys(obj)); // []
console.log(Object.getOwnPropertyNames(obj)); // []

//getOwnPropertySymbols 메서드는 인수로 전달한 객체의 심벌 프로퍼티 키를 배열로 반환
console.log(Object.getOwnPropertySymbols(obj)); // [Symbol(mySymbol)]

//getOwnPropertySymbols 메서드로 심벌 값도 찾을 수 있음
const symbolKey1 = Object.getOwnPropertySymbols(obj)[0];
console.log(obj[symbolKey1]); // 1
```

## 심벌과 표준 빌트인 객체 확장

- 일반적으로 표준 빌트인 객체에 사용자 정의 메서드를 직접 추가하여 확장하는 것은 권장하지 않는다.
- 미래에 표준 사양으로 추가될 메서드의 이름이 중복될 수 있기 때문이다.
- 중복될 가능성이 없는 심벌 값으로 프로퍼티 키를 생성하여 표준 빌트인 객체를 확장하면 어떤 프로퍼티 키와도 충돌할 위험이 없어 안전하게 확장할 수 있다. ⇒ 하위 호환성을 보장

```jsx
Array.prototype[Symbol.for('sum')] = function () {
  return this.reduce((acc, cur) => acc + cur, 0);
};

[1, 2][Symbol.for('sum')](); // 3
```

## Well-known Symbol

- 자바스크립트가 기본 제공하는 빌트인 심벌 값이 있는데, ECMAScript 사양에서는 이를 Well-known Symbol이라 부른다.
- 순회 가능한 iterable 은 Well-known Symbol 인 Symbol.iterator를 키로 갖는 메소드를 가진다.
- 빌트인 이터러블이 아닌 일반 객체를 이터러블처럼 동작하도록 구현하고 싶다면 이터레이션 프로토콜을 따르면 된다.
