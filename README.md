# 모던 자바스크립트 Deep Dive 스터디

프로그래머의 역할은 요구사항을 기반으로 문제를 해결하기 위한 방안을 고안하고, 
이를 코드로 구현하는 것입니다. 구현된 코드는 의도한 대로 정확히 동작해서 문제를 해결해야 합니다. 이때 자신이 구현한 코드가 컴퓨터 내부에서 어떻게 동작할 것인지 예측 가능해야 하며, 이를 명확히 설명할 수 있어야 합니다.

그렇다면 프로그래밍 언어의 기본 개념과 동작원리를 정확히 이해하는 것이 중요합니다.
기본 개념과 동작 원리를 이해하지 못한 상태에서 복사 붙여넣기로 단순히 동작하는 코드를 만들고 거기에 만족한다면 여러분이 구현한 코드는 언제 무너져도 이상할 것이 없는 사상누각일뿐입니다. 
그리고 여러분의 문제 해결 능력은 어느 선에서 성장을 멈추고 말 것입니다.


기본 개념과 동작원리의 이해는 어렵고 생소한 용어로 이루어진 기술적 의사소통을 가능케 하고, 자신의 머릿속에서 코드를 실행해 볼 수 있는 능력을 갖게 합니다. 이를 통해 다른 사람이 작성한 코드를 읽고 이해하는 것은 물론 의도 또한 파악할 수 있습니다. 즉, 기본 개념과 동작 원리에 대한 이해는 안정적이고 효율적인 코드를 생산할 수 있는 기본기입니다. **기본기는 아무리 강조해도 지나지 않습니다.**


## 대상

✅ 언어 공부부터 시작한게 아니라 개발하면서 구글링한 js로 대충 개발을 해왔던 개발자

✅ JS 기본기가 부족하다고 느껴지는 사람들

✅ 최신 버전 자바스크립트에 적응하고 싶은 사람

✅ JS 에서 자주쓰이는 API나 함수 활용법을 익히고 싶은 사람

## 🏁 Goal 🏁
자바스크립트의 기본 개념과 동작 원리에 대해 깊이있게 학습합니다.

## 범위
4장 변수 ~ 48장 모듈

## 기한 

2023.08.21 ~ 2023.10.16 (약 2개월) 


## 스터디 방식

- 매주 2회 스터디를 진행하며, 1회기당 3장씩 학습을 진행합니다. 
- 매주 학습한 내용을 자신이 원하는 곳 (노션, 벨로그, 티스토리, 깃허브 등)에 정리하여 인증합니다.


## 🧑‍💻 팀원 소개

  <table>
    <tr>
      <td align="center"><img src="https://github.com/flowersayo.png" width="160"></td>
      <td align="center"><img src="https://github.com/YelynnOh.png" width="160"></td>
    <td align="center"><img src="https://github.com/dooli1971039.png" width="160"></td>
      <td align="center"><img src="https://github.com/helpingstar.png" width="160"></td>
    </tr>
    <tr>
      <td align="center"> 김서연 (스터디장) </td>
      <td align="center"> 오예린 </td>
        <td align="center"> 이진경 </td>
      <td align="center"> 박우성 </td>
    </tr>
    <tr>
      <td align="center"><a href="https://github.com/flowersayo" target="_blank">@flowersayo</a></td>
      <td align="center"><a href="https://github.com/YelynnOh" target="_blank" width="160">@YelynnOh</a></td>
      <td align="center"><a href="https://github.com/dooli1971039" target="_blank" width="160">@dooli1971039</a></td>
      <td align="center"><a href="https://github.com/helpingstar" target="_blank" width="160">@helpingstar</a></td>
    </tr>
  </table>



## Curriculum


아래는 요청하신 스크럼 담당자를 HTML 링크 태그로 대체한 표입니다.

