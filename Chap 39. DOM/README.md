> **Documented Object Model** 
HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조
> 

## 노드

### HTML 요소와 노드 객체

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/e5a1676d-bea1-4638-8421-f82720548248/Untitled.png)

- HTML 요소는 렌더링 엔진에 의해 파싱되어 DOM을 구성하는 요소 노드 객체로 변환된다.
    - 어트리뷰트는 어트리뷰트 노드로
    - 텍스트 콘텐츠는 텍스트 노드로
- HTML 요소 간에는 중첩 관계에 의해 계층적인 부자 관계가 형성된다.

**트리 자료 구조**

- 부모 노드와 자식 노드로 구성 되어 노드 간의 계층적 구조를 표현하는 비선형 자료구조
- 노드 객체들로 구성된 트리 자료구조를 DOM 이라고 한다.

### 노드 객체의 타입

```jsx
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="style.css">
  </head>
  <body>
    <ul>
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script src="app.js"></script>
  </body>
</html>
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/b30e879c-b5cd-426a-9368-063f20460edd/Untitled.png)

- 렌더링 엔진은 위 HTML 문서를 파싱하여 다음과 같이 DOM을 생성한다.
- 노드 객체의 계층적인 구조로 구성되어 있다.
- 노드 객체의 종류
    - 문서 노드 : DOM 트리의 최 상위에 존재하는 window.document 객체(진입점)
    - 요소 노드 : HTML 요소를 가리키는 객체
    - 어트리뷰트 노드 : HTML 요소의 어트리뷰트를 가리키는 객체
        - 어트리뷰트 노드는 요소 노드에만 연결되어 있고 부모노드가 없다.
    - 텍스트 노드 : HTML 요소의 텍스트를 가리키는 객체 ( 항상 리프 노드 )

### 노드 객체의 상속 구조

- DOM 을 구성하는 노드 객체는 자신의 구조와 정보를 제어할 수 있는 DOM API 를 사용할 수 있다.
- 노드 객체는 표준 빌트인 객체가 아니라 브라우저 환경에서 추가적으로 제공하는 호스트 객체이다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/64cacf7b-d587-468f-bb20-c47c1107f888/Untitled.png)

- 모든 노드 객체는 Object, EventTarget, Node 인터페이스를 상속 받는다.
- 특정 노드 객체는 프로토타입 체인에 있는 모든 프로토타입의 프로퍼티나 메서드를 상속받아 사용할 수 있다.
- 노드 객체의 상속 구조는 개발자 도구의 Elements → Properties 패널에서 확인할 수 있다.
    
    
    | input 요소 노드 객체의 특성 | 프로토타입을 제공하는 객체 |
    | --- | --- |
    | 객체 | Object |
    | 이벤트를 발생시키는 객체 | EventTarget |
    | 트리 자료구조의 노드 객체 | Node |
    | 브라우저가 렌더링할 수 있는 웹 문서의 요소(HTML, XML, SVG)를 표현하는 객체 | Element |
    | 웹 문서의 요소 중에서 HTML 요소를 표현하는 객체 | HTMLElement |
    | HTML 요소 중에서 input 요소를 표현하는 객체 | HTMLInputElement |

- 이벤트와 관련한 기능 : https://developer.mozilla.org/en-US/docs/Web/API/Event/target
- 노드 정보 제공 기능 : https://developer.mozilla.org/en-US/docs/Web/API/Node
- HTML 요소가 갖는 공통적인 기능 :  https://developer.mozilla.org/en-US/docs/Web/HTML/Element

⇒ **DOM은 HTML 문서의 계층적 구조와 정보를 표현하는 것은 물론 노드 객체의 종류. 즉 노드 타입에 따라 필요한 기능을 프로퍼티와 메서드의 집합인 DOM API 로 제공한다.** 이 **DOM API를 통해 HTML 구조나 내용 또는 스타일 등을 동적으로 조작**할 수 있다.

## 요소 노드 취득

### id 를 이용한 요소 노드 취득

> **Document.prototype.getElementById( string id )** 
: 인수로 전달한 id 값을 갖는 하나의 요소 노드를 탐색하여 반환 ( 중복될 경우 첫번째 요소만 반환 )
> 

- 존재하지 않으면 null 을 반환한다.
- HTML 요소에 id 어트리뷰트를 부여하면 **id 값과 동일한 이름의 전역 변수가 암묵적으로 선언**되고 해당 노드 객체가 할당되는 부수 효과가 있다.

### 태그 이름을 이용한 요소 노드 취득

> **Document.prototype.getElementByTagName( string tag_name )** 
: 해당 태그 이름을 갖는 모든 요소들을 탐색하여 유사배열 객체로 반환
> 

- `Element.prototype.getElementsByTagName` 은 특정 요소 노드의 자손 노드 중에서 해당 태그를 지닌 노드를 탐색하여 반환
- DOM 컬렉션 객체인 HTMLCollection 를 반환한다.

### 클래스를 이용한 요소 노드 취득

> **Document.prototype.getElementByClassName( string class_name )** 
: 해당 클래스 이름을 갖는 모든 요소들을 탐색하여 유사배열 객체로 반환
> 

- `Element.prototype.getElementsByClassName` 은 특정 요소 노드의 자손 노드 중에서 해당 클래스를 지닌 노드를 탐색하여 반환
- DOM 컬렉션 객체인 HTMLCollection 를 반환한다.

### CSS 선택자를 이용한 요소 노드 취득

> **Document.prototype.querySelector( string query )** 
: 해당 css 선택자를 만족시키는 **하나의 요소 노드**를 탐색하여 반환.
> 

> **Document.prototype.querySelectorAll( string query )** 
: 해당 css 선택자를 만족시키는 **모든 요소 노드**를 탐색하여 반환.
> 

- `Element.prototype.getElementsByClassName` 은 특정 요소 노드의 자손 노드 중에서 해당 쿼리를 만족하는 노드를 탐색하여 반환
- DOM 컬렉션 객체인 NodeList 를 반환한다.
- **getElementBy*** 메서드 보다 다소 느리다. 그러나 좀 더 구체적인 조건으로 요소 노드를 취득할 수 있다.**
    
    → id 만 getElementBy 로 처리하고 나머지는 querySelector 사용하기!
    

```css
/* 전체 선택자: 모든 요소를 선택 */
* { ... }

