## 16.1 내부 슬롯과 내부 메서드

- 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 *ECMAScript 사양에서 사용하는 의사 프로퍼티와 의사 메서드
- 이중 대괄호 ([[…]])로 감싼 이름들
- 개발자가 직접 접근할 수 있도록 외부로 공개된 객체의 프로퍼티는 아니다.
- 모든 객체는 [[ Prototype ]] 이라는 내부 슬롯을 갖는다.
    - 원칙적으로는 직접 접근할 수 없지만 __proto__ 를 통해 간접적으로 접근할 수 있다.

```jsx
const o = {};

o.[[Prototype]]	// Uncaught SyntaxError: Unexpected token '['
// 일부 내부 슬롯과 내부 메서드에 한하여 간접적으로 접근 가능하다.
o.__proto__		// Object.prototype
```

*ECMA스크립트(ECMAScript, 또는 ES)란, **Ecma International이 ECMA-262 기술 규격에 따라 정의하고 있는 표준화된 스크립트 프로그래밍 언어**를 말한다. 자바스크립트를 표준화하기 위해 만들어졌다.

## 16.2 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체

- 자바스크립트 엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동 정의한다.
- 프로퍼티 상태
    - 프로퍼티의 값 (value)
    - 값의 갱신 가능 여부 (writable)
    - 열거 가능 여부 (enumerable)
    - 재 정의 가능 여부 (configurable)

- 프로퍼티 어트리뷰트는 자바스크립트 엔진이 관리하는 내부 상태 값인 내부 슬롯이다.

→ 값에 직접 접근할 수는 없지만 `Object.getOwnPropertyDescriptor` 메서드를 사용하여 간접적으로 확인할 수는 있다.

```jsx
const person = {
  name: 'kozel',
  age: '100'
}

console.log(Object.getOwnPropertyDescriptor(person, 'name'); // 객체의 참조, 객체 프로퍼티 키
// {value: "kozel", writable: true, enumerable: true, configurable: true} -> 프로퍼티 디스크립터 객체 반환

// ES8부터 가능
console.log(Object.getOwnPropertyDescriptors(person);
/*
{
	name: {value: "kozel", writable: true, enumerable: true, configurable: true}
    age: {value: 100, writable: true, enumerable: true, configurable: true}
}
*/
```

## 16.3 데이터 프로퍼티와 접근자 프로퍼티

- 데이터 프로퍼티 : 키와 값으로 구성된 일반적인 프로퍼티
- 접근자 프로퍼티 : 자체적으로는 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 프로퍼티

### 16.3.1 데이터 프로퍼티

| 프로퍼티 어트리뷰트 | 프로퍼티 디스크립터 객체의 프로퍼티 | 설명 |
| --- | --- | --- |
| [[Value]] | Value | • 프로퍼티 키를 통해 프로퍼티 값에 접근하면 반환되는 값이다.
• 프로퍼티 키를 통해 프로퍼티 값을 변경하면 [[Value]]에 값을 재할당한다. 이때 프로퍼티가 없으면 프로퍼티를 동적 생성하고 생성된 프로퍼티의 [[Value]]에 값을 저장한다. |
| [[Writable]] | Writable | • 프로퍼티 값의 변경 가능 여부를 나타내며 불리언 값을 갖는다.
• [[Writable]]의 값이 false인 경우 해당 프로퍼티의 [[Value]]의 값을 변경할 수 없는 읽기 전용 프로퍼티가 된다. |
| [[Enumerable]] | Enumerable | • 프로퍼티의 열거 가능 여부를 나타내며 불리언 값을 갖는다.
• [[Enumerable]]의 값이 false인 경우 해당 프로퍼티는 for ... in 문이나 Object.keys 메서드 등으로 열거할 수 없다. |
| [[Configurable]] | Configurable | • 프로퍼티의 재정의 가능 여부를 나타내며 불리언 값을 갖는다.
• [[Configurable]]의 값이 false인 경우 해당 프로퍼티의 삭제, 프로퍼티 어트리뷰트 값의 변경이 금지된다. 단, [[Writable]]이 true인 경우 [[Value]]의 변경과 [[Writable]]을 false로 변경하는 것은 허용된다. |

⇒ 프로퍼티가 생성될 때나 동적 추가될 때 [[Value]]의 값은 프로퍼티 값으로 초기화 되며 
[[Writable]], [[Enumerable]], [[Configurable]]의 값이 **모두 true로 초기화** 된다.

