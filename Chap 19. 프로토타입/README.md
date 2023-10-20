## 객체 지향 프로그래밍

> 프로그램을 명령어 또는 함수의 목록으로 보는 전통적인 명령형 프로그래밍의 절차지향적 관점에서 벗어나 **여러개의 독립적인 단위, 즉 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임**
> 

- 실세계의 실체를 인식하는 철학적 사고를 프로그래밍에 접목하려는 시도에서 시작했다.
- 실체는 특징이나 성질을 나타내는 **속성**을 지닌다.
    - 사람의 경우 이름, 주소, 성별, 나이 등
- 다양한 속성 중에서 프로그램에 필요한 속성만 간추려 표현하는 것을 **추상화**라고 한다.
- 속성을 통해 여러 개의 값을 하나의 단위로 구성한 복합적인 자료구조를 **객체**라고 한다.
    - 객체의 **상태**를 나타내는 데이터
    - 상태 데이터를 조작할 수 있는 **동작**
- **객체지향 프로그래밍**은 독립적인 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임이다.
- 각 객체는 고유의 기능을 갖는 독립적인 부품으로 볼 수 있지만, 자신의 고유한 기능을 수행하면서 다른 객체와 관계성을 가질 수 있다.

## 상속과 프로토타입

- **상속**이란, **어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것**을 말한다.
    - 자바스크립트는 프로토타입을 기반으로 상속을 구현하여 불필요한 중복을 제거한다.

- 생성자 함수는 동일한 프로퍼티(메서드 포함) 구조를 갖는 객체를 여러개 생성할 때 유용하다. 하지만 아래 예제의 생성자 함수는 문제가 있다.
    
    ```jsx
    function Circle(radius) {
      this.radius = radius;
      this.getArea = function () {
        return Math.PI * this.radius ** 2;
      };
    }
    
    const circle1 = new Circle(1);
    const circle2 = new Circle(2);
    
    console.log(circle1.getArea === circle2.getArea);	// false
    console.log(circle1.getArea()); // 3.14159...
    console.log(circle2.getArea());	// 12.56637...
    ```
    
    - radius 프로퍼티 값은 일반적으로 인스턴스마다 다르다.
    - 그러나 getArea 메서드는 모든 인스턴스가 동일한 내용의 메서드를 사용하므로 
    단 하나만 생성하여 모든 인스턴스가 공유해서 사용하는 것이 바람직하다.
    - 그런데 Circle 생성자 함수는 매번 인스턴스를 생성할 때마다 getArea 메서드를 중복 생성하고 모든 인스턴스가 중복 소유한다. → 메모리의 불필요한 낭비
        
        ex) 만약 10개의 인스턴스를 생성하면 내용이 동일한 메서드도 10개 생성된다.
        
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/d9e05c48-0a8e-403d-b6eb-eed99682651f/Untitled.png)
    
- 자바스크립트는 **프로토타입을 기반으로 상속을 구현**할 수 있다.
    
    ```jsx
    function Circle(radius) {
      this.radius = radius; // 개별 소유
    }
    // 모든 인스턴스가 getArea 메서드를 사용할 수 있도록 프로토타입에 추가한다.
    // 프로토타입은 Circle 생성자 함수의 prototype 프로퍼티에 바인딩되어 있다.
    Circle.prototype.getArea = function () { // 공동 소유
      return Math.PI * this.radius ** 2;
    };
    
    const circle1 = new Circle(1);
    const circle2 = new Circle(2);
    
    // **Circle 생성자 함수가 생성하는 모든 인스턴스는 하나의 getArea 메서드를 공유**한다.
    console.log(circle1.getArea === circle2.getArea);	// true
    console.log(circle1.getArea()); // 3.14159...
    console.log(circle2.getArea());	// 12.56637...
    ```
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/86b4cf7c-21eb-4e0f-9729-ef417097a0d4/Untitled.png)
    
    - **Circle 생성자 함수가 생성한 모든 인스턴스는 자신의 프로토타입, 즉 상위(부모) 객체 역할을 하는 Circle.prototype의 모든 프로퍼티와 메서드를 상속받는다.**
    - getArea 메서드는 **단 하나만 생성**되어 프로토타입이 Circle.prototype 의 메서드로 할당되어 있다.
    
    ⇒ 모든 인스턴스가 공통으로 사용할 프로퍼티나 메서드는 프로토타입에 미리 구현해두면 재사용하기 유용하다!
    
    ## 프로토타입 객체
    
    - 프로토타입 객체란 객체 간 상속을 구현하기 위해 사용된다.
    - 프로토타입은 어떤 객체의 상위(부모) 객체의 역할을 하는 객체로서 다른 객체에 공유 프로퍼티(메서드 포함)를 제공한다.
    - **모든 객체는 [[prototype]] 이라는 슬롯을 가지며, 이 내부 슬롯의 값은 프로토타입의 참**조이다.
        
        ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/06141f05-6211-4e16-846e-05696f8bf80b/Untitled.png)
        
    
    - 저장되는 프로토타입은 객체 생성 방식에 의해 결정된다.
        - 객체 리터럴에 의하면 Object.prototype
        - 생성자 함수에 의해 생성된 객체의 프로토타입은 생성자 함수의 prototype  프로퍼티에 바인딩되어 있는 객체이다.