/* 태그 선택자: 모든 p 태그 요소를 모두 선택 */
p { ... }

/* id 선택자: id 값이 'foo'인 요소를 모두 선택 */
#foo { ... }

/* class 선택자: class 값이 'foo'인 요소를 모두 선택 */
.foo { ... }

/* 어트리뷰트 선택자: input 요소 중에 type 어트리뷰트 값이 'text'인 요소를 모두 선택 */
input[type=text] { ... }

/* 후손 선택자: div 요소의 후손 요소 중 p 요소를 모두 선택 */
div p { ... }

/* 자식 선택자: div 요소의 자식(직계 후손) 요소 중 p 요소를 모두 선택 */
div > p { ... }

/* 인접 형제 선택자: p 요소의 형제 요소 중에 p 요소 바로 뒤에 위치하는 ul 요소를 선택 */
p + ul { ... }

/* 일반 형제 선택자: p 요소의 형제 요소 중에 p 요소 뒤에 위치하는 ul 요소를 모두 선택 */
p ~ ul { ... }

/* 가상 클래스 선택자: hover 상태인 a 요소를 모두 선택 */
a:hover { ... }

/* 가상 요소 선택자: p 요소의 콘텐츠의 앞에 위치하는 공간을 선택 일반적으로 content 프로퍼티와 함께 사용된다. */
p::before { ... }
```

### 특정 요소 노드를 취득할 수 있는지 확인

> **Element.prototype.matches** 메서드는 인수로 전달한  CSS  선택자를 통해 특정 요소 노드를 취득할 수 있는지 확인한다.
> 

- 이벤트 위임을 사용할 때 유용하다.

### HTMLCollection과 NodeList

- DOM 컬렉션 객체인 HTMLCollection과 NodeList는 **DOM API가 여러 개의 결과값을 반환하기 위한 DOM 컬렉션 객체**다.
- **HTMLCollection과 NodeList는 모두 유사 배열 객체이면서 이터러블**이다. 따라서 **for ... of** 문으로 순회할 수 있으며 **스프레드 문법**을 사용하여 간단히 배열로 변환할 수 있다.
- HTMLCollection과 NodeList의 중요한 특징은 **노드 객체의 상태 변화를 실시간으로 반영**하는 **살아 있는 객체**라는 것이다.

**HTML Collection** 

- 노드 객체의 상태 변화를 **실시간으로 반영하는 살아 있는** DOM 컬렉션 객체
    
    ! HTMLCollection 객체를 for문으로 순회하면서 노드 객체의 상태를 변경해야 할 때 주의해야한다.
    
    ⇒ for 문을 역방향으로 순회하는 방법으로 회피할 수 있다.
    
    ⇒ while 문을 사용하여 더이상 HTMLCollection 객체에 노드 객체가 남아 있지 않을 때까지 무한 반복하는 방법
    
    ⇒ 배열로 변환하여 사용
    

**Node List**

- 앞선 HTMLCollection 객체의 부작용을 해결하기 위해서  **querySelector류의 메서드를 사용할 수 있다.**
- NodeList 는 **실시간으로 노드 객체의 상태 변경을 반영하지 않는** 객체이다.
- forEach, item, entries, keys, values 메서드를 제공한다.
- 대부분은  non-live 이지만, **childNodes 프로퍼티가 반환하는 Node List 객체는 live 객체로 동작**하므로 주의한다.
    
    **⇒ 노드 객체의 상태 변경과 상관없이 안전하게 DOM 컬렉션을 이용하려면 HTML Collection 이나 Node List 객체를 배열로 변환하여 사용하는 것을 권장한다.**
    

## 노드 탐색

이처럼 DOM 트리 상의 노드를 탐색할 수 있도록 Node, Element 인터페이스는 트리 탐색 프로퍼티를 제공한다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/efcfe813-6ebf-415d-a1a3-92074b248b01/Untitled.png)

- parentNode, previousSibling, firstChild, childNodes 프로퍼티는 Node.portotype이 제공하고, 프로퍼티키에 Element가 포함된 previousElementSibling, nextElementSibling과 children 프로퍼티는 Element.prototype이 제공한다.
- 노드 탐색 프로퍼티는 모두 참조만 가능한 읽기 전용 접근자 프로퍼티이다.

### 공백 텍스트 노드

: HTML 요소 사이의 스페이스, 탭, 줄바꿈(개행) 등의 공백 문자는 텍스트 노드를 생성한다. 

→  노드 탐색 시 유의하기

### 자식 노드 탐색

| 프로퍼티 | 설명 |
| --- | --- |
| Node.prototype.childNodes | 자식 노드를 모두 탐색하여 DOM 컬렉션 객체인 NodeList에 담아 반환한다. childNodes 프로퍼티가 반환한 NodeList에는 요소 노드뿐만 아니라 텍스트 노드도 포함되어 있을 수 있다. |
| Element.prototype.children | 자식 노드 중에서 요소 노드만 모두 탐색하여 DOM 컬렉션 객체인 HTMLCollection에 담아 반환한다. children 프로퍼티가 반환한 HTMLCollection에는 텍스트 노드가 포함되지 않는다. |
| Node.prototype.firstChild | 첫 번째 자식 노드를 반환한다. 텍스트 노드이거나 요소 노드를 반환한다. |
| Node.prototype.lastChild | 마지막 자식 노드를 반환한다. 텍스트 노드이거나 요소 노드를 반환한다. |
| Element.prototype.firstElementChild | 첫 번째 자식 요소 노드를 반환한다. 요소 노드를 반환한다. |
| Element.prototype.lastElementChild | 마지막 자식 요소 노드를 반환한다. 요소 노드를 반환한다. |

### 자식 노드 존재 확인

> **Node.prototype.hasChildNodes 
:** 자식 노드가 존재하면 true, 자식 노드가 존재하지 않으면 false ( 텍스트 노드를 포함 )
> 

- 텍스트 노드를 제외하고 확인하려면 children.length 또는 childElementCount 프로퍼티 사용

### 요소 노드의 텍스트 노드 탐색

: 요소 노드의 텍스트 노드는 firstChild 프로퍼티로 접근할 수 있다.

### 부모 노드 탐색

> **Node.prototype.parentNode**
> 

```html
<!DOCTYPE html>
<html><body><ul id="fruits"><li class="apple">Apple</li><li class="banana">Banana</li><li class="orange">Orange</li></ul></body><script>
    // 노드 탐색의 기점이 되는 .banana 요소 노드를 취득한다.
    const $banana = document.querySelector(".banana");

    // .banana 요소 노드의 부모 노드를 탐색한다.
    console.log($banana.parentNode); // ul#fruits
  </script></html>
