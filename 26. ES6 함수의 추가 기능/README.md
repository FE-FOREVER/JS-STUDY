# 핵심 개념 간략 정리: 26장 ES6 함수의 추가 기능

## 26.1 함수의 구분

- ES6 이전의 함수
  - 별다른 구분 없이 다양한 목적으로 함수를 사용
    - 일반적인 함수로서 호출, `new` 연산자와 함께 호출해 생성자 함수로서 호출, 객체에 바인딩된 메서드로서 호출
  - 모두 `callable` + `constructor` 다.
    - `constructor` 로 인해 생성자 함수로 호출 할 필요 없는 함수도 불필요한 프로토타입 객체를 생성하는 문제점 존재
  - ⇒ 사용 목적에 따른 명확한 구분이 없어 **호출 방식 제약 X** + 생성자 함수로 호출되지 않아도 **프로토타입 객체를 생성하는 문제**가 있었다.
- ES6 함수의 구분
  - 일반 함수, 메서드, 화살표 함수
  - **일반 함수**: 함수 선언문/함수 표현식으로 정의한 함수 (→ ES6 이전 함수와 차이 X)
  - **ES6 메서드/화살표 함수**: `non-constructor` (→ ES6 이전 함수와 명확한 차이 O)

## 26.2 메서드

- ES6 사양에서 메서드란 메서드 축약 표현으로 정의된 함수만을 의미

  ```jsx
  const obj = {
    x: 1,
    // foo는 메서드다.
    foo() {
      return this.x;
    },
    // bar에 바인딩된 함수는 메서드가 아닌 일반 함수다.
    bar: function () {
      return this.x;
    },
  };

  console.log(obj.foo()); // 1
  console.log(obj.bar()); // 1
  ```

- ES6 메서드의 특징
  - 인스턴스를 생성할 수 없는 `non-constructor`
    - 따라서 `prototype` 프로퍼티 X, 프로토타입 생성 X
  - 자신을 바인딩한 객체를 가리키는 내부슬롯 `[[HomeObject]]` 를 가진다.
    - `super` 키워드 사용 가능
- 본연의 기능인 `super` 를 추가 + 의미적으로 맞지 않는 기능인 `constructor` 는 제거
  - 메서드 정의 시 프로퍼티 값으로 익명함수 표현식을 할당하는 ES6 이전 방식은 사용하지 않는 것이 좋다.

## 26.3 화살표 함수

- 화살표(`=>`)를 사용해 기존 함수 정의 방식보다 **간략하게 함수를 정의**하는 방식
  - 표현뿐 아니라 **내부 동작 또한 간략**하다.
  - **콜백 함수 내부**에서 **this**가 **전역 객체를 가리키는 문제를 해결**하는 대안으로 유용하다.

### 26.3.1 화살표 함수 정의

- 함수 정의
  - 함수 표현식으로**만** 정의 가능
  - → `const multiply = (x, y) => x * y;`
- 매개변수 선언
  - 매개변수가 여러 개인 경우 → 소괄호 안에 매개변수 선언
  - 매개변수가 한 개인 경우 → 소괄호 생략 가능
  - 매개변수가 없는 경우 → 소괄호 생략 불가
- 함수 몸체 정의
  - 함수 몸체가 하나의 문으로 구성된 경우 → 함수 몸체를 감싸는 중괄호 생략 가능
  - 함수 몸체가 하나의 문이어도 표현식이 아닌 경우 → 중괄호 생략 불가
  - 객체 리터럴을 반환하는 경우 → 반드시 소괄호로 감싸줘야 함
    - `const create = (id, content) => ({ id, content });`
  - 함수 몸체가 여러 개의 문으로 구성된 경우 → 중괄호 생략 불가
    - 반환값도 명시적으로 반환 필요
- 화살표 함수의 또 다른 특징
  - 즉시 실행 함수로 사용 가능
  - 고차함수에 인수로 전달 가능 (→ 일급객체니까)
