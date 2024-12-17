## 정리
- **내부 슬롯과 내부 메서드**
  - 자바스크립트에서엔진의 구현 알고리즘을 설명하기 위해 ECMAScript 사양에서 사용하는 의사 프로퍼티와 메서드로, 이중 대괄호([[]])로 감싼 이름
  - 자바스크립트 엔진의 내부 로직으로, 특정 수단을 제외하고 일반적으로는 직접적으로 접근하거나 호출 불가능
  - ex. [[Prototype]] 내부 슬롯의 경우, __proto__를 통해 간접적으로 접근 가능

- **프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체**
  - 프로퍼티 상태
    - 프로퍼티 값, 값의 갱신 여부, 열거 가능 여부, 재정의 가능 여부
  - 프로퍼티 어트리뷰트
    - 자바스크립트 엔진이 관리하는 내부 상태 값인 내부 슬롯 `[[Value]]`, `[[Writable]]`, `[[Enumerable]]`, `[[Configurable]]`
    - 직접 접근 불가, `Object.getOwnPropertyDescriptor` 메서드를 사용하여 간접적으로 확인 가능
  - 프로퍼티 디스크립터 객체
    - 프로퍼티 어트리뷰트의 정보를 제공하는 객체

- **데이터 프로퍼티**
    - 키와 값으로 구성된 일반적인 프로퍼티
    - 지금까지 살펴본 모든 프로퍼티
    - `[[Value]]`, `[[Writable]]`, `[[Enumerable]]`, `[[Configurable]]`
      - 기본값은 프로퍼티 값 / true
- **접근자 프로퍼티**
    - 자체적으로는 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 프로퍼티
    - `[[Get]]`, `[[Set]]`, `[[Enumerable]]`, `[[Configurable]]`
    - getter, setter 함수

- **프로퍼티 정의**
- - 새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 명시적으로 정의하거나, 기존 프로퍼티의 프로퍼티 어트리뷰트를 재정의하는 것
    - ex. 프로퍼티 값을 갱신 가능하도록 할 것인지, 프로퍼티를 열거 가능하도록 할 것인지 등
  - Object.defineProperty or Object.defineProperties 메서드 사용

- **객체 변경 방지**
  - 객체 확장 금지(`Object.preventExtensions`)
  - 객체 밀봉(`Object.seal`)
  - 객체 동결(`Object.freeze`)
  - 각 메서드마다 객체의 변경을 금지하는 강도가 다름

## 짚고 넘어가기

**strict mode?**
- 자바스크립트에서 더 엄격한 문법과 실행 방식을 적용하도록 만드는 것
- ES5에서 도입, 오류 감지 쉬움 & 코드의 잠재적 문제 예방 도움
- 전역, 함수 단위로 선언해서 적용 가능

```jsx
function strict() {
  // 함수-레벨 strict mode 문법
  "use strict";
  function nested() {
    return "And so am I!";
  }
  return "Hi!  I'm a strict mode function!  " + nested();
}
function notStrict() {
  return "I'm not strict.";
}

```