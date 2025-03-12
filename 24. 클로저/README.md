# 24장. 클로저

- **함수와 그 함수가 선언된 렉시컬 환경과의 조합**
- 자바스크립트 고유의 개념이 아닌, 함수를 일급 객체로 취급하는 함수형 프로그래밍 언어에서 사용되는 특성
- 자바스크립트는 렉시컬 스코프를 따르는 프로그래밍 언어

## 24.1 렉시컬 스코프

- 자바스크립트 엔진은 어디서 호출했는지가 아닌, 함수를 어디서 정의했는지에 따라 상위 스코프를 결정함 *13.5 렉시컬 스코프
- “함수의 상위 스코프를 결정한다” === “렉시컬 환경의 외부 렉시컬 환경에 대한 참조에 저장할 참조값을 결정한다”
- **렉시컬 환경의 외부 렉시컬 환경에 대한 참조에 저장할 참조값, 즉 상위 스코프에 대한 참조는 함수 정의가 평가되는 시점에 함수가 정의된 환경(위치)에 따라 결정됨**

## 24.2 함수 객체의 내부슬롯 **`[[Environment]]`**

- 함수가 정의된 환경(위치)과 호출되는 환경(위치)는 다를 수 있으므로 렉시컬 스코프가 가능하려면 함수는 자신이 호출되는 환경과 상관없이 자신이 정의된 환경, 즉 상위스코프를 기억해야 함
- **함수 객체의 내부 슬롯`[[Environment]]`에 저장된 현재 실행 중인 실행 컨텍스트의 렉시컬 환경의 참조가 바로 상위 스코프**
  - 자신이 호출되었을 때 생성될 함수 렉시컬 환경의 “외부 렉시컬 환경에 대한 참조”에 저장될 참조값
- **함수 객체는 내부 슬롯`[[Environment]]`에 저장한 렉시컬 환경의 참조, 즉 상위 스코프를 자신이 존재하는 한 기억함**

## 24.3 클로저와 렉시컬 환경

- 클로저
  - 외부 함수보다 중첩 함수가 더 오래 유지되는 경우, 중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수 참조 가능 → 이러한 중첩 함수를 클로저라고 함
  - 자바스크립트의 모든 함수는 자신의 상위 스코프를 기억하며, 이는 함수를 어디서 호출하든 상관없이 유지되므로 함수는 언제나 자신이 기억하는 상위 스코프의 식별자 참조 및 식별자에 바인딩된 값 변경이 가능

**클로저 예제**

```jsx
const x = 1;

// (1)
function outer() {
  const x = 10;
  const inner = function () {
    console.log(x);
  }; // (2)
  return inner;
}

// outer 함수를 호출하면 중첩 함수 inner를 반환
// 그리고, outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 팝되어 제거됨
const innerFunc = outer(); // (3)
innerFunc(); // (4) 10
```

→ outer 함수를 호출(3)하면 outer 함수는 중첩 함수 inner를 반환하고, 생명주기를 마감함. 즉, outer 함수의 실행이 종료되면 outer 함수의 실행 컨텍스트는 스택에서 제거됨. 이때 outer
함수의 지역 변수 x와 변수 값 10을 저장하고 있던 outer 함수의 실행 컨텍스트가 제거되었으므로 outer 함수의 지역 변수 x 또한 생명주기를 마감함. 따라서 outer 함수의 지역 변수 x는 더는 유효하지
않게 되어 x 변수에 접근 할 수 있는 방법이 없는 것처럼 보임.

하지만, 위 코드의 실행 결과(4)는 outer 함수의 지역 변수 x의 값인 10이고, 이미 생명 주기가 종료되어 실행 컨텍스트 스택에서 제거된 outer 함수의 지역 변수 x가 다시 부활이라도 한 듯이 동작하고
있음……

1. outer 함수가 평가되어 함수 객체를 생성할 때(1) 현재 실행 중인 실행 컨텍스트의 렉시컬 환경, 즉 전역 렉시컬 환경을 outer 함수 객체의`[[Environment]]`내부 슬롯에 상위 스코프로서
   저장함.
2. outer 함수를 호출하면 outer 함수의 렉시컬 환경이 생성되고 앞서 outer 함수 객체의`[[Environment]]`내부 슬롯에 저장된 전역 렉시컬 환경을 outer 함수 렉시컬 환경의 “외부 렉시컬
   환경에 대한 참조”에 할당
