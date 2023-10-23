## 25.1 클래스는 프로토타입의 문법적 설탕인가?

- ES5 에서는 클래스 없이도 다음과 같이 생성자 함수와 프로토타입을 통해 객체지향 언어의 상속을 구현할 수 있다.
    
    ```tsx
    // ES5 생성자 함수
    var Person = (function () {
      // 생성자 함수
      function Person(name) {
        this.name = name;
      }
    
      // 프로토타입 메서드
      Person.prototype.sayHi = function () {
        console.log('Hi! My name is ' + this.name);
      };
    
      // 생성자 함수 반환
      return Person;
    }());
    
    // 인스턴스 생성
    var me = new Person('Lee');
    me.sayHi(); // Hi! My name is Lee
    ```
    
- 기존 프로토타입 기반 객체지향 프로그래밍보다 클래스 기반에 익숙한 프로그래머가 더욱 빠르게 학습할 수 있도록 새로운 객체 생성 매커니즘을 제시한다.
- 클래스와 생성자 함수는 모두 프로토타입 기반의 인스턴스를 제공하지만 정확히 동일하게 동작하지는 않는다.

<aside>
❓ **클래스와 생성자 함수의 차이**

1. 클래스는 new 연산자 없이 호출하면 에러 발생. 생성자 함수는 new 연산자 없이 호출하면 일반 함수로서 호출.
2. 클래스는 extends와 super 키워드로 상속 가능. 생성자 함수는 불가능.
3. **클래스는 호이스팅이 발생하지 않는 것처럼 동작**. 생성자 함수는 호이스팅이 발생.
4. **클래스 내부에는 자동으로 strict mode가 지정**되며 해제할 수 없다. 생성자 함수는 그렇지 않다.
5. 클래스의 constructor, 프로토타입 메서드, 정적 메서드는 모두 열거 불가능. (`[[Enumerable]]` ⇒ `false`)
</aside>

⇒ **클래스는 생성자 함수 기반의 객체 생성 방식보다 견고하고 명료**하다. ⇒ 새로운 객체 생성 매커니즘!

## 25.2 클래스 정의

```tsx
// 클래스 선언문
class Person {}
```

```tsx
// 익명 클래스 표현식
const Person = class {};

// 기명 클래스 표현식
const Person = class MyClass {};
```

- 클래스 이름은 파스칼 케이스
- 클래스는 값으로 평가될 수 있는 일급 객체이다.
    - 무명 리터럴로 생성 가능. 즉, 런타임에 생성이 가능.
    - 변수나 자료 구조에 저장 가능.
    - 함수의 매개변수에 전달 가능.
    - 함수의 반환값으로 사용 가능.
- 클래스 몸체에는 0 개 이상의 메서드만 정의할 수 있다. → **constructor(생성자), 프로토타입 메서드, 정적 메서드**의 세 가지가 있다.
    
    ```tsx
    // 클래스 선언문
    class Person {
      // 생성자
      constructor(name) {
        // 인스턴스 생성 및 초기화
        this.name = name; // name 프로퍼티는 public하다.
      }
    
      // 프로토타입 메서드
      sayHi() {
        console.log(`Hi! My name is ${this.name}`);
      }
    
      // 정적 메서드
      static sayHello() {
        console.log('Hello!');
      }
    }
    
    // 인스턴스 생성
    const me = new Person('Lee');
    
    // 인스턴스의 프로퍼티 참조
    console.log(me.name); // Lee
    // 프로토타입 메서드 호출
    me.sayHi(); // Hi! My name is Lee
    // 정적 메서드 호출
    Person.sayHello(); // Hello!
    ```
    

클래스와 생성자 함수의 정의 방식을 비교해보면 다음과 같다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/6eab7dd5-22f3-4480-94c7-89c522362666/Untitled.png)

## 25.3 클래스 호이스팅

- 클래스는 함수로 평가된다.

```tsx
// 클래스 선언문
class Person {}

console.log(typeof Person); // function
```

- 클래스 선언문으로 정의한 클래스는 함수 선언문과 같이 소스코드 평가 과정, 즉 **런타임 이전에 먼저 평가되어 함수 객체를 생성**한다.
- 이때, 클래스가 평가되어 생성된 함수 객체는 생성자 함수로서 호출할 수 있는 함수, 즉 constructor이다.
- 생성자 함수로서 호출할 수 있는 함수는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다. ( 프로토타입과 생성자 함수는 언제나 쌍으로 존재한다. )
- 단, 클래스는 클래스 정의 이전에 참조할 수 없다. → let, const 처럼 선언은 호이스팅 되지만 **할당은 호이스팅되지 않는다.**

## 25.4 인스턴스 생성

- 클래스는 생성자 함수이며 **반드시 new 연산자와 함께 호출**되어 인스턴스를 생성한다.
    
    ```tsx
    class Person {}
    
    // 인스턴스 생성
    const me = new Person();
    console.log(me); // Person {}
    ```
    
- 클래스 표현식으로 정의된 클래스의 경우, 다음 예제와 같이 클래스를 가리키는 식별자를 사용해 인스턴스를 생성하지 않고 기명 클래스 표현식의 클래스 이름을 사용해 인스턴스를 생성하면 에러가 발생한다.
    
    ```tsx
    const Person = class MyClass {};
    
    // 함수 표현식과 마찬가지로 클래스를 가리키는 식별자로 인스턴스를 생성해야 한다.
    const me = new Person();
    
    // **클래스 이름 MyClass는 함수와 동일하게 클래스 몸체 내부에서만 유효한 식별자**다.
    console.log(MyClass); // ReferenceError: MyClass is not defined
    
    const you = new MyClass(); // ReferenceError: MyClass is not defined
    ```
    

## 25.5 메서드

<aside>
💡 **클래스 정의에 대한 새로운 제안 사양**

**인스턴스 프로퍼티는 반드시 constructor 내부에서 정의**해야 한다.
하지만 클래스 몸체에 메서드 뿐만 아니라 프로퍼티를 직접 정의할 수 있는 새로운 표준 사양이 제안되어 있다.
머지않아 몸체에서 메서드 + 프로퍼티를 정의할 수 있게 될 것으로 보인다.