```

### 형제 노드 탐색

- 어트리뷰트 노드는 제외하고 요소/텍스트 노드만 반환한다.

| 프로퍼티 | 설명 |
| --- | --- |
| Node.prototype.previousSibling | 부모 노드가 같은 형제 노드 중에서 자신의 이전 형제 노드를 탐색하여 반환한다. previousSibling 프로퍼티가 반환하는 형제 노드는 요소 노드뿐만 아니라 텍스트 노드일 수도 있다. |
| Node.prototype.nextSibling | 부모 노드가 같은 형제 노드 중에서 자신의 다음 형제 노드를 탐색하여 반환한다. 요소 노드뿐만 아니라 텍스트 노드일 수도 있다. |
| Element.prototype.previousSibling | 부모 노드가 같은 형제 노드 중에서 자신의 이전 형제 요소 노드를 탐색하여 반환한다. 요소 노드만 반환한다. |
| Element.prototype.nextSibling | 부모 노드가 같은 형제 노드 중에서 자신의 다음 형제 요소 노드를 탐색하여 반환한다. 요소 노드만 반환한다. |

## 노드 정보 취득

노드 객체에 대한 정보를 취득하려면 다음과 같은 노드 정보 프로퍼티를 사용한다.

| 프로퍼티 | 설명 |
| --- | --- |
| Node.prototype.nodeType | 노드 객체의 종류, 즉 노드 타입을 나타내는 상수를 반환한다. 노드 타입 상수는 Node에 정의되어 있다.
- Node.ELEMENT_NODE: 요소 노드 타입을 나타내는 상수 1을 반환
- Node.TEXT_NODE: 텍스트 노드 타입을 나타내는 상수 3을 반환
- Node.DOCUMENT_NODE: 문서 노드 타입을 나타내는 상수 9를 반환 |
| Node.prototype.nodeName | 노드의 이름을 문자열로 반환한다.
- 요소 노드: 대문자 문자열로 태그 이름("UL", "LI"등)을 반환
- 텍스트 노드: 문자열 "#text"를 반환
- 문서 노드: 문자열 "#document"를 반환 |

## 요소 노드의 텍스트 조작

### nodeValue

> Node.prototype.nodeValue
> 

- setter 와 getter 모두 존재하는 접근자 프로퍼티
- 노드 객체의 값을 반환 → **“텍스트 노드”의 텍스트**

```jsx
<!DOCTYPE html>
<html>
  <body>
    <div id="foo">Hello</div>
  </body>
  <script>
    // 1. #foo 요소 노드의 자식 노드인 텍스트 노드를 취득한다.
    const $textNode = document.getElementById("foo").firstChild;

    // 2. nodeValue 프로퍼티를 사용하여 텍스트 노드의 값을 변경한다.
    $textNode.nodeValue = "World";

    console.log($textNode.nodeValue); // World
  </script>