3. 중첩 함수 inner가 평가(2)되고(함수 표현식 정의므로 런타임에 평가), 이때 중첩함수 inner는 자신의`[[Environment]]`내부 슬롯에 현재 실행 중인 실행 컨텍스트의 렉시컬 환경, 즉
   outer 함수의 렉시컬 환경을 상위 스코프로서 저장
4. outer 함수 실행이 종료되고 반환하면서 outer 함수의 생명 주기도 종료됨(3). 즉, outer 함수의 실행 컨텍스트가 실행 컨텍스트 스택에서 제거됨. 이때 outer 함수의 실행 컨텍스트는 실행
   컨텍스트 스택에서 제거되지만 outer 함수의 렉시컬 환경까지 소멸하는 것은 아님. outer 함수의 렉시컬 환경은 inner 함수의`[[Environment]]`내부 슬롯에 의해 참조되고 있고, inner
   함수는 전역 변수 innerFunc에 의해 참조되고 있으므로 가비지 컬렉션의 대상이 되지 않음.
5. outer 함수가 반환한 inner 함수를 호출(4)하면 inner 함수의 실행 컨텍스트가 생성되고, 실행 컨텍스트 스택에 푸시됨. 그리고 렉시컬 환경의 외부 렉시컬 환경에 대한 참조에는 inner 함수
   객체의`[[Environment]]`내부 슬롯에 저장되어 있는 참조값이 할당됨.

⇒ 중첩 함수 inner는 외부 함수 outer보다 더 오래 생존했고, 이때 외부 함수보다 더 오래 생존한 중첩 함수는 외부 함수의 생존 여부(실행 컨텍스트 생존 여부)와 상관없이 자신이 정의된 위치에 의해 결정된
상위 스코프를 기억함. 이처럼 중첩 함수 inner 내부에서는 상위 스코프를 참조 및 상위 스코프 식별자 참조, 식별자 값 변경 가능

- **중첩 함수가 상위 스코프의 식별자를 참조하고 있고, 중첩 함수가 외부 함수보다 더 오래 유지되는 경우에 한정하는 것이 일반적**
  - 상위 스코프의 식별자 중에서 클로저가 참조하고 있는 식별자만을 기억함
- 자유 변수
  - 클로저에 의해 참조되는 상위 스코프의 변수
  - 클로저란 자유 변수에 묶여있는 함수

## 24.4 클로저의 활용

> 클로저는 `상태`(state)를 **안전하게 변경하고 유지**하기 위해 사용한다.
>
>
> 상태가 의도치 않게 변경되지 않도록 상태를 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용하기 위해 사용한다.
>

1. `카운트 상태`(num 변수의 값)는 increase 함수가 호출되기 전까지 **변경되지 않고 유지**되어야 한다.
2. 이를 위해 `카운트 상태`(num 변수의 값)는 increase 함수만이 변경할 수 있어야 한다.

`카운트 상태`는 **전역 변수를 통해 관리**되고 있기 때문에 언제든지 누구나 접근할 수 있고 변경할 수 있다. (암묵적 결합)

```jsx
// 카운트 상태 변수
let num = 0;

// 카운트 상태 변경 함수
const increase = function () {
  // 카운트 상태를 1만큼 증가시킨다.
  return ++num;
}

console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3
```

- 이는 의도치 않게 상태가 변경될 수 있다.
- 만약 누군가에 의해 의도치 않게 `카운트 상태`(전역 변수 num)의 값이 변경되면 → 오류 발생

전역 변수 `num`을 **increase 함수의 지역 변수로** 바꾸어 의도치 않은 상태 변경을 방지해보자.

```jsx
// 카운트 상태 변경 함수
const increase = function () {
  // 카운트 상태 변수
  let num = 0;

  // 카운트 상태를 1만큼 증가시킨다.
  return ++num;
};

// 이전 상태를 유지하지 못한다.
console.log(increase()); // 1
console.log(increase()); // 1
console.log(increase()); // 1
```

- 카운트 상태를 안전하게 변경하고 유지하기 위한 전역 변수 `num`을 increase 함수의 지역 변수로 변경하여 의도치 않은 상태 변경은 방지했다.
  - 이제 num 변수의 `상태`는 increase 함수만이 변경할 수 있다.
