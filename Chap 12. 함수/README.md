## 12.1 함수란?

- 일련의 과정을 문으로 구현하고 코드 블록으로 감싸서 하나의 실행 단위로 정의한 것
- 함수 정의

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/98e671d6-6b90-42b2-9ed7-00bc879b12b1/Untitled.png)

- 함수 호출

```jsx
// 함수 add에 인수 2, 5를 전달하면서 호출하면 반환값 7을 반환한다.
var result = add(2, 5);

console.log(result); // 7
```

## 12.2 함수를 사용하는 이유

- 동일한 작업을 반복적으로 수행해야한다면 미리 정의된 함수를 재사용하는 것이 효율적
- 코드의 재사용성, 유지보수의 편의성을 높이고 코드의 신뢰성, 코드의 가독성을 높이는 효과가 있다.

## 12.3 함수 리터럴

- **자바스크립트의 함수는 객체 타입의 값이다. → 다른 프로그래밍 언어와 구분되는 자바스크립트의 중요한 특징!**
    - 일반 객체는 호출할 수 없지만 함수는 호출할 수 있다.
    - 일반 객체에는 없는 함수 객체만의 고유한 프로퍼티를 갖는다.
- 함수 리터럴의 구성요소는 다음과 같다.

| 구성요소 | 설명 |
| --- | --- |
| 함수 이름 | • 함수 이름은 식별자다. 따라서 식별자 네이밍 규칙을 준수해야 한다.
• 함수 이름은 함수 몸체 내에서만 참조할 수 있는 식별자다.
• 함수 이름은 생략할 수 있다. 이름이 있는 함수를 기명 함수, 이름이 없는 함수를 무명/익명 함수라 한다. |
| 매개변수 목록 | • 0개 이상의 매개변수를 소괄호로 감싸고 쉼표로 구분한다.
• 각 매개변수에는 함수를 호출할때 지정한 인수가 순서대로 할당된다.
• 매개변수는 함수 몸체 내에 변수와 동일하게 취급된다. 따라서 매개변수도 식별자 네이밍 규칙을 준수해야한다. |
| 함수 몸체 | - 함수가 호출되었을 때 일괄적으로 실행될 문들을 하나의 실행 단위로 정의한 코드 블록이다.
- 함수 몸체는 함수 호출에 의해 실행된다. |

## 12.4 함수 정의

- 함수를 정의하는 방법에는 4가지가 있다.
- 모든 함수 정의 방식은 정의한다는 면에서는 동일하지만, 미묘하지만 중요한 차이가 있다.
    - 함수 선언문
    - 함수 표현식
    - Function 생성자 함수
    - 화살표 함수(ES6)

### 12.4.1 함수 선언문

