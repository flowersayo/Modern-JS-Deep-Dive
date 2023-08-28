피연산자는 연산의 대상이 되어야 하므로 값으로 평가할 수 있어야 하며,

연산자는 값으로 평가된 피연산자를 연산해 새로운 값을 만든다.

## 7.1 산술 연산자

- 피연산자를 대상으로 수학적 계산을 수행해 새로운 숫자 값을 만든다.
- 산술 연산이 불가능할 경우 NaN을 반환한다.
- 종류 : 이항 산술 연산자, 단항 산술 연산자

### 7.1.1 이항 산술 연산자

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/45fd7519-ece7-4d45-a3f6-b165aafc0ea0/Untitled.png)

- 어떤 산술 연산을 해도 피연산자의 값이 바뀌는 경우는 없다.

### 7.1.2 단항 산술 연산자

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e48a4443-b942-40e0-bc5b-ee56cd28ba2e/Untitled.png)

- 1개의 피연산자를 산술 연산하여 숫자 값을 만든다.
- 피연산자 앞에 위치한 전위 증가/감소 연산자는 먼저 피연산자의 값을 증가/감소 시킨후 다른 연산을 수행한다.
- 피연산자 뒤에 위치한 후위 증가/감소 연산자는 먼저 다른 연산을 수행한 후, 피연산자의 값을 증가/감소 시킨다.
- 숫자 타입이 아닌 피 연산자에 +/- 단항 연산자를 사용하면 피 연산자를 숫자 타입으로 변환하여 반환한다.
    - 이 때 피연산자를 변경하는 것은 아니고, 숫자 타입으로 변환한 값을 생성해서 반환한다. ( = 부수효과가 없다. )

```jsx
var x = '1';

+x; // 1

x = true;

+x; // 1

-'10'; // -10

-true; // -1 

-'Hello'; // NaN
```

### 7.1.3 문자열 연결 연산자

- **+ 연산자는 피연산자 중 하나 이상이 문자열인 경우 문자열 연결 연산자로 동작한다.**
- 그 외의 경우는 산술 연산자로 동작한다.

```jsx
'1' + 2; // '12'

1 + '2'; // '12'

1 + 2; // 3

1 + true; // 2

1 + false; // 1

1 + null; //1 

+ undefined; // NaN
1 + undefined; // NaN
```

- 개발자의 의도와는 상관없이 자바스크립트 엔진에 의해 암묵적으로 타입이 자동변환되기도 한다!

⇒ **암묵적 타입 변환, 강제 타입변환** 

## 7.2 할당 연산자

- 우항에 있는 피연산자의 평가 결과를 좌항에 있는 변수에 할당한다.
- 좌항의 변수에 값을 할당하므로 변수의 값이 변하는 부수 효과가 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c748f5d5-1f93-475d-b786-9fd64edb63c7/Untitled.png)

- 할당문은 값으로 평가되는 표현식인 문으로서 할당된 값으로 평가된다.

⇒ 여러 변수에 동일한 값을 연쇄 할당할 수 있다.

```jsx
var a, b, c;

a = b = c = 0;
```

## 7.3 비교 연산자

- 좌항 우항을 비교한 후 그 결과를 불리언 값으로 반환한다.

### 7.3.1 동등/일치 비교 연산자

- `==` 와 `===` 는 비교하는 엄격성의 정도가 다르다.
- 동등비교는 느슨한 비교를 하지만 일치 비교는 엄격한 비교를 한다.

**동등비교** 

- 동등 비교(==) 연산자는 좌항과 우항의 피연산자를 비교할 때, 먼저 암묵적 타입 변환을 통해 타입을 일치 시킨 후 같은 값인지 비교한다.
    - 따라서 동등 비교 연산자는 좌항과 우항의 피연산자가 타입은 다르더라도 암묵적 타입 변환 후에 같은 값일 수 있다면 true를 반환한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/facecb76-e0de-467e-adcb-8d870b9d4100/Untitled.png)

```jsx
// 동등비교 

5 == 5;  // true

// 타입은 다르지만 암묵적 타입 변환을 통해 타입을 일치시키면 동등하다.
5 == '5'; // true
```

```jsx
'0' ==''; // false
0 ==''; // true
0 == '0'; // true
false =='false'; // false
false == '0'; // true
false == null; // false
false == undefined; // false
```

- 동등 비교 연산자는 예측하기 어려운 결과를 만들어내므로 사용하지 않는 편이 좋다.

**일치비교**