| 회기 | 날짜  | 주제 | 스크럼 담당 |
|:----:|:----:|:---:|:----------:|
|  1   | 8/24 | 04장 변수, 05장 표현식과 문, 06장 데이터 타입 | <a href="https://github.com/dooli1971039" target="_blank" width="160">@dooli1971039</a> |
|  2   | 8/28 | 07장 연산자, 08장 제어문, 09장 타입변환과 단축 평가 | <a href="https://github.com/YelynnOh" target="_blank" width="160">@YelynnOh</a> |
|  3   | 8/31 | 10장 객체리터럴, 11장 원시값과 객체의 비교, 12장 함수 | <a href="https://github.com/helpingstar" target="_blank" width="160">@helpingstar</a> |
|  4   | 9/4  | 13장 스코프, 14장 전역 변수의 문제점, 15장 let,const 키워드와 블록 레벨 스코프 | <a href="https://github.com/flowersayo" target="_blank">@flowersayo</a> |
|  5   | 9/7  | 16장 프로퍼티 어트리뷰트, 17장 생성자 함수에 의한 객체 생성, 18장 함수와 일급 객체 | <a href="https://github.com/dooli1971039" target="_blank" width="160">@dooli1971039</a> |
|  6   | 9/11 | 19장 프로토타입, 20장 strict mode | <a href="https://github.com/YelynnOh" target="_blank" width="160">@YelynnOh</a> |
|  7   | 9/14 | 21장 빌트인 객체, 22장 this, 23장 실행 컨텍스트 | <a href="https://github.com/helpingstar" target="_blank" width="160">@helpingstar</a> |
|  8   | 9/18 | 24장 클로저, 25장 클래스, 26장 ES6 함수의 추가 기능 | <a href="https://github.com/flowersayo" target="_blank">@flowersayo</a> |
|  9   | 9/21 | 27장 배열, 28장 Number, 29장 Math, 30장 Date, 31장 RegExp | <a href="https://github.com/dooli1971039" target="_blank" width="160">@dooli1971039</a> |
| 10   | 9/25 | 32장 String, 33장 7번째 데이터 타입 Symbol, 34장 이터러블 | <a href="https://github.com/YelynnOh" target="_blank" width="160">@YelynnOh</a> |
| 11   | 10/2 | 35장 스프레드 문법, 36장 디스트럭처링 할당, 37장 Set과 Map | <a href="https://github.com/helpingstar" target="_blank" width="160">@helpingstar</a> |
| 12   | 10/5 | 38장 브라우저의 렌더링 과정 | <a href="https://github.com/flowersayo" target="_blank">@flowersayo</a> |
| 13   | 10/12 | 39장 DOM | <a href="https://github.com/dooli1971039" target="_blank" width="160">@dooli1971039</a> |
| 14   | 10/16 | 40장 이벤트 | <a href="https://github.com/YelynnOh" target="_blank" width="160">@YelynnOh</a> |
| 15   | 10/19 | 41장 타이머, 42장 비동기 프로그래밍, 43장 Ajax | <a href="https://github.com/helpingstar" target="_blank" width="160">@helpingstar</a> |
| 16   | 10/23 | 44장 REST API, 45장 프로미스 | <a href="https://github.com/flowersayo" target="_blank" width="160">@flowersayo</a> |
| 17   | 10/26 | 46장 제너레이터와 async/await, 47장 에러처리, 48장 모듈 | <a href="https://github.com/dooli1971039" target="_blank" width="160">@dooli1971039</a> |


(9.28 추석 10.9 한글날 빨간날로 쉬어갑니다.)



## Rules
- 매주 월요일, 목요일 저녁 10시에 화상으로 만나서, 대표자 한명이 1주간 배웠던 내용에 대한 scrum을 진행합니다.
- 스터디 스크럼은 스터디 구성원들끼리 돌아가면서 진행합니다.
- 화상 스크럼 전날(일요일 23:59, 수요일 23:59)까지 과제를 노션에 제출합니다.
- 해당 주차를 학습하며 학습한 내용 & 실습했던 코드를 본 레포지토리에 업로드합니다. (선택)

- **벌금 제도**
    - 과제 미제출: 4,000원
    - 과제 지각제출(12시간 내): 2,000원
    - 스터디 스크럼 미진행: 4,000원
    - 스터디 결석: 2,000원
 => 누적된 벌금은 스터디 종료시에 구성원들에게 1/n 합니다.