</aside>

### 25.5.1 consturctor

```tsx
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }
}
```

- **인스턴스를 생성하고 초기화하기 위한 특수 메서드**
- 클래스는 평가되어 함수 객체가 된다. → 함수 객체 고유의 프로퍼티.
- 클래스 내부
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/42a9df62-d3c5-47f8-9306-e1fc38c230b9/Untitled.png)
    
- 인스턴스 내부
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/f31ae343-680f-42de-961b-2c5ce795ecfc/Untitled.png)
    
- 클래스가 평가되어 생성된 함수 객체나 인스턴스 어디에도 constructor 메서드가 보이지 않는다. → 단순한 메서드가 아님!
- 메서드로 해석되는 것이 아니라 **클래스가 평가되어 생성한 함수 객체 코드의 일부**가 된다.
- 즉, 클래스 정의가 평가되면 constructor의 기술된 동작을 하는 함수 객체가 생성된다.

<aside>
💡 **클래스의 constructor 메서드와 프로토타입의 constructor 프로퍼티**

**이름이 같아 혼동하기 쉽지만 직접적인 관련이 없다.** 프로토타입의 constructor 프로퍼티는 모든 프로토타입이 가지고 있는 프로퍼티이며, 생성자 함수를 가리킨다.

</aside>

<aside>
💡 **클래스 constructor 와 생성자 함수의 차이점**

1. constructor 는 클래스 내에 최대 한개만 존재할 수 있다.
2. 클래스 constructor 를 생략해도 암묵적으로 빈 constructor 가 정의된다. → 빈 객체 생성
3. 프로퍼티가 추가되어 초기화된 인스턴스를 생성하려면 constructor 내부에서 this 에  인스턴스 프로퍼티를 추가한다. 
</aside>

### 25.2 프로토타입 메서드

```tsx
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }

  // 프로토타입 메서드
  sayHi() {
    console.log(`Hi! My name is ${this.name}`);
  }
}

const me = new Person('Lee');
me.sayHi(); // Hi! My name is Lee
```

- 클래스의 prototype 프로퍼티에 메서드를 추가하지 않아도 (Person.prototype.sayHi) 기본적으로 프로토타입 메서드가 된다.
    
    ⇒ 클래스의 메서드는 기본적으로 프로토타입 메서드가 된다. 
    
- 생성자 함수와 마찬가지로 클래스가 생성한 인스턴스는 프로토타입 체인의 일원이 된다.
    
    ```tsx
    // me 객체의 프로토타입은 Person.prototype이다.
    Object.getPrototypeOf(me) === Person.prototype; // -> true
    me instanceof Person; // -> true
    
    // Person.prototype의 프로토타입은 Object.prototype이다.
    Object.getPrototypeOf(Person.prototype) === Object.prototype; // -> true
    me instanceof Object; // -> true
    
    // me 객체의 constructor는 Person 클래스다.
    me.constructor === Person; // -> true
    ```
    

### 25.5.3 정적 메서드

- 정적 메서드란 **인스턴스를 생성하지 않아도 호출될 수 있는 메서드**를 말한다.
- = 클래스에 바인딩된 메서드
- 인스턴스의 프로토타입 체인 상에는 클래스가 존재하지 않으므로 정적메서드는 인스턴스로 호출할 수 없다.

```tsx
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }

  // 정적 메서드
  static sayHi() {
    console.log('Hi!');
  }
}

Person.sayHi(); // 정적메서드 호출 
```

### 25.5.4 정적 메서드와 프로토타입 메서드의 차이

1. 정적 메서드와 프로토타입 메서드는 **프로토타입 체인**이 다르다.
2. 정적 메서드는 **클래스로 호출**하고 프로토타입 메서드는 **인스턴스로 호출**.
3. 정적 메서드는 **인스턴스 프로퍼티 참조 불가능**. 프로토타입 메서드는 가능.
    
    ```tsx
    class Square {
      // 정적 메서드
      static area(width, height) { // 정적 메서드 내부의 this는  인스턴스가 아닌 클래스를 가리킨다. 
        return width * height;
      }
    }
    
    console.log(Square.area(10, 10)); // 100
    ```
    
    ```tsx
    class Square {
      constructor(width, height) {
        this.width = width;
        this.height = height;
      }
    
      // 프로토타입 메서드
      area() {
        return this.width * this.height;
      }
    }
    
    const square = new Square(10, 10);
    console.log(square.area()); // 100
    ```
    
    - **this 를 사용하지 않는 메서드는 정적 메서드로 정의하는 것이 좋다** → 인스턴스를 불필요하게 생성하지 않아도 됨.

```tsx
// 표준 빌트인 객체의 정적 메서드
Math.max(1, 2, 3);          // -> 3
Number.isNaN(NaN);          // -> true
JSON.stringify({ a: 1 });   // -> "{"a":1}"
Object.is({}, {});          // -> false
Reflect.has({ a: 1 }, 'a'); // -> true
```

- 클래스 또는 생성자 함수를 하나의 네임 스페이스로 사용하여 정적 메서드를 모아 놓으면 이름 충돌 가능성을 줄여주고 관련 함수들을 구조화할 수 있는 효과가 있다.

### 25.5.5 클래스에서 정의한 메서드 특징

1. function 키워드를 생략한 메서드 축약 표현을 사용한다.
2. 클래스에서 메서드를 정의할 때는 콤마가 필요없다.
3. 암묵적으로 strict mode로 실행된다.
4. `for … in` 또는 `Object.keys` 로 열거 불가능하다. → 프로퍼티의 열거 가능 여부 == false
5. 내부 메서드 `[[Contructor]]`를 갖지 않는 non-constructor이다. new 연산자와 함께 호출 할 수 없다.

## 25.6 클래스 인스턴스 생성 과정

- new 연산자와 함께 클래스를 호출하면, 클래스의 내부 메서드 [[Construct]] 가 호출된다.

```jsx
class Person {
  // 생성자
  constructor(name) {
    // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
    console.log(this); // Person {}
    console.log(Object.getPrototypeOf(this) === Person.prototype); // true

    // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
    this.name = name;

    // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
  }
}
```

