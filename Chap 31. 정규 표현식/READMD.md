## 31.1 정규 표현식이란?

> 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어
⇒ 패턴 매칭 기능 제공
> 

## 31.2 정규 표현식의 생성

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/09e6f04b-0846-4db3-a759-023b4177d742/Untitled.png)

1. **정규 표현식 리터럴 사용** 

```jsx
const target = 'Is this all there is?';
const regexp = /is/i;
regexp.test(target); // true
```

1. **RegExp 생성자 함수 사용**

<aside>
💡 **new RegExp(pattern[, flags])

pattern : 정규 표현식의 패턴**

- pattern : 패턴
****- flags 정규 표현식의 플래그 ( g, i, m, u, y )

</aside>

```jsx
const target = 'Is this all there is?';
const regexp = new RegExp(/is/i);
regexp.test(target);
```

## 31.3 RegExp 메서드

### 31.3.1 RegExp.prototype.exec

: 인수로 전달받은 문자열에 대해 패턴을 검색하여 매칭 결과를 배열로 반환

```jsx
const target = 'Is this all there is?';
const regExp = /is/;
regExp.exec(target);
// [ 'is', index: 5, input: 'Is this all there is?', groups: undefined ]
```

### 31.3.2 RegExp.prototype.test

:  패턴을 검색하여 매칭 결과를 boolean 값으로 반환 

```jsx
const target = 'Is this all there is?';
const regExp = /is/;
regExp.test(target); // true
```

### 31.3.3 String.prototype.match

: 문자열과 대상 정규 표현식과의 매칭 결과를 배열로 반환

```jsx
const target = 'Is this all there is?';
const regExp = /is/;
target.match(regExp);
// [ 'is', index: 5, input: 'Is this all there is?', groups: undefined ]
```

## 31.4 플래그

| 플래그 | 의미 | 설명 |
| --- | --- | --- |
| i | ignore case | 대소문자를 구별하지 않고 패턴을 검색한다. |
| g | Global | 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다. |
| m | Multi line | 문자열의 행이 바뀌더라도 패턴 검색을 계속한다. |

```jsx
const target = 'Is this all there is?';

target.match(/is/g); // ["is", "is"]
target.match(/is/ig); // ["Is", "is", "is"]
```

## 31.5 패턴

> 일정한 규칙(패턴)을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어
> 

### 31.5.1 문자열 검색

- 대소문자 구별
- 정규 표현식과 매치한 첫번째 결과만 반환

```jsx
const target = 'Is this all there is?';
const regExp = /is/;
const regExp2 = /is/i;
const regExp3 = /is/ig;
regExp.test(target); // true (is가 있는지)
target.match(regExp2); // [ 'Is', index: 0, input: 'Is this all there is?', groups: undefined ]
target.match(regExp3); // ["Is", "is", "is"]
```

### 31.5.2 임의의 문자열 검색

- `.` → 임의의 문자 한 개

```jsx
const target = 'Is this all there is?';
const regExp = /.../g;
target.match(regExp);  // [ 'Is ', 'thi', 's a', 'll ', 'the', 're ', 'is?' ]
```

### 31.5.3 반복 검색

- {m,n}  앞선 패턴이 최소 m번, 최대 n번 반복되는 문자열
- {m} 앞선 패턴이 m번 반복되는 문자열

⚠️ `,` 뒤에 공백이 있으면 제대로 작동 X

```jsx
const target = 'A AA B BB Aa Bb AAA';
const regExp = /A{1,2}/g;
target.match(regExp); // [ 'A', 'AA', 'A', 'AA', 'A' ]

const regExp2 = /A{2}/g; 
target.match(regExp2);// [ 'AA', 'AA' ]
```

- `+` 는 앞선 패턴이 **최소 한번 이상** 반복되는 문자열을 의미한다. = `{1,}`

```jsx
const target = 'A AA B BB Aa Bb AAA';
const regExp = /A+/g;
target.match(regExp); // [ 'A', 'AA', 'A', 'AAA' ]
```

- `?` 는 앞선 패턴이 **최소 0번 ~ 최대 1번** 반복되는 문자열을 의미한다. = `{0,1}`

```jsx
const target = 'color colour';
const regExp = /colou?r/g;
target.match(regExp); // [ 'color', 'colour' ]
```

- `*` 는 앞선 패턴이 0번 이상 반복되는 문자열을 의미한다. = `{0, }`

### 31.5.4 OR 검색

- `|` 는 `or` 의 의미를 갖는다.

```jsx
const target = `A AA B BB Aa Bb`;
cosnt regExp = /A|B/g;

// A또는 B를 전역 검색
target.match(regExp); // ["A", "A", "A", "B", "B", "B", "A", "B"]
```

- `[ ]` 내의 문자는 or로 동작한다. [AB] = A or B
    - 범위를 지정하려면 [] 내에 -를 사용한다.

```jsx
const target = 'A AA B BB Aa Bb AAA';
const regExp = /[A-Z]+/g; // 대문자 판별 정규 표현식
target.match(regExp);  // [ 'A', 'AA', 'B', 'BB', 'A', 'B', 'AAA' ]
```

```jsx
const target = 'AA BB Aa Bb 12';
const regExp = /[A-Za-z]+/g;  // A-Z or a-z 가 1번 이상 반복
target.match(regExp); // [ 'AA', 'BB', 'Aa', 'Bb' ]
```