### 16.3.2 접근자 프로퍼티

- 접근자 함수는 getter/setter 함수라고도 부른다.
- 접근자 프로퍼티는 getter와 setter 함수를 모두 정의할 수도 있고 하나만 정의할 수도 있다.

| 프로퍼티 어트리뷰트 | 프로퍼티 디스크립터 객체의 프로퍼티 | 설명 |
| --- | --- | --- |
| [[Get]] | Value | 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 읽을 때 호출되는 접근자 함수다. 즉, 접근자 프로퍼티 키로 프로퍼티 값에 접근하면 프로퍼티 어트리뷰트 [[Get]]의 값, 즉 getter 함수가 호출되고 그 결과가 프로퍼티 값으로 반환된다. |
| [[Set]] | Writable | 접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 저장할 때 호출되는 접근자 함수다. 즉, 접근자 프로퍼티 키로 프로퍼티 값을 저장하면 프로퍼티 어트리뷰트 [[Set]]의 값, 즉 setter 함수가 호출되고 그 결과가 프로퍼티 값으로 저장된다. |
| [[Enumerable]] | Enumerable | 데이터 프로퍼티의 [[Enumerable]]과 같다. |
| [[Configurable]] | Configurable | 데이터 프로퍼티의 [[Configurable]]과 같다. |

```jsx
const person = {
  // 데이터 프로퍼티
  firstName: 'Kozel',
  lastName: 'Gu',

  // fullName은 접근자 함수로 구성된 접근자 프로퍼티다.
  // getter 함수
  get fullName(){
    return `${this.firstName} ${this.lastName}`;
  },
  // setter 함수
  set fullName(name){
    // 배열 디스트럭처링 할당
    [this.firstName, this.lastName] = name.split(' ');
  }
};

// 데이터 프로퍼티를 통한 프로퍼티 값의 참조
console.log(person.firstName + ' ' + person.lastName);	// Kozel Gu
// 접근자 프로퍼티 fullName에 값을 저장하면 setter 함수가 호출
person.fullName = 'Foo Bar';
// **접근자 프로퍼티 fullName에 접근하면 getter 함수가 호출**
console.log(person.fullName);	// Foo Bar

// firstName 은 데이터 프로퍼티여서 프로퍼티 어트리뷰트를 가짐
// fullName 은 접근자 프로퍼티여서 접근자 프로퍼티만의 어트리뷰트를 가짐
```

접근자 프로퍼티 fullName으로 프로퍼티 값에 접근하면 내부적으로 [[Get]] 내부 메서드가 호출되어 다음과 같이 동작한다.

1. 프로퍼티 키가 유효한지 확인한다. 프로퍼티 키는 문자열 또는 심벌이어야 한다.
2. 프로토타입 체인에서 프로퍼티를 검색한다.
3. 검색된 fullName 프로퍼티가 데이터 프로퍼티인지 접근자 프로퍼티인지 확인한다.
4. 접근자 프로퍼티 fullName의 프로퍼티 어트리뷰트 [[Get]]의 값, 즉 getter 함수를 호출하여 그 결과를 반환한다.

<aside>
🗨️ **프로토타입**

어떤 객체의 상위(부모) 객체의 역할을 하는 객체다. 프로토타입은 하위(자식) 객체에게 자신의 프로퍼티와 메서드를 상속한다.
프로토타입 객체의 프로퍼티나 메소드를 상속받은 하위 객체는 자신의 프로퍼티 또는 메서드인 것 처럼 자유롭게 사용할 수 있다.

</aside>

? 접근자 프로퍼티와 데이터 프로퍼티를 구별하는 방법 ? 

프로퍼티 디스크립터 객체의 프로퍼티가 다른 것을 통해 확인할 수 있다.

```jsx
// 일반 객체의 __proto__는 접근자 프로퍼티다.
Object.getOwnPropertyDescriptor(Object.prototype, '__proto__');
// {get: f, set: f, enumerable: false, configurable: true}

// 함수 객체의 prototype은 데이터 프로퍼티다.
Object.getOwnPropertyDescriptor(function(){}, 'prototype');
// {value: {...}, writable: true, enumerable: false, configurable: false}
```

## 16.4 프로퍼티 정의

- 새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 명시적으로 정의하거나
- 기존 프로퍼티의 프로퍼티 어트리뷰트를 재정의하는 것을 말한다.

⇒ 객체의 프로퍼티가 어떻게 동작해야 하는지를 명확히 정의

