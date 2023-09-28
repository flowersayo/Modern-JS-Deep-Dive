## Set

: 중복되지 않는 유일한 값들의 집합이다. Set 객체는 배열과 유사하지만 차이점도 존재한다.

| 구분 | 배열 | set객체 |
| --- | --- | --- |
| 동일한 값을 중복하여 포함할 수 있다. | O | X |
| 요소 순서에 의미가 있다. | O | X |
| 인덱스로 요소에 접근할 수 있다. | O | X |

이러한 Set 객체의 특성은 **수학적 집합**의 특성과 일치한다.

### Set 객체의 생성

- Set 객체는 Set 생성자 함수로 생성한다. Set 생성자 함수에 인수를 전달하지 않으면 빈 Set 객체가 생성된다.
- Set 생성자 함수는 이터러블을 인수로 전달받아 Set 객체를 생성한다. 이때 이터러블의 **중복된 값은 Set 객체에 요소로 저장되지 않는다**.
    
    ```jsx
    const set = new Set();
    console.log(set); // Set(0) {}
    
    const set1 = new Set('hello');
    console.log(set1); // Set(4) {"h", "e", "l", "o"}
    ```
    
- 중복을 허용하지 않는 Set 객체의 특성을 활용하여 **배열에서 중복된 요소를 제거**할 수 있다.
    
    ```jsx
    const uniq = array => [...new Set(array)];
    console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [2, 1, 3, 4]
    ```
    

### 요소 개수 확인

- Set 객체의 요소 개수를 확인할 때는 Set.prototype.size 프로퍼티를 활용한다.
- `size` 프로퍼티는 setter 함수 없이 getter 함수만 존재하는 접근자 프로퍼티다. 따라서 size 프로퍼티에 숫자를 할당하여 Set 객체의 요소 개수를 변경할 수 없다.

```jsx
const set = new Set([1, 2, 3]);

console.log(set.size); // 3

set.size = 10; // 무시된다
console.log(set.size); // 3
```

### 요소 추가

- Set 객체에 요소를 추가할 때에는 `add` 메서드를 사용한다.

```jsx
const set = new Set();

set.add(1);
console.log(set); // Set(1) {1}
```

- add 메서드는 새로운 요소가 추가된 Set 객체를 반환한다. 따라서 add 메서드를 호출한 후에 add 메서드를 연속적으로 호출할 수 있다.

```jsx
const set = new Set();

set.add(1).add(2);
console.log(set); // Set(2) {1, 2}
```

### 요소 존재 여부 확인

- Set 객체에 특정 요소가 존재하는지 확인하려면 **Set.prototype.has** 메서드를 사용한다. has 메서드는 **특정 요소의 존재 여부를 나타내는 불리언 값을 반환**한다.

```jsx
const set = new Set([1, 2, 3]);

console.log(set.has(2)); // true
```

### 요소 삭제

- Set 객체의 특정 요소를 삭제하려면 **Set.prototype.delete** 메서드를 사용한다. delete 메서드는 삭제 성공 여부를 나타내는 불리언 값을 반환한다.

```jsx
const set = new Set([1, 2, 3]);

set.delete(2);
console.log(set); // Set(2) {1, 3}
```

- 존재하지 않는 요소를 삭제하려고 하면 에러없이 무시된다.
- delete 메서드는 삭제 성공 여부를 나타내는 불리언 값을 반환한다. 따라서 Set.prototype.add 메서드와 달리 연속적으로 호출할 수 없다.

### 요소 일괄 삭제

- Set 객체의 모든 요소를 일괄 삭제하려면 **Set.prototype.clear** 메서드를 사용한다. clear 메서드는 언제나 undefined를 반환한다.

```jsx
const set = new Set([1, 2, 3]);

set.clear(); // 전체 비우기 
console.log(set); // set(0) {}
```

### 요소 순회

- Set 객체의 요소를순회하려면 **Set.prototype.forEach** 메서드를 사용한다. Set.prototype.forEach 메서드는 Array.prototype.forEach 메서드와 유사하게 콜백 함수와 forEach 메서의 콜백 함수 내부에서 this로 사용될 객체를 인수로 전달한다.

> **Set.prototype.forEach( ( (현재 순회 중인 요소 값, 현재 순회 중인 요소값 ,현재 순회 중인 Map 객체 자체)⇒{},this 로 사용될 객체 ))**
> 