### 19.3.1 __ proto __   접근자 프로퍼티

모든 객체는  __proto__ 접근자 프로퍼티를 통해 자신의 프로토타입, 즉 [[prototype]] 내부 슬롯에 간접적으로 접근할 수 있다.

> **__proto__  는 *접근자 프로퍼티이다.**
> 

__proto__ **는** [[Get]] [[Set]] 등의 접근자 함수를 통해 내부 값을 불러오고 새로운 값을 할당한다.

*자체적으로는 값을 가지지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수 

즉, [[Get]] [[Set]] 으로 구성된 프로퍼티이다.

> **__proto__  접근자 프로퍼티는 상속을 통해 사용된다.**
> 

- 객체가 직접 소유하는 프로퍼티가 아닌, Object.prototype의 프로퍼티이다.

```jsx
const person = { name: 'LEE' };
// 객체는 __proto__ 프로퍼티를 소유하지 않는다.
console.log(person.hasOwnProperty('__proto__'));	// false

// **__proto__ 프로퍼티는 Object.prototype의 접근자 프로퍼티다.**
conosle.log(Object.getOwnPropertyDescriptor(Object.prototype, '__proto__'));
// {get: f, set: f, enumerable: false, configurable: true}

// **모든 객체는 Object.prototype의 __proto__를 상속받아 사용할 수 있다.**
console.log({}.__proto__ === Object.prototype);	// true
```

<aside>
📌 **Object.prototype**

모든 객체는 프로토타입 계층 구조인 **프로토타입 체인**에 묶여있다. 
해당 객체에 현재 접근하려는 프로퍼티가 없다면 __proto__ 접근자 프로퍼티가 가리키는 참조를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색한다.
프로토타입 체인의 종점, **최상위 객체는 Object.prototype**이다 .

</aside>

> **__proto__  접근자 프로퍼티를 통해 프로토타입에 접근하는 이유**
> 

