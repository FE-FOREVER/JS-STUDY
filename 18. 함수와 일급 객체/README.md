# 18장 함수와 일급 객체

## 18.1 일급 객체

### 일급 객체의 조건

1. **무명의 리터럴로 생성** 가능
2. **변수나 자료구조에 저장** 가능
3. **함수의 매개변수에 전달** 가능
4. **함수의 반환값으로 사용** 가능

⇒ 자바스크립트의 <U>함수</U>는 위의 조건을 모두 만족하는 일급 객체‼️

### 일급 객체로서 함수가 가지는 가장 큰 특징

1. **함수의 매개변수에 전달** 가능
2. **함수의 반환값으로 사용** 가능

⇒ <U>함수형 프로그래밍</U>을 가능하게 하는 JS의 장점 중 하나‼️

## 18.2 함수 객체의 프로퍼티

1. **arguments** 프로퍼티: 호출 시 전달된 인수들의 정보를 나타내는 프로퍼티
2. **caller** 프로퍼티: 함수 자신을 호출한 함수를 가리키는 프로퍼티 (✔️ 비표준 프로퍼티)
3. **length** 프로퍼티: 함수 정의 시 선언한 매개변수의 개수를 나타내는 프로퍼티
4. **name** 프로퍼티: 함수 이름을 나타내는 프로퍼티
5. **\_\_proto**\_\_ 접근자 프로퍼티: [[Prototype]] 내부 슬롯이 가리키는 프로토타입 객체에 간접 접근하기 위한 프로퍼티
6. **prototype** 프로퍼티: 생성자 함수로 호출할 수 있는 함수 객체만이 소유하는 프로퍼티

## 짚고 넘어가기

1. `Object.hasOwnProperty()` 메소드는 객체 **고유의 프로퍼티** key인 경우에만 true를 반환한다.
   따라서 상속받은 프로퍼티 키인 경우 false를 반환한다.
2. 아래 코드에서 `makeCounter()` 내의 num 값이 유지되는 이유는 **클로저**와 연관되어있다고 한다.
   클로저 함수는 자신이 생성될 때의 환경을 기억하는 함수다. 이에 대해서는 24장에서 더 자세히 살펴보자.

   ```jsx
   const increase = function (num) {
     return ++num;
   };

   const decrease = function (num) {
     return --num;
   };

   const auxs = { increase, decrease };

   function makeCounter(aux) {
     let num = 0;

     return function () {
       num = aux(num);
       return num;
     };
   }

   const increaser = makeCounter(auxs.increase);
   const decreaser = makeCounter(auxs.decrease);

   console.log(increaser());
   console.log(decreaser());
   ```