- ⇒ 화살표 함수는 콜백 함수로서 정의할 때 유용
  - 표현뿐 아니라 일반 함수의 기능 또한 간략화 + this도 편리하게 설계됨

### 26.3.2 화살표 함수와 일반 함수의 차이

- 화살표 함수는 인스턴스를 생성할 수 없는 `non-constructor`
- 화살표 함수는 중복된 매개변수 선언 불가
- 화살표 함수는 함수 자체의 `this`, `arguments`, `super`, `new.target` 바인딩을 갖지 않음
  - 화살표 함수 내부에서 위 4가지를 참조 시 스코프 체인을 통해 상위 스코프의 것을 참조하게 된다.

### 26.3.3 this

- 화살표 함수의 this는 일반 함수의 this와 다르게 동작
  - 화살표 함수는 콜백 함수 내부의 this 문제(→ 콜백 함수 내부의 this와 외부 함수의 this가 다른 것)를 해결하기 위해 의도적으로 설계된 것
- [콜백 함수 내부의 this 문제 예제로 살펴보기](https://github.com/FE-FOREVER/JS-STUDY/blob/main/26.%20ES6%20%ED%95%A8%EC%88%98%EC%9D%98%20%EC%B6%94%EA%B0%80%20%EA%B8%B0%EB%8A%A5/%EC%A1%B0%EB%AF%BC%EC%A7%80.md#%EC%BD%9C%EB%B0%B1-%ED%95%A8%EC%88%98-%EB%82%B4%EB%B6%80%EC%9D%98-this-%EB%AC%B8%EC%A0%9C)
- ES6 이전 “콜백 함수 내부의 this” 문제 해결법
  - **this**를 회피시켜 변수에 저장해 콜백 함수 내부에서 사용하기
  - `Array.prototype.map` 메서드 사용 시 두 번째 인수에 메서드를 호출한 인스턴스를 가리키는 **this** 전달하기
  - `Function.prototype.bind` 메서드를 사용해 메서드를 호출한 인스턴스를 가리키는 **this**를 바인딩하기
- ES6에서 “콜백 함수 내부 this” 문제 해결법
  - 화살표 함수 사용하기
  - 화살표 함수는 함수 자체의 **this**를 갖지 않고, 상위 스코프의 **this**를 그대로 참조한다. → **lexical this**
- 화살표 함수의 this 바인딩 더 알아보기
  - 화살표 함수끼리 서로 중첩된 경우, 스코프 체인 상에서 가장 가까운 화살표 함수가 아닌 상위 함수의 **this**를 참조
  - 화살표 함수가 전역 함수인 경우, 화살표 함수의 **this**는 전역 객체
  - 화살표 함수를 **프로퍼티에 할당**한 경우, 마찬가지로 **스코프 체인 상 가장 가까운** 화살표 함수가 아닌 **상위 함수의 this**를 참조
  - 화살표 함수는 `Function.prototype.call` , `Function.prototype.apply` , `Function.prototype.bind` 메서드를 사용해도 화살표 함수 내부의 **this** 교체 불가
    - 함수 자체에서 **this** 바인딩을 갖지 않기 때문이다.
  - [예제 코드와 함께 살펴보기](https://github.com/FE-FOREVER/JS-STUDY/blob/main/26.%20ES6%20%ED%95%A8%EC%88%98%EC%9D%98%20%EC%B6%94%EA%B0%80%20%EA%B8%B0%EB%8A%A5/%EC%A1%B0%EB%AF%BC%EC%A7%80.md#%ED%99%94%EC%82%B4%ED%91%9C-%ED%95%A8%EC%88%98%EC%9D%98-this-%EB%B0%94%EC%9D%B8%EB%94%A9-%EB%8D%94-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0)
- 메서드(객체 내 프로퍼티에 할당되는 함수)를 정의할 땐 ES6 메서드를 사용하자.

  - 화살표 함수가 가리키는 this가 기존 의도인 메서드를 호출한 객체를 가리키지 않기 때문이다.

  ```jsx
  const person = {
    name: 'Cho',
    sayHi() {
      console.log(`Hi ${this.name}`);
    },
  };

  person.sayHi(); // Hi Cho
  ```

- 일반적인 방식으로 프로토타입 객체의 프로퍼티를 동적 추가할 때도 화살표 함수가 아닌 일반 함수를 사용하자.

  - 위와 동일하게 this가 가리키는 것이 상이하다.

  ```jsx
  function Person(name) {
    this.name = name;
  }

  Person.prototype.sayHi = function () {
    console.log(`Hi ${this.name}`);
  };

  const person = new Person('Cho');
  person.sayHi(); // Hi Cho
  ```

- 클래스에서 메서드를 정의할 때 ES6 메서드를 사용하자.

  - 클래스 필드 정의 제안 방식으로 화살표 함수를 사용하면 인스턴스 메서드가 된다. ⇒ 프로토타입 메서드가 될 수 있게 ES6 메서드로 사용하자.

  ```jsx
  class Person {
    // 클래스 필드 정의
    name = 'Cho';

    sayHi() {
      console.log(`Hi ${this.name}`);
    }
  }
  const person = new Person();
  person.sayHi(); // Hi Cho
  ```

### 26.3.4 `super`

- 화살표 함수는 함수 자체의 `super` 바인딩을 갖지 않는다.
  - **this**와 마찬가지로 화살표 함수 내부에서 `super` 참조 시 상위 스코프의 `super` 를 참조
- `super` → 내부 슬롯 `[[HomeObject]]` 를 갖는 ES6 메서드내에서만 사용할 수 있는 키워드
- 화살표 함수 내부에서 `super` 키워드를 참조해도 에러가 발생하지 않고, 상위 스코프의 `super` 를 알아서 참조

### 26.3.5 `arguments`

- 화살표 함수는 함수 자체의 `arguments` 바인딩을 갖지 않는다.
  - **this**, `super` 와 마찬가지로 화살표 함수 내부에서 `arguments` 참조 시 상위 스코프의 `arguments` 를 참조
- `arguments` 객체는 함수 정의 시 가변 인자 함수 구현 시 유용
  - 화살표 함수에선 `arguments` 객체 참조 시 상위 스코프의 것을 참조하기에 자신에게 전달된 인수 목록은 확인 불가
  - 💡 화살표 함수에서 가변인자 함수를 구현하는 법 → Rest 파라미터 사용

## 26.4 Rest 파라미터

### 26.4.1 기본 문법

- 매개변수 이름 앞에 세 개의 점 `…` 을 붙여 정의한 매개변수
- 함수에 전달된 인수들의 목록을 **배열**로 전달받음
- 일반 매개변수와 함께 사용 가능
- 반드시 마지막 파라미터로 사용해야 함
- 단 하나만 선언해야 함
- 함수 객체의 `length` 프로퍼티에 영향을 주지 않음

### 26.4.2 Rest 파라미터 `argumets` 객체

- `arguments` 객체의 문제점 → 유사 배열 객체인 것
  - 배열 메서드 사용 시 배열로 변환해야 하는 번거로움 존재
- Rest 파라미터 → `arguments` 객체가 가진 문제점을 해결
  - 가변 인자 함수의 인수 목록을 배열로 직접 전달 받음
- 화살표 함수에선 `arguments` 객체, Rest 파라미터 모두 사용 가능
  - ‼️ But, 화살표 함수로 가변 인자 함수를 구현할 땐 반드시 Rest 파라미터를 사용하자.

## 26.5 매개변수 기본값

- 자바스크립트는 매개변수 개수만큼 인수를 전달하지 않아도 에러 발생 X
  - 자바스크립트 엔진의 경우 매개변수의 개수와 인수의 개수를 체크하지 않기 때문
  - 따라서 방어코드 필요 ⇒ 매개변수에 인수가 잘 전달되었는지 확인 & 인수가 전달되지 않은 경우 매개변수에 기본값을 할당하는 과정 필요
- ES6 이전 방어 코드

  ```jsx
  function sum(x, y) {
    // 인수가 전달되지 않아 매개변수의 값이 undefined인 경우 기본값을 할당한다.
    x = x || 0;
    y = y || 0;

    return x + y;
  }

  console.log(sum(1, 2)); // 3
  console.log(sum(1)); // 1
  ```

- ES6 매개변수 기본값 사용

  - 인수 체크 및 초기화 간소화 됨

  ```jsx
  function sum(x = 0, y = 0) {
    return x + y;
  }

  console.log(sum(1, 2)); // 3
  console.log(sum(1)); // 1
  ```

- ⚠️ 매개변수 기본값 관련 주의사항
  - 매개변수 기본값은 매개변수에 인수를 전달하지 않는 경우와 **undefined**를 전달한 경우에만 유효 (→ null은 해당하지 않는다.)
  - Rest 파라미터에는 기본값 지정 불가
  - 매개변수 기본값은 함수 객체의 `length` 프로퍼티, `arguments` 객체에 영향을 주지 않음

## 짚고 넘어가기

- [26.3.3 this](https://github.com/FE-FOREVER/JS-STUDY/blob/main/26.%20ES6%20%ED%95%A8%EC%88%98%EC%9D%98%20%EC%B6%94%EA%B0%80%20%EA%B8%B0%EB%8A%A5/%EC%A1%B0%EB%AF%BC%EC%A7%80.md#2633-this) - `Array.prototype.map` 말고 인수에 this로 콜백함수 내부에서 사용할 객체를 전달할 수 있는 메서드가 또 있을까?

  - `Array`의 [순회 메서드](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array#%EC%88%9C%ED%9A%8C_%EB%A9%94%EC%84%9C%EB%93%9C)에 속하는 경우 모두 아래와 같은 형태를 가진다.
    ```jsx
    method(callbackFn, thisArg);
    ```
  - 순회 메서드 종류
    - [`every()`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/every),[`filter()`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/filter),[`find()`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/find),[`findIndex()`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex),[`findLast()`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/findLast),[`findLastIndex()`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/findLastIndex),[`flatMap()`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/flatMap),[`forEach()`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach),[`map()`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/map),[`some()`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/some)
  - ✅ 이러한 순회 메서드에서 두 번째 인수인 `thisArg` 에 넘긴 값을 콜백 함수 내부에서 this로 사용한다.

- [26.5 매개변수 기본값](https://github.com/FE-FOREVER/JS-STUDY/blob/main/26.%20ES6%20%ED%95%A8%EC%88%98%EC%9D%98%20%EC%B6%94%EA%B0%80%20%EA%B8%B0%EB%8A%A5/%EC%A1%B0%EB%AF%BC%EC%A7%80.md#265-%EB%A7%A4%EA%B0%9C%EB%B3%80%EC%88%98-%EA%B8%B0%EB%B3%B8%EA%B0%92) - 기본값이 지정된 매개변수는 왜 매개변수의 개수를 나타내는 함수 객체의 `length` 프로퍼티에 포함되지 않을까?
  - `Function.prototype.length` → 함수가 기대하는 인자(매개변수)의 개수를 나타낸다.
    - 여기서 인자의 개수 → 함수 호출 시 필수적으로 전달해야 하는 인수를 의미한다. (⇒ 필수 인자)
  - 기본값이 정해진 매개변수는 인수가 전달되지 않아도 함수 실행에 문제가 없는 선택적 매개변수이기 때문이다.
  - ✅ `Function.prototype.length` 는 기본값이 명시되지 않은 매개변수의 개수만을 나타낸다.