```jsx
const parert = {};
const child = {};

// child의 프로토타입을 parent로 설정
child.__proto__ = parent;
// parent 프로토타입을 child로 설정
parent.__proto__ = child;	// TypeError: Cyclic __proto__ value
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/8fe0322e-cc1c-4170-bb2b-002b3ff2edd6/Untitled.png)

⇒ 상호 참조에 의해 프로토타입 체인이 생성되는 것을 막기 위해서! 

⇒ **아무런 체크 없이 무조건적으로 프로토타입이 교체되는 것을 막도록** 검사 후에 교체하도록 접근자 프로퍼티를 활용한다! 

> **__proto__  접근자 프로퍼티를 코드 내에서 직접 사용하는 것은 권장하지 않는다.**
> 

- 모든 브라우저에서 사용할 수 있는 것이 아니다.
- 그래서 __proto__ 보다 **Object.get(set)PrototypeOf** 메서드를 사용하는 편이 좋다.

```jsx
// obj는 직접 상속을 받았으므로 프로토타입 체인의 종점이다.
const obj = Object.create(null);
// 그러므로 Object.__proto__를 상속받을 수 없다.
console.log(obj.__proto__);		// undefined
// 그래서 __proto__ 보다 Object.getPrototypeOf 메서드를 사용하는 편이 좋다.
console.log(Object.getPrototypeOf(obj));	// null
```

### 함수 객체의 prototype 프로퍼티

- **함수 객체만이 소유하는 prototype 프로퍼티**는 **생성자 함수가 생성할 인스턴스의 프로토타입**을 가리킨다.
- non-constructor 인 화살표함수와 ES6 메서드 축약 표현으로 정의한 메서드는 prototype 프로퍼티를 소유하지 않으며 프로토타입도 생성하지 않는다.
- 그러나 생성자 함수로 호출하기 위해 정의하지 않은 일반 함수(함수 선언문, 함수 표현식)도 prototype 프로퍼티를 소유하지만 **객체를 생성하지 않는 일반 함수의 prototype 프로퍼티는 아무런 의미가 없다.**

⇒ 모든 객체가 가지고 있는 __proto__ 접근자 프로퍼티와 함수 객체만이 가지고 있는 prototype 프로퍼티는 결국 동일한 프로토타입을 가리킨다. 그러나 프로퍼티를 사용하는 주체가 다르다.

| 구분 | 소유 | 값 | 사용 주체 | 사용 목적 |
| --- | --- | --- | --- | --- |
| __proto__ 접근자 프로퍼티 | 모든 객체 | 프로토타입의 참조 | 모든 객체 | 객체(인스턴스)가 자신의 프로토타입에 접근 또는 교체하기 위해 사용 |
| prototype 프로퍼티 | constructor | 프로토타입의 참조 | 생성자 함수 | 생성자 함수가 자신이 생성할 객체(인스턴스)의 프로토타입을 할당하기 위해 사용 |

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/473f8991-4c44-4188-a842-e9e1a9043469/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/3dd30b5d-2e68-4a16-8183-a938256ca13f/Untitled.png)

### 프로토타입의 constructor 프로퍼티와 생성자 함수

- **모든 프로토타입은 constructor 프로퍼티를 갖는다.**
- constructor 프로퍼티는 prototype 의 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킨다.
- 이 연결은 생성자 함수가 생성될 때, 즉 함수 객체가 생성될 때 이루어진다.

```jsx
function Person(name){
	this.name = name;

}

const me = new Person('Lee');

console.log(me.constructor === Person
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/bb7a733d-d282-467e-806c-d1f2b61062d7/Untitled.png)

## 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

- 리터럴 표기법에 의해 생성된 객체도 물론 프로토타입이 존재한다.
- 그러나 이경우 반드시 constructor 프로퍼티가 가리키는 생성자 함수가 반드시 객체를 생성한 생성자 함수라고 단정할 수는 없다.

```jsx
const obj = {};

console.log(obj.constructor === Object ); // true 
```

→ 객체 리터럴에 의해 생성된 객체는 사실 내부적으로 Object  생성자 함수로 생성되는 것일까?

→ 아니다! 그러나 리터럴 표기법에 의해 생성된 객체도 상속을 위해 프로토타입이 필요하다. 따라서 가상적인 생성자 함수를 갖는다.

⇒ **프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재한다.** 

큰 틀에서 생각해보면 리터럴 표기법으로 생성한 객체도 생성자 함수로 생성한 객체와 본질적인 면에서 큰 차이는 없다. 따라서 프로토타입의 constructor 프로퍼티를 통해 연결되어 있는 생성자 함수를 리터럴 표기법으로 생성한 객체를 생성자 함수로 생각해도 큰 무리는 없다.

| 리터럴 표기법 | 생성자 함수 | 프로토 타입 |
| --- | --- | --- |
| 객체 리터럴 | Object | Object.prototype |
| 함수 리터럴 | Function | Function.prototype |
| 배열 리터럴 | Array | Array.prototype |
| 정규 표현식 리터럴 | RegExp | RegExp.prototype |

<aside>
📌 **[[prototype]] 과 __proto__ 와 prototype** 

[[prototype]]  : 프로토타입의 참조를 저장하는 내부 슬롯
__proto__ :  프로토타입에 접근할 수 있게 해주는 접근자 프로퍼티
prototype : 함수 객체만이 소유. 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다. 
****

- **모든 객체는 [[prototype]] 이라는 슬롯을 가지며, 이 내부 슬롯의 값은 프로토타입의 참**조이다.
- 원칙적으로는 직접 접근할 수 없지만 **__proto__ 를 통해 간접적으로 접근**할 수 있다.
- 모든 객체는  **__proto__ 접근자 프로퍼티**를 통해 자신의 프로토타입, 즉 [[prototype]] 내부 슬롯에 간접적으로 접근할 수 있다.
- **함수 객체만이 소유하는 prototype 프로퍼티**는 **생성자 함수가 생성할 인스턴스의 프로토타입**을 가리킨다.
</aside>