</html>
```

### textContent

> Node.prototype.textContent
> 

: 요소 노드의 콘텐츠 영역(시작 태그와 종료 태그 사이) 내의 텍스트를 모두 반환

⇒ 요소 노드의  childNodes 프로퍼티가 반환한 모든 노드들의 텍스트 노드의 값을 반환.

```html
<!DOCTYPE html>
<html><body><div id="foo">Hello <span>world!</span></div></body><script>
    // #foo 요소 노드의 텍스트를 모두 취득한다. 이때 HTML 마크업은 무시된다.
    console.log(document.getElementById("foo").textContent); // Hello world!
  </script></html>
```

- 콘텐츠 영역에 **자식 요소 노드가 없고 텍스트만 있다면** textContent 를 사용하는 것이 간편
- 유사한 동작을 하는 innerText 프로퍼티가 있으나 사용하지 않는 것이 좋다.
    - CSS에 순종적 ( ex. visibility: hidden 이면 반환하지 않음 )
    - CSS 를 고려하므로 textContent 보다 느리다.

## DOM 조작

: 새로운 노드를 생성하여 DOM 에 추가하거나 기존 노드를 삭제 또는 교체하는 것 

- 새로운 노드가 추가되거나 삭제되면 리플로우와 리페인트가 발생하는 원인이 되므로 성능에 영향을 준다.

### innerHTML

> Element.prototype.innerHTML
> 

- setter와 getter 모두 존재하는 접근자 프로퍼티
- 요소 노드의 **HTML 마크업을 취득**하거나 변경한다.
- 요소 노드의 콘텐츠 영역(시작~종료 사이) 내에 포함된 모든 HTML 마크업을 문자열로 반환한다.
- 문자열을 할당하면 요소 노드의 자식 노드가 모두 제거되고 할당한 HTML 마크업을 파싱하여 요소 노드의 자식노드로서 DOM에 반영한다.
- 단점
    - 입력받은 데이터를 그대로 할당하는 것은 크로스 사이트 스크립팅 공격에 취약하므로 위험하다.
    - 기존 요소 노드의 모든 자식 노드를 제거하고 새롭게 교체한다. ( 비효율적 )
    - 새로운 요소를 삽입할 때 삽입될 위치를 지정할 수 없다.
    
    ⇒ 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입해야 하는 경우 사용하지 않는 것이 좋다.
    

<aside>
💬 **HTML 새니티제이션**

사용자로부터 입력받은 데이터에 의해 발생할 수 있는 크로스 사이트 스크립팅 공격을 예방하기 위해 잠재적 위험을 제거하는 기능. 직접 구현할 수도 있겠지만 DOMPurify 라이브러리 사용 권장.

</aside>

### insertAdjacentHTML 메서드

> Element.prototype.insertAdjacentHTML ( position, HTML markup string )

- position  : 'beforebegin', 'afterbegin', 'beforeend', 'afterend'
> 

```html
<!-- beforebegin -->
<p><!-- afterbegin -->
  foo
  <!-- beforeend -->
