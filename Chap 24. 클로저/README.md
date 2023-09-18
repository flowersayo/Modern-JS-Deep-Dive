> 클로저는 함수와 그 함수가 선언된 렉시컬 환경과의 조합이다.
> 

## 24.1 렉시컬 스코프

- 자바스크립트 엔진은 함수를 어디서 호출했는지가 아니라, 함수를 어디에 정의했는지에 따라 상위 스코프를 결정한다.
- 렉시컬 환경의 “외부 렉시컬 환경에 대한 참조”에 저장할 참조값. 즉 상위 스코프에 대한 참조는 함수 정의가 평가되는 시점에 함수가 정의된 환경(위치)에 의해 결정된다. 이것이 바로 렉시컬 스코프이다.

## 24.2 함수 객체의 내부 슬롯

- **함수는 자신의 내부 슬롯에 자신이 정의된 환경, 즉 상위 스코프의 참조를 저장**한다.
    - 함수 정의가 평가되어 함수 객체를 생성할 때 자신이 정의된 환경(위치)에 의해 결정된 상위 스코프의 참조를 함수 객체 자신의 내부 슬롯 [[Environment]]에 저장한다.
    - 자신의 내부 슬롯 [[Environment]] 에 저장된 상위 스코프의 참조는 현재 실행 중인 실행 컨텍스트의 렉시컬 환경을 가리킨다.
- **함수 객체의 내부 슬롯 [[Environment]] 에 저장된 현재 실행 중인 실행 컨텍스트의 렉시컬 환경의 참조가 바로 상위 스코프이다.**
- 또한 자신이 호출되었을 때 생성될 함수 렉시컬 환경의 “외부 렉시컬 환경에 대한 참조”에 저장될 참조값이다.
- 함수 객체는 내부 슬롯 [[Environment]] 에 저장한 렉시컬 환경의 참조, 즉 상위스코프를 자신이 존재하는 한 기억한다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/1e13074e-0e9a-4f4c-9798-d91cd8557dff/Untitled.png)
    
- 함수 렉시컬 환경의 구성 요소인 외부 렉시컬 환경에 대한 참조에는 **함수 객체의 내부 슬롯 [[Environment]]에 저장된 렉시컬 환경의 참조가 할당**된다.

## 24.3 클로저와 렉시컬 환경

```jsx
const x = 1;

function outer(){
  const x = 10;
  const inner = function() {console.log(x)};
  return inner;
}

// 분명 outer 함수의 실행 컨텍스트는 pop되어 제거되었을텐데 outer함수의 지역변수 x가 부활이라도 한듯 동작한다.
const innerFunc = outer();
innerFunc(); // 10
```

⇒ 외부함수보다 중첩함수가 더 오래 유지되는 경우, 중첩 함수는 이미 생명주기가 종료한 외부 함수의 변수를 참조할 수 있다.⇒ **클로저**

1. outer 함수가 평가되어 함수 객체를 생성할 때, 전역 렉시컬 환경을 [[Environment]] 내부 슬롯에 상위 스코프로서 저장한다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/0f1589b5-6715-4ed0-8efc-b487551ef03d/Untitled.png)
    
2. outer 함수를 호출하면 outer 함수의 렉시컬 환경이 생성되고, 외부 렉시컬 환경에 대한 참조에 outer 함수의 [[Environment]] 내부 슬롯에 저장되어있던 값을 할당한다.
3. 중첩함수 inner 가 평가되어 outer 함수의 렉시컬 환경을 [[Environment]] 내부 슬롯************************************************************************에 상위 스코프로서 저장한다.************************************************************************
4. outer 함수의 실행이 종료하면 inner 함수를 반환하면서 outer 함수의 생명 주기가 종료된다. **이 때, outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 제거되지만 outer 함수의 렉시컬 환경까지 소멸하는 것은 아니다.** 
    
    → outer 함수의 렉시컬 환경은 inner 함수의 [[Environment]] 슬롯에 의해 참조되고 있고, inner 함수는 전역 변수  innerFunc에 의해 참조되고 있으므로 가비지 컬렉션의 대상이 되지 않기 때문이다. 가비지 컬렉터는 누군가가 참조하고 있는 메모리 공간을 함부로 해제하지 않는다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/a619ae90-423d-4ea3-830d-edc945ff5cbc/Untitled.png)
    