- 함수 리터럴은 함수 이름을 생략할 수 있으나 함수 선언문은 함수 이름을 생략할 수 없다.
- 함수 선언문은 표현식이 아닌 문이다.
- 그러나 아래 예시는 함수 선언문이 변수에 할당되는 것처럼 보인다.
    
    ```jsx
    var add = function add(x,y) {
      retrun x + y;
    };
    
    console.log(add(2,5)); // 7
    ```
    
    - 그 이유는 코드의 문맥에 따라 동일한 함수 리터럴을 **1. 표현식이 아닌 문인 함수 선언문으로 해석하는 경우**와 **2. 함수 리터럴 표현식으로 해석하는 경우**가 있기 때문이다.
    - 함수 선언문은 함수 리터럴과 형태가 동일하다. (중의적 표현)
        - 함수 이름이 있는 기명 함수 리터럴은 함수 선언문 또는 함수 리터럴 표현식으로 해석될 가능성이 있다.
        - 선언문은 값으로 평가될 수 없지만, 리터럴은 값으로 평가될 수 있다.
        - 값으로 평가되어야 하는 문맥이면 함수 리터럴 표현식으로 해석한다.
        - 동일한 코드도 문맥에 따라 해석이 달라질 수 있다.
    
    ```jsx
    function foo() { console.log('foo') }; // 함수 선언문으로 해석
    foo(); // foo
    
    (function bar() { console.log('bar') } ); // ( ) 는 값으로 평가되는 문맥이므로 함수 리터럴 표현식으로 해석
    bar(); // error
    ```
    
    - 위 예제에서 foo는 호출할 수 있고, bar는 호출할 수 없는 이유?
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f0757884-6c17-4d8f-b42d-128729610af9/Untitled.png)
        
        - 함수 이름은 함수 몸체 내에서만 참조할 수 있는 식별자이다.
        - 리터럴로 함수 객체가 생성이 되긴 하나, 함수를 가리키는 식별자가 없다.
            
            ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ac54907a-c59d-4494-a4be-b70994f08553/Untitled.png)
            
        - 하지만 위 예제에서 함수 선언문으로 정의한 함수는 foo라는 이름으로 호출할 수 있었다.
        - foo라는 이름으로 함수를 호출하려면 foo는 함수 이름이 아니라
        함수 객체를 가리키는 식별자여야 한다.
        - foo는 함수 내부에서만 유효한 식별자인 이름이므로 foo로 함수를 호출할 수가 없어야하는데 어떻게 된일일까?
        - foo를 선언한적도 할당한 적도 없는데 호출이 되는 이유는 **자바스크립트에서 암묵적으로 식별자를 생성**하기 때문이다.
        - foo 는 자바스크립트 엔진이 암묵적으로 생성한 식별자이다.
    
    ⇒  **자바스크립트 엔진은 선언문으로 생성된 함수를 호출하기 위해 함수 이름과 동일한 이름의 식별자를 암묵적으로 생성하고, 거기에 함수 객체를 할당한다.**
    
    - 함수는 함수 이름으로 호출하는 것이 아니라 **함수 객체를 가리키는 식별자로 호출한다.**
        - 함수 선언문으로 생성한 함수를 호출한 것은 함수 이름 add 가 아니라, 자바스크립트 엔진이 암묵적으로 생성한 식별자 add이다.
        - 자바스크립트 엔진은 함수 선언문을 함수 표현식으로 변환하여 함수 객체를 생성한다.
        - 단, 함수 선언문과 함수 표현식이 정확히 동일하게 동작하는 것은 아니다.
    
    ### 12.4.2 함수 표현식
    
    - 자바스크립트의 함수는 객체 타입의 값이다.
    - 값처럼 변수에 할당할 수도 있고, 프로퍼티 값이 될 수도 있으며 배열의 요소가 될 수도 있다.
    - 값의 성질을 갖는 객체를 일급 객체라고 한다.
    - 자바스크립트의 함수는 일급 객체이다. ( = 함수를 값처럼 자유롭게 사용할 수 있다. )
    - 함수 리터럴로 생성한 함수 객체를 변수에 할당할 수 있다 = 함수 표현식
    - 함수 표현식의 함수 리터럴은 이름을 생략하는 것이 일반적이다.
    - 함수 선언문과 함수 표현식이 정확히 동일하게 동작하지는 않는다.
        - 함수 선언문은 “표현식이 아닌 문”이고 함수 표현식은 “표현식인 문”이다. 따라서 미묘하지만 중요한 차이가 있다.
    
    ### 12.4.3 함수 생성 시점과 함수 호이스팅
    
    ```jsx
    console.dir(add) // f add(x,y)
    console.dir(sub); // undefined
    
    console.log( add(2,5) ); // 7
    console.log( sub(2,5) ); // TypeError: sub is not a function
    
    // 함수 선언문
    function add(x,y) {
      return x + y;
    }
    
    // 함수 표현식
    var sub = function(x,y) {
     return x - y;
    }
    ```
    
    - 함수 선언문으로 정의된 함수는 함수 선언문 이전에 호출할 수 있다.
    - 그러나 함수 표현식으로 정의한 함수는 표현식 이전에 호출할 수 없다.
    - **함수 선언문으로 정의한 함수와 함수 표현식으로 정의한 함수의 생성 시점이 다르기 때문이다.**
        - 함수 선언문도 런타임 이전에 자바스크립트 엔진에 의해 먼저 실행된다.
            - 런타임 이전에 함수 객체가 먼저 생성된다.
            - 함수이름과 동일한 이름의 식별자를 암묵적으로 생성하고 생성된 함수 객체를 할당한다.
            - 런타임에는 이미 함수 객체 생성 + 식별자에 할당까지 완료된 상태
            - 함수 선언문이 코드의 선두로 끌어올려진 것 처럼 동작하는 자바스크립트 고유의 특징 ⇒ 함수 호이스팅
        - 함수 표현식은 변수 선언문 + 변수 할당문 이다.
            - 변수 선언은 런타임 이전에 실행되어 undefined로 초기화되지만 변수 할당문의 값은 런타임에 평가된다.
            - 따라서 함수 표현식의 함수 리터럴도 함수 할당문이 실행되는 시점에 평가되어 함수 객체가 된다. ( = 런타임에 함수 객체가 생성되고 할당된다. )
            - 함수 호이스팅이 아닌, 변수 호이스팅이 발생한다.
    
    ⇒ 함수 선언문 대신 함수 표현식을 사용할 것을 권장
    
    <aside>
    ❓ **함수 호이스팅과 변수 호이스팅의 차이**
    
    변수 호이스팅은 undefined로 초기화되지만, 함수 호이스팅은 함수 선언문을 통해 암묵적으로 생성된 식별자가 함수 객체로 초기화된다. 따라서 변수 선언문 이전에 변수를 참조하면 변수 호이스팅에 의해 undefined로 평가되지만, 함수 선언문으로 정의한 함수를 함수 선언문 이전에 호출하면 함수 호이스팅에 의해 호출이 가능하다. 
    
    </aside>
    
    ### 12.4.4 Function 생성자 함수
    
    ```jsx
    var add = new Function('x','y','return x+y');
    ```
    
    ⇒  일반적이지 않고 바람직하지도 않음
    
    ### 12.4.5 화살표 함수
    
    ```jsx
    const add = (x,y) => x+y;
    ```
    
    - 기존 함수보다 내부 동작 또한 간략화되어 있다.

