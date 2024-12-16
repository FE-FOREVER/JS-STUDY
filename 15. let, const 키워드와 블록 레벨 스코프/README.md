# 15장 let, const 키워드와 블록 레벨 스코프

## 15.1 var 키워드로 선언한 변수의 문제점

### 15.1.1 변수 중복 선언 허용

```jsx
var x = 1;
var y = 1;

// var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용한다.
// 초기화문이 있는 변수 선언문은 JS 엔진에 의해 var 키워드가 없는 것처럼 동작한다.
var x = 100;
var y; // 초기화 문이 없는 변수 선언문은 무시

console.log(x); // 100
console.log(y); // 1
```

var 키워드로 선언한 변수를 **중복 선언**하면 초기화문(변수 선언과 동시에 초기값을 할당하는 문) 유무에 따라 다르게 동작한다.

`초기화문 O` → JS 엔진에 의해 var 키워드가 없는 것처럼 동작

`초기화문 X` → 변수 선언문은 무시, 에러는 발생X

동일한 이름의 변수가 이미 선언되어 있는 것을 모르고 변수를 중복 선언하면서 값까지 할당했다면 의도치 않게 먼저 선언된 변수 값이 변경되는 부작용이 발생한다.

### 15.1.2 함수 레벨 스코프

var 키워드로 선언한 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정한다.

따라서 함수 외부에서 var 키워드로 선언한 변수는 코드 블록 내에서 선언해도 모두 전역 변수가 된다.

```jsx
var x = 1;
if (true) {
  // x는 전역 변수로 이미 선언된 전역 변수 x가 있으므로 변수는 중복 선언된다.
  // 이는 의도치 않게 변수 값이 변경되는 부작용을 발생
  var x = 10;
}
console.log(x); // 10
```

```jsx
var i = 10;
// for문에서 선언한 i는 전역 변수다. 이미 i가 있으므로 중복 선언 된다.
for (var i = 0; i < 5; i++) {
  console.log(i); // 0 1 2 3 4
}
// 의도치 않게 i 변수의 값이 변경되었다.
console.log(i); // 5
```

### 15.1.3 변수 호이스팅

var 키워드로 변수를 선언하면 변수 호이스팅(* 4.4절 변수 선언의 실행 시점과 변수 호이스팅)에 의해 변수 선언문이 스코프의 선두로 끌어 올려진 것처럼 동작한다.

즉, 변수 호이스팅에 의해 var 키워드로 선언한 변수는 변수 선언문 이전에 참조할 수 있다. 단, 할당문 이전에 변수를 참조하면 undefined를 반환한다.

```jsx
// 변수 호이스팅에 의해 이미 foo 변수가 선언되었다. (1. 선언 단계)
// 변수 foo는 undefined로 초기화 된다. (2. 초기화 단계)
console.log(foo); // undefined

foo = 123; //변수에 값을 할당 (3. 할당 단계)
console.log(foo); // 123

var foo; // 변수 선언은 런타임 이전 JS 엔진에 의해 암묵적으로 실행된다.
```

변수 선언문 이전에 변수를 참조하는 것은 변수 호이스팅에 의해 에러를 발생시키지는 않지만 프로그램의 흐름상 맞지 않고, 가독성을 떨어트리고, 오류를 발생시킬 여지를 남긴다.

## 15.2 let 키워드

ES6에서 var 키워드의 단점을 보완하기 위해 새로운 키워드인 `let`과 `const`를 도입했다.

var 키워드와의 차이점을 중심으로 let 키워드를 살펴본다.

### 15.2.1 변수 중복 선언 금지

var 키워드로 이름이 동일한 변수를 중복 선언하면 아무런 에러가 발생하지 않는다. 이때 값까지 할당했다면 의도치 않게 먼저 선언된 변수 값이 재할당되어 변경되는 부작용이 발생한다.

하지만 let 키워드는 이름이 같은 변수를 중복 선언하면 문법 에러가 발생한다.

```jsx
var foo = 123;
var foo = 456; // 중복 선언 허용, var 키워드가 없는 것처럼 동작
let bar = 123;
let bar = 456; // SyntaxError: Identifier 'bar' has already been declared
```

### 15.2.2 블록 레벨 스코프

var 키워드로 선언한 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정하는 함수 레벨 스코프를 따른다.

let 키워드로 선언한 변수는 모든 코드 블록(함수, if 문, for 문, while 문, try/catch 문 등)을 지역 스코프로 인정하는 블록 레벨 스코프를 따른다.