5. outer 함수가 반환한 inner 함수를 호출하면 inner 함수의 실행 컨텍스트가 생성되고 렉시컬 환경의 외부 렉시컬 환경에 대한 참조에는 inner 함수 객체의 [[Environment]] 내부 슬롯에 저장되어 있는 outer 함수의 렉시컬 환경이 할당된다.
    
    ![외부 함수가 소멸해도 반환된 중첩 함수는 외부 함수의 변수를 참조할 수 있다.](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/f26621b5-962c-49d9-8cab-b8ed44afbad5/Untitled.png)
    
    외부 함수가 소멸해도 반환된 중첩 함수는 외부 함수의 변수를 참조할 수 있다.
    

⇒ 자바스크립트의 모든 함수는 상위 스코프를 기억하므로 이론적으로 모든 함수는 클로저다. 하지만 상위 스코프의 어떤 식별자도 참조하지 않는 함수는 클로저가 아니다. 상위 스코프의 어떤 식별자도 참조하지 않는 경우 대부분의 모던 브라우저는 최적화를 통해 상위 스코프를 기억하지 않는다. 

```jsx
<!DOCTYPE html>
<html>
<body>
  <script>
    function foo() {
      const x = 1;

      // 일반적으로 클로저라고 하지 않는다.
      // bar 함수는 클로저였지만 곧바로 소멸한다.
      function bar() {
        debugger;
        // 상위 스코프의 식별자를 참조한다.
        console.log(x);
      }
      bar();
    }

    foo();
  </script>
</body>
</html>
```

- 중첩 함수 bar 가 상위 스코프의 식별자를 참조하고 있으므로 클로저다.
- 하지만 외부함수 foo의 외부로 bar 가 반환되지 않는다. 즉, foo보다 중첩함수 bar 의 생명주기가 더 짧다.
- 이런 경우, 생명주기가 종료된 외부 함수의 식별자를 참조할 수 있다는 클로저의 본질에 부합하지 않으므로 일반적으로 클로저라고 하지 않는다.

```jsx
<!DOCTYPE html>
<html>
<body>
  <script>
    function foo() {
      const x = 1;
      const y = 2;

      // 클로저
      // 중첩 함수 bar는 외부 함수보다 더 오래 유지되며 상위 스코프의 식별자를 참조한다.
      function bar() {
        debugger;
        console.log(x);
      }
      return bar;
    }

    const bar = foo();
    bar();
  </script>
</body>
</html>
```

- 위 예제의 중첩 함수 bar 는 상위 스코프 식별자를 참조하고 있으며 중첩 함수가 외부 함수보다 더 오래 살아남으므로 클로저이다.
- 다만 모던 브라우저는 최적화를 통해 상위 스코프의 식별자 중에서 클로저가 참조하고 있는 식별자만을 기억한다.
- 클로저에 의해 참조되는 상위 스코프의 변수를 자유변수라고 부른다.
- 클로저 = “ 함수가 자유 변수에 대해 닫혀있다.” ⇒ 자유 변수에 묶여있는 함수

## 24.4 클로저의 활용

- **상태를 안전하게 변경하고 유지**하기 위해 사용
- 상태가 의도치 않게 변경되지 않도록 **상태를 안전하게 은닉**하고 특정 함수에게만 상태 변경을 허용하기 위해 사용한다.

```jsx
let num = 0;

const increase = function() {
  return ++num;
};

console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3
```

- 위 코드는 잘 동작하지만 오류 발생 가능성이 있는 코드이다.
- 위 예제가 바르게 동작하려면 다음의 전제조건을 지켜야한다.
    - 카운트 상태(num변수의 값)는 increase 함수가 호출되기 전가지 변경되지않고 유지되어야한다.
    - **이를 위해 카운트 상태는 increase 함수만이 변경할수 있어야한다.**

