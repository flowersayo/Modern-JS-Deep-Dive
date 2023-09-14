## 22.1  this 키워드

- 동작을 나타내는 메서드는 자신이 속한 객체의 상태, 즉 프로퍼티를 참조하고 변경할 수 있어야한다.
- 자신이 속한 객체의 프로퍼티를 참조하려면 먼저 **자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야한다.**
- this 는 **자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수**이다. this 를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.
- 단 this 가 가리키는 값, 즉  this 바인딩은 함수 호출 방식에 의해 동적으로 결정된다.

<aside>
✍️ **this 바인딩**

바인딩이란 식별자와 값을 연결하는 과정을 의미한다. 예를 들어, 변수 선언은 변수 이름(식별자)와 확보된 메모리 공간의 주소를 바인딩 하는것. this 바인딩이란 this와 this가 가리킬 객체를 바인딩 하는 것이다.

</aside>

```jsx
const circle = {
  radius: 5,
  getDiameter() {
    return 2 * this.radius; // this 는 메서드를 호출한 객체, 즉 circle을 가리킨다.
  }
};

console.log(circle.getDiameter()); // 10
```

```jsx
function Circle(radius){
  this.radius = radius;
}

Circle.prototype.getDiameter = function() {
  return 2 * this.radius; // this는 생성자 함수가 생성할 인스턴스를 가리킨다. 
};

const circle = new Circle(5);
console.log(circle.getDiameter()); // 10
```

```jsx
// 전역에서 this는 전역 객체를 가리킨다.
console.log(this); // window

function square(number){
  // 일반 함수 내에서도 this는 전역 객체를 가리킨다.
  console.log(this); // window
// strict mode 가 적용된 일반 함수 내부의 this에는 undefined가 바인딩된다.
  return number * number;
}
square(2);

const person = {
  name : 'Lee',
  getName() {
    // 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
    console.log(this); // {name : "Lee', getName : f}
    return this.name;
  }
};
console.log(person.getName()); // Lee

function Person(name) {
  this.name = name;
  console.log(this); // Person {name: "Lee"};
}

const me = new Person("Lee");
```

## 22.2 함수 호출 방식과 this 바인딩

 

- **자바스크립트의 this 는 함수가 호출되는 방식에 따라  this에 바인딩 될 값, 즉  this 바인딩이 동적으로 결정된다.**

<aside>
✍️ **렉시컬 스코프와 this 바인딩은 결정 시기가 다르다.**

함수의 상위스코프를 결정하는 방식인 **렉시컬 스코프는** 함수 정의가 평가되어 **함수 객체가 생성되는 시점에 상위 스코프를 결정**한다. 하지만 **this 바인딩은 함수 호출시점에 결정**된다.

=> 요약하면, 렉시컬 스코프는 함수의 정의 위치에 따라 스코프가 결정되며 코드 작성 시점에 정적으로 확정됩니다. 반면에 this 바인딩은 함수의 호출 방식에 따라 동적으로 결정되며 함수가 호출될 때마다 this가 달라질 수 있습니다.

</aside>

동일한 함수도 다양한 방식으로 호출할 수 있다.

> 1. 일반 함수 호출
2. 메서드 호출
3. 생성자 함수 호출
4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출
> 

```jsx
nst foo = function () {
  console.dir(this);
};

// 1. 일반 함수 호출
// 일반 함수 내부의 this는 전역 객체를 가리킨다.
foo(); // window

// 2. 메서드 호출
// 메서드 호출한 객체를 가리킨다.
const obj = { foo };
obj.foo(); // obj

// 3. 생성자 함수 호출
// 생성자 함수가 생성한 인스턴스를 가리킨다.
new foo(); // foo {}

// 4. Function.prototype.apply/call/bind 메서드에 의한 호출
// foo 함수 내부의 this는 인수에 의해 결정된다.
const bar = { name: 'bar' };

foo.call(bar); // bar
foo.apply(bar); // bar
foo.bind(bar)(); // bar
```

### 22.2.1 일반 함수 호출