## 12.5 함수 호출

- 함수를 가리키는 식별자 + 함수호출 연산자 `()`
- 함수를 호출하면 현재의 실행 흐름을 중단하고 호출된 함수로 실행 흐름을 옮긴다.

### 12.5.1 매개변수와 인수

- 매개변수는 함수를 정의할 때 선언하며, 함수 몸체 내부에서 변수와 동일하게 취급된다.
- 매개변수는 함수 몸체 내부에서만 참조할 수 있다. ⇒ 매개변수의 스코프(유효범위)는 함수 내부이다.
- 함수는 매개변수의 개수와 인수의 개수가 일치하는지 체크하지 않는다.
    - 인수가 부족해서 할당되지 않은 매개변수의 값은 undefined이다.
    - 매개변수보다 인수가 더 많은 경우 초과된 인수는 무시된다.
        - 그냥 버려지는 것은 아니고 arguments 객체의 프로퍼티로 보관된다.
        - argument 객체는 함수를 정의 할 때, 매개 변수를 확정할 수 없는 가변 인자 함수를 구현할 때 유용하게 사용된다,
    

### 12.5.2 인수 확인

- 자바스크립트는 매개변수의 타입을 사전에 지정할 수 없다. ⇒ 정적 타입을 선언할 수 있는 타입스크립트 이용
- 함수를 정의할 때 적절한 타입의 인수가 전달되었는지 확인할 필요가 있다.
    - 단축 평가 `||` 를 활용해 매개변수에 기본값을 할당한다.
    - 매개변수 기본 값(값을 전달하지않거나, undefined를 넘긴 경우)을 사용하면 인수 체크 및 초기화를 간소화할 수 있다.
    

### 12.5.3 매개변수의 최대 개수

- 최대 제한은 없지만 코드를 이해하는데 방해되는 요소이므로 적을수록 좋다.
    - 3개 이상을 넘지 않는 것을 권장한다.
    - 그 이상이 필요하다면 객체를 인수로 전달하는 것이 유리하다,
    - 하지만 함수 내부로 전달한 객체를 함수 내부에서 변경하면 함수 외부의 객체가 변경되는 부수효과가 발생한다.
- 함수는 한가지 일만 해야하며 가급적 작게 만들어야한다.

### 12.5.4 반환문

- return + 표현식(반환값) 으로 이뤄진 반환문 사용
- 함수 호출은 표현식이다. 따라서 return 키워드가 반환한 표현식의 평가 결과, 즉 반환값으로 평가된다.
- 반환문의 역할
    - 함수의 실행을 중단하고 함수 몸체를 빠져나간다.
    - return 뒤에 오는 표현식을 평가해 반환한다.
        - return; 이면 undefined를 반환한다.