⇒ 따라서 카운트 상태를 안전하게 변경하고 유지하기 위해서는 increase함수 만이 num 변수를 참조하고 변경할 수 있도록 해야 한다.

```jsx
const increase = (function () {
  let num = 0;

  //클로저
  return function() {
    return ++num;
  };
}());

console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3
```

- increase 변수에 할당된 함수는 자신이 정의된 위치에 의해 결정된 상위 스코프인 즉시 실행 함수의 렉시컬 환경을 기억하는 클로저다.
- 즉시 실행 함수는 호출된 이후 소멸되지만 즉시 실행 함수가 반환한 클로저는  increase 변수에 할당되어 호출된다.
- 이때 즉시 실행 함수가 반환한 클로저는 자신이 정의된 위치에 의해 결정된 상위 스코프인 즉시 실행 함수의 렉시컬 환경을 기억하고 있다.
- 따라서 카운트 상태를 유지하기 위한 자유 변수 num을 언제 어디서 호출하든지 참조하고 변경할 수 있다.
- 즉시 실행 함수는 한번만 실행되므로 increase가 호출될 때마다 num변수가 재차 초기화될 일은 없다.
- 또한, num 변수는 외부에서 직접 접근할 수 없는 은닉된 private 변수이므로 전역 변수를 사용했을 때와 같이 의도되지 않은 변경을 걱정할 필요도 없기 때문에 더 안정적인 프로그래밍이 가능하다.

⇒ **이처럼 클로저는 상태가 의도치 않게 변경되지 않도록 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용하여 상태를 안전하게 변경하고 유지하기 위해 사용한다.**

```jsx
const counter = (function () {
  let num = 0; // -> 이 친구는 public?

  //클로저
  return {
    increase() { return ++num },
    decrease() { return num > 0 ? --num : 0 },
  }
}());

console.log(counter.increase()); // 1
console.log(counter.increase()); // 2

console.log(counter.decrease()); // 1
console.log(counter.decrease()); // 0
```

- 증가 + 발전 모두 가능하도록 발전한 형태이다.

```jsx
const Counter = (function () {
  let num = 0;

  function Counter() {
    // this.num = 0; // num을 전역 변수로 한 이유 : 프로퍼티는 public하므로 은닉되지 않는다.
  }

  Counter.prototype.increase = function () {
    return ++num;
  };

  Counter.prototype.decrease = function () {
    return num > 0 ? --num : 0 ;
  };

  return Counter;
}());

const counter = new Counter(); // 여기서 new 연산자를 호출??

console.log(counter.increase()); // 1
console.log(counter.increase()); // 2

console.log(counter.decrease()); // 1
console.log(counter.decrease()); // 0
```

- 위 예제를 생성자 함수로 표현하면 위와 같다.
- 즉시 실행 함수 내에서 선언된 num 변수는 인스턴스를 통해 접근할 수 없으며, 즉시 실행 함수 외부에서도 접근할 수 없는 은닉된 변수다.
- 생성자 함수 Counter는 프로토타입을 통해  increase, decrease 메서드를 상속받는 인스턴스를 생성한다.
- increase, decrease 함수는 모두 자신의 함수 정의가 평가되어 함수 객체가 될 때 즉시 실행 함수 실행 컨텍스트의 렉시컬 환경을 기억하는 클로저이다.
- 따라서 num 변수의 값은 increase, decrease 메서드만이 변경할 수 있다.
- 외부 상태 변경이나 가변 데이터를 피하고 불변성을 지향하는 함수형 프로그래밍에서 부수 효과를 최대한 억제하여 오류를 피하고 프로그램의 안정성을 높이기 위해 클로저는 적극적으로 사용된다.

