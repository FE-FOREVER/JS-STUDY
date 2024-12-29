## 정리

- **생성자 함수**
    - new 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수
    - 생성자 함수에 의해 생성된 객체 → 인스턴스
- **생성자 함수에 의한 객체 생성**
    - 동일한 프로퍼티를 갖는 객체를 여러 개 생성해야 하는 경우 매번 같은 프로퍼티를 기술해야 하는 객체 리터럴에 의한 객체 생성 방식의 문제점을 해결
    - 자바스크립트에서의 생성자 함수는 일반 함수와 동일한 방법으로 생성자 함수를 정의하고 new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작
- **생성자 함수의 인스턴스 생성 과정**
    1. 인스턴스 생성과 this 바인딩
    2. 인스턴스 초기화
    3. 인스턴스 반환
- **내부 메서드 `[[Call]]`과 `[[Construct]]`**

  ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6ed1bb57-6799-4eb4-9443-b0ae6a81c22e/28226e58-846f-4fb9-a610-5c065dd871a9/image.png)

    - 함수가 일반 함수로서 호출되면 함수 객체의 내부 메서드 `[[Call]]`이 호출되고 new 연산자와 함께 생성자 함수로서 호출되면 내부 메서드 `[[Construct]]`가 호출됨
    - 함수 선언문과 표현식으로 정의된 함수만이 constructor이고 ES6의 화살표 함수와 메서드 축약 표현으로 정의된 함수는 non-constructor
        - 생성자 함수로서 호출될 것을 기대하고 정의하지 않은 일반 함수(callable이면서 constructor)에 new 연산자를 붙여 호출하면 생성자 함수처럼 동작 가능
- **new.target**
    - 생성자 함수가 new 연산자 없이 호출되는 것을 방지하기 위해 ES6에서는 new.target을 지원

## 짚고 넘어가기

```jsx
// 생성자 함수
function Circle(radius) {
	this.radius = radius;
	this.getDiameter = function () {
		return 2 * this.radius;
	};
}

// 인스턴스 생성
const circle1 = new Circle(5);
const circle2 = new Circle(10);

const circle3 = Circle(15); // 일반 함수

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20

console.log(circle3); // undefined
console.log(radius); // 15, 일반 함수로 호출된 Circle 내의 this는 전역 객체
```

ㄴ 위 예제에서 circle3 을 출력하면 undefined를 반환하는데, radius를 출력하면 15를 반환하는 이유

→ new 연산자 없이 호출할 경우 일반 함수로 호출되고(⇒반환값x), undefined를 반환

→ 일반함수에서 this는 전역객체를 가리킴 → radius는 전역객체를 가리키는 것으로, 전역객체의 radius인 15를 반환