</p><!-- afterend -->
```

- 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입한다. → innerHTML 보다 효율적

### 노드 생성과 추가

**요소 노드 생성** 

> document.prototype.**createElement(tagName)**
> 

**텍스트 노드 생성** 

> document.prototype.c**reateTextNode(text)**
> 

```html
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li id="1">1번</li>
      <li id="2">2번</li>
    </ul>
    <script>
      const table =document.querySelector('ul');
      const liNode = document.createElement('li'); // 요소 노드 생성
      const textNode = document.createTextNode('text'); // 텍스트 노드 생성
      liNode.appendChild(textNode); // 요소 노드에 자식으로 텍스트 노드 추가
			// li.textContent = 'text' 
      table.appendChild(liNode); // table에 자식으로 요소 노드 추가 (이 때만 리플로우,리페인트 발생)
    </script>
  </body>
</html>
```

**요소노드를 DOM에 추가**

> Node.prototype.**appendChild(childNode)**
: 해당 요소 노드의 마지막 자식 요소로 추가한다.
> 

### 복수의 노드 생성과 추가

- DOM 을 변경하는 것은 높은 비용이 드는 처리이므로 요소 노드를 반복하여 추가하는 것은 비효율적이다.
- 추가해야할 요소를 컨테이너 DocumentFragment  요소에 자식노드로 추가하고, 해당 컨테이너 요소를 실제 돔에  삽입한다.
    
    > document.createDocumentFragment()
    > 
    - 부모 노드가 없어서 기존 DOM 과는 별개로 존재한다.
    - 컨테이너 요소와 같이 별도의 서브 DOM 을 구성하여 기존 DOM 에 추가하기 위한 용도로 사용한다.
    - 실제 DOM 변경은 한번 뿐이며 리플로우와 리페인트도 한 번만 실행된다.
    - DocumentFragment 노드를 DOM에 추가하면 자신은 제거되고 자신의 자식 노드만 DOM에 추가된다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/9accc4ec-09bc-4536-832e-0e475f2bcf8b/Untitled.png)
    

### 노드 삽입

**마지막 노드로 추가**

> **Node.prototype.appendChild(newNode)** : newNode를 마지막 자식 노드로 추가
> 

**지정한 위치에 노드 삽입**

> **Node.prototype.insertBefore(newNode,childNode)** : childNode 앞에 newNode 삽입
> 

- 두번째 인수가  null이면 appendChild처럼 동작한다.

### 노드 이동

- DOM 에 이미 존재하는 노드를 다시 추가하면 현재 위치에서 노드를 제거하고 새로운 위치에 노드를 추가한다.

### 노드 복사

> **Node.prototype.cloneNode(true | false)**
> 
> - true : **깊은 복사**, 모든 자식 노드가 포함된 노드의 사본
> - false | 생략 : **얕은 복사**, 노드 자신만의 사본 -> 자식 노드를 복사하지 않으므로 텍스트 노드도 없다.

### 노드 교체

> **Node.prototype.replaceChild(newChild,oldChild)**
> 

- 자신을 호출한 노드의 자식노드를 교체한다 ( old → new )

### 노드 삭제

> **Node.prototype.removeChild(child)**
: child 매개변수에 인수로 전달한 노드를 DOM에서 삭제한다.
> 

## 어트리뷰트

### 어트리뷰트 노드와 attributes 프로퍼티

- HTML 요소의 어트리뷰트는 어트리뷰트 노드로 변환되어 요소 노드와 연결된다.
- HTML 어트리뷰트당 하나의 어트리뷰트가 생성된다.
- 모든 어트리뷰트 노드의 참조는 유사 배열 객체이자 이터러블인 NamedNodeMap 객체에 담겨서 요소 노드의 attributes 프로퍼티에 저장된다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/dfe91413-2810-4718-a73b-9bd990f0fd29/Untitled.png)
    

### HTML 어트리뷰트 조작

> **Element.prototype.getAttribute(attributeName) : 참조
Element.prototype.setAttribute(attributeName,attributeValue) : 변경
Element.prototype.hasAttribute(attributeName) : 존재 여부 확인
Element.prototype.removeAttribute(attributeName) : 삭제**
> 

### HTML 어트리뷰트 vs DOM 프로퍼티

1. 요소 노드의 attributes  프로퍼티에서 관리하는 **어트리뷰트 노드**
    - HTML 요소의 초기 상태를 지정하는 것 → 상태가 변경되어도 변하지 않는다.
    - 초기 상태 값을 취득하거나 변경하려면 getAttribute/setAttribute 이용
    - 어트리뷰트 노드에서 관리하는 HTML 요소에 지정한 어트리뷰트 값, 즉 초기 상태 값이다.
2. HTML 어트리뷰트에 대응하는 **요소 노드의 프로퍼티** ( DOM 프로퍼티 ) 
    - 사용자가 입력한 최신 상태 관리
    - $input.value

!  사용자 입력에 의한 상태 변화와 관련이 없는 어트리뷰트와 DOM 프로퍼티는 항상 동일한 값으로 연동한다.

( = 일반적으로 어트리뷰트는 초기 값, 프로퍼티는 최신 값을 관리하지만 사용자의 입력과 관련이 없을 경우 프로퍼티 값이 변하면 어트리뷰트 값도 같이 변한다.)

**HTML 어트리뷰트와 DOM 프로퍼티의 대응 관계**

언제나 1:1 대응하는 것은 아니며, 프로퍼티 키가 반드시 일치하는 것도 아니다.

- id 어트리뷰트는 id 프로퍼티와 대응
- class 어트리뷰트는 className,classList 프로퍼티와 대응
- for 어트리뷰트는 htmlFor 프로퍼티와 대응
- 어트리뷰트에 대응하는 프로퍼티는 카멜 케이스임 (htmlFor)

**DOM 프로퍼티 타입**

getAttribute 메서드로 취득한 어트리뷰트의 값은 항상 문자열이지만 DOM 프로퍼티로 취득한 최신 값은 문자열이 아닐 수도 있다.

```jsx
<!DOCTYPE html>
<html>
  <body>
    <input id="check" type="checkbox" checked>
    <script>
      const $input = document.getElementById('check');
			$checkbox.getAttribute('checked'); //' '
      console.log($input.checked); // true
    </script>
  </body>