! 첫 번째 인수와 두 번째 인수는 같은 값이다. 이처럼 동작하는 이유는 Set 객체는 순서에 의미가 없어 배열 같이 인덱스를 갖지 않기 때문이다.

```jsx
const set = new Set([1, 2, 3]);

set.forEach((v, v2, set) => console.log(v, v2, set));
/*
1 1 Set(3) { 1, 2, 3}
2 2 Set(3) { 1, 2, 3}
3 3 Set(3) { 1, 2, 3}
*/
```

- **Set 객체는 이터러블이다.** === for...of 문으로 순회할 수 있으며, 스프레드 문법과 구조분해 할당의 대상이 될 수도 있다.

```jsx
const set = new Set([1, 2, 3]);

for (const value of set) {
  console.log(value); // 1 2 3
}

console.log([...set]); // [1, 2, 3]
```

### 집합 연산

**교집합**

교집합 A ∩ B는 집합 A와 집합 B의 공통 요소로 구성된다.

```jsx
Set.prototype.intersection = function (set) {
  const result = new Set();

  for (const value of set) {
    // 2개의 set의 요소가 공통되는 요소이면 교집합의 대상이다.
    if (this.has(value)) result.add(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 교집합
console.log(setA.intersection(setB)); // Set(2) {2, 4}
// setB와 setA의 교집합
console.log(setB.intersection(setA)); // Set(2) {2, 4}
```

**합집합** 

합집합 A ∪ B는 집합 A와 집합 B의 중복 없는 모든 요소로 구성된다.

```jsx
Set.prototype.union = function (set) {
  // this(Set 객체)를 복사
  const result = new Set(this);

  for(const value of set) {
    // 합집합은 2개의 Set 객체의 모든 요소로 구성된 집합이다. 중복된 요소는 포함되지 않는다.
   result.add(value);
  };

  const setA = new Set([1, 2, 3, 4]);
  const setB = new Set([2, 4]);

  // setA와 setB의 합집합
  console.log(setA.union(setB)); // Set(4) {1, 2, 3, 4}
  // setB와 setA의 합집합
  console.log(setB.union(setA)); // Set(4) {2, 4, 1, 3}
```

**차집합** 

차집합 A-B는 집합 A에는 존재하지만 집합 B에는 존재하지 않는 요소로 구성된다.

```jsx
Set.prototype.difference = function (set) {
  // this(Set 객체)를 복사
  const result = new Set(this);

  for (const value of set) {
    // 차집합은 어느 한쪽 집합에는 존재하지만 다른 한쪽 집합에는 존재하지 않는 요소로 구성된 집합이다.
    result.delete(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA에 대한 setB의 차집합
console.log(setA.difference(setB)); // Set(2) {1, 3}
// setB에 대한 setA의 차집합
console.log(setB.difference(setA)); // Set(0) {}
```

**부분 집합과 상위 집합**

```jsx
// this가 subset의 상위 집합인지 확인한다.
Set.prototype.isSuperset = function (subset) {
  for (const value of subset) {
    // superset의 모든 요소가 subset의 모든 요소를 포함하는지 확인
    if (!this.has(value)) return false;
  }

  return true;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA가 setB의 상위 집합인지 확인한다.
console.log(setA.isSuperset(setB)); // true
// setB가 setA의 상위 집합인지 확인한다.
console.log(setB.isSuperset(setA)); // false
```

## Map

- `Map` 객체는 키와 값의 쌍으로 이루어진 컬렉션이다.
- **객체를 포함한 모든 값을 키로 사용할 수 있다.**
- 일반 객체는 이터러블이 아니지만 **Map 객체는 이터러블이다.** 그리고 length가 아닌 **size로 개수를 확인한다.**

### Map 객체의 생성

- Map 객체는 Map 생성자 함수로 생성한다. **Map 생성자 함수는 이터러블을 인수로 전달받아 Map 객체를 생성한다.**
- 이때 인수로 전달되는 이터러블은 **키와 값의 쌍으로 이루어진 요소**로 구성되어야 한다.

```jsx
const map1 = new Map([['key1', 'value1'], ['key2', 'value2']]);
console.log(map1); // Map(2) {"key1" => "value1", "key2" => "value2"}

const map2 = new Map([1, 2]); // TypeError
```

### 요소 개수 확인

- Map 객체의 요소 개수를 확인할 때는 **Map.prototype.size** 프로퍼티를 사용한다.