- 일치 비교(===) 연산자는 좌항과 우항의 **피연산자가 타입도 같고 값고 같은 경우**에 한하여 true를 반환한다.
- 다시 말해, 암묵적 타입 변환을 하지 않고 값을 비교한다. 따라서 예측이 쉽다.
- 일치 비교에서 주의할 것은 NaN이다.
    - NaN은 자신과 일치하지 않는 유일한 값이다.
    
    ```jsx
    NaN === NaN; // false
    ```
    
    - 따라서 숫자가 NaN인지 조사하려면 빌트인 함수인 `Number.isNaN` 을 사용한다.
    
    ```jsx
    Number.isNaN(NaN); // true
    Number.isNaN(10); // false
    Number.isNaN(1+undefined); // true
    ```
    
    - 자바스크립트에는 양의 0 과 음의 0 이 있는데 이들을 비교하면 true를 반환한다.
    
    ```jsx
    0 === -0; // true
    0 == -0; // true
    ```
    
    <aside>
    📌 [Object.is](http://Object.is) 메서드
    
    예측 가능한 정확한 비교 결과를 반환한다.
    
    </aside>
    
    ```jsx
    -0 === +0;
    [Object.is](http://Object.is)(-0,+0);
    
    NaN === NaN; // false
    Object.is(NaN, NaN); // true
    ```
    

### 7.3.2 대소 관계 비교 연산자

- 피연산자의 크기를 비교하여 불리언 값을 반환한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/174bef76-96b0-4e26-88a7-fadf8f448359/Untitled.png)

## 7.4 삼항 조건 연산자

- 조건식의 평가 결과에 따라 반환할 값을 결정한다.
- 유일한 삼항연산자이며, 부수효과는 없다.

<aside>
📌 조건식 ? 조건식이 true 일 때 반환할 값 : 조건식이 false 일 때 반환할 값

</aside>

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7897a8f5-f279-4bf1-a29a-bdcf173a13c7/Untitled.png)

- 만약 조건식의 결과가 불리언 값이 아니면 불리언 값으로 암묵적 타입 변환된다.
- 삼항 조건 연산자 표현식은 **값으로 평가할 수 있는 표현식인 문이다.**

## 7.5 논리 연산자

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/da283b6d-0b85-401e-a4bb-9ff71f5fb0c3/Untitled.png)

- 논리 부정연산자는 언제나 불리언 값을 반환한다.
    - 논리 부정(!) 연산자의 피연산자가 불리언 값이 아니라면 불리언 타입으로 암묵적 타입 변환된다.
    
    ```jsx
    !0; // true
    !'hello' // false
    ```
    
- 논리합 또는 논리곱 연산자의 표현식의 평가 결과는 불리언 값이 아닐 수도 있다. 논리합 또는 논리곱 연산자의 표현식은 언제나 2개의 피연산자 중 어느 한쪽으로 평가된다.
    
    ```jsx
    'Cat' && 'Dog'; // 'Dog' 
    ```
    

<aside>
📌 **드 모르간의 법칙**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0ae21591-c3c7-4054-8a85-17f21f3e369d/Untitled.png)

복잡한 표현식을 좀 더 가독성 좋은 표현식으로 변환할 수 있다.

</aside>

## 7.6 쉼표 연산자

- 왼쪽 피연산자부터 차례대로 피연산자를 평가하고, 마지막 피연산자의 평가가 끝나면 마지막 피연산자의 평가 결과를 반환한다.

```jsx
var x, y, z;

x = 1, y = 2, z = 3; // 3
```

## 7.7 그룹 연산자

- 소괄호 `()` 로 피연산자를 감싸는 그룹연산자는 피연산자인 표현식을 가장 먼저 평가한다.
- 연산자 우선순위가 가장 높다.

## 7.8 typeof 연산자

- 피연산자의 데이터타입을 문자열로 변환한다.
- 7가지 ( number, boolean, undefined, symbol, object, function ) 중 하나를 반환한다.
- null 을 반환하는 경우는 없다.
    - type of null 은 object 를 반환한다. → 첫번째 버전의 버그!
    - 따라서 값이 null 타입인지 확인할때에는 typeof 대신에 `===` 연산자를 사용하자!

```jsx
var foo = null;

typeof foo === null; // false
foo === null; // true 
```

- 선언하지 않은 식별자를 typeof 연산자로 연산해보면 referenceError 대신에 undefined를 반환한다.

```jsx
typeof undefined; // undefined
```

## 7.9 지수 연산자

- 좌항의 피연산자를 밑으로, 우항의 피연산자를 지수로 거듭 제곱하여 숫자 값을 반환한다.

```jsx
2 ** 2; //4 
2 ** 2.5; // 5.656854 ..
2 ** 0; // 1 
2 ** -2; // 0.25
```

- Math.pow 메서드를 활용할 수도 있다.
- 이항 연산자 중 우선 순위가 가장 높다.

## 7.10 그 외의 연산자

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/640ab83e-359d-456a-955e-77e34785fda6/Untitled.png)

## 7.11 연산자의 부수효과

- 대부분의 연산자는 다른 코드에 영향을 주지 않는다.
- 하지만 일부 연산자는 다른 코드에 영향을 주는 부수효과가 있다. ex) =, ++/—, delete

## 7.12 연산자의 우선순위

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6aca9486-8d82-4d43-8698-632a5ee3d4a9/Untitled.png)

- 종류가 많기 때문에 모두 기억하기 어렵고 실수하기 쉬움.

⇒ 그룹 연산자를 사용하여 명시적으로 우선순위 조정! 

## 7.13 연산자 결합 순서

- 연산자의 어느 쪽부터 평가를 수행할 것인지를 나타내는 순서

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/56bb5616-e8a7-4c52-a0ee-27230522feb7/Untitled.png)