```jsx
// 함수를 인수로 전달받고 함수를 반환하는 고차 함수
// 이 함수는 카운트 상태를 유지하기 위한 자유 변수 counter를 기억하는 클로저를 반환한다.
function makeCounter(predicate) {
  // 카운트 상태를 유지하기 위한 자유 변수
  let counter = 0;

  // 클로저를 반환
  return function () {
    // 인수로 전달 받은 보조 함수에 상태 변경을 위임한다.
    counter = predicate(counter);
    return counter;
  };
}

// 보조 함수
function increase(n) {
  return ++n;
}

// 보조 함수
function decrease(n) {
  return --n;
}

// 함수로 함수를 생성한다.
// makeCounter 함수는 보조 함수를 인수로 전달받아 함수를 반환한다
const increaser = makeCounter(increase); // ①
console.log(increaser()); // 1
console.log(increaser()); // 2

// increaser 함수와는 별개의 독립된 렉시컬 환경을 갖기 때문에 카운터 상태가 연동하지 않는다.
const decreaser = makeCounter(decrease); // ②
console.log(decreaser()); // -1
console.log(decreaser()); // -2
```

- 보조함수를 인자로 전달받고 함수를 반환하는 고차함수이다.
- makeCounter 가 반환하는 함수는 자신이 생성됐을 때의 렉시컬 환경인 makeCounter 함수의 스코프에 속한 counter 변수를 기억하는 클로저다.
- 인자로 전달받은 보조함수를 합성하여 자신이 반환하는 함수의 동작을 변경할 수 있다.
- 이때 주의할 것은 **makeCounter 함수를 호출해 함수를 반환할 때 반환된 함수는 자신만의 독립된 렉시컬 환경을 갖는다**는 것이다.
    - 함수를 **호출할때마다 새로운 makeCounter 함수 실행 컨텍스트의 렉시컬 환경이 생성**되기 때문이다.
- 과정
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/85318bb0-5766-4a8c-8047-65513c2e84d5/Untitled.png)
    
    - 1에서 makeCounter 함수를 호출하면 makeCounter 함수의 실행 컨텍스트가 생성된다.
    - 그리고 makeCounter 함수는 함수 객체를 생성하여 반환한 후 소멸된다.
    - makeCounter 함수가 반환한 함수는 makeCounter 함수의 렉시컬 환경을 상위 스코프로서 기억하는 클로저이며, 전역 변수인 increaser 에 할당된다. 이 때 makeCounter 함수의 실행 컨텍스트는 소멸되지만 makeCounter 함수 실행 컨텍스트의 렉시컬 환경은 makeCounter 함수가 반환한 함수의 [[Environment]]  슬롯에 의해 참조되고 있기 때문에 소멸되지 않는다.
    - 2에서 makeCounter 함수를 호출하면 새로운 makeCounter 함수의 실행 컨텍스트가 생성된다.
    - 같은 과정을 반복한다.
- 따라서 increaser 와 decreaser 에 할당된 함수는 각각 자신만의 독립된 렉시컬 환경을 갖기 때문에 카운트를 유지하기 위한 자유 변수 counter를 공유하지 않아 카운터의 증감이 연동되지 않는다.
- 따라서 독립된 카운터가 아니라 연동하여 증감이 가능한 카운터를 만들고 싶으면 렉시컬 환경을 공유하는 클로저를 만들어야한다.
- 이를 위해서는 makeCounter를 두번 호출하지 말아야 한다.
    
    ```jsx
    // 함수를 반환하는 고차 함수
    // 이 함수는 카운트 상태를 유지하기 위한 자유 변수 counter를 기억하는 클로저를 반환한다.
    const counter = (function () {
      // 카운트 상태를 유지하기 위한 자유 변수
      let counter = 0;
    
      // 함수를 인수로 전달받는 클로저를 반환
      return function (aux) {
        // 인수로 전달 받은 보조 함수에 상태 변경을 위임한다.
        counter = aux(counter);
        return counter;
      };
    }());
    
    // 보조 함수
    function increase(n) {
      return ++n;
    }
    
    // 보조 함수
    function decrease(n) {
      return --n;
    }
    
    // 보조 함수를 전달하여 호출
    console.log(counter(increase)); // 1
    console.log(counter(increase)); // 2
    
    // 자유 변수를 공유한다.
    console.log(counter(decrease)); // 1
    console.log(counter(decrease)); // 0
    ```
    

## 24.5 캡슐화와 정보 은닉