## 프로토타입의 생성 시점

- 프로토타입은 **생성자 함수가 생성되는 시점**에 더불어 생성된다.

<aside>
📌 **Object.create 메서드와 클래스에 의한 객체 생성**

Object.create 메서드와 클래스로 생성한 객체도 생성자 함수와 연결되어 있다.

</aside>

### 사용자 정의 생성자 함수와 프로토타입 생성 시점

- 일반 함수로 정의한 함수 객체( 내부 메서드 [[Constructor]] 를 갖는 함수 객체 )는 new 연산자와 함께 생성자 함수로서 호출할 수 있다. ( non-constructor 는 프로토타입이 생성되지 않는다. )
- constructor 는 **함수 정의가 평가되어 함수 객체를 생성하는 시점**에 프로토타입도 더불어 생성된다.
    - ❓ 함수 선언문은 런타임 이전에 함수 객체가 생성되지만, 함수 표현식을 통한 생성자 함수로 객체를 생성하게되면 프로토타입 생성 시점이 어떻게 되나?
        
        함수 표현식을 통해 생성자 함수로 객체를 생성하면 프로토타입은 런타임에 해당 표현식을 맞닥뜨리고 평가가 진행될 때 생성!
        
        ```jsx
        //print(Person.prototype); error -> 아직 함수 객체가 생성되지 않은 시점! (함수 표현식은 함수 객체생성 및 할당까지 호이스팅 x)
        
        const Person = function(name, age) {
          this.name = name;
          this.age = age;
        };
        
        const person1 = new Person('John', 30);
        const person2 = new Person('Jane', 25);
        
        print(Person.prototype);
        
        ```
        
    
    ```tsx
    // 함수 정의(constructor)가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.
    console.log(Person.prototype); // {constructor: ƒ} -> 함수 호이스팅
    
    // 생성자 함수
    function Person(name) {
      this.name = name;
    }
    ```
    
    ⇒ 생성된 프로토타입은 Person 생성자 함수의 prototype 프로퍼티에 바인딩된다. 
    
- 처음 생성되는 프로토타입은 오직 constructor 프로퍼티만을 갖는 객체이다.
- 프로토타입도 객체이고 모든 객체는 프로토타입을 가지므로 **프로토타입도 자신의 프로토타입을 갖는다.**
- **생성된 프로토타입의 프로토타입은 Object.prototype** 이다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/3dd30b5d-2e68-4a16-8183-a938256ca13f/Untitled.png)
    

### 빌트인 생성자 함수와 프로토타입 생성 시점

- 모든 빌트인 생성자 함수는 **전역 객체가 생성되는 시점**에 생성된다.
- 즉, 객체 생성 이전에 생성자 함수와 프로토타입이 이미 객체화되어 존재한다.
- 이후 생성자 함수 또는 리터럴 표기법으로 객체를 생성하면 프로토타입은 **생성된 객체의 [[prototype]] 내부 슬롯에 할당**된다.

## 객체 생성 방식과 프로토타입의 결정

- 다양한 객체 생성 방식
    - 객체 리터럴
    - Object 생성자 함수
    - 생성자 함수
    - Object.create 메서드
    - 클래스

⇒ 추상 연산 OrdinaryObjectCreate 에 의해 생성된다는 공통점!

<aside>
📌 **추상 연산 OrdinaryObjectCreate** 

필수적으로 자신이 **생성할 객체의 프로토타입**을 인수로 전달 받는다.
⇒ 추상 연산에 전달되는 인수에 의해 결정된다. 이 인수는 객체가 생성되는 시점에 **객체 생성 방식에 의해 결정**된다.

</aside>

### 객체 리터럴에 의해 생성된 객체의 프로토타입

⇒ Object.prototype 

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/264517c2-d033-4378-991c-51dbe0405b00/Untitled.png)

### Object 생성자함수에 의해 생성된 객체의 프로토타입

⇒ Object.prototype 

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/e25e1ad2-a025-4f5a-8fde-97fefc96264d/Untitled.png)