### 1. 인스턴스 생성과 this 바인딩

- constructor 코드가 실행되기에 앞서 암묵적으로 빈 객체가 생성된다.
- 해당 인스턴스의 프로토타입으로 클래스의 prototype 프로퍼티가 가리키는 객체가 설정된다.
- 그리고 암묵적으로 생성된 빈 객체, 인스턴스는 this에 바인딩된다.
- 따라서 constructor 내부의 this 는 클래스가 생성한 인스턴스를 가리킨다.

### 2. 인스턴스 초기화

constructor 의 내부 코드가 실행되어 this 에 바인딩 되어있는 인스턴스를 초기화한다. ( 프로퍼티 추가 ) 

### 3. 인스턴스 반환

클래스의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.

## 25.7 프로퍼티

### 25.7.1 인스턴스 프로퍼티

- 인스턴스 프로퍼티는 constructor 내부에서 정의해야 한다.

```jsx
class Person {
    constructor(name) {
        // 인스턴트 프로퍼티
        this.name = name;	// name 프로퍼티는 public하다.
    }
}
```

- constructor 내부에서 this에 추가한 프로퍼티는 언제나 클래스가 생성한 인스턴스의 프로퍼티가 된다.
- ES6의 클래스는 다른 객체지향 언어처럼 private, public, protected 키워드와 같은 접근 제한자를 지원하지 않는다.(2021년 1월 기준)
    
    ⇒ 따라서 인스턴스 프로퍼티는 언제나 public하지만 다행히 [private한 프로퍼티를 정의할 수 있는 사양](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Private_class_fields)이 현재 제공되고 있다.
    

### 25.7.2 접근자 프로퍼티

- 접근자 프로퍼티는 자체적으로는 값(`[[Value]] 내부 슬롯`)을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수, 즉 getter 와 setter 로 구성되어 있다.
- 접근자 프로퍼티는 클래스에서도 사용할 수 있다.
    
    ```jsx
    class Person {
        constructor(firstName, lastName) {
            this.firstName = firstName;
            this.lastName = lastName;
        }
    
        // fullName은 접근자 함수로 구성된 접근자 프로퍼티다.
        // getter 함수
        **get fullName**() {
            return `${this.firstName} ${this.lastName}`;
        }
    
        // setter 함수
        **set fullName**(name) {
            [this.firstName, this.lastName] = name.split(' ');
        }
    }
    
    const me = new Person('Ungmo', 'Lee');
    
    // 접근자 프로퍼티를 통한 프로퍼티 값의 저장
    me.fullName = 'Heegun Lee';
    console.log(me);    // {firstName: "Heegun", lastName: "Lee"}
    
    // 접근자 프로퍼티를 통한 프로퍼티 값의 참조
    // 접근자 프로퍼티 fullName에 접근하면 getter 함수가 호출된다.
    console.log(me.fullName);   // Heegun Lee
    
    // fullName은 접근자 프로퍼티다.
    // 접근자 프로퍼티는 get, set, enumerable, configurable 프로퍼티 어트리뷰트를 갖는다.
    console.log(Object.getOwnPropertyDescriptor(Person.prototype, 'fullName'));
    // {get: f, set: f, enumerable: false, configurable: true}
    ```
    
- 접근자 프로퍼티는 자체적으로는 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수로 구성되어 있다.
    - `getter`
        - **인스턴스 프로퍼티에 접근할 때마다** 프로퍼티 값을 조작하거나 별도의 행위가 필요할 때 사용한다.
        - 메서드 이름 앞에 get 키워드를 사용해 정의한다.
        - 프로퍼티처럼 참조 시 내부적으로 getter가 호출된다.
        - 반드시 무언가를 반환해야 한다.
    - `setter`
        - **인스턴스 프로퍼티에 값을 할당할 때마다** 프로퍼티 값을 조작하거나 별도의 행위가 필요할 때 사용한다.
        - 메서드 이름 앞에 set 키워드를 사용해 정의한다.
        - 프로퍼티처럼 값을 할당하는 형식으로 하며 할당 시 내부적으로 setter가 호출된다.
        - 반드시 매개변수가 있어야 한다. 이때 단 하나의 매개변수만 선언할 수 있다.
- getter 와 setter 의 이름은 인스턴스 프로퍼티처럼 사용한다. `인스턴스.이름` 으로 접근
    
    ⇒ 호출하는 것이 아니라, 프로퍼티처럼 참조하는 형식으로 사용한다. ( 참조 시에 내부적으로 호출되는 형태 ) 
    