- 기본적으로  전역객체가 바인딩된다.
- strict mode 가 적용되면 undefined가 바인딩된다.
- 메서드 내에서 정의된 중첩 함수도 일반 함수로 호출되면 전역 객체가 바인딩된다.
    - 중첩함수나 콜백 함수의 this 바인딩을 일치시키려면 다음과 같이 해준다.
        1. this 바인딩(obj)을 변수 that에 할당
        
        ```jsx
        var value = 1;
        
        const obj = {
          value: 100,
          foo() {
            // this 바인딩(obj)을 변수 that에 할당한다.
            const that = this;
        
            // 콜백 함수 내부에서 this 대신 that을 참조한다.
            setTimeout(function () {
        		console.log(that.value); // 100
              	console.log(this.value); // 1, 일반함수 내부의 this는 window이다.
            }, 100);
          }
        }
        
        obj.foo();
        ```
        
        1. 명시적으로 this 를 바인딩 
        
        ```jsx
        var value = 1;
        
        const obj = {
          value: 100,
          foo() {
            // 명시적으로 this 를 바인딩 
            setTimeout(function () {
              	console.log(this.value); // 100. 여기서 말하는 this 가 obj 로 인식
            }.bind(this), 100);
          }
        }
        
        obj.foo();
        ```
        
        1. 화살표 함수 이용
        
        ```jsx
        var value = 1;
        
        const obj = {
          value: 100,
          foo() {
        
            // 화살표 함수 내부의 this는 상위 스코프의 this를 가리킨다.
            setTimeout(() => {
              	console.log(this.value); // 100
            }, 100);
          }
        }
        
        obj.foo();
        ```
        
    - 위 방법 이외에도 this를 명시적으로 바인딩할 수 있는 Function.prototype.apply, Function.prototype.call, Function.prototype.bind
        
        메서드를 제공
        
- 콜백 함수가 일반함수로 호출된다면 콜백 함수 내부의 this 에도 전역 객체가 바인딩 된다.

**⇒ 어떠한 함수라도 일반 함수로 호출되면 this에 전역 객체가 바인딩된다.**

### 22.2.2 메서드 호출

- 메서드 내부의 this에는 메서드를 호출한 객체, 즉 마침표 연산자 앞에 기술한 객체가 바인딩 된다.
- 주의할 것은 **메서드를 소유한 객체가 아닌 메서드를 호출한 객체에 바인딩 된다는 것**이다.

```jsx
const person = {
  name: "Lee",
  getName() {
    // 메서드 내부의 this는 호출한 객체에 바인딩된다.
    return this.name;
  }
};

// 메서드 getName을 호출한 객체는 person이다.
console.log(person.getName()); // Lee
```

- 메서드는 프로퍼티에 바인딩된 함수이다.
- 즉, person 객체의 getName 프로퍼티가 가리키는 **함수 객체는** person 객체에 포함된 것이 아니라 **독립적으로 존재하는 별도의 객체**이다.  getName 프로퍼티가 함수 객체를 가리키고 있을 뿐이다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/478c71c6-12ba-4b6d-9cf5-1bc41a66818e/Untitled.png)

- 따라서  getName 메서드는 다른 객체의 프로퍼티에 할당하는 것으로 다른 객체의 메서드가 될 수도 있고, 일반 변수에 할당하여 일반 함수로 호출할 수도 있다.

```jsx
const anotherPerson = {
  name: 'Kim'
};

// getName 메서드를 anotherPerson 객체의 메서드로 할당
antoehrPerson.getName = person.getName;

// getName 메서드를 호출한 객체는 anotherPerson이다.
console.log(anotherPerson.getName()); // Kim

// getName 메서드를 변수에 할당
const getName = person.getName;

// getName 메서드를 일반 함수로 호출
console.log(getName()); // ''
// 일반 함수로 호출된 getName 함수 내부의 this.name은 브라우저 환경에서 window.name과 같다.
// 브라우저 환경에서 window.name은 브라우저 창의 이름을 나타내는 빌트인 프로퍼티이며 기본 값은 "" 이다.
// Node.js 환경에서 this.name은 undefined 이다.
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/32aa3a08-828c-40bc-a2c2-9fb550766667/Untitled.png)

- 프로토타입 메서드 내부에서 사용된 this도 일반 메서드와 마찬가지로 **해당 메서드를 호출한 객체에 바인딩**된다.

```jsx
function Person(name){
  this.name = name;
}

Person.prototype.getname = function (){
  return this.name;
};

const me = new Person('Lee');

// getName 메서드를 호출한 객체는 me 이다.
console.log(me.getName()); // Lee

Person.prototype.name = "kim";