```jsx
let foo = 1; // 전역 변수
{
  let foo = 2; // 지역 변수
  let bar = 3; // 지역 변수
}
console.log(foo); // 1
console.log(bar); // ReferenceError: bar is not defined
```

### 15.2.3 변수 호이스팅

var 키워드와 달리 let 키워드로 선언한 변수는 변수 호이스팅이 발생하지 않는 것처럼 동작한다.

→ 하지만 호이스팅이 실행되지 않는 것은 아니다.

```jsx
console.log(foo);
ReferenceError: foo
is
not
defined
let foo;
```

**var 키워드로 선언한 변수**

```jsx
// 런타임 이전에 선언 단계와 초기화 단계가 실행
// 변수 선언문 이전에 변수 참조 가능
console.log(foo); // undefined

var foo;
console.log(foo); // undefined

foo = 1; // 할당문에서 할당 단계가 실행
console.log(foo); // 1
```

![image](https://github.com/user-attachments/assets/cfbb31b5-dc05-4c74-8461-02ee5ed38235)

**let 키워드로 선언한 변수**

```jsx
// 런타임 이전에 선언 단계가 실행, 아직 초기화 X
// 초기화 이전의 일시적 사각지대에서는 변수를 참조할 수 없다.
console.log(foo); // ReferenceError: foo is not defined

let foo; // 변수 선언문에서 초기화 단계 실행
console.log(foo); // undefined

foo = 1; // 할당문에서 할당 단계가 실행
console.log(foo); // 1
```

![image](https://github.com/user-attachments/assets/67b0af24-5d22-48f0-8f9c-94a20bc34e5f)


let 키워드로 선언한 변수는 변수 호이스팅이 발생하지 않는 것처럼 보인다. 하지만 그렇지 않다.

```jsx
let foo = 1;
{
  console.log(foo); // ReferenceError: Cannot access 'foo' before initialization
  let foo = 2; // 지역 변수
}
```

- let 키워드로 선언한 변수의 경우 변수 호이스팅이 발생하지 않는다면 위 예제는 전역변수 foo의 값을 출력해야 한다.
- 하지만 let 키워드로 선언한 변수도 여전히 호이스팅이 발생하기 때문에 참조에러가 발생한다.

### 15.2.4 전역 객체와 let

- var 키워드로 선언한 `전역 변수와 전역 함수`, 그리고 `선언하지 않은 변수에 값을 할당한 암묵적 전역`은 전역 객체 **window의 프로퍼티가 된다**.
- 전역 객체의 프로퍼티를 참조할 때 window를 생략할 수 있다.

```jsx
var x = 1; // 전역 변수
y = 2; // 암묵적 전역
function foo() {
} // 전역 함수

// var 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티다.
console.log(window.x); // 1
// 전역 객체 window의 프로퍼티는 전역 변수처럼 사용할 수 있다.
console.log(x); // 1

// 암묵적 전역은 전역 객체 window의 프로퍼티다.
console.log(window.y); // 2
console.log(y); // 2

// 함수 선언문으로 정의한 전역 함수는 전역 객체 window의 프로퍼티다.
console.log(window.foo); // f foo() {}
// 전역 객체 window의 프로퍼티는 전역 변수처럼 사용할 수 있다.
console.log(foo); // f foo() {}
```

**let 키워드로 선언한 변수는 전역 객체의 프로퍼티가 아니다.**

즉, window.foo와 같이 접근할 수 없다.

let 전역 변수는 보이지 않는 개념적인 블록(전역 렉시컬 환경의 선언적 환경 레코드. * 23장 실행 컨텍스트)내에 존재하게 된다.

```jsx
let x = 1;

// let, const 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티가 아니다.
console.log(window.x); // undefined
console.log(x); // 1
```

## 15.3 const 키워드

const 키워드는 상수(constant)를 선언하기 위해 사용한다. 하지만 반드시 상수만을 위해 사용하지는 않는다.

const의 특징은 let 키워드와 대부분 동일하므로 let 키워드와 다른 점을 중심으로 살펴본다.

### 15.3.1 선언과 초기화

> const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야한다. 그렇지 않으면 문법 에러가 발생한다.
>

```jsx
const foo; // SyntaxError: Missing initializer in const declaration
```

let 키워드로 선언한 변수와 마찬가지로 블록 레벨 스코프를 가지며, 변수 호이스팅이 발생하지 않는 것처럼 동작한다.

```jsx
{
  console.log(foo); // ReferenceError: Cannot access 'foo' before initialization
  const foo = 1;
  console.log(foo); // 1
}

// 블록 레벨 스코프를 갖는다.
console.log(foo); // ReferenceError: foo is not defined
```

### 15.3.2 재할당 금지

var 또는 let 키워드로 선언한 변수는 재할당이 자유로우나 **const로 선언한 변수는 재할당이 금지된다.**

```jsx
const foo = 1;
foo = 2; // TypeError: Assignment to constant variable.
```

### 15.3.3 상수

const 키워드로 선언한 변수에 원시 값을 할당한 경우 변수 값을 변경할 수 없다. 원시 값은 변경 불가능한 값(immutable value)이므로 재할당 없이 값을 변경할 수 있는 방법이 없다.

이러한 특징을 이용해 const 키워드를 상수를 표현하는 데 사용하기도 한다.

`변수` ↔ `상수(재할당이 금지된 변수)`

- 상수도 값을 저장하기 위한 메모리 공간이 필요하므로 변수라고 할 수 있다.
- 변수는 언제든지 재할당을 통해 변수 값을 변경할 수 있지만 상수는 재할당이 금지된다.
- 상수는 상태 유지와 가독성, 유지보수의 편의를 위해 적극적으로 사용해야 한다.

const 키워드로 선언된 변수는 재할당이 금지된다.

**const 키워드로 선언된 변수에 원시 값을 할당한 경우 원시 값은 변경할 수 없는 값이고, const 키워드에 의해 재할당이 금지되므로 할당된 값을 변경할 수 있는 방법은 없다.**

일반적으로 상수의 이름은 대문자로 선언해 상수임을 명확히 나타낸다.

여러 단어로 이뤄진 경우에는 언더스코어(_)로 구분해서 스네이크 케이스로 표현하는 것이 일반적이다.

```jsx
const TAX_RATE = 0.1;
let preTaxPrice = 100;
let afterTaxPrice = preTaxPrice + (preTaxPrce * TAX_RATE);
console.log(afterTaxPrice); // 110
```

### 15.3.4 const 키워드와 객체

> const 키워드로 선언된 변수에 원시 값을 할당한 경우 값을 변경할 수 없다.
**하지만 const 키워드로 선언된 변수에 객체를 할당한 경우 값을 변경할 수 있다.**
>

변경 불가능한 값인 원시 값은 재할당 없이 변경(교체)할 수 있는 방법이 없지만 변경 가능한 값인 객체는 재할당 없이도 직접 변경이 가능하기 때문이다.

```jsx
const person = {
  name: 'Lee'
};

person.name = 'Kim';

console.log(person); // {name: "Kim"}
```

const 키워드는 재할당을 금지할 뿐, “불변”을 의미하지 않는다. (* 11.1.1절 변경 불가능한 값)

새로운 값을 재할당하는 것은 불가능하지만 프로퍼티 동적 생성, 삭제, 프로퍼티 값의 변경을 통해 객체를 변경하는 것은 가능하다. 이때 객체가 변경되더라도 변수에 할당된 참조 값은 변경되지 않는다.

## 15.4 var vs. let vs. const

변수 선언에는 기본적으로 const를 사용하고 let은 재할당이 필요한 경우에 한정해 사용하는 것이 좋다.

- ES6를 사용한다면 var 키워드는 사용하지 않는다.
-  변수를 선언할 때는 일단 const 키워드를 사용하자.
- 반드시 재할당이 필요하다면(반드시 재할당이 필요한지 한 번 생각해 볼 일이다) 그때 const 키워드를 let 키워드로 변경해도 결코 늦지 않다.

# 알아보기

1. 객체가 변경되더라도 변수에 할당된 참조 값은 변경되지 않는다.

- 객체는 주소값 저장 → 값은 변경될 수 있으나, 주소값은 변경 X

2. 암묵적 전역?

- 변수로 선언하지 않은 식별자에 값을 할당하는 경우, 참조 에러가 발생하지 않고 마치 전역 변수(전역 객체의 프로퍼티)로 사용할 수 있게 된다.
- 변수로 선언하지 않은 식별자란 어떠한 스코프에서도 해당 식별자에 대한 정보를 찾을 수 없음을 의미한다.
  - var, let, const 등의 키워드 사용 없이 식별자에 값이 할당된 경우 ex) `x = 10`
- 함수 내(혹은 다른 블록 스코프)에 존재하는 식별자여도 전역 변수로 취급된다.