### 생성자함수에 의해 생성된 객체의 프로토타입

⇒ 생성자 함수의 prototype 에 바인딩 되어있는 객체

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/b036a540-504d-4156-956c-612d629f01cc/Untitled.png)

- Person 생성자 함수를 통해 생성된 모든 객체는 프로토타입에 추가된 sayHello 메서드를 상속받아 사용할 수 있다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/532294c1-26cf-4a94-b09c-c58e123811c8/Untitled.png)

<aside>
📌  **Object.prototype 과 사용자 정의 생성자 함수의 프로토타입**

생성된 프로토 타입 object.prototype은 다양한 빌트인 메서드를 갖고 있다. 하지만 사용자 정의 생성자 함수 Person과 더불어 생성된 프로토타입 Person.prototype의 프로퍼티는 constructor 뿐이다.

</aside>

## 프로토타입 체인

- 자바스크립트는 객체의 프로퍼티(메서드 포함)에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면 [[Prototype]] 내부 슬롯의 참조를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색한다.
- 프로토타입 체인은 상속과 프로퍼티 검색을 위한 매커니즘이다.
- 이때, 호출한 메서드의 this 에는 자신을 호출한 객체가 바인딩된다.
    
    > Object.prototype.hasOwnProperty.call(me,’name’);
    > 
- Object.prototype은 모든 프로토타입 체인의 종점이다. Object.prototype의 [[prototype]] 내부 슬롯의 값은 null이다.
- 만일 모든 프로토타입체인에서 프로퍼티를 검색하지 못하면 undefined 를 반환한다.
- 스코프 체인과 프로토타입 체인은 서로 협력하여 식별자와 프로퍼티를 검색하는 데 사용된다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/041affe3-866c-4428-9c08-a9de36ba03e1/Untitled.png)

## 오버라이딩과 프로퍼티 섀도잉

- 프로토타입이 소유한 프로퍼티 → 프로토타입 프로퍼티
- 인스턴스가 소유한 프로퍼티 → 인스턴스 프로퍼티
- 프로토타입 프로퍼티와 같은 이름의 프로퍼티를 인스턴스에 추가하면 프로토타입 프로퍼티를 덮어쓰는 것이 아니라 인스턴스의 프로퍼티로 추가한다.
- 인스턴스 메서드는 프로토타입 메서드를 *오버라이딩 한다. 상속관계에 의해 프로퍼티가 가려지는 현상 ⇒ **프로퍼티 섀도잉**
    
    *상위 클래스가 가지고 있는 메서드를 하위 클래스가 재정의하여 사용하는 방식이다.
    
- 하위 객체를 통해 상위 프로토타입의 프로퍼티를 변경 또는 삭제하는 것은 불가능하다. 하위 객체를 통해 프로토타입에 get 액세스는 허용되나 set 액세스는 허용되지 않는다.
    
    ⇒ 프로토타입 프로퍼티를 변경 또는 삭제하려면 프로토타입에 직접 접근해야한다. 
    

## 프로토타입의 교체

- 프로토타입은 임의의 다른 객체로 변경할 수 있다. ⇒ 객체 간의 상속 관계를 동적으로 변경할 수 있다.
- 프로토타입은 생성자 함수 또는 인스턴스에 의해 교체할 수 있다.

### 생성자 함수에 의한 프로로타입의 교체

```jsx
// Person이라는 생성자 함수가 있다고 가정.

Person.prototype = {
  sayHello() {
    console.log(`Hi! My name is ${this.name}`);
  }
};

const me = new Person('Lee');

// 프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다
console.log(me.constructor === Person); // false 

// 프로토타입 체인을 따라 Object.prototype의 constructor 프로퍼티가 검색된다.
console.log(me.constructor === Object); // true
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/2bd6d074-9a10-4763-a396-8e24c7dc47e0/Untitled.png)

### 인스턴스에 의한 프로로타입의 교체

- 인스턴스의 **__proto__** 접근자 프로퍼티를 통해 프로토타입을 교체할 수 있다.
- 이 경우, 이미 생성된 객체의 프로토타입을 교체한다.

```jsx
function Person(name) {
  this.name = name;
}
const me = new Person('Cho');
const parent = {
  sayHello() {
    console.log(`Hi! My name is ${this.name}`);
  }
};
// me 객체의 프로토타입을 parent로 교체한다.
Object.setPrototypeOf(me, parent);
// 위 코드는 아래의 코드와 동일하게 동작한다.
me.__proto__ = parent;