- 반환문은 생략할 수 있다.
- return 키워드와 반환값 사이 줄바꿈이 없도록 유의한다.

## 12.6 참조에 의한 전달과 외부 상태의 변경

- 함수의 매개변수 또한 타입에 따라 값에 의한 전달, 참조에 의한 전달 방식을 그대로 따른다.
- 함수 내부에서 매개변수 값을 변경할 경우 원시 값은 원본이 훼손되지 않으며 객체는 원본이 훼손된다.

- 함수가 외부 상태를 변경하면 상태 변화를 추적하기 어려워진다. → 코드의 복잡성을 증가시키고 가독성을 해치는 원인
- 객체의 변경을 추적하려면 **옵저버 패턴** 등을 통해 객체를 참조를 공유하는 모든 이들에게 변경사실을 통지하고 이에 대처하는 추가 대응이 필요하다.
- 이러한 문제의 해결 방법 중 하나는 **객체를 불변 객체로 만들어 사용하는 것!**
    - 객체의 복사비용은 들지만 객체를 마치 원시 값처럼 변경 불가능한 값으로 동작하게 만드는 것.
    - 객체의 상태 변경을 원천 봉쇄하고 상태 변경이 필요한 경우에는 방어적 복사를 통해 원복 객체를 완전히 복제, 즉 깊은 복사를 통해 새로운 객체를 생성하고 재할당을 통해 교체한다.
- 외부 상태를 변경하지 않고 외부 상태에 의존하지 않는 함수를 순수함수라고 한다. ⇒ 순수함수를 통해 **부수 효과를 최대한 억제하여 오류를 피하고 프로그램의 안정성을 높이려는 프로그래밍 패러다임을 함수형 프로그래밍**이라고 한다.

## 12.7 다양한 함수의 형태

### 12.7.1 즉시 실행 함수

- 함수 정의와 동시에 즉시 호출되는 함수
- 즉시 실행 함수는 단 한번만 호출되며 다시 호출할 수 없다.
- 기명 실행 함수도 가능한데, 그룹 연산자 (..) 내의 기명 함수는 선언문이 아니라 리터럴로 평가 되고 함수 이름은 함수 몸체에서만 참조할 수 있는 식별자이므로 즉시 실행 함수를 다시 호출할 수는 없다.
- 그룹 연산자로 함수를 묶는 이유는 함수 리터럴을 평가해서 함수 객체를 생성하기 위해서이다.
- 즉시 실행 함수도 일반 함수처럼 값을 반환할 수 있고, 인자를 전달할 수 있다.

```jsx
(function foo(){
var a = 3;
var b = 5;
return a * b ;
}());

foo(); // error
```

**에러 케이스들** 

```jsx
function () {
}(); // 함수선언문의 형식에 맞지 않으므로 에러 -> 함수 이름을 생략할 수 없음

function foo() {

}();
// 세미콜론 자동 삽입 기능에 의해 함수 선언문이 끝나는 위치, 
// 즉 `{}` 뒤에 중괄호 ; 가 암묵적으로 삽입되므로 
// () 가 그룹 연산자로 해석되어서 error

typeof (function f(){})); // function
typeof (function (){})); // function
```

여러가지 방식들 → 함수 리터럴로 평가되어야하는 맥락을 만든다!

```jsx
(function foo(){ 

}());

(function foo(){ 

})();

!function foo(){ 

}();

+function foo(){ 

}();
```

### 12.7.2 재귀함수

- 함수가 자기자신을 호출하는 것
- 함수 이름은 함수 몸체 내부에서만 유효하므로 자기 자신을 호출할 수 있다.
- 함수 표현식으로 정의된 함수 내부에서는 함수 이름은 물론 함수를 가리키는 식별자로도 자기 자신을 재귀호출 할 수 있다.

```jsx
var factorial = function foo(n){

factorial(); // 가능!

};
```

### 12.7.3 중첩 함수

```jsx
function outer() {
  var x = 1;

  // 중첩 함수
  function inner() {
    var y = 2;
    // 외부 함수의 변수를 참조할 수 있다.
    console.log(x + y); // 3
  }

  inner();
}

outer();
```