```jsx
const { size } = new Map([['key1', 'value1'], ['key2', 'value2']]);
console.log(size); // 2
```

### 요소 추가

- Map 객체에 요소를 추가할 때는 **Map.prototype.set** 메서드를 사용한다. 새로운 요소가 추가된 Map 객체를 반환한다.

```jsx
const map = new Map();
console.log(map); // Map(0) {}

map.set('key1', 'value1');
console.log(map); // Map(1) { "key1" => "value1" }
```

- 객체는 문자열 또는 심벌 값만 키로 사용할 수 있다. 하지만 **Map 객체는 키 타입에 제한이 없다**. 따라서 **객체를 포함한 모든 값을 키로 사용할 수 있다.**

```jsx
const map = new Map();

const lee = { name: 'Lee' };

map.set(lee, 'developer'); // Map(1) { {name: "Lee"} => "developer" }
```

### 요소 취득

- Map 객체에서 특정 요소를 취득하려면 **Map.prototype.get** 메서드를 사용한다.
- get 메서드의 **인수로 키를 전달**하면 Map 객체에서 인수로 전달한 키를 갖는 값을 반환한다.

```jsx
const map = new Map();

const lee = { name: 'Lee' };

map.set(lee, 'developer');

console.log(map.get(lee)); // developer
```

### 요소 존재 여부 확인

- Map 객체에서 특정 요소가 존재하는지 확인하려면 **Map.prototype.has** 메서드를 사용한다.

```jsx
const map = new Map();

const lee = { name: 'Lee' };

map.has(name); // true
```

### 요소 삭제

- Map 객체의 요소를 삭제하려면 **Map.prototype.delete** 메서드를 사용한다.

```jsx
const map = new Map();

const lee = { name: 'Lee' };

map.delete(lee); // 삭제 성공 여부를 나타내는 boolean 값 반환 
```

### 요소 일괄 삭제

- Map 객체의 요소를 일괄 삭제하려면 **Map.prototype.clear** 메서드를 사용한다.

```jsx
const lee = { name: 'Lee' };
const map = new Map([[lee, 'developer'], [kim, 'designer']]);

map.clear();
```

### 요소 순회

- Map 객체의 요소를 순회하려면 **Map.prototype.forEach** 메서드를 사용한다.
- Map.prototype.forEach 메서드는 Array.prototype.forEach 메서드와 유사하게 콜백 함수와 forEach 메서의 콜백 함수 내부에서 this로 사용될 객체를 인수로 전달한다.
    
    > **Map.prototype.forEach ( (**현재 순회 중인 요소 값, 현재 순회 중인 요소키 ,현재 순회 중인 Map 객체 자체)⇒{},this 로 사용될 객체 )
    > 

```jsx
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

map.forEach((v, k, map) => console.log(v, k, map));

/*
developer {name: "Lee"} Map(2) {
	{name: "Lee"} => "developer",
    {name: "Kim"} => "designer"
}

developer {name: "Kim"} Map(2) {
	{name: "Lee"} => "developer",
    {name: "Kim"} => "designer"
}
*/
```

- **Map 객체는 이터러블이다.** 따라서 for...of문으로 순회할 수 있으며, 스프레드 문법과 배열 구조 할당의 대상이 될 수도 있다.

```jsx
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

for(const entry of map) {
  console.log(entry); // [{name: "Lee"}, "developer"] [{name: "Kim"}, "designer"]
}

console.log([...map]); // [[{name: "Lee"}, "developer"], [{name: "Kim"}, "designer"]]

const [a, b] = map;
console.log(a, b); // [{name: "Lee"}, "developer"] [{name: "Kim"}, "designer"]
```

- Map 객체는 **이터러블이면서 동시에 이터레이터인 객체를 반환**하는 메서드를 제공한다.
    - **Map.prototype.keys** : Map 객체에서 요소키를 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환한다.
    - **Map.prototype.values** : Map 객체에서 요소값을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환한다.
    - **Map.prototype.entries** : Map 객체에서 요소키와 요소값을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환한다.

```jsx
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

for(const key of map.keys()) {
  console.log(key); // {name: "Lee"} {name: "Kim"}
}

for(const valule of map.keys()) {
  console.log(value); // 'developer' 'designer'
}

for(const entry of map.entries()) {
  console.log(entry); //  [{name: "Lee"}, "developer"] [{name: "Kim"}, "designer"]
}
```