- 클래스의 접근자 프로퍼티 또한 인스턴스 프로퍼티가 아닌 프로토타입의 프로퍼티가 된다.
    
    ```jsx
    // Object.getOwnPropertyNames는 비열거형(non-enumerable)을 포함한 모든 프로퍼티 이름을 반환한다. (상속 제외)
    Object.getOwnPropertyNames(me);                         // ["firstName", "lastName"]
    Object.getOwnPropertyNames(Object.getPrototypeOf(me));  // ["constructor", "fullName"]
    ```
    
    ![클래스의 접근자 프로퍼티는 프로토타입 프로퍼티이다.](https://velog.velcdn.com/images/kozel/post/89dc7fcd-01a6-42f5-93e2-10241df7f4e3/image.jpeg)
    
    클래스의 접근자 프로퍼티는 프로토타입 프로퍼티이다.
    

### 25.7.3 클래스 필드 정의 제안

- 클래스 필드(필드 또는 멤버):  클래스 기반 객체지향 언어에서 **클래스가 생성할 인스턴스의 프로퍼티를 가리키는 용어**
- 자바스크립트의 클래스 몸체에는 메서드만 선언할 수 있다. 따라서 클래스 몸체에 자바와 유사하게 클래스 필드를 선언하면 문법 에러(SyntaxError)가 발생한다.
    
    위 예제는 에러가 발생할 것 같지만 최신 브라우저(Chrome 72 이상) 또는 최신 Node.js(버전 12 이상)에서 실행하면 정상 동작한다. 그 이유는 자바스크립트에서도 인스턴스 프로퍼티를 마치 클래스 기반 객체지향 언어의 클래스 필드처럼 정의할 수 있는 새로운 표준 사양인 "Class field declarations"가 TC39 프로세스의 stage3(candidate)에 제안되었기 때문이다.
    
    ⇒ 따라서 다음 예제와 같이 클래스 필드를 클래스 몸체에 정의할 수 있다.
    
    ```jsx
    class Person {
      // 클래스 필드 정의
      name = 'Lee';
    }
    
    const me = new Person('Lee');
    ```
    
    - 클래스 몸체에서 클래스 필드를 정의하는 경우 this에 클래스 필드를 바인딩해서는 안 된다. this 는 클래스의 constructor와 메서드 내에서만 유효하다.
        
        ```jsx
        class Person {
          // this에 클래스 필드를 바인딩해서는 안된다.
          this.name = ''; // SyntaxError: Unexpected token '.'
        }
        ```
        
    - 클래스 필드를 참조하는 경우에는 반드시 this 를 사용해야한다.
    - 클래스 필드에 초기값을 할당하지 않으면 undefined를 갖는다.
        
        ```jsx
        class Person {
          // 클래스 필드를 초기화하지 않으면 undefined를 갖는다.
          name;
        }
        
        const me = new Person();
        console.log(me); // Person {name: undefined}
        ```
        
    - **외부의 초기값으로 클래스 필드를 초기화해야 할 필요가 있다면** constructor 에서 매개변수를 받아서 초기화해야한다.
    - 인스턴스를 생성할 때 클래스 필드를 초기화할 필요가 있다면 constructor 밖에서 클래스 필드를 또 정의할 필요가 없다.
        
        ```jsx
        class Person {
          name; // 필요없는 변수
        
          constructor(name) {
            // 클래스 필드 초기화.
            this.name = name;
          }
        }
        
        const me = new Person('Lee');
        console.log(me); // Person {name: "Lee"}
        ```
        
    - 함수는 일급 객체 (= 값으로 평가될 수 있음) 이므로 클래스 필드에 할당할 수 있다. 따라서 클래스 필드를 통해 메서드를 정의할 수도 있다.
        
        ```jsx
        class Person {
          name = 'Lee'; // 클래스 필드에 문자열 할당 
        
          // 클래스 필드에 함수를 할당
          getName = function () {
            return this.name;
          }
          // 화살표 함수로 정의할 수도 있다.
          // getName = () => this.name;
        }
        
        const me = new Person();
        console.log(me);	// Person {name: "Lee", getName: f}
        console.log(me.getName());	// Lee
        ```
        
        ⇒ **클래스 필드에 함수를 할당하는 경우, 이 함수는 프로토타입 메서드가 아닌 인스턴스 메서드가 된다.** **모든 클래스 필드는 인스턴스 프로퍼티가 되기 때문이다. 따라서 클래스 필드에 함수를 할당하는 것은 권장하지 않는다.** 
        
    
    <aside>
    📌 **클래스 필드와 화살표 함수**
    
    클래스 필드에 화살표 함수를 할당하여 화살표 함수 내부의 this가 인스턴스를 가리키게 하는 경우도 있다.
    
    ```jsx
    <!DOCTYPE html>
    <html>
    <body>
      <button class="btn">0</button>
      <script>
        class App {
          constructor() {
            this.$button = document.querySelector('.btn');
            this.count = 0;
    
            // increase 메서드를 이벤트 핸들러로 등록
            // 이벤트 핸들러 increase 내부의 this는 DOM 요소(this.$button)를 가리킨다.
            // 하지만 increase는 화살표 함수로 정의되어 있으므로
            // increase 내부의 this는 인스턴스를 가리킨다.
            this.$button.onclick = this.increase;
    
            // 만약 increase가 화살표 함수가 아니라면 bind 메서드를 사용해야 한다.
            // $button.onclick = this.increase.bind(this);
          }
    
          // 인스턴스 메서드
          // 화살표 함수 내부의 this는 언제나 상위 컨텍스트의 this를 가리킨다.
          increase = () => this.$button.textContent = ++this.count;
        }
        new App();
      </script>
    </body>
    </html>
    ```
    
    그러나 인스턴스가 여러개 생성된다면 이 방법도 메모리의 손해를 감수할 수밖에 없다. 
    
    </aside>
    

⇒ 외부 초기값으로 클래스 필드를 초기화할 필요가 있다면 constructor 에서 정의, 그렇지 않으면 두 방식 모두 사용가능.

### 25.7.4 private 필드 정의 제안

- 인스턴스 프로퍼티는 인스턴스를 통해 클래스 외부에서 언제나 참조(public)할 수 있다.
- 그러나 2021년 1월, private 필드를 정의할 수 있는 새로운 표준 사양이 제안되었다.
- private 필드의 선두에는 #을 붙여주며 참조할 때도 #을 붙여주어야 한다.

```jsx
class Person {
  // private 필드 정의
  #name = '';

  constructor(name) {
    // private 필드 참조
    this.#name = name;
  }
}

const me = new Person('Lee');
console.log(me.#name);	// SyntaxError: Private field '#name' must be declared in an enclosing class
```

- public 필드는 참조할 수 있지만 private 필드는 클래스 내부에서만 참조할 수 있다.
    
    
    | 접근 가능성 | public | private |
    | --- | --- | --- |
    | 클래스 내부 | O | O |
    | 자식 클래스 내부 | O | X |
    | 클래스 인스턴스를 통한 접근 | O | X |
- 클래스 외부에서 private 필드에 직접 접근하는 방법은 없다. 그러나 접근자 프로퍼티를 통해 간접적으로 접근하는 방법은 유효하다.
    
    ```jsx
    class Person {
      #name = '';
    
      constructor(name) {
        this.#name = name;
      }
    	// name은 접근자 프로퍼티이다. 
      get name() {
    		// private 필드를 참조하여 trim한 다음 반환한다. 
        return this.#name.trim();
      }
    }
    
    const me = new Person('Lee');
    console.log(me.name);	// Lee
    ```
    
- private 필드는 반드시 클래스 몸체에 정의해야 한다.
    
    ```jsx
    class Person {
      constructor(name) {
        // private 필드는 클래스 몸체에서 정의해야 한다. 직접 consturctor 에 정의하면 에러가 발생한다.
        this.#name = name; // SyntaxError: Private field '#name' must be declared in a enclosing class
      }
    }
    ```
    

### 25.7.5 static 필드 정의 제안

- 클래스에서 static 키워드를 사용해서 정적 메서드를 정의할 수 있지만, 정적 필드를 정의할 수는  없었다.
- 그러나 static public 필드, static private 필드, static private 메서드를 정의할 수 있는 새로운 표준 사양인 "Static class features"가 TC39 프로세스의 stage 3(candidate)에 제안되어 있다.
    
    ```jsx
    class MyMath {
        // static public 필드 정의
        static PI = 22 / 7;
    
        // static private 필드 정의
        static #num = 10;
    
        // static 메서드
        static increment() {
            return ++MyMath.#num;
        }
    }
    
    console.log(MyMath.PI);           // 3.142857142857143
    console.log(MyMath.increment());  // 11
    ```
    

## 25.8 상속에 의한 클래스 확장

### 25.8.1 클래스 상속과 생성자 함수 상속

- 프로토타입 기반 상속은 프로토타입 체인을 통해 다른 객체의 자산을 상속받는 개념이지만 ***상속에 의한 클래스 확장은 기존 클래스를 상속받아 새로운 클래스를 확장하여 정의***하는 것이다.

!https://velog.velcdn.com/images/kozel/post/39b65dd3-962a-4729-a489-6b750f47c2f7/image.png

- 클래스는 상속을 통해 기존 클래스를 확장할 수 있는 문법이 기본적으로 제공되지만 생성자 함수는 그렇지 않다. **상속을 통해 클래스의 속성을 그대로 사용하면서 자신만의 고유한 속성만 추가하여 확장할 수 있다. ⇒ 재사용 관점에서 매우 유용하다.**

!https://velog.velcdn.com/images/kozel/post/832bb56c-1db6-486c-a5b0-e7f0cf878f53/image.png

```jsx
class Animal {
    constructor(age, weight) {
        this.age = age;
        this.weight = weight;
    }

    eat() { return 'eat'; }

    move() { return 'move'; }
}

// 상속을 통해 Animal 클래스를 확장한 Bird 클래스
class Bird extends Animal {
    fly() { return 'fly'; }
}

const bird = new Bird(1, 5);

console.log(bird);                      // Bird {age: 1, weight: 5}
console.log(bird instanceof Bird);      // true
console.log(bird instanceof Animal);    // true

console.log(bird.eat());    // eat
console.log(bird.move());   // move
console.log(bird.fly());    // fly
```

![상속에 의해 확장된 클래스 Bird를 통해 생성된 인스턴스의 프로토타입 체인](https://velog.velcdn.com/images/kozel/post/e8621feb-cb90-4031-bfed-7b7d1c7d865e/image.png)

상속에 의해 확장된 클래스 Bird를 통해 생성된 인스턴스의 프로토타입 체인

### 25.8.2 extends 키워드

- 상속을 통해 클래스를 확장하려면 **extends 키워드**를 사용하여 상속받을 클래스를 정의한다.
    
    ```jsx
    // 수퍼(베이스/부모)클래스
    class Base {}
    
    // 서브(파생/자식)클래스
    class Derived extends Base {}
    ```
    
- **extends 키워드는 수퍼클래스와 서브클래스 간의 상속 관계를 설정하는 역할**을 한다. 클래스도 프로토타입을 통해 상속 관계를 구현한다.

!https://velog.velcdn.com/images/kozel/post/e7127721-be79-4325-808a-040f5a52cfaf/image.png

- 수퍼클래스와 서브클래스는 인스턴스의 프로토타입 체인뿐 아니라 클래스 간의 프로토타입 체인도 생성한다. 이를 통해 프로토타입 메서드, 정적 메서드 모두 상속이 가능하다.

### 25.8.3 동적 상속

- extends 키워드는 클래스뿐만 아니라 생성자 함수를 상속받아 클래스를 확장할 수 있다. 단, extends 키워드 앞에는 반드시 클래스가 와야 한다.
    
    ```jsx
    // 생성자 함수
    function Base(a) {
        this.a = a;
    }
    
    // 생성자 함수를 상속받는 서브클래스
    class Derived extends Base {}
    
    const derived = new Derived(1);
    console.log(derived);   // Derived {a: 1}
    ```
    
- extends 키워드 다음에는 클래스뿐만 아니라 [[Construct]] 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식을 사용할 수 있다. 이를 통해 동적으로 상속받을 대상을 결정할 수 있다.
    
    ```jsx
    function Base1() {}
    
    class Base2 {}
    
    let condition = true;
    
    // 조건에 따라 동적으로 상속 대상을 결정하는 서브 클래스
    class Derived extends (condition ? Base1 : Base2) {}
    
    const derived = new Derived();
    console.log(derived);   // Derived {}
    
    console.log(derived instanceof Base1);  // true
    console.log(derived instanceof Base2);  // false
    ```
    

### 25.8.4 서브클래스의 constructor

- 클래스에서 constructor를 생략하면 클래스에 비어있는 constructor가 암묵적으로 정의된다.
- 서브클래스에서 constructor를 생략하면 클래스에 다음과 같은 constructor가 암묵적으로 정의된다. args는 new 연산자와 함께 클래스를 호출할 때 전달한 인수의 리스트다.
    
    ```jsx
    constructor(...args) { super(...args); }
    ```
    
    - super()는 수퍼클래스의 constructor(super-constructor)를 호출하여 인스턴스를 생성한다.

### 25.8.5 super 키워드

- super 키워드는 함수처럼 호출할 수도 잇고 this와 같이 식별자처럼 참조할 수 있는 특수한 키워드다.super는 다음과 같이 동작한다.
    - super를 호출하면 **수퍼클래스의 constructor(super-constructor)**를 호출한다.
    - super를 참조하면 **수퍼클래스의 메서드를 호출**할 수 있다.

**super 호출**

- super를 호출하면 수퍼클래스의 constructor(super-constructor)를 호출한다.
- 인스턴스 초기화를 위해 전달한 인수는 수퍼클래스와 서브클래스에 배분되고 상속 관계의 두 클래스는 서로 협력하여 인스턴스를 생성한다. ( 수퍼 클래스의 생성자에 전달할 필요가 있는 인수는 super 를 통해 전달한다. )
    
    ```jsx
    class Base {
        constructor(a, b) {	// ➃
            this.a = a;
            this.b = b;
        }
    }
    
    class Derived extends Base {
        constructor(a, b, c) {	// ➁
            super(a, b);		// ➂
            this.c = c;
        }
    }
    
    const derived = new Derived(1, 2, 3);	// ➀
    console.log(derived);					// Derived {a: 1, b: 2, c: 3}
    ```
    

<aside>
⚠️ **super를 호출할 때 주의할 사항**

1. 서브클래스에서 constructor를 생략하지 않는 경우 **서브클래스의 constructor에서는 반드시 super를 호출해야 한다.**
    
    ```jsx
    class Base {}
    
     class Derived extends Base {
        constructor() {
            // ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor
            console.log('constructor call');
        }
    
     }
    
     const derived = new Derived();
    ```
    
2. 서브클래스의 constructor에서 **super를 호출하기 전에는 this를 참조할 수 없다.**
    
    ```jsx
    class Base {}
    
     class Derived extends Base {
        constructor() {
            // ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor
            this.a = 1;
            super(); // 생성자로 초기화
        }
     }
    
     const derived = new Derived(1);
    ```
    
3. super는 반드시 **서브클래스의 constructor에서만 호출**한다. 서브클래스가 아닌 클래스의 constructor나 함수에서 super를 호출하면 에러가 발생한다.
    
    ```jsx
    class Base {
        constructor() {
            super();    // SyntaxError: 'super' keyword unexpected here
        }
     }
    
     function Foo() {
        super();    // SyntaxError: 'super' keyword unexpected here
     }
    ```
    
</aside>

**super 참조**

: 메서드 내에서 super를 참조하면 수퍼클래스의 메서드를 호출할 수 있다.

1. 서브클래스의 프로토타입 메서드 내에서 super.sayHi는 수퍼클래스의 프로토타입 메서드 sayHi를 가리킨다.
    
    ```jsx
     class Base {
        constructor(name) {
            this.name = name;
        }
    
        sayHi() {
            return `Hi ${this.name}`;
        }
     }
    
     class Derived extends Base {
        sayHi() {
            // super.sayHi는 수퍼클래스의 프로토타입 메서드를 가리킨다.
            return `${super.sayHi()}. how are you doing?`;
        }
     }
    
     const derived = new Derived('Lee');
     console.log(derived.sayHi());   // Hi Lee. how are you doing?
    ```
    
    - super 참조를 통해 수퍼클래스의 메서드는 참조하려면 super가 수퍼클래스의 메서드가 바인딩된 객체, 즉 수퍼클래스의 prototype 프로퍼티에 바인딩된 프로토타입을 참조할 수 있어야 한다. 위 예제는 다음 예제와 동일하게 동작한다.
        
        ```jsx
        class Base {
            constructor(name) {
                this.name = name;
            }
        
            sayHi() {
                return `Hi ${this.name}`;
            }
         }
        
         class Derived extends Base {
            sayHi() {
                // __super는 Base.prototype을 가리킨다.
                const __super = Object.getPrototypeOf(Derived.prototype);
                return `${__super.sayHi.call(this)} how are you doing?`;
            }
         }
        ```
        
    - super는 자신을 참조하고 있는 메서드가 바인딩되어 있는 객체의 프로토타입을 가리킨다
        
        ⇒ 이를 위해 메서드는 내부 슬롯 [[HomeObject]]를 가지며, 자신을 바인딩하고 있는 객체를 가리킨다.
        
        ```jsx
        /*
         [[HomeObject]]는 메서드 자신을 바인딩하고 있는 객체를 가리킨다.
         [[HomeObject]]를 통해 메서드 자신을 바인딩하고 있는 객체의 프로토타입을 찾을 수 있다.
         */
         super = Object.getPrototypeOf([[HomeObject]])
        ```
        
        - **주의할 것은 ES6의 메서드 축약 표현으로 정의된 함수만이 [[HomeObject]]를 갖는다는 것이다.**
        
        ```jsx
         const obj = {
           // foo는 ES6의 메서드 축약 표현으로 정의한 메서드다. 따라서 [[HomeObject]]를 갖는다.
           foo() {},
           // bar는 ES6의 메서드 축약 표현으로 정의한 메서드가 아니라 일반 함수다.
           // 따라서 [[HomeObject]]를 갖지 않는다.
           bar: function () {}
         };
        ```
        
        - [[HomeObject]]를 가지는 ES6  메서드 축약표현으로 정의된 함수만이 super 참조를 할 수 있다. 단, super 참조는 수퍼클래스의 메서드를 참조하기 위해 사용하므로 서브클래스의 메서드에서 사용해야 한다.
        - super 참조는 클래스의 전유물이 아니다. **객체 리터럴에서도 super 참조를 사용할 수 있다**. 단 ES6의 메서드 축약 표현으로 정의된 함수만 가능하다.
        
        ```jsx
        const base = {
           name: 'Lee',
           sayHi() {
             return `Hi! ${this.name}`;
           }
         };
        
         const derived = {
           __proto__: base,
           // ES6 메서드 축약 표현으로 정의한 메서드다. 따라서 [[HomeObject]]를 갖는다.
           sayHi() {
             return `${super.sayHi()}. how are you doing?`;
           }
         };
        
         console.log(derived.sayHi());	// Hi Lee. how are you doing?
        ```
        
2. **서브클래스의 정적 메서드 내에서 super.sayHi는 수퍼클래스의 정적 메서드 sayHi를 가리킨다.**
    
    ```jsx
    class Base {
       static sayHi() {
         return 'Hi!';
       }
     }
    
     class Derived extends Base {
       static sayHi() {
         // super.sayHi는 수퍼클래스의 정적 메서드를 가리킨다.
         return `${super.sayHi()} how are you doing?`;
       }
     }
    
     console.log(Derived.sayHi());	// Hi how are you doing?
    ```
    

### 25.8.6 상속 클래스의 인스턴스 생성 과정

다음의 Rectangle 클래스와 상속받은 ColorRectangle 클래스를 보자.

```jsx
class Rectangle {
    constructor(width, height) {
        this.width = width;
        this.height = height;
    }

    getArea() {
        return this.width * this.height;
    }

    toString() {
        return `width = ${this.width}, height = ${this.height}`;
    }
}

class ColorReactangle extends Rectangle {
    constructor(width, height, color) {
        super(width, height);
        this.color = color;
    }

    // 메서드 오버라이딩
    toString() {
        return super.toString() + `, color = ${this.color}`;
    }
}

const colorReactangle = new ColorReactangle(2, 4, 'red');
console.log(colorReactangle);   // ColorRectangle {width: 2, height: 4, color: "red"}

console.log(colorReactangle.getArea());     // 8
console.log(colorReactangle.toString());    // width = 2, height = 4, color = red
```

colorRectangle 클래스에 의해 생성된 인스턴스의 프로토타입 체인은 다음과 같다.

!https://velog.velcdn.com/images/kozel/post/8fc55ba2-e4fd-4ee7-9187-4c331776e1c0/image.png

서브클래스 colorRectangle이 new 연산자와 함께 호출되면 다음 과정을 통해 인스턴스를 생성한다.

1. **서브클래스의 super 호출**
    - 수퍼클래스와 서브클래스를 구분하기 위해 "base" 또는 "derived"를 값으로 갖는 내부 슬롯 [[ConstructorKind]]를 갖는다.
    - 다른 클래스를 상속받지 않는 클래스(그리고 생성자 함수)는 내부 슬롯 [[ConstructorKind]]의 값이 "base"로 설정되지만 다른 클래스를 상속받는 서브클래스는 내부 슬롯 [[ConstructorKind]]의 값이 "derived"로 설정된다. 그리고 이를 통해 호출되었을 때의 동작이 구분된다.
        - base 클래스는 new 연산자와 함께 호출되었을 대 암묵적으로 빈 객체, 즉 인스턴스를 생성하고 이를 this에 바인딩b다.
        - **서브클래스는 자신이 직접 인스턴스를 생성하지 않고 수퍼클래스에게 인스턴스 생성을 위임한다. 이것이 바로 서브클래스의 constructor에서 반드시 super을 호출해야 하는 이유다.**
            - 서브클래스가 new 연산자와 함께 호출되면 서브클래스 constructor 내부의 super 키워드가 함수처럼 호출되고 super가 호출되면 수퍼클래스가 평가되어 생성된 함수 객체의 코드가 실행되기 시작한다.
            - 그래서 서브클래스 constructor 내부에 super 호출이 없으면 에러가 발생한다.
            - 실제로 **인스턴스를 생성하는 주체는 수퍼클래스**이므로 수퍼클래스의 constructor를 호출하는 super가 호출되지 않으면 인스턴스를 생성할 수 없기 때문이다.
2. **수퍼클래스의 인스턴스 생성과 this 바인딩**
    - 수퍼클래스의 constructor 내부의 this는 생성된 인스턴스를 가리킨다.
    
    ```jsx
    class Rectangle {
        constructor(width, height) {
          // 암묵적으로 빈 객체, 즉 인스턴스가 생성되고 this에 바인딩된다.
          console.log(this);	// ColorRectangle {}
          // new 연산자와 함께 호출된 함수, 즉 new.target은 ColorRectangle이다.
          console.log(new.target);	// ColorRectangle
    ...
    ```
    
    - 이때 인스턴스는 수퍼클래스가 생성한 것이다. 하지만 new 연산자와 함께 호출된 클래스가 서브클래스라는 것이 중요하다. 즉, new 연산자와 함께 호출된 함수를 가리키는 new.target은 서브클래스를 가리킨다.
    
    ⇒ 따라서 **인스턴스는 new.target이 가리키는 서브클래스가 생성한 것으로 처리된다.**
    
    ⇒ 따라서 **생성된 인스턴스의 프로토타입은** 수퍼클래스의 prototype 프로퍼티가 가리키는 객체가 아니라 **new.target, 즉 서브클래스의 prototype 프로퍼티가 가리키는 객체**이다.
    
    ```jsx
    class Rectangle {
        constructor(width, height) {
          // 암묵적으로 빈 객체, 즉 인스턴스가 생성되고 this에 바인딩된다.
          console.log(this);	// ColorRectangle {}
          // new 연산자와 함께 호출된 함수, 즉 new.target은 ColorRectangle이다.
          console.log(new.target);	// ColorRectangle
    
          console.log(Object.getPrototypeOf(this) === ColorRectangle.prototype);	// true
          console.log(this instanceof ColorRectangle);	// true
          console.log(this instanceof Rectangle);		// true
    ...
    ```
    
3. **수퍼클래스의 인스턴스 초기화**
    
    수퍼클래스의 constructor가 실행되어 this에 바인딩되어 있는 인스턴스를 초기화한다. 즉, this에 바인딩되어 있는 인스턴스 프로퍼티를 추가하고 constructor가 인수로 전달받은 초기값으로 인스턴스의 프로퍼티를 초기화한다.
    
    ```jsx
    class Rectangle {
        constructor(width, height) {
          // 암묵적으로 빈 객체, 즉 인스턴스가 생성되고 this에 바인딩된다.
          console.log(this);	// ColorRectangle {}
          // new 연산자와 함께 호출된 함수, 즉 new.target은 ColorRectangle이다.
          console.log(new.target);	// ColorRectangle
    
          console.log(Object.getPrototypeOf(this) === ColorRectangle.prototype);	// true
          console.log(this instanceof ColorRectangle);	// true
          console.log(this instanceof Rectangle);		// true
    
          // 인스턴스 초기화
          this.width = width;
          this.height = height;
    
          console.log(this);	// ColorRectangle {width: 2, height: 4}
    ...
    ```
    
4. **서브클래스 constructor로의 복귀와 this 바인딩**
    
    super의 호출이 종료되고 제어 흐름이 서브클래스 constructor로 돌아온다. **이때 super가 반환한 인스턴스가 this에 바인딩된다. 서브클래스는 별도의 인스턴스를 생성하지 않고 super가 반환한 인스턴스를 this에 바인딩하여 그대로 사용한다.**
    
    ```jsx
    class ColorRectangle extends Rectangle {
      constructor(width, height, color) {
        super(width, height);
    
        // super가 반환한 인스턴스가 this에 바인딩된다.
        console.log(this);	// ColorRectangle {width: 2, height: 4}
    ...
    ```
    
    **이처럼 super가 호출되지 않으면 인스턴스가 생성되지 않으며, this 바인딩도 할 수 없다. 서브클래스의 constructor에서 super를 호출하기 전에는 this를 참조할 수 없는 이유가 바로 이 때문이다.** 따라서 서브클래스 constructor 내부의 인스턴스 초기화는 반드시 super 호출 이후에 처리되어야 한다.
    
5. **서브클래스의 인스턴스 초기화**
    
    super 호출 이후, 서브클래스의 constructor에 기술되어 있는 인스턴스 초기화가 실행된다. 즉, this에 바인딩되어 있는 인스턴스에 프로퍼티를 추가하고 constructor가 인수로 전달받은 초기값으로 인스턴스의 프로퍼티를 초기화한다.
    
6. **인스턴스 반환**
    
    클래스의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
    
    ```jsx
    class ColorRectangle extends Rectangle {
      constructor(width, height, color) {
        super(width, height);
    
        // super가 반환한 인스턴스가 this에 바인딩된다.
        console.log(this);	// ColorRectangle {width: 2, height: 4}
    
        // 인스턴스 초기화
        this.color = color;
    
        // 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
        console.log(this);	// ColorRectangle {width: 2, height: 4, color: "red"}
      }
    ...
    ```
    

### 25.8.7 표준 빌트인 생성자 함수 확장

- String, Number, Array 같은 표준 빌트인 객체도 [[Construct]] 내부 메서드를 갖는 생성자 함수이므로 extends 키워드를 사용하여 확장할 수 있다.
    
    ```jsx
    // Array 생성자 함수를 상속받아 확장한 MyArray
    class MyArray extends Array {
        // 중복된 배열 요소를 제거하고 반환한다: [1, 1, 2, 3] => [1, 2, 3]
        uniq() {
            return this.filter((v, i, self) => self.indexOf(v) === i);
        }
    
        // 모든 배열 요소의 평균을 구한다: [1, 2, 3] => 2
        average() {
            return this.reduce((per, cur) => pre + cur, 0) / this.length;
        }
    }
    
    const myArray = new MyArray(1, 1, 2, 3);
    console.log(myArray);   // MyArray(4) [1, 1, 2, 3]
    
    // MyArray.prototype.uniq 호출
    console.log(myArray.uniq());    // MyArray(3) [1, 2, 3]
    // MyArray.prototype.average 호출
    console.log(myArray.average()); // 1.75
    ```
    
    - Array 생성자 함수를 상속받아 확장한 MyArray 클래스가 생성한 인스턴스 Array.prototype과 MyArray.prototype의 모든 메서드를 사용할 수 있다.
    - 이때 주의할 것은 Array.prototype의 메서드 중에서 map, filter와 같이 새로운 배열을 반환하는 메서드가 MyArray 클래스의 인스턴스를 반환한다는 것이다.
        
        ```jsx
        console.log(myArray.filter(v => v % 2) instanceof MyArray);	// true
        ```
        
    - 만약 새로운 배열을 반환하는 메서드가 MyArray 클래스의 인스턴스를 반환하지 않고 Array의 인스턴스를 반환하면 MyArray 클래스의 메서드와 메서드 체이닝(method chaining)이 불가능하다.
        
        ```jsx
        // 메서드 체이닝
        // [1, 1, 2, 3] => [ 1, 1, 3 ] => [ 1, 3 ] => 2
        console.log(myArray.filter(v => v % 2).uniq().average());	// 2
        ```
        
    - myArray.filter가 반환하는 인스턴스는 MyArray 클래스가 생성한 인스턴스로 uniq 메서드를 연이어 호출(메서드 체이닝)할 수 있다.
    - uniq 메서드가 반환하는 인스턴스는 Array.prototype.filter에 의해 생성되었기 때문에 Array 생성자 함수가 생성한 인스턴스로 생각할 수도 있겠다. 하지만 uniq 메서드가 반환하는 인스턴스도 MyArray 타입이다. 따라서 uniq 메서드가 반환하는 인스턴스로 average 메서드를 연이어 호출(메서드 체이닝)할 수 있다.
    - 만약 MyArray 클래스의 uniq 메서드가 MyArray 클래스가 생성한 인스턴스가 아닌 Array가 생성한 인스턴스를 반환하게 하려면 다음과 같이 Symbol.species를 사용하여 정적 접근자 프로퍼티를 추가한다.
        
        ```jsx
        // Array 생성자 함수를 상속받아 확장한 MyArray
        class MyArray extends Array {
          	// 모든 메서드가 Array 타입의 인스턴스를 반환하도록 한다.
          	static get [Symbol.species]() { return Array; }
        
            // 중복된 배열 요소를 제거하고 반환한다: [1, 1, 2, 3] => [1, 2, 3]
            uniq() {
                return this.filter((v, i, self) => self.indexOf(v) === i);
            }
        
            // 모든 배열 요소의 평균을 구한다: [1, 2, 3] => 2
            average() {
                return this.reduce((per, cur) => pre + cur, 0) / this.length;
            }
        }
        
        const myArray = new MyArray(1, 1, 2, 3);
        
        console.log(myArray.uniq() instanceof MyArray);	// false
        console.log(myArray.uniq() instanceof Array);	// true
        
        // 메서드 체이닝
        // uniq 메서드는 Array 인스턴스를 반환하므로 average 메서드를 호출할 수 없다.
        console.log(myArray.uniq().average());
        // TypeError: myArray.uniq( ... ).average is not a function
        ```