</html>
```

### data 어트리뷰트와 dataset 프로퍼티

- HTML 요소에 정의한 사용자 정의 어트리뷰트와 자바스크립트 간에 데이터를 교환할 수 있다.
- data- 접두사 다음에 임의의 이름을 붙여서 지정한다.
- HTMLElement.dataset 프로퍼티는 모든 data 어트리뷰트를 담은 **DOMStringMap** 객체 반환
- DOMStringMap 객체는 사용자정의 어트리뷰트 이름을 카멜케이스로 변환한 프로퍼티를 갖는다.
    
    > **$node.dataset.사용자정의 attribute 이름**
    > 

```jsx
<!DOCTYPE html>
<html>
  <body>
    <ul>
      <li data-user-id="1" data-sex="male">John</li>
      <li data-user-id="2" data-sex="female">May</li>
    </ul>
    <script>
      const users = [...document.querySelector('ul').children];
      const 여자 = users.find(x => x.dataset.sex==='female');
      console.log(여자.textContent); // May
      console.log(여자.dataset.userId); // 2
    </script>
  </body>
</html>
```

## 스타일

### 인라인 스타일 조작

- [HTMLElement.prototype.style](http://HTMLElement.prototype.style) 프로퍼티는 setter 와 getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 인라인 스타일을 취득하거나 추가 또는 변경한다.
- CSSStyleDeclaration 객체의 프로퍼티는 카멜케이스를 따른다.
- 케밥 케이스의 CSS 프로퍼티를 그대로 사용하려면 객체의 마침표 표기법 대신 대괄호 표기법을 사용한다.
- 반드시 단위를 지정해야한다.

```jsx
<!DOCTYPE html>
<html>
  <body>
   <div style="width: 100px; height : 100px; background-color : red;"></div>
    <script>
      const $div = document.querySelector('div');
      console.log($div.style); // CSSStyleDeclaration 객체
      $div.style.backgroundColor = 'blue'; // 파란색으로 변환
      $div.style['background-color'] = 'green'; // css 표기법으로 적용하고 싶을 때
    </script>
  </body>
