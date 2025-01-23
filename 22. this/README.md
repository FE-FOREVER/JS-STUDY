## **핵심 개념 간략 정리: 22장 this**

### this?

- 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 **자기 참조 변수**
  - 이를 이용해 가리키는 객체의 프로퍼티나 메서드 참조 가능
- 코드 어디에서든 참조 가능
- 자바스크립트에서 this는 함수 호출 방식에 따라 동적으로 결정 됨
  - this는 상황에 따라 가리키는 대상이 달라짐

### 함수 호출 방식과 this 바인딩

1. **일반 함수 호출: this → 전역 객체**
   - strict mode에선 undefined가 바인딩
   - 메서드 내부 중첩 함수 / 콜백 함수가 외부 함수가 아닌 전역 객체를 가리키는 문제점 존재
     - that 변수, `Function.prototype.apply/call/bind` 메서드, 화살표 함수를 활용해 this 바인딩을 메서드의 this 바인딩과 일치 시킬 수 있음
2. **메서드 호출: this → 메서드를 호출한 객체**
   - 메서드는 프로퍼티에 바인딩된 함수로 메서드를 소유한 객체(메서드를 가리키고 있는 객체)와 독립적으로 존재하는 별도의 객체임
3. **생성자 함수 호출: this → 생성자 함수가 생성할 인스턴스**
   - `new` 연산자 없이 함수를 호출하는 경우 일반 함수로 동작 함‼️
4. **`Function.prototype.apply/call/bind` 메서드에 의한 간접 호출: this → 메서드에 첫번째 인수로 전달한 객체**
   - 위 메서드는 `Function.prototype` 의 메서드라 모든 함수가 상속받아 사용 가능
   - **`Function.prototype.apply/call`** : 호출할 함수에 인수를 전달하는 방식(두 번째 인수)만 다를 뿐 this로 사용할 객체 전달 & 함수 호출의 역할은 동일
     - 유사 배열 객체에 배열 메서드를 사용하는 경우 유용하게 사용
   - **`Function.prototype.bind`**: 함수 호출 없이 첫 번째 인수의 값으로 this 바인딩이 교체된 함수를 새롭게 생성 및 반환
     - 메서드의 this와 메서드 내부 중첩 함수 or 콜백 함수의 this 불일치 문제 해결에 유용하게 사용

## 짚고 넘어가기

- **22.2.2** - 프로토타입 메서드 내부에서 사용된 this 예제에서 me 인스턴스의 name 프로퍼티의 값

  ```jsx
  function Person(name) {
    this.name = name;
  }

  Person.prototype.getName = function () {
    return this.name;
  };

  const me = new Person('Lee');

  // getName 메서드를 호출한 객체는 me다.
  console.log(me.getName()); // ① Lee

  Person.prototype.name = 'Kim';

  // getName 메서드를 호출한 객체는 Person.prototype이다.
  console.log(Person.prototype.getName()); // ② Kim

  // 기존 me 인스턴스의 name 프로퍼티 값은 변함없다.
  console.log(me.getName()); // Lee
  ```

  - `Person.prototype` 객체 자체에 name 프로퍼티를 새롭게 추가하면서 값을 “Kim”으로 할당했으므로 이후 기존 me 인스턴스의 name은 변함없이 Lee가 된다. ✔︎