⇒ `Object.defineProperty` 메서드 사용! 

```jsx
const person = {};

// 데이터 프로퍼티 정의
Object.defineProperty(person, 'firstName', {
  value: 'Kozel',
  writable: true,
  enumerable: true,
  configurable: true // 삭제 or 재정의
});
// 디스크립트 객체의 프로퍼티를 누락시키면 undefined, false가 기본값이다.
Object.defineProperty(person, 'lastName', {
  value: 'Lee'
});

// [[Enumerable]]의 값이 false인 경우 해당 프로퍼티는 for...in 문이나 Object.keys 등으로 열거할 수 없다.
console.log(Object.keys(person));	// ["firstName"]

// [[Writable]]의 값이 false인 경우 해당 프로퍼티의 [[Value]]의 값을 변경할 수 없다.
person.lastName = 'Kim';	// 에러는 발생하지 않고 무시된다.

// [[Configurable]]의 값이 false인 경우 해당 프로퍼티를 삭제하거나 재정의 할 수 없다.
delete person.lastName;	// 삭제하면 에러는 발생하지 않고 무시된다.
						// 재정의 할 경우 에러가 발생한다.

Object.defineProperty(person, 'fullName', {
 get fullName(){
    return `${this.firstName} ${this.lastName}`;
  },
  // setter 함수
  set fullName(name){
    // 배열 디스트럭처링 할당
    [this.firstName, this.lastName] = name.split(' ');
  },  
  enumerable: true,
  configurable: true // 삭제 or 재정의

});
```

프로퍼티 디스크립터 객체에서 생략된 어트리뷰트는 다음과 같이 기본값이 적용된다.

| 프로퍼티 디스크립터 객체의 프로퍼티 | 대응하는 프로퍼티 어트리뷰트 | 생략했을 때의 기본값 |
| --- | --- | --- |
| value | [[Value]] | undefined |
| get | [[Get]] | undefined |
| set | [[Set]] | undefined |
| writable | [[Writable]] | false |
| enumerable | [[Enumerable]] | false |
| configurable | [[Configurable]] | false |

- Object.defineProperties 메서드를 사용하면 여러 개의 프로퍼티를 한 번에 정의할 수 있다.

```jsx
const person = {};

Object.defineProperties(person, {
  // 데이터 프로퍼티 정의
  Name: {
    value: 'Kozel',
    writable: true,
    enumerable: true,
    configurable: true
  },
  // 접근자 프로퍼티 정의
  referenceName: {
    get() {
      return `${this.Name}`;
    },
    set(name){
      this.Name = name;
    },
    enumerable: true,
    configurable: true
  }
});
```

## 16.5 객체 변경 방지

- 자바스크립트는 객체의 변경을 방지하는 다양한 메서드를 제공한다. 각각 객체의 변경을 금지하는 강도가 다르다.

| 구분 | 메서드 | 프로퍼티 추가 | 프로퍼티 삭제 | 프로퍼티 값 읽기 | 프로퍼티 값 쓰기
(기존 값 변경) | 프로퍼티 어트리뷰트 재정의 |
| --- | --- | --- | --- | --- | --- | --- |
| 객체 확장 금지 | Object.preventExtensions | X | O | O | O | O |
| 객체 밀봉 | Object.seal | X | X | O | O | X |
| 객체 동결 | Object.freeze | X | X | O | X | X |

### 16.5.1 객체 확장 금지

- 확장이 금지된 객체는 프로퍼티 추가가 금지된다.
- `**Object.isExtendsible**` 메서드로 확인할 수 있다.

### 16.5.2 객체 밀봉

- 밀봉된 객체는 읽기와 쓰기만 가능하다.
- **`Object.isSealed`**메서드로 확인할 수 있다.

### 16.5.3 객체 동결

- 동결된 객체는 읽기만 가능하다.
- **`Object.isFrozen`**메서드로 확인할 수 있다.

### 16.5.4 불변 객체

- 지금까지 살펴본 변경 방지 메서드들은 얕은 변경 방지로 직속 프로퍼티만 변경이 방지되고 중첩 객체까지는 영향을 주지 못한다.
- 객체를 동결해도 중첩 객체까지 동결할 수 없다.
- 객체의 중첩 객체까지 동결하여 변경이 불가능한 읽기 전용의 불변 객체를 구현하려면 객체를 값으로 갖는 모든 프로퍼티에 대해 재귀적으로
    
    **`Object.freeze` 메서드를 호출해야한다.**