- 함수 내부에 정의된 함수
- 중첩 함수를 포함하는 함수는 외부 함수, 내부에 정의된 함수는 내부 함수
- 중첩 함수는 자신을 포함하는 외부 함수를 돕는 헬퍼 함수의 역할을 한다.
- ES6부터 함수 정의는 문이 위치할 수 있는 문맥이라면 어디든지 가능하다.
- 호이스팅으로 인해 혼란이 발생할 수 있으므로, if 문이나 for 문 등의 코드 블록에서 함수 선언문을 통해 함수를 정의하는 것은 바람직하지 않다.

### 12.7.4 콜백 함수

- **함수 합성** ⇒ **함수의 변하지 않는 공통 로직은 미리 정의**해 두고, 경우에 따라 변경되는 로직은 추상화해서 함수 외부에서 함수 내부로 전달

```jsx
// n만큼 어떤 일을 반복한다
function repeat1(n) {
  // i를 출력한다.
  for (var i = 0; i < n; i++) console.log(i);
}

repeat1(5); // 0 1 2 3 4

// n만큼 어떤 일을 반복한다
function repeat1(n) {
  // i를 출력한다.
  for (var i = 0; i < n; i++){

		if(i%2==1) console.log(i);
		
}

repeat2(5); // 1 3
```

```jsx
// 외부에서 전달받은 f를 n만큼 반복 호출한다
function repeat(n, f) {
  for (var i = 0; i < n; i++) {
    f(i); // i를 전달하면서 f를 호출
  }
}

var logAll = function (i) {
  console.log(i);
};

// 반복 호출할 함수를 인수로 전달한다.
repeat(5, logAll); // 0 1 2 3 4

var logOdds = function (i) {
  if (i % 2) console.log(i);
};

// 반복 호출할 함수를 인수로 전달한다.
repeat(5, logOdds); // 1 3
```

- 경우에 따라 변경되는 일을 함수 f로 추상화
- 자바스크립트의 함수는 일급 객체이므로 함수의 매개변수를 통해 함수를 전달할 수 있다.
- repeat 함수는 더 이상 내부 로직에 강력히 의존하지 않고 외부에서 로직의 일부분을 함수로 전달받아 수행하므로 더욱 유연한 구조를 가지게 되었다.
- 이처럼 함수의 매개변수를 통해 다른 함수의 내부로 전달되는 함수를 **콜백 함수**라고 하며
- 매개변수를 통해 함수의 외부에서 콜백 함수를 전달받은 함수를 **고차 함수**라고 한다.
- **고차 함수는 콜백 함수를 자신의 일부분으로 합성한다.**
- 고차 함수에서는 매개변수를 통해 전달받은 콜백 함수의 호출시점을 결정해서 호출한다.
- 콜백함수는 고차함수에 의해 호출되며 이때 고차함수는 필요에 따라 콜백함수에 인자를 전달할 수 있다.
    - 콜백함수가 고차함수 내부에만 호출된다면 **콜백함수를 익명함수 리터럴로 정의하면서 곧바로 고차함수에 전달**하는 것이 일반적이다.
    - 콜백 함수로서 전달된 함수 리터럴은 고차 함수가 호출될 때마다 평가되어 함수 객체를 생성한다.
    - 따라서 다른곳에서도 호출할 필요가 있거나, 콜백 함수를 전달받는 함수가 자주 호출된다면 외부에서 정의하는게 효율적이다.
- 콜백함수는 비동기처리 뿐 아니라, 배열 고차 함수에서도 사용된다.

### 12.7.5 순수함수와 비순수 함수

- 순수함수 : 동일한 인수가 전달되면 언제나 동일한 값을 반환하는 함수
    - 외부 상태(전역변수, 서버 데이터, 파일 ,dom)에 의존하지 않고 오직 매개변수를 통해 전달된 인수에게만 의존
    - 내부 상태에 의존한다고 하더라도 현재 시간과 같이 호출될 때마다 변화하는 값에 의존한다면 순수함수가 아니다.
- 비순수함수 : 외부상태에 의존하는 함수
    - 함수의 외부 상태를 변경하는 부수효과가 있다.
    - 매개변수를 통해 객체를 전달받으면 비순수함수가 된다.
- 함수가 외부 상태를 변경하게 되면 상태 변화를 추적하기 어려워진다.
- 함수형 프로그래밍은 순수 함수와 보조 함수의 조합을 통해 외부 상태를 변경하는 부수 효과를 최소화해서 불변성을 지향하는 프로그래밍 패러다임이다.