- 하지만 increase 함수가 호출될 때마다 지역 변수 num은 다시 선언되고 0으로 초기화되기 때문에 출력 결과는 언제나 1이다.
  - 다시 말해, **상태가 변경되기 이전 상태를 유지하지 못한다**.

이전 **상태를 유지할 수 있도록 클로저를 사용**해보자.

```jsx
// 카운트 상태 변경 함수
const increase = (function () {
  // 카운트 상태 변수
  let num = 0;

  // 클로저
  return function () {
    // 카운트 상태를 1만큼 증가시킨다.
    return ++num;
  };
}());

console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3
```

- 위 코드가 실행되면 즉시 실행 함수가 호출되고 즉시 실행 함수가 반환한 함수가 increase 변수에 할당된다.
- increase 변수에 할당된 함수는 자신이 정의된 위치에 의해 결정된 **상위 스코프**인 **즉시 실행 함수의 렉시컬 환경**을 기억하는 **클로저**다.

## 24.5 캡슐화와 정보 은닉

> **캡슐화**
>
> - 객체의 상태를 나타내는 `프로퍼티`와 프로퍼티를 참조하고 조작할 수 있는 동작인 `메서드`를 하나로 묶는 것을 말한다.
> - 캡슐화는 객체의 특정 프로퍼티나 메서드를 감출 목적으로 사용하기도 하는데 이를 **정보 은닉**이라 한다.

> **정보 은닉**
>
> - 외부에 공개할 필요가 없는 구현의 일부를 외부에 공개되지 않도록 감추어 적절치 못한 접근으로부터 객체의 상태가 변경되는 것을 방지해 **정보를 보호**
> - 객체 간의 상호 의존성, 즉 결합도를 낮춘다.

```jsx
const me = new Person('Lee', 20);
me.sayHi(); // Hi! My name is Lee. I am 20.

const you = new Person('Kim', 30);
you.sayHi(); // Hi! My name is Kim. I am 30.

// _age 변수 값이 변경된다!
me.sayHi(); // Hi! My name is Lee. I am 30.
```

- 이는 `Person.prototype.sayHi` 메서드가 단 한 번 생성되는 클로저이기 때문에 발생하는 현상이다.
  - `Person.prototype.sayHi` 메서드는 즉시 실행 함수가 호출될 때 생성된다.
- 이때 `Person.prototype.sayHi` 메서드는 자신의 상위 스코프인 즉시 실행 함수의 실행 컨텍스트의 렉시컬 환경의 참조를 `[[Environment]]` 에 저장하여 기억한다.
  - 따라서 Person 생성자 함수의 모든 인스턴스가 상속을 통해 호출할 수 있는 `Person.prototype.sayHi` 메서드의 상위 스코프는 어떤 인스턴스로 호출하더라도 하나의 동일한 상위 스코프를
    사용하게 된다.
- 이러한 이유로 Person 생성자 함수가 여러 개의 인스턴스를 생성할 경우 위와 같이 _age 변수의 상태가 유지되지 않는다.

- 이처럼 **JS는 정보 은닉을 완전하게 지원하지 않는다.**
  - 인스턴스 메서드를 사용한다면 자유 변수를 통해 private을 흉내 낼 수는 있지만 프로토타입 메서드를 사용하면 이마저도 불가능해진다.
  - ES6의 Symbol 또는 WeakMap을 사용하여 private한 프로퍼티를 흉내 내기도 했으나 근본적인 해결책이 되지는 않는다.
  - 2021년 1월 기준, TC39 프로세스의 stage 3(candidate)에는 클래스에 private 필드를 정의할 수 있는 새로운 표준 사양이 제안되어 있다.

    표준 사양으로 승급이 확실시되는 이 제안은 현재 최신 브라우저(Chrome 74 이상)와 최신 Node.js(버전 12 이상)에 이미 구현되어 있다.

    (* 25.7.4절 private 필드 정의 제안)

## 24.6 자주 발생하는 실수

**클로저를 사용하지 않은 경우**

```jsx
jsx
복사편집
var funcs = [];

for (var i = 0; i < 3; i++) {
  funcs[i] = function () {
    return i;
  };
}

for (var j = 0; j < funcs.length; j++) {
  console.log(funcs[j]()); // 3 3 3
}

```

