## 이터레이션 프로토콜

> **순회 가능한 데이커 컬렉션(자료구조)을 만들기 위해** ECMAScript 사양에 정의하여 미리 약속한 규칙
> 

ES6에서는 순회 가능한 데이터 컬렉션을 이터레이션 프로토콜을 준수하는 이터러블로 통일하여 

for...of문, 스프레드 문법, 배열 디스트럭처링 할당의 대상으로 사용할 수 있도록 일원화했다.

이터레이션 프로토콜에는 **이터러블** 프로토콜과 **이터레이터** 프로토콜이 있다.

- **이터러블 프로토콜**
    
    Well-known Symbol인 Symbol.iterator를 프로퍼티 키로 사용한 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환한다. 이러한 규약을 이터러블 프로토콜이라 하며, **이터러블 프로토콜을 준수한 객체를 이터러블이라 한다. 이터러블은 for ... of 문으로 순회할 수 있으며 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용할 수 있다.**
    
- **이터레이터 프로토콜**
    
    이터러블의 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 **이터레이터**를 반환한다. 이터레이터는 next 메서드를 소유하며 next 메서드를 호출하면 이터러블을 순회하며 value와 done 프로퍼티를 갖는 **이터레이터 리절트 객체**를 반환한다. 이러한 규약을 이터레이터 프로토콜이라 하며, **이터레이터 프로토콜을 준수한 객체를 이터레이터라 한다.** **이터레이터는 이터러블의 요소를 탐색하기 위한 포인터 역할**을 한다.
    

### 이터러블

: 이터러블 프로토콜을 준수한 객체.

```jsx
const isIterable = v => v !== null && typeof v[Symbol.iterator] === 'function';

// 배열, 문자열, map, set 등은 이터러블이다.
isIterable([]); // true
isIterable(''); // true
isIterable(new Map()); // true
isIterable(new Set()); // true
isIterable({}); // false

const arr = [1, 2, 3];

//📌 배열은 Array.prototype의 Symbol.iterator 메서드를 상속받는 이터러블이다
console.log(Symbol.iterator in arr); // true

//📌 이터러블인 배열은 for...of문으로 순회 가능하다.
for(const item of arr) {
  console.log(item);
}

//📌 이터러블인 배열은 스프레드 문법의 대상으로 사용할 수 있다.
console.log([...arr]); // [1, 2, 3]

//📌 이터러블인 배열은 배열 디스트럭처링 할당의 대상으로 사용할 수 있다.
const [a, ...rest] = arr;
console.log(a, rest); // 1 [2, 3]
```

- 일반 객체는 이터러블 프로토콜을 준수한 이터러블이 아니다.

```jsx
const obj = { a: 1, b: 2 };

// 일반 객체는 Symbol.iterator 메서드를 구현하거나 상속받지 않는다.
console.log(Symbol.iterator in obj); // false

// 이터러블이 아닌 객체는 for...of문으로 순회할 수 없다.
for(const item of obj) {
  console.log(item); // TypeError
}

// 이터러블이 아닌 객체는 배열 디스트럭처링 할당의 대상으로 사용할 수 없다.
const [a, b] = obj; // TypeError

// 📌 객체 리터럴 내부에서 스프레드 문법의 사용을 허용한다.
console.log({ ...obj }); // { a: 1, b: 2 }
```

### 이터레이터

이터러블의 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환한다. 

- 이때 이터레이터는 next 메서드를 갖는다.

```jsx
const arr = [1, 2, 3]; // 이터러블

const iterator = arr[Symbol.iterator](); //이터레이터를 반환

// next 메서드를 갖는다.
console.log('next' in iterator); // true
```

- 이터레이터의 next 메서드는 이터러블의 **각 요소를 순회하기 위한 포인터**의 역할을 한다.
    - value: 값
    - done : 순회결과

```jsx
const arr = [1, 2, 3];

const iterator = arr[Symbol.iterator](); // 이터레이터 반환

//📌 next 메서드를 호출하면 이터러블을 순회하며 순회 결과를 나타내는 이터레이터 리절트 객체를 반환
// 이터레이터 리절트 객체는 value와 done 프로퍼티를 갖는다.
// done 프로퍼티는 순회 완료 여부를 나타냄
console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: 3, done: false }
console.log(iterator.next()); // { value: undefined, done: true }
```

## 빌트인 이터러블

자바스크립트는 이터레이션 프로토콜을 준수한 객체인 **빌트인 이터러블**을 제공한다.

| 빌트인 이터러블 | Symbol.iterator 메서드 |
| --- | --- |
| Array | Array.prototype[Symbol.iterator] |
| String | String.prototype[Symbol.iterator] |
| Map | Map.prototype[Symbol.iterator] |
| Set | Set.prototype[Symbol.iterator] |
| TypedArray | TypedArray.prototype[Symbol.iterator] |
| arguments | arguments[Symbol.iterator] |
| DOM 컬렉션 | HTMLCollection.prototype[Symbol.iterator], NodeList.prototype[Symbol.iterator] |

## for...of 문

- for...of 문은 이터러블을 순회하면서 이터러블의 요소(값)를 변수에 할당한다.
    
    > `for(변수선언문 of 이터러블) { ... }`
    > 

```jsx
// 내부적으로 next 메서드를 호출하여 이터러블을 순회하며
// 이터레이터 리절트 객체의 value값을 변수에 할당
// done 프로퍼티 값이 true이면 이터러블의 순회를 중단
for(const item of [1, 2, 3]) {
  // item 변수에 순차적으로 1, 2, 3이 할당된다.
  console.log(item); // 1 2 3
}
```

