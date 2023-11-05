## 호출 스케쥴링

- 함수를 명시적으로 호출하지 않고 일정 시간이 경과된 이후에 호출되도록 함수 호출을 예약하기 위해 타이머 함수를 사용하는 것
- 타이머를 생성할 수 있는  타이머 함수인 setTimeout 과 setInterval
- 타이머를 제거할 수 있는 타이머 함수 clearTimeout 과 clearInterval
- 자바스크립트 엔진은 단 하나의 실행 컨텍스트 스택을 갖기 때문에 두 가지 이상의 태스크를 동시에 실행할 수 없다. ⇒ 싱글 스레드
- setTimeout 과 setInterval 은 비동기 처리 방식으로 동작

## 타이머 함수

### setTimeout / setInterval

> const timerId = setTimeout(func[, delay, param1, … )
> 

```jsx
// setTimeout 함수는 생성된 타이머를 식별할 수 있는 고유한 타이머 id를 반환한다.
const timerId = setTimeout(() => console.log('Hi!'), 1000);

// 타이머 id를 clearTimeout 함수의 인수로 ㅓㅈㄴ달하여 타이머를 취소한다.
clearTimeout(timerId)
```

- `setTimeout`함수는 두 번째 인수로 전달받은 시간으로 단 한 번 동작하는 타이머를 생성한다. 이후 타이머가 만료되면 첫 번째 인수로 받은 콜백 함수가 단 한번 호출된다.
- setTimeout 함수는 생성된 타이머를 식별할 수 있는 고유한 타이머 id를 반환한다.
- setTimeout 함수가 반환한 id를 clearTimeout 함수의 인수로 전달하여 타이머를 취소할 수 있다.

### setInterval 과 clearInterval

> const timerId = setInterval(func[, delay, param1, … )
> 

- setInterval 함수는 두 번째 인수로 전달받은 시간으로 동작하는 타이머를 생성한다.
- 이후 타이머가 만료될 때마다 취소될때까지 첫 번째 인수로 전달받은 콜백 함수가 반복 호출된다.
- setTimeout와 마찬가지로 clearInterval 인자에 id값을 통해 타이머를 취소할 수 있다.

```jsx
let count = 1;

// 1초(1000ms) 후 타이머가 만료될 때마다 콜백 함수가 호출된다.
// setInterval 함수는 생성된 타이머를 식별할 수 있는 고유한 타이머 id를 반환한다.
const timeoutId = setInterval(() => {
  console.log(count); // 1 2 3 4 5
  // count가 5이면 setInterval 함수가 반환한 타이머 id를 clearInterval 함수의
  // 인수로 전달하여 타이머를 취소한다. 타이머가 취소되면 setInterval 함수의 콜백 함수가
  // 실행되지 않는다.
  if (count++ === 5) clearInterval(timeoutId);
}, 1000);
```

## 디바운스와 스로틀

- scroll, resize, input 같은 이벤트는 짧은 시간 간격으로 연속해서 발생하기 때문에 과도하게 호출되어 성능에 문제를 일으킬 수 있다.
- 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 과도한 이벤트 핸들러의 호출을 방지하는 프로그래밍 기법

### 디바운스

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/10a00f65-7cb3-492e-8afa-33ae77b4c1cf/Untitled.png)

- 짧은 시간 간격으로 발생하는 이벤트를 그룹화해서 마지막에 단 한 번만 이벤트 핸들러가 호출되도록 한다.
- 만일 Ajax 요청과 같은 무거운 처리를 수행한다면, 입력을 완료했을 때에만 요청을 처리하는 것이 바람직
- 일정 시간(delay) 동안 입력 필드에 값을 입력하지 않으면 입력이 완료된 것으로 간주
- resize, 입력 필드 자동완성, **버튼 중복 클릭 방지 처리 등에 유용**
- 실무에서는 Underscore 나 Lodash 의 debounce 함수 사용 권장

```jsx
const $input = document.querySelector('input');
    const $msg = document.querySelector('.msg');

    const debounce = (callback, delay) => {
      let timerId;
      // debounce 함수는 timerId를 기억하는 클로저를 반환한다.
      return event => {
        // delay가 경과하기 이전에 이벤트가 발생하면 이전 타이머를 취소하고
        // 새로운 타이머를 재설정한다.
        // 따라서 delay보다 짧은 간격으로 이벤트가 발생하면 callback은 호출되지 않는다.
        if (timerId) clearTimeout(timerId);
        timerId = setTimeout(callback, delay, event);
      };
    };

    // debounce 함수가 반환하는 클로저가 이벤트 핸들러로 등록된다.
    // 300ms보다 짧은 간격으로 input 이벤트가 발생하면 debounce 함수의 콜백 함수는
    // 호출되지 않다가 300ms 동안 input 이벤트가 더 이상 발생하면 한 번만 호출된다.
    $input.oninput = debounce(e => {
      $msg.textContent = e.target.value;
    }, 300);
```

### 스로틀

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/b19e4487-dfef-4395-9201-8b3dc303ca25/6c6c37c5-a248-4bdb-9763-63040acb50a1/Untitled.png)

- 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 일정 시간 단위로 이벤트 핸들러가 호출되도록 호출 주기를 만든다.
- scroll 이벤트 처리, 무한 스크롤 구현 등에 유용
- 실무에서는 Underscore 나 Lodash 의 throttle  함수 사용 권장

```jsx
const $container = document.querySelector('.container');
    const $normalCount = document.querySelector('.normal-count');
    const $throttleCount = document.querySelector('.throttle-count');

    const throttle = (callback, delay) => {
      let timerId;
      // throttle 함수는 timerId를 기억하는 클로저를 반환한다.
      return event => {
        // delay가 경과하기 이전에 이벤트가 발생하면 아무것도 하지 않다가
        // delay가 경과했을 때 이벤트가 발생하면 새로운 타이머를 재설정한다.
        // 따라서 delay 간격으로 callback이 호출된다.
        if (timerId) return;
        timerId = setTimeout(() => {
          callback(event);
          timerId = null;
        }, delay, event);
      };
    };

    let normalCount = 0;
    $container.addEventListener('scroll', () => {
      $normalCount.textContent = ++normalCount;
    });

    let throttleCount = 0;
    // throttle 함수가 반환하는 클로저가 이벤트 핸들러로 등록된다.
    $container.addEventListener('scroll', throttle(() => {
      $throttleCount.textContent = ++throttleCount;
    }, 100));
```

💡 **디바운드와 스로틀의 차이**

짧은 시간 간격으로 발생하는 이벤트를 처리할 때, 
가장 마지막 이벤트를 기준으로 함수를 호출하면 디바운스,
가장 첫 이벤트 기준으로 함수를 호출하면 스토틀,