- `var`는 **함수 레벨 스코프**이므로 `i`가 전역 변수로 동작하여 3으로 최종 업데이트됨
- 따라서 모든 함수가 `i`의 최종 값 3을 참조하게 되어 원하는 결과(0, 1, 2)가 나오지 않음

**클로저를 사용하여 해결**

```jsx
jsx
복사편집
var funcs = [];

for (var i = 0; i < 3; i++) {
  funcs[i] = (function (id) {
    return function () {
      return id;
    };
  })(i);
}

for (var j = 0; j < funcs.length; j++) {
  console.log(funcs[j]()); // 0 1 2
}

```

- 즉시 실행 함수를 사용해 `i`의 현재 값을 `id`에 저장하여 유지
- 함수가 반환될 때 `id`가 클로저에 의해 기억됨

**let을 사용한 해결 방법**

```jsx
jsx
복사편집
const funcs = [];

for (let i = 0; i < 3; i++) {
  funcs[i] = function () {
    return i;
  };
}

for (let i = 0; i < funcs.length; i++) {
  console.log(funcs[i]()); // 0 1 2
}

```

- `let`은 **블록 레벨 스코프**를 가지므로 반복문이 실행될 때마다 새로운 `i`가 생성됨
- `i`가 반복문 실행마다 개별적으로 유지되므로 원하는 결과가 나옴

**함수형 프로그래밍 기법을 활용한 해결 방법**

```jsx
jsx
복사편집
const funcs = Array.from(new Array(3), (_, i) => () => i);

funcs.forEach(f => console.log(f())); // 0 1 2

```

- `Array.from()`과 **고차 함수**를 활용하여 간결하게 해결
- `(_, i) => () => i` 형태로 화살표 함수 내부에서 `i`를 클로저로 유지

---

# 짚고 넘어가기

### 24.5절 캡슐화와 정보 은닉

**즉시 실행 함수**를 사용하여 Person 생성자 함수와 Person.prototype.sayHi 메서드를 하나의 함수 내에 모으는 방법

> 1. Person 생성자 함수가 여러 개의 인스턴스를 생성할 경우 왜 **_age 변수의 상태가 유지되지 않는가?**
>

```jsx
const Person = (function () {
  let _age = 0; // private

  // 생성자 함수
  function Person(name, age) {
    this.name = name; // public
    _age = age;
  }

  // 프로토타입 메서드
  Person.prototype.sayHi = function () {
    console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
  };

  // 생성자 함수를 반환
  return Person;
}());

const me = new Person('Lee', 20);
me.sayHi(); // Hi! My name is Lee. I am 20.
console.log(me.name); // Lee
console.log(me._age); // undefined

const you = new Person('Kim', 30);
me.sayHi(); // Hi! My name is Kim. I am 30.
console.log(you.name); // Kim
console.log(you._age); // undefined
```

```jsx
const me = new Person('Lee', 20);
me.sayHi(); // Hi! My name is Lee. I am 20.

const you = new Person('Kim', 30);
you.sayHi(); // Hi! My name is Kim. I am 30.

// _age 변수 값이 변경된다!
me.sayHi(); // Hi! My name is Lee. I am 30.
```

- 이는 `Person.prototype.sayHi` 메서드가 단 한 번 생성되는 클로저이기 때문에 발생하는 현상이다.
  - `Person.prototype.sayHi` 메서드는 즉시 실행 함수가 호출될 때 생성된다.
- 이때 `Person.prototype.sayHi` 메서드는 자신의 상위 스코프인 즉시 실행 함수의 실행 컨텍스트의 렉시컬 환경의 참조를 `[[Environment]]` 에 저장하여 기억한다.
  - 따라서 Person 생성자 함수의 모든 인스턴스가 상속을 통해 호출할 수 있는 `Person.prototype.sayHi` 메서드의 상위 스코프는 어떤 인스턴스로 호출하더라도 하나의 동일한 상위 스코프를
    사용하게 된다.
- 이러한 이유로 Person 생성자 함수가 여러 개의 인스턴스를 생성할 경우 위와 같이 _age 변수의 상태가 유지되지 않는다.

> **2. _age와는 다르게 name은 왜 값이 유지되는가?**
>

- `this.name = name;` → **각 인스턴스의 고유한 프로퍼티로 저장된다.**
- `me.name`과 `you.name`은 서로 독립적이라 덮어쓰이지 않는다.