// 프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다
console.log(me.constructor === Person); // false 

// 프로토타입 체인을 따라 Object.prototype의 constructor 프로퍼티가 검색된다.
console.log(me.constructor === Object); // true
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/6f2509e1-7249-4a8b-bf7d-e8681933b794/Untitled.png)

<aside>
📌 **생성자 함수에 의한 프로토타입 교체 vs 인스턴스에 의한 프로토타입 교체**

- 생성자 함수에 의한 프로로타입의 교체 : 미래에 생성할 인스턴스의 프로토타입을 교체
- 인스턴스에 의한 프로토타입 교체  : 이미 생성된 인스턴스의 프로토타입을 교체

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/d514ac0b-290a-4243-aea2-bdd9dd71827a/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/aeb010c4-bd7e-4047-95f0-cbd37b27ae39/Untitled.png)

</aside>

<aside>
📌 **프로토타입으로 교체한 객체 리터럴에 파괴된 생성자 함수와 프로토타입 간의 연결을 되살리기**

```jsx
const Person = (function () {
  function Person(name) {
    this.name = name;
  }

  // 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
  Person.prototype = {
    // constructor 프로퍼티와 생성자 함수 간의 연결을 설정
    constructor: Person,
    sayHello() {
      console.log(`Hi! My name is ${this.name}`);
    }
  };

  return Person;
}());

const me = new Person('Lee');

// constructor 프로퍼티가 생성자 함수를 가리킨다.
console.log(me.constructor === Person); // true
console.log(me.constructor === Object); // false
```

- 프로토타입 교체를 통해 객체 간의 상속 관계를 동적으로 변경하는 것은 꽤나 번거롭다.

**⇒ 프로토타입은 직접 교체하지 않는 것이 좋다.**

</aside>

## instanceof 연산자

> **객체 instanceof 생성자함수**
: 우변의 생성자 함수의 prototype에 바인딩된 객체가 좌변의 객체의 프로토타입 체인 상에 존재하면 true
> 

```jsx
function Person(name) {
  this.name = name;
}
const me = new Person('Cho');
const parent = {
  sayHello() {
    console.log(`Hi! My name is ${this.name}`);
  }
};
// me 객체의 프로토타입을 parent로 교체한다.
Object.setPrototypeOf(me, parent);

// Person 생성자 함수와 parent 객체는 연결되어 있지 않다.
console.log(Person.prototype === parent); // false
console.log(Person.constructor === Person ); // false 

console.log(me instanceof Person ); // false 
console.log(me instanceof Object ); // true 

Person.prototype = parent;  // 직접 연결하기

⇒ 프로토타입으로 교체한  parent 객체를 Person 생성자 함수의 prototype 프로퍼티에 바인딩하면  me instanceof Person 은 true 로 평가 될 것이다. 

console.log(me instanceof Person ); // true 
console.log(me instanceof Object ); // true 

```

⇒ 이처럼 instanceof 연산자는 프로토타입의 constructor 프로퍼티가 가리키는 생성자 함수를 찾는 것이 아니라, **생성자 함수의 prototype 에 바인딩 된 객체가 프로토타입 체인 상에 존재하는지 확인한다.**

**결과**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/7cd89f89-5cb6-4ba7-ab50-fe11c10efb70/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/66320b95-9549-4c4e-bb30-60ea85c4c27e/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/d514ac0b-290a-4243-aea2-bdd9dd71827a/Untitled.png)

⇒ 생성자 함수에 의해 프로토타입이 교체되어 constructor  프로퍼티와 생성자 함수 간의 연결이 파괴되어도 **생성자 함수의 prototype 프로퍼티와 프로토타입 간의 연결은 파괴되지 않으므로** instanceof 는 아무런 영향을 받지 않는다. 

## 직접 상속

### Object.create 에 의한 직접 상속

: 명시적으로 프로토타입을 지정하여 새로운 객체를 생성하는 방법