- 캡슐화는 **객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있는 동작인 메서드를 하나로 묶는 것**을 말한다.
- 캡슐화는 **객체의 특정 프로퍼티나 메서드를 감출 목적으로 사용**하기도 하는데 이를 **정보 은닉**이라고 한다.
    - 적절히 못한 접근으로 객체의 상태가 변경되는 것을 방지해 정보를 보호
    - 객체 간의 상호 의존성 (결합도) 를 낮추는 효과

**1.인스턴스 메서드에서 지역 변수 참조**

```jsx
function Person(name, age) {
  this.name = name; // public
  let _age = age;   // private

  // 인스턴스 메서드
  this.sayHi = function () {
    console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
  };
}

const me = new Person('Lee', 20);
me.sayHi(); // Hi! My name is Lee. I am 20.
console.log(me.name); // Lee
console.log(me._age); // undefined

const you = new Person('Kim', 30);
you.sayHi(); // Hi! My name is Kim. I am 30.
console.log(you.name); // Kim
console.log(you._age); // undefined
```

- 위 예제의 name 프로퍼티는 현재 외부로 공개되어 있어서 자유롭게 참조하거나 변경할 수 있다. (public)
- 하지만 _age 변수는 Person 생성자 함수의 지역 변수이므로 Person 생성자 함수 외부에서 참조하거나 변경할 수 없다. (Private)
- 그러나 위 예제의 sayHi 메서드는 인스턴스 메서드이므로 Person 객체가 생성될 때마다 중복 생성된다.

**2.프로토타입 메서드에서 지역 변수 참조**

```jsx
function Person(name, age) {
  this.name = name; // public
  let _age = age;   // private
}

// 프로토타입 메서드
Person.prototype.sayHi = function () {
  // Person 생성자 함수의 지역 변수 _age를 참조할 수 없다
  console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
};
```

- sayHi를 프로토타입 메서드로 변경하여 중복 생성을 방지했다.
- 이 때, 지역변수 _age 를 참조할 수 없는 문제가 발생한다.

**3.프로토타입 메서드를 클로저로 활용**

```jsx
const Person = (function () {
  let _age = 0; // private

  // 생성자 함수
  function Person(name, age) {
    this.name = name; // public
    _age = age;
  }

  // 프로토타입 메서드 -> new 연산자로 ************************************************************************************실행했을 때에는 새롭게 호출되지 않음.************************************************************************************
  Person.prototype.sayHi = function () {
    console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
  };

  // 생성자 함수를 반환
  return Person;
}());

const me = new Person('Lee', 20); 
me.sayHi(); // Hi! My name is Lee. I am 20.
console.log(me.name); // Lee
console.log(me._age); // undefined

const you = new Person('Kim', 30);
you.sayHi(); // Hi! My name is Kim. I am 30.
console.log(you.name); // Kim
console.log(you._age); // undefined
```

- 따라서 다음과 같이 즉시 실행 함수를 사용하여 Person 생성자 함수와 Person.prototype.sayHi 메서드를 하나의 함수 내에 모은다.
- 위 패턴을 사용하면 접근 제한자를 제공하지 않는 자바스크립트 환경에서도 정보 은닉이 가능한 것처럼 보인다.
- Person 생성자 함수와 sayHi 메서드는 이미 종료되어 소멸한 즉시 실행 함수의 지역 변수 _age를 참조할 수 있는 클로저이다.
- 하지만 이 예제의 경우도  Person 함수가 여러개의 인스턴스를 생성할 경우 _age의 상태를 유지하지 않는다. ( 여러개의 인스턴스가 하나의 age 를 공유함 )
    
    ```
    const me = new Person('Lee', 20);
    me.sayHi(); // Hi! My name is Lee. I am 20.
    
    const you = new Person('Kim', 30);
    you.sayHi(); // Hi! My name is Kim. I am 30.
    
    // _age 변수 값이 변경된다!
    me.sayHi(); // Hi! My name is Lee. I am 30.
    ```
    
    - Person.prototype.sayHi 메서드가 즉시 실행 함수가 실행될 때 단 한번 생성되는 클로저이기 때문에 발생.
    - Person.prototype.sayHi 메서드는 자신의 상위 스코프인 즉시 실행 함수의 실행 컨텍스트의 렉시컬 환경의 참조를 [[Environment]]에 저장하여 기억한다.
    - 따라서  Person 생성자 함수의 모든 인스턴스가 상속을 통해 호출할 수 있는 Person.prototype.sayHi 메서드의 상위 스코프는 어떤 인스턴스로 호출하더라도 하나의 동일한 상위 스코프를 사용하게 된다.