</html>
```

### 클래스 조작

**className** 

: 클래스를 문자열로 반환한다.

```jsx
<!DOCTYPE html>
<html>
  <head>
    <style>
      .box{
        width: 100px;
        height: 100px;
      }
      .green{
        background-color: green;
      }
    </style>
  </head>
  <body>
    <div class="box green"></div>
    <script>
      const $div = document.querySelector('div');
      console.log($div.className); // box green
    </script>
  </body>
</html>
```

**classList**

- class 어트리뷰트의 정보를 **DOMTokenList** 객체에 담아 반환
- DOMTokenList 객체는 여러가지 메서드 제공

<aside>
💬 **DOMTokenList 객체의 메서드**

- add(...className) : 인수로 전달한 문자열을 class 어트리뷰트에 추가
- remove(...className) : 삭제, 인수로 전달한 값이 없어도 에러 발생X
- item(index) : index에 해당하는 클래스를 반환
- contains(className) : true/false 반환
- replace(oldClassName,newClassName)
- toggle(className,[조건식]) : 있으면 삭제, 없으면 추가조건식은 선택사항, true면 강제로 추가, false면 강제 삭제
</aside>

### 요소에 적용되어 있는 CSS 스타일 참조

- style 프로퍼티는 인라인 스타일만 반환한다.
- 모든 CSS 스타일을 참조해야할 경우 `getComputedStyle` 을 사용한다.
    
    > **getComputedStyle  (element[, pseudo])** 
    : 첫번째 인수로 전달한 요소 노드에 적용되어 있는 최종 스타일을 반환 (CSSStyleDeclaration 객체)
    두번째 인수로 :after, :before 과 같은 의사 요소를 지정하는 문자열을 전달할 수 있다.
    > 

## DOM 표준

- HTML과 DOM 표준은 W3C와 WHATWG가 협력으로 공통된 표준을 만들어 왔다.
- 2018년부터 구글, 애플, 마이크로소프트, 모질라로 구성된 WHATWG가 단일 표준을 제공하기로 두 단체가 합의했다.
- DOM은 현재 4개의 버전이 있음