> **Object.create(프로토타입으로 지정할 객체, 생성할 객체의 프로퍼티키와 프로퍼티 디스크립터 객체로 이루어진 객체)**
> 
> 
> ```
> /**
> * 지정된 프로토타입 및 프로퍼티를 갖는 새로운 객체를 생성하여 반환한다.
> * @param {Object} prototype - 생성할 객체의 프로토타입으로 지정할 객체
> * @param {Object} [propertiesObject] - 생성할 객체의 프로퍼티를 갖는 객체
> * @returns {Object} 지정된 프로토타입 및 프로퍼티를 갖는 새로운 객체
> */
> Object.create(prototype[, propertiesObject])
> ```
> 

```jsx
// 프로토타입이 null인 객체를 생성한다. 생성된 객체는 프로토타입 체인의 종점에 위치한다.
// obj -> null
let obj = Object.create(null);
console.log(Object.getPrototypeOf(obj) === null);
// true

**// Object.create(null); 은 프로토타입 체인의 종점에 위치하며 Object.prototype 을 상속받지 못한다.**
Object.create(null);
console.log(obj.toString());
// TypeError: obj.toString is not a function

// obj -> Object.prototype => null
// obj = {};와 동일
obj = Object.create(Object.prototype);
console.log(Object.getPrototypeOf(obj) === Object.prototype);
// true

// obj -> Object.prototype -> null
// obj = { x: 1 };와 동일
obj = Object.create(Object.prototype, {
   x: {value: 1, writable: true, enumerable: true, configurable: true}
});
// 위 코드는 아래와 동일하다.
// obj = Object.create(Object.prototype);
// obj.x = 1;
console.log(obj.x);
// 1
console.log(Object.getPrototypeOf(obj) === Object.prototype);
// true

const myProto = { x: 10 };
// 임의의 객체를 직접 상속받는다.
// obj -> myProto -> Object.prototype -> null
obj = Object.create(myProto);
console.log(Object.getPrototypeOf(obj) === myProto);
// true

// 생성자 함수
function Person(name) {
   this.name = name;
}
// obj -> Person.prototype -> Object.prototype -> null
// obj = new Person('Lee')와 동일
obj = Object.create(Person.prototype);
obj.name = 'Lee';
console.log(obj.name);
// Lee
console.log(Object.getPrototypeOf(obj) === Person.prototype);
// true
```

<aside>
📌 **Object.create 메서드의 장점**

- new 연산자가 없이도 객체를 생성할 수 있다.
- 프로토타입을 지정하면서 객체를 생성할 수 있다.
- 객체 리터럴에 의해 생성된 객체도 상속받을 수 있다.
</aside>

- **Object.prototype 의 빌트인 메서드를 객체가 직접 호출하는 것을 권장하지 않는다.**
- 왜냐하면 Object.create 메서드를 통해 프로토타입 체인의 종점에 위치하는 객체를 생성할 수 있기 때문이다.
- 따라서 에러를 발생시킬 위험을 없애기 위해 아래처럼 ****간접적으로 호출하는 것이 좋다.

```jsx
 Object.prototype.hasOwnProperty.call(obj,’a’); // obj 에 a 라는 프로퍼티가 있는지
```

### 객체 리터럴 내부에서 **__proto__ 에 의한 직접 상속**

```jsx
const myProto = { x: 10 };

// 객체 리터럴에 의해 객체를 생성하면서 프로토타입을 지정하여 직접 상속받을 수 있다.
const obj = {
  y: 20,
  // 객체를 직접 상속받는다.
  // obj → myProto → Object.prototype → null
  __proto__: myProto
};
/* 위 코드는 아래와 동일하다.
const obj = Object.create(myProto, {
  y: { value: 20, writable: true, enumerable: true, configurable: true }
});
*/

console.log(obj.x, obj.y); // 10 20
console.log(Object.getPrototypeOf(obj) === myProto); // true
```

## 정적 프로퍼티/메서드

: 인스턴스를 생성하지 않아도 참조/호출할 수 있는 프로퍼티/메서드

⇒ 생성자 함수 또한 객체이므로 그 자체로 자신의 프로퍼티/메서드를 소유할 수 있다. ⇒ **정적 프로퍼티/메서드**

<aside>
📌 **정적 프로퍼티/메서드 VS 프로토타입 프로퍼티/메서드**

정적 프로퍼티/메서드 → 생성자 함수 자체에서 호출할 수 있는 메서드
프로토타입 프로퍼티/메서드 → 해당 프로토타입으로 만들어진 객체(인스턴스)에서만 호출할 수 있는 메서드