⇒ 이처럼 자바스크립트는 정보 은닉을 완전하게 지원하지 않는다. 

⇒ 다행히 private 필드를 정의할 수 있는 새로운 표준 사양이 제안되어있다.

## 24.6 자주 발생하는 실수

```jsx
var funcs = [];

for (var i = 0; i < 3; i++) {
  funcs[i] = function () { return i; }; // ①
}

for (var j = 0; j < funcs.length; j++) {
  console.log(funcs[j]()); // ②
}
```

- 실행결과는 0, 1, 2 일 것 같지만 3 3 3 을 반환한다.
- var 키워드로 선언한 i 변수는 블록 레벨 스코프가 아닌 함수 레벨 스코프를 갖기 때문에 전역 변수이다.
- 전역 변수 i 에는 0 → 1 → 2 가 순차적으로 할당되고 funcs 배열의 요소로 추가한 함수를 호출하면 전역 변수  i 를 참조하여 모두 3이 출력된다.

```jsx
var funcs = [];

for (var i = 0; i < 3; i++){
  funcs[i] = (function (id) { // ①
    return function () {
      return id;
    };
  }(i));
}

for (var j = 0; j < funcs.length; j++) {
  console.log(funcs[j]());
}
```

- 클로저를 사용하여 올바르게 고치면 다음과 같다.
- 1에서 즉시 실행 함수는 전역 변수 i 에 현재 할당되어 있는 값을 인수로 전달받아 매개변수 id에 할당한 후 중첩 함수를 반환하고 종료한다.
- 이 때 즉시 실행 함수의 매개 변수  id는  즉시 실행 함수가 반환한 중첩 함수의 상위 스코프에 존재한다.
- 중첩 함수는 자신의 상위 스코프(즉시 실행 함수의 렉시컬 환경)를 기억하는 클로저이고, 매개변수 id는 즉시 실행 함수가 반환한 중첩 함수에 묶여있는 자유 변수가 되어 그 값이 유지된다.
- 사실 아까 예제에서 var → let 키워드를 사용하면 문제가 깔끔히 해결된다.
    
    ```
    const funcs = [];
    
    for (let i = 0; i < 3; i++) {
      funcs[i] = function () { return i; };
    }
    
    for (let i = 0; i < funcs.length; i++) {
      console.log(funcs[i]()); // 0 1 2
    ```
    
    - let 키워드로 선언한 변수를 사용하면 for 문의 코드 블럭이 반복 실행될 때마다 **새로운 렉시컬 환경이 생성**된다.
    - 그리고 코드 블록 내에서 정의한 함수의 상위 스코프는 for 문 코드 블럭의 새로운 렉시컬 환경이다.
    - let, const 키워드를 사용하는 반복문은 코드 블록을 반복 실행할 때마다 새로운 렉시컬 환경을 생성하여 반복할 당시의 상태를 마치 스냅숏을 찍는 것처럼 저장한다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/540416be-01ae-47c6-957b-a216b07b4a0a/Untitled.png)
    

```jsx
// 요소가 3개인 배열을 생성하고 배열의 인덱스를 반환하는 함수를 요소로 추가한다.
// 배열의 요소로 추가된 함수들은 모두 클로저다.
const funcs = Array.from(new Array(3), (_, i) => () => i); // (3) [ƒ, ƒ, ƒ]

// 배열의 요소로 추가된 함수 들을 순차적으로 호출한다.
funcs.forEach(f => console.log(f())); // 0 1 2
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/266959bb-8b99-46a8-81f8-2763ede5d425/Untitled.png)

- 함수형 프로그래밍 기법인 고차함수를 활용하는 방법도 있다.
