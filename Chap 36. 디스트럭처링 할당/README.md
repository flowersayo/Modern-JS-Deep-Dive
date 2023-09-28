구조분해 할당은 구조화된 배열과 같은 이터러블 또는 객체를 비구조화하여 1개 이상의 변수에 개별적으로 할당하는 것을 의미한다. 배열과 같은 이터러블 또는 객체 리터럴에서 **필요한 값만 추출하여 변수에 할당**할 때 유용하다.

## 배열 디스트럭처링 할당

- 구조분해 할당의 대상은 이터러블이어야 하며, 할당 기준은 배열의 인덱스다. 즉, 순서대로 할당된다.
- 구조분해 할당을 위해서는 할당 연산자 왼쪽에 값을 할당받을 변수를 선언해야 한다. 이때 변수를 배열 리터럴 형태로 선언한다.
    
    ```jsx
    const arr = [1, 2, 3];
    
    const [one, two, three] = arr;
    
    console.log(one, two, three); // 1 2 3
    ```
    
- 변수의 개수와 이터러블의 요소 개수가 반드시 일치할 필요는 없다.
    
    ```jsx
    const [a, b] = [1];
    
    console.log(a, b); // 1 undefined
    ```
    
- 기본 값을 지정할 수 있다.
    
    ```jsx
    const [a, b ,c=3] = [1,2];
    
    console.log(a, b, c); // 1 2 3
    ```
    
- 구조분해 할당을 위한 변수에 Rest 파라미터와 유사하게 Rest 요소 (...)를 사용할 수 있다. Rest 요소는 Rest 파라미터와 마찬가지로 반드시 마지막에 위치해야 한다.
    
    ```jsx
    const [x, ...y] = [1, 2, 3];
    console.log(x, y); // 1 [2, 3]
    ```
    

## 객체 구조분해 할당

- ES6에서는 객체를 구조분해 하여 변수에 할당하기 위해서는 프로퍼티 키를 사용해야 한다. **할당 기준은 프로퍼티 키**이며 순서는 의미가 없다.
    
    ```jsx
    const user = { firstName: 'Ungmo', lastName: 'Lee' };
    
    const { lastName, firstName } = user;
    ```
    
- 기존 프로퍼티와 다른 변수 이름으로 할당받을 수도 있다.
    
    ```jsx
    const { lastName, firstName } = user;
    
    const {lastName: last, firstName: first} = user; // 위 코드와 같다
    ```
    
- 마찬가지로 기본값을 설정할 수도 있다.

⇒ **객체에서 프로퍼티 키로 필요한 프로퍼티 값만 추출하여 변수에 할당하고 싶을 때** 유용하다.

```jsx
const str = 'Hello';
// String 래퍼 객체로부터 length 프로퍼티만 추출한다.
const { length } = str;
console.log(length); // 5

const todo = { id: 1, content: 'HTML' };
const { id } = todo;
console.log(id); // 1
```

- 객체 구조분해 할당은 **객체를 인수로 전달받는 함수의 매개변수**에도 사용할 수 있다.

```jsx
function printTodo({ content, completed }) {
  console.log(`할일 ${content}은 ${completed ? '완료' : '비완료'} 상태입니다.`);
}

printTodo({ id: 1, content: 'HTML', completed: true });
```

- 배열의 요소가 객체인 경우 `배열 구조분해 할당` 과 `객체 구조분해 할당` 을 혼용할 수 있다.

```jsx
const todos = [
  {id: 1, content: 'HTML', completed: true },
  {id: 2, content: 'CSS', completed: false },
  {id: 3, content: 'JS', completed: false }
];

const [, { id }] = todos;
console.log(id); // 2
```

- 중첩 객체의 경우는 다음과 같이 사용한다.

```jsx
const user = {
  name: 'Lee',
  address: {
    zipCode: '03068',
    city: 'Seoul'
  }
};

const { address: {city} } = user;
// const city = user.address.city 와 같은 표현
```

- 객체 디스트럭처링 할당을 위한 변수에 Rest 파라미터나 Rest 요소와 유사하게 Rest 프로퍼티 … 을 사용할 수 있다.

```jsx
const {x,...rest} =  { x:1, y:2,z:3 } ;

console.log(rest); // { y : 2, z:3 } 
```