</aside>

```jsx
// 생성자 함수
function Person(name) {
   this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
   console.log(`Hi! My name is ${this.name}`);
};

// 정적 프로퍼티
Person.staticProp = 'static prop';

// 정적 메서드
Person.staticMethod = function () {
   console.log('staticMethod');
};

const me = new Person('Lee');

// 생성자 함수에 추가한 정적 프로퍼티/메서드는 생성자 함수로 호출한다.
Person.staticMethod(); // staticMethod

// **정적 프로퍼티/메서드는 생성자 함수가 생성한 인스턴스로 참조/호출할 수 없다.**
// 인스턴스로 참조/호출할 수 있는 프로퍼티/메서드는 **프로토타입 체인 상**에 존재해야한다. 
// => 생성자 함수에 있는 프로퍼티/메서드는 활용할 수 없음 
me.staticMethod(); // TypeError: me.staticMethod is not a function
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/c582da96-5a23-4cab-8b90-efe98f36f12a/Untitled.png)

[EX] 

Object.create ⇒ 생성자 함수의 정적 메서드 ⇒ 인스턴스로는 호출할 수 없음

Object.prototype.hasOwnProperty ⇒ Object.prototype 의 메서드 

⇒ 만약 인스턴스/프로토타입 메서드 내에서 this를 사용하지 않았다면 정적 메서드로 변경할 수 있다.

## 프로퍼티 존재 확인

:  객체 내에 특정 프로퍼티가 존재하는지 여부를 확인한다.

### in 연산자

> key **in** object
> 

- 단순히 확인 대상 객체뿐 아니라 **확인 대상 객체가 상속받는 모든 프로토타입의 프로퍼티를 확인**하므로 주의가 필요하다.
- Reflect.has(객체,프로퍼티) 도 동일하게 동작한다.

### Object.prototype.hasOwnProperty

- in 연산자와 다르게 **객체 고유의 프로퍼티 키인 경우에만 true** 를 반환한다. ( 부모까지 거슬러 올라가지 x )

## 프로퍼티 열거

### for … in 문

> for ( 변수선언문 in 객체 ) { … }
> 

```jsx
const person = {
  name: 'Lee',
  address: 'Seoul',
};
for(const key in person) { // 순회하며 프로퍼티 키를 할당 
  console.log(key + ': ' + person[key]);
}
// name: Lee
// address: Seoul
```

- in 연산자처럼 순회 대상 객체의 프로퍼티뿐만이 아니라 상속 받은 프로토타입의 프로퍼티까지 열거한다.
- 그러나 Object.prototype 의 프로퍼티가 열거되지 않는 이유는 [[Enumerable]] 의 값이 false 이기 때문이다.
- 프로퍼티 키가 심벌인 프로퍼티는 열거하지 않는다.
- 만일 객체 자신의 고유 프로퍼티만 열거하려면 `Object.prototype.hasOwnProperty` 와 결합해서 자신의 메서드인지 확인해야 한다.
- **열거할때 순서를 보장하지 않는다.**

**⇒  객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 [[Enumerable]] 의 값이  true 인 프로퍼티를 순회하며 열거한다.** 

⇒ 배열을 열거할 때에는 일반적인 for 문이나 **for…of** 문 또는 **Array.prototype.forEach** 메서드를 사용하기를 권장한다. 

! 사실 배열도 객체이므로 프로퍼티와 상속받은 프로퍼티를 가질 수 있다.

### Object.keys/values/entries 메서드

**객체 자신의 고유 프로퍼티만** 열거할때 유용!

- Object.keys 메서드객체 자신의 열거 가능한 프로퍼티 키를 배열로 반환한다.

```jsx
const person = {
  name: 'Lee',
  address: 'Seoul',
  \__proto\__: { age: 20 }
};
console.log(Object.keys(person)); // ["name", "address"]
```

- Object.values 메서드 (ES8)객체 자신의 열거 가능한 프로퍼티 값을 배열로 반환한다.

```jsx
console.log(Object.values(person)); // ["Lee", "Seoul"]
```

- Object.entries 메서드 (ES8)객체 자신의 열거 가능한 프로퍼티 키와 값의 쌍의 배열을 배열에 담아 반환한다.

```jsx
console.log(Object.entries(person));
// [["name", "Lee"], ["address", "Seoul"]]
```
