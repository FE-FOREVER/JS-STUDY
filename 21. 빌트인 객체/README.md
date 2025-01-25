# 21장 빌트인 객체

## 21.1 자바스크립트 객체의 분류

자바스크립트 객체는 크게 3가지로 분류된다

1. **표준 빌트인 객체 (Standard Built-in Objects)**

- **ECMAScript 사양에 정의된 객체**로, 공통 기능을 제공한다.
- JS 실행 환경에 관계없이 언제나 사용할 수 있으며, 전역 객체의 프로퍼티로 제공되어 별도의 선언 없이 참조 가능

2. **호스트 객체 (Host Objects)**

- ECMAScript 사양에 없지만 JS 실행 환경(브라우저, Node.js)에서 제공하는 객체
- **브라우저**: DOM, BOM, fetch, Web API 등
- **Node.js**: Node.js 고유 API

3. **사용자 정의 객체 (User-defined Objects)**

- 표준 빌트인 객체나 호스트 객체가 아닌, 사용자가 직접 정의한 객체

## 21.2 표준 빌트인 객체

- 약 40개의 표준 빌트인 객체 제공
  - `Object`, `String`, `Number`, `Boolean`, `Symbol`, `Date`, `Math`, `Array`, `Map`, `Set`, `Function`, `Promise`, `Reflect`, `Proxy`, `JSON`, `Error`
    등
- 대부분 생성자 함수로 인스턴스를 생성할 수 있음 (`Math`, `Reflect`, `JSON` 제외)

### **생성자 함수 객체의 특징**

- `프로토타입 메서드`와 `정적 메서드`를 제공
- 생성된 인스턴스는 표준 빌트인 객체의 `prototype` 프로퍼티에 바인딩된 객체를 상속받음

    ```jsx
    const strObj = new String("Lee");
    console.log(Object.getPrototypeOf(strObj) === String.prototype); // true
    
    ```

### **정적 메서드**

- 인스턴스 없이 호출 가능
- `Number.isInteger()`, `JSON.stringify()`

- 생성자 함수로 객체 생성

    ```jsx
    const strObj = new String("Lee"); // String {"Lee"}
    const numObj = new Number(123); // Number {123}
    const boolObj = new Boolean(true); // Boolean {true}
    const arr = new Array(1, 2, 3); // Array [1, 2, 3]
    
    ```

- 프로토타입 메서드와 정적 메서드

    ```jsx
    const numObj = new Number(1.5);
    console.log(numObj.toFixed()); // "2" (프로토타입 메서드)
    console.log(Number.isInteger(0.5)); // false (정적 메서드)
    
    ```

## 21.3 원시값과 래퍼 객체

### **래퍼 객체 (Wrapper Object)**

- 원시값(문자열, 숫자, 불리언 등)에 대해 **객체처럼 동작하도록 JS 엔진이 임시로 생성하는 객체**
- 원시값 자체는 객체가 아니므로 프로퍼티나 메서드를 가질 수 없지만, **객체처럼 접근하면** JS 엔진이 암묵적으로 관련된 래퍼 객체를 생성해준다

### **작동 방식**

- **원시값에 객체처럼 접근**
  - JS 엔진이 임시 래퍼 객체를 생성하여 프로퍼티와 메서드를 사용할 수 있도록 한다
  - 사용이 끝나면 래퍼 객체는 삭제되고, 원시값으로 되돌아감

    ```jsx
    const str = 'hello';
    console.log(str.length); // 5
    console.log(str.toUpperCase()); // "HELLO"
    ```

  - 문자열에 접근 시 `String` 래퍼 객체 생성 → 메서드 실행 → 다시 원시값으로 복귀

### **래퍼 객체의 특징**

- **임시 객체**로 생성되며, 프로토타입 메서드와 프로퍼티를 참조할 수 있음
  - `String.prototype`, `Number.prototype` 등의 메서드 사용 가능
- 처리가 끝난 후 래퍼 객체는 **가비지 컬렉션의 대상**이 됨

### **예시**

- 문자열: `String` 래퍼 객체

    ```jsx
    const str = 'hi';
    console.log(str.length); // 2
    console.log(str.toUpperCase()); // "HI"
    ```

- 숫자: `Number` 래퍼 객체

    ```jsx
    const num = 1.5;
    console.log(num.toFixed()); // "2"
    ```

- 불리언: `Boolean` 래퍼 객체 (하지만 실사용은 거의 없음)

- 래퍼 객체는 임시로 생성되므로 **동적으로 추가된 프로퍼티는 유지되지 않음**

    ```jsx
    const str = 'hello';
    str.name = 'Lee'; // 동적 프로퍼티 추가
    console.log(str.name); // undefined
    ```

- **ES6의 Symbol도 래퍼 객체를 생성**.
- **null과 undefined는 래퍼 객체를 생성하지 않음** → 객체처럼 접근 시 에러 발생.

### **권장 사항**

- `String`, `Number`, `Boolean` 생성자 함수를 **new 연산자로 호출하는 것은 권장하지 않음**.
  - 기본적으로 원시값을 사용하고, 암묵적으로 생성되는 래퍼 객체를 활용하는 것이 더 효율적임.

## 21.4 전역 객체(Global Object)

- 코드 실행 이전에 JS 엔진에 의해 생성되는 최상위 객체
- **환경별 명칭**
  - 브라우저: `window`, `self`, `frames`
  - Node.js: `global`
  - ES11(ES2020) 표준: `globalThis` (환경 상관없이 사용 가능)

### **전역 객체의 특징**

1. 개발자가 생성 불가
2. 전역 객체의 프로퍼티 참조 시 `window`나 `global` 생략 가능
3. 표준 빌트인 객체와 환경별 호스트 객체, var로 선언된 전역 변수/함수를 포함
4. let, const로 선언한 전역 변수는 전역 객체의 프로퍼티로 등록되지 않음

---

### **빌트인 전역 프로퍼티**

1. **`Infinity`**: 양/음의 무한대 숫자
2. **`NaN`**: 숫자가 아님(Not-a-Number)을 나타내는 숫자
3. **`undefined`**: 정의되지 않은 값

---

### **빌트인 전역 함수**

1. **`eval`**

- 문자열로 된 JS 코드를 실행
- 보안과 성능 문제로 사용 금지 권장
- strict 모드에서 기존 스코프를 수정하지 않음

2. **`isFinite`**

- 유한수 여부 검사
- 숫자로 변환 후 검사

3. **`isNaN`**

- NaN 여부 검사
- 숫자로 변환 후 검사

4. **`parseFloat`**

- 문자열을 부동 소수점 숫자로 변환

5. **`parseInt`**

- 문자열을 정수로 변환
- 두 번째 인수로 진법(2~36) 지정 가능
- 반환값은 항상 10진수