QUIZ  A,AA .., B,BB.., 

- `\d` : 숫자 , `\D` : not 숫자 == 문자

```jsx
const target = 'AA BB 12,345';
const regExp = /[0-9]+/g;
target.match(regExp); // ["12", "345"]

let regExp2 = /[\d,]+/g;
target.match(regExp2); // [ '12,345' ]
```

- `\w` :  알파벳, 숫자, 언더스코어 , `\W` : not 알파벳, 숫자, 언더스코어 (특수문자??)

```jsx
const target = 'Aa Bb 12,345 _$%&';
let regExp = /[\w,]+/g;
target.match(regExp); // [ 'Aa', 'Bb', '12,345', '_' ]

let regExp = /[\W,]+/g;
target.match(regExp); // [ ' ',' ',',',',' $%&' ]
```

### 31.5.5 NOT 검색

- […]  내의 ^ 는 not 의 의미를 갖는다.
- \D == [^0-9]

### 31.5.6 시작 위치로 검색

- […] 밖의 ^은 문자열의 **시작**을 의미한다.

```jsx
const target = 'https://google.com';
const regExp = /^https/;
regExp.test(target); // true
```

### 31.5.7 마지막 위치로 검색

- $ 는 문자열의 **마지막**을 의미한다.

```jsx
const target = 'https://google.com';
const regExp = /com$/;
regExp.test(target); // true
```

## 31.6 자주 사용하는 정규표현식

### 31.6.1 특정 단어로 시작하는지 검사

- https:// or http:// 인지 검사

```jsx
const url= 'https://example.com';

// [...]밖의 ^는 문자열의 시작을 의미
// | 는 or
// ? 는 앞선 패턴('s')가 0~1번 반복
/^https?:\/\//.test(url); // true
```

### 31.6.2 특정 단어로 끝나는지 검사

- html 로 끝나는지 검사

```jsx
const fileName= 'idnex.html';

// $는 문자열의 마지막을 의미
/html$/.test(fileName); // true
```

### 31.6.3 숫자로만 이루어진 문자열인지 검사

⚠️ /d+  는 #111$ 같은 문자열도 매칭하므로 안된다. 

- 숫자로만 시작하고 끝나는지 검사
    
    ```jsx
    const target= '12345';
    
    // [...]밖의 ^는 문자열의 시작
    // $ 는 문자열의 마지막
    // \d 는 숫자
    // + 는 앞선 패턴이 최소 한 번 이상 반복되는 문자열을 의미
    /**^\d+$**/.test(target); // true
    ```
    

### 31.6.4 하나 이상의 공백으로 시작하는지 검사

- `\s` 는 여러 가지 공백문자를 의미
- `\s` == [\t\r\n\v\f]

```jsx
const target= '  hi!';

/^[\s]+/.test(target); // true
```

- `/^[\s]+/` vs `/^\s+/`
    
    <aside>
    💬
    
    예, **`/^\s+/`**와 **`/^[\s]+/`**은 정규 표현식의 관점에서 거의 동일한 의미를 가집니다. 두 정규 표현식 모두 문자열의 시작 부분에서 하나 이상의 공백 문자를 찾는 데 사용됩니다.
    
    여기서 차이점은 **`[ ]`**를 사용하는 **`/^[\s]+/`** 버전이 **`\s`**를 문자 클래스 안에서 사용하고 있기 때문에 **좀 더 일반적으로 특정한 공백 문자에 대한 표현식**을 나타낼 때 사용할 수 있습니다. 예를 들어, **`[ \t]`**는 스페이스와 탭 문자를 나타냅니다.
    
    하지만 **`/^\s+/`**는 단순히 스페이스 문자에 대한 정규 표현식이므로 문자 클래스를 사용하지 않고도 공백 문자를 검사할 수 있습니다. 따라서 대부분의 경우, **`/^\s+/`**을 사용하여 문자열의 시작 부분에서 하나 이상의 공백 문자를 찾는 데 충분합니다.
    
    결론적으로, 두 표현식은 비슷한 동작을 하지만 **`/^[\s]+/`**은 공백 문자의 확장된 정의를 다루기 위해 사용될 수 있습니다.
    
    </aside>
    

### 31.6.5 아이디로 사용 가능한지 검사

- 검색 대상 문자열이 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4~10자리인지 확인

```jsx
const id = 'abc123';

/^[A-Za-z0-9]{4,10}$/.test(id); // true
```

### 31.6.6 메일 주소 형식에 맞는지 검사

```jsx
const email = 'ywc95518@gmail.com';

/^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/.test(email); --> true
```

### 31.6.7 핸드폰 번호 형식에 맞는지 검사

```jsx
const cellphone = '010-1234-5678';

/^\d{3}-\d{3,4}-\d{4}$/.test(cellphone); // --> true
```

### 31.6.8 특수 문자 포함 여부 검사

```jsx
const target = 'abc#123';

(/[^A-Za-z0-9]/gi).test(target); // -> true
```

아니면 [ ] 안에 특정 특수문자를 넣는 방식도 있음.

- 특수 문자를 제거할때에는 string.replace 메서드를 사용

```jsx
target.replace(\[^A-Za-z0-9]/gi,''); 
```