- for...in문의 형식과 매우 유사하지만 프로퍼티(키) 를 순회하며 열거한다.
    
    > `for(변수선언문 in 객체) { ... }`
    > 

## 이터러블과 유사 배열 객체

- 유사 배열 객체는 인덱스로 프로퍼티값에 접근할 수 있고 length 프로퍼티를 가지나 이터러블이 아닌 일반 객체이다.
    - Symbol.iterator 메서드가 없기 때문에 for...of 문으로 순회할 수 없다.

```jsx
// 유사 배열 객체
const arrayLike = {
  0: 1,
  1: 2,
  2: 3
}
// 유사 배열 객체는 순회할 수 없다.
for (const item of arrayLike) {
  console.log(item); // 1 2 3
} // TypeError
```

- 단 arguments, NodeList, HTMLCollection은 유사 배열 객체이면서 이터러블 이다. 배열도 마찬가지로 이터러블이 도입되면서 Symbol.iterator 메서드를 구현하여 이터러블이 되었다.
- 그러나 **모든 유사 배열 객체가 이터러블인 것은 아니다.**
- Array.from 메서드를 통해 유사 배열 객체 또는 이터러블을 인수로 전달받아 배열로 변환하여 반환할 수 있다.

```jsx
const arrayLike = {
  0: 1,
  1: 2,
  2: 3
};

const arr = Array.from(arrayLike);
console.log(arr); // [1, 2, 3]
```

## 이터레이션 프로토콜의 필요성

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/8ec684cc-840f-4d21-ae5a-d8ef09a903b5/Untitled.png)

이터레이션 프로토콜은 다양한 데이터 공급자(Array, String, Map/Set, DOM 컬렉션)가 **하나의 순회 방식**을 갖도록 규정하여 데이터 소비자(for...of, 스프레드 문법, 배열 디스트럭처링 할당, Map/Set 생성자)가 효율적으로 다양한 데이터 공급자를 사용할 수 있도록 데이터 소비자와 데이터 공급자를 연결하는 인터페이스의 역할을 한다.

## 사용자 정의 이터러블

### 사용자 정의 이터러블 구현

- 사용자 정의 이터러블은 이터레이션 프로토콜을 준수하도록 Symbol.iterator 메서드를 구현하고 Symbol.iterator 메서드가 next 메서드를 갖는 이터레이터를 반환하도록 한다.
- next 메서드는 done과 value 프로퍼티를 가지는 이터레이터 리절트 객체를 반환한다. for...of문은 done프로퍼티가 true가 될 때까지 반복하다가 true가 되면 반복을 중지한다.

### 이터러블을 생성하는 함수

```jsx
// 사용자 정의 이터러블을 반환하는 함수
// 수열의 최대값을 인수로 전달받음
const fibonacciFunc = function (max) {
  let [pre, cur] = [0, 1]; // 배열 디스트럭처링 할당

  // Symbol.iterator 메서드를 구현한 이터러블을 반환
  return {
    [Symbol.iterator]() {
      return {
        next() {
          [pre, cur] = [cur, pre + cur];
          return { value: cur, done: cur >= max }; // 리절트 객체 반환
        }
      };
    }
  };
};

for (const num of fibonacciFunc(10)) {
  console.log(num) // 1 2 3 5 8
}
```

### 이터러블이면서 이터레이터인 객체를 생성하는 함수

- 이터러블이면서 이터레이션인 객체를 생성하면 Symbol.iterator 메서드를 호출하지 않아도 된다.

```jsx
// 이터러블이면서 이터레이터인 객체를 반환하는 함수
const fibonacciFunc = function (max) {
  let [pre, cur] = [0, 1]; // 배열 디스트럭처링 할당

  // Symbol.iterator 메서드와 next 메서드를 소유한
  **return {
    [Symbol.iterator]() { return this; },
    next() {
      [pre, cur] = [cur, pre + cur];
      return { value: cur, done: cur >= max }; // 리절트 객체 반환
    }
  };**
};

let iter = fibonacciFunc(10) // 이터러블이면서 이터레이터

// 이터러블이므로 for of 문 순회가능
for (const num of fibonacciFunc(10)) {
  console.log(num) // 1 2 3 5 8
}

// 이터레이터이므로 이터레이션 리절트 객체를 반환하는 next 메서드를 소유함
console.log(iter.next()); // { value: 1, done: false }
console.log(iter.next()); // { value: 2, done: false }
...
console.log(iter.next()); // { value: 13, done: true }
```

### 무한 이터러블과 지연 평가

- 배열, 문자열은 미리 모든 데이터를 미리 메모리에 확보하고 데이터를 공급한다.
- 그러나 이터러블은 **지연평가** 를 통해 데이터를 생성한다.
    - 데이터가 필요한 시점 이전까지는 데이터를 생성하지 않다가 필요한 시점이 되면 그때야 비로소 데이터를 생성한다.
    - 평가 결과가 필요할 때까지 평가르르 늦추는 기법
    - 지연평가를 활용하면 불필요한 데이터를 미리 생성하지 않고 필요한 데이터를 필요한 순간에 생성하므로 빠른 실행 속도를 기대할 수 있고 불필요한 메모리를 소비하지 않아 무한도 표현할 수 있다.