// getName 메서드를 호출한 객체는 Person.prototype이다.
console.log(Person.prototype.getName()); // Kim
```

**⇒ this 에 바인딩될 객체는 호출시점에 결정된다!**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/f0697e7d-0d11-4164-a200-c25465d0e5fb/Untitled.png)

### 22.2.3 생성자 함수 호출

: **생성자 함수가 미래에 생성할 인스턴스**가 바인딩된다.

```jsx
function Circle(radius){
  this.radius = radius; //this 는 생성자 함수가 미래에 생성할 인스턴스를 가리킨다.
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

const circle1 = new Circle(5);
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

### 22.2.4 Function.prototype.apply/call/bind 메서드에 의한 간접 호출

- apply, call, bind 메서드는 Function.prototype의 메서드이다. 이들 메서드는 모든 함수가 상속받아 사용할 수 있다.
- this로 사용할 객체와 인수 리스트를 인수로 전달 받아 함수를 호출한다.

> 📌 **apply, call 사용법** 
: 본질은 함수를 호출하는 것! 호출할 함수에 인자를 전달하는 방식만 다르다.
> 
> 
> ```jsx
> /**
> * 주어진 this 바인딩과 인수 리스트 배열을 사용하여 함수를 호출한다.
> * @params thisArg - this로 사용할 객체
> * @parmas argsArray - 함수에게 전달할 인수 리스트의 배열 또는 유사 배열 객체
> * @returns 호출된 함수의 반환값
> */
> Function.prototype.apply(thisArg[, argsArray])
> 
> /**
> * 주어진 this 바인딩과 ,로 구분된 인수 리스트를 사용하여 함수를 호출한다.
> * @params * thisArg - this로 사용할 객체
> * @parmas arg1, arg2, ... - 함수에게 전달할 인수 리스트
> * @returns 호출된 함수의 반환값
> */
> Function.prototype.call(thisArg[, arg1[, arg2[, ...]]]);
> ```
> 

```jsx
function getThisBinding(){
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

console.log(getThisBinding()); // window

// getThisBinding 함수를 호출하면서 인수로 전달한 객체 getThisBinding this에 바인딩한다.
console.log(getThisBinding.apply(thisArg)); // { a: 1 }
console.log(getThisBinding.call(thisArg)); // { a: 1 }
```

- arguments 객체와 같은 유사 배열 객체에 배열 메서드를 사용하는 경우에 대표적으로 이용한다.
- arguments 객체는 배열이 아니기 때문에 Array.prototype.slice 같은 배열의 메서드를 사용할 수 없었으나 apply, call 메서드를 이용하면 가능하다.

```jsx
function convertArgstoArray() {
  console.log(arguments);

  // arguments 객체를 배열로 전환
  // **Array.prototype.slice를 인수 없이 호출하면 배열의 복사본을 생성**한다.
  const arr = Array.prototype.slice.call(argurments);
  // const arr = Array.prototype.slice.apply(arguments);
  console.log(arr);

  return arr;
}

convertArgstoArray(1,2,3); // [1,2,3]
```

**Function.prototype.bind**

- 함수를 호출하지 않고 첫번째 인수로 전달한 값으로 this 바인딩이 교체된 함수를 새롭게 생성해 반환한다.

```jsx
function getThisBinding(){
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

// bind 메서드는 첫 번째 인수로 전달한 thisArg로 this 바인딩이 교체된
// getThisBinding 함수를 새롭게 생성해 반환한다.
console.log(getThisBinding.bind(thisArg)); // getThisBinding

// bind 메서드는 함수를 호출하지는 않으므로 명시적으로 호출해야 한다.
console.log(getThisBinding.bind(thisArg)()); // {a: 1}
```

- **bind 메서드는 메서드의 this와 메서드 내부의 중첩 함수 또는 콜백함수 this가 불일치하는 문제를 해결하기 위해 유용하게 사용**된다.

```jsx
const person = {
  name: "Lee",
  foo(callback) {
    setTimeout(callback.bind(this), 100);
  }
};

person.foo(function() {
  console.log(`Hi! my name is ${this.name}.`); // Hi! my name is Lee.
});
```

| 함수 호출 방식 | this  바인딩 |
| --- | --- |
| 일반 함수 호출 | 전역 객체 |
| 메서드 호출 | 메서드를 호출한 객체 |
| 생성자 함수 호출 | 생성자 함수가 (미래에) 생성할 인스턴스 |
| Function.prototype.apply/call/bind 메서드에 의한 간접 호출  | Function.prototype.apply/call/bind 메서드에 첫번째 인수로 전달한 객체 |
