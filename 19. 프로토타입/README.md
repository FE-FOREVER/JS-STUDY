## 정리

### 19.1 ~ 19.7

- 자바스크립트는 객체 기반의 프로그래밍 언어이며, 자바스크립트를 이루고 있는 거의 “모든 것”이 객체
- 객체지향 프로그래밍
    - 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임
    - 객체의 상태를 나타내는 데이터(프로퍼티)와 상태 데이터를 조작할 수 있는 동작(메서드)을 하나의 논리적인 단위로 묶어서 생각
- 상속과 프로토타입
    - 프로토타입을 기반으로 상속을 구현하여 불필요한 중복 제거
    - 생생성자 함수가 생성한 모든 인스턴스는 자신의 프로토타입(===상위 객체의 역할을 하는 객체)의 모든 프로퍼티와 메서드를 상속 받은 후 동일한 메서드는 상속을 통해 공유해서 사용
- 프로토타입 객체
    - 프로토타입을 상속받은 하위 객체는 상위 객체의 프로퍼티를 자신의 프로퍼티처럼 사용 가능
    - 프로토타입은 자신의 `constructor` 프로퍼티를 통해 생성자 함수에 접근 가능
    - 생서자 함수는 자신의 `prototype` 프로퍼티를 통해 프로토타입에 접근 가능
    - 하위 객체들은 `__proto__` 접근자 프로퍼티를 통해 내부슬롯이 가리키는 프로토타입에 간접적으로 접근 가능
    - 객체가 생성될 때, 객체 생성 방식에 따라 프로토타입이 결정되고 `[[Prototype]]`에 저장됨 *19.6

  ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6ed1bb57-6799-4eb4-9443-b0ae6a81c22e/9e0726ba-3ccf-4e51-8a84-5e8d2ba890e3/image.png)

    - **`__proto__` 접근자 프로퍼티**
        - `__proto__` 는 접근자 프로퍼티
        - 상속을 통해 사용됨 → 객체 직접소유X, `Object.prototype`의 프로퍼티
    - **함수 객체의 prototype 프로퍼티**
        - 함수 객체만이 소유하는 프로퍼티로, 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킴
        - 생성자 함수로서 호출할 수 없는 함수는 소유X, 프로토타입 생성도 X

  **⇒ 모든 객체가 가지고있는 `__proto__` 접근자 프로퍼티와, 함수 객체만이 가지고 있는 `prototype` 프로퍼티는 프로퍼티를 사용하는 주체는 다르나, 결국 동일한 프로토타입을 가리킴**

  ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6ed1bb57-6799-4eb4-9443-b0ae6a81c22e/69bae894-360b-45e5-aa2a-60d4b9fe7c26/image.png)

    - **프로토타입의 `constructor` 프로퍼티와 생성자 함수**
        - 모든 프로토타입은 `constructor` 프로퍼티 소유
        - 자신을 참조하고 있는 생성자 함수를 가리키며, 이 연결은 생성자 함수가 생성될 때(===함수 객체가 생성될 때) 이뤄짐

  ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6ed1bb57-6799-4eb4-9443-b0ae6a81c22e/c77602b7-d104-4d6c-b68c-fd03d3d21cdc/image.png)

- **리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입**
    - 리터럴 표기법에 의해 생성된 객체도 프로토타입이 존재하나, 이 경우 프로토타입의 constructor 프로퍼티가 가리키는 생성자 함수가 반드시 객체를 생성한 생성자 함수라고 단정할 수 없음
        - 생성자 함수에 의해 생성된 인스턴스는 프로토타입의 constructor 프로퍼티에 의해 생성자 함수와 연결 → constructor 프로퍼티가 가리키는 생성자 함수는 인스턴스를 생성한 생성자 함수
        - 단, 큰 틀에서 봤을 때 생성자 함수로 생성한 객체와 객체 리터럴로 생성한 객체와 본질적인 면에서 차이X → 객체로서 동일한 특성을 가짐
    - 프로토타입과 생성자 함수는 언제나 쌍으로 존재
- **프로토타입의 생성 시점**
    - 생성자 함수가 생성되는 시점에 더불어 생성됨, 언제나 쌍으로 존재하기 때문
    - **사용자가 직접 정의한 사용자 정의 생성자 함수와 프로토타입 생성 시점**
        - `constructor`는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성 (런타임 이전)
        - 생성된 프로토타입은 생성자 함수의 `prototype` 프로퍼티에 바인딩

      **→ 사용자 정의 생성자 함수는 자신이 평가되어 함수 객체로 생성되는 시점에 프로토타입도 더불어 생성되며, 생성된 프로토타입은 언제나 `Object.prototype`**

    - **빌트인 생성자 함수와 프로토타입 생성 시점**
        - 모든 빌트인 생성자 함수는 전역 객체가 생성된느 시점에 생성
        - 생성된 프로토타입은 생성자 함수의 `prototype` 프로토타입에 바인딩

  **⇒ 생성자 함수 or 리터럴 표기법으로 객체를 생성하면 프로토타입은 생성된 객체의 `[[Prototype]]` 내부 슬롯에 할당되고, 생성된 객체는 프로토타입을 상속 받음**

- **객체 생성 방식과 프로토타입의 결정**
    - 다양한 방식으로 생성된 모든 객체는 모두 추상연산 OrdinaryObjectCreate에 의해 생성된다는 공통점 → 즉, 프로토타입은 추상연산 OrdinaryObjectCreate에 전달되는 인수에 의해 결정됨
    - **객체 리터럴에 의해 생성된 객체의 프로토타입**
        - Object.prototype을 프로토타입으로 갖게 되고, Object.prototype을 상속 받음
    - **Object 생성자 함수에 의해 생성된 객체의 프로토타입**
        - Object.prototype을 프로토타입으로 갖게 되고, Object.prototype을 상속 받음
    - **생성자 함수에 의해 생성된 객체의 프로토타입**
        - 생성자 함수에 의해 생성되는 객체의 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체
- **프로토타입 체인**
    - 생성자 함수에 의해 생성된 객체는 생성자 함수의 prototype뿐만 아니라 Object.prototype도 상속 받음

  ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/6ed1bb57-6799-4eb4-9443-b0ae6a81c22e/ed054155-c075-442f-9726-fd314b0dc288/image.png)

    - 자바스크립트는 객체의 프로퍼티에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면 `[[Prototype]]` 내부 슬롯의 참조를 따라 자신의 부모 역할을 하는 프로퍼티를 순차적으로 검색
    - 프로토타입 체인의 최상위에 위치하는 객체는 언제나 Object.prototype이므로, 모든 객체는 Object.prototype을 상속받음 → 프로토타입 체인의 종점
        - 프로토타입 체인의 종점까지 프로퍼티를 검색할 수 없는 경우 undefined 반환
    - **스코프 체인과 프로토타입 체인은 서로 연관없이 별도로 동작하는 것이 아닌, 서로 협력하여 식별자와 프로퍼티를 검색하는 데 사용**

---

## 19.8 오버라이딩과 프로퍼티 섀도잉

#### **프로퍼티 섀도잉**

- 인스턴스가 프로토타입과 같은 이름의 프로퍼티를 가지면 프로토타입의 프로퍼티는 가려짐

```javascript
const Person = function (name) {
  this.name = name;
};
Person.prototype.sayHello = function () {
  console.log(`Hello, I'm ${this.name}`);
};

const me = new Person('Lee');
me.sayHello = function () {
  console.log(`Hi, I'm ${this.name}`);
};

me.sayHello(); // Hi, I'm Lee
```

#### **오버라이딩과 오버로딩**

- **오버라이딩**: 하위 클래스에서 상위 클래스의 메서드를 재정의
- **오버로딩**: 같은 이름의 메서드를 매개변수 유형/개수에 따라 다르게 정의. (JS는 직접 지원하지 않음, `arguments`로 구현 가능)

#### **프로토타입 및 인스턴스 프로퍼티 삭제**

- 인스턴스에서 삭제된 프로퍼티는 프로토타입의 것을 참조

```javascript
delete me.sayHello;
me.sayHello(); // Hello, I'm Lee
```

## 19.9 프로토타입의 교체

#### **생성자 함수로 프로토타입 교체**

- 생성자 함수의 `prototype`을 변경해 새 객체의 프로토타입 설정

```javascript
function Person(name) {
  this.name = name;
}

Person.prototype = {
  constructor: Person,
  sayHello() {
    console.log(`Hi, I'm ${this.name}`);
  },
};

const me = new Person('Lee');
me.sayHello(); // Hi, I'm Lee
```

#### **인스턴스의 프로토타입 교체**

- `Object.setPrototypeOf` 또는 `__proto__`로 기존 객체의 프로토타입 교체

```javascript
const parent = {
  sayHello() {
    console.log(`Hello!`);
  }
};
Object.setPrototypeOf(me, parent);
me.sayHello(); // Hello!
```

## 19.10 instanceof 연산자

- **좌변 객체**가 **우변 생성자 함수의 프로토타입 체인**에 있으면 `true`

```javascript
console.log(me instanceof Person); // true
console.log(me instanceof Object); // true
```

- 프로토타입을 교체하면 `instanceof` 결과가 달라질 수 있음

```javascript
const parent = {};
Object.setPrototypeOf(me, parent);
console.log(me instanceof Person); // false
```

## 19.11 직접 상속

#### **Object.create로 직접 상속**

- 명시적으로 프로토타입 설정

```javascript
const parent = {
  sayHello() {
    console.log(`Hello from parent!`);
  }
};
const child = Object.create(parent);
child.sayHello(); // Hello from parent!
```

## 19.12 정적 프로퍼티/메서드

### **정적 프로퍼티/메서드란?**

- **정적 프로퍼티/메서드**는 인스턴스를 생성하지 않고도 생성자 함수 자체로 접근/호출할 수 있는 프로퍼티/메서드를 의미
- 생성자 함수가 객체로서 직접 소유

```javascript
// 생성자 함수 정의
function Person(name) {
  this.name = name;
}

// 정적 프로퍼티와 메서드 추가
Person.staticProp = 'I am static';
Person.staticMethod = function () {
  console.log('This is a static method');
};

// 사용 예시
console.log(Person.staticProp); // I am static
Person.staticMethod(); // This is a static method

// 인스턴스를 생성
const person1 = new Person('Lee');

// 정적 메서드는 인스턴스를 통해 호출할 수 없다.
console.log(person1.staticProp); // undefined
person1.staticMethod(); // TypeError: person1.staticMethod is not a function
```

### **정적 프로퍼티/메서드와 프로토타입 메서드 차이**

- **정적 메서드**: 생성자 함수에서 직접 호출 가능, 인스턴스에서 호출 불가능
- **프로토타입 메서드**: 인스턴스에서 호출 가능, 생성자 함수에서는 호출 불가능

```javascript
// 프로토타입 메서드 정의
Person.prototype.sayHello = function () {
  console.log(`Hello, I'm ${this.name}`);
};

// 인스턴스를 통해 프로토타입 메서드 호출
person1.sayHello(); // Hello, I'm Lee

// 생성자 함수에서는 호출 불가능
Person.sayHello(); // TypeError: Person.sayHello is not a function
```

## 19.13 프로퍼티 존재 확인

### **19.13.1 `in` 연산자**

- 객체 내 특정 프로퍼티가 존재하는지 확인
- 프로토타입 체인 전체를 검색

```javascript
const person = {
  name: 'Lee',
  age: 25,
};

console.log('name' in person); // true
console.log('toString' in person); // true (Object.prototype에서 상속)
```

### **19.13.2 `Object.prototype.hasOwnProperty`**

- 객체 자신의 프로퍼티만 확인, 상속받은 프로퍼티는 제외

```javascript
console.log(person.hasOwnProperty('name')); // true
console.log(person.hasOwnProperty('toString')); // false
```

---

## 19.14 프로퍼티 열거

### **19.14.1 `for...in` 문**

- 객체의 열거 가능한 프로퍼티를 순회하며 출력
- 상속받은 프로퍼티 포함

```javascript
const person = {
  name: 'Lee',
  age: 25,
};

for (const key in person) {
  console.log(key, person[key]); // name Lee, age 25
}
```

- 상속 프로퍼티 제외하기

```javascript
for (const key in person) {
  if (person.hasOwnProperty(key)) {
    console.log(key, person[key]); // name Lee, age 25
  }
}
```

---

### **19.14.2 `Object.keys`, `Object.values`, `Object.entries`**

- **`Object.keys()`**: 열거 가능한 키를 배열로 반환
- **`Object.values()`**: 열거 가능한 값을 배열로 반환
- **`Object.entries()`**: 키-값 쌍 배열을 반환

```javascript
const person = { name: 'Lee', age: 25 };

console.log(Object.keys(person)); // ['name', 'age']
console.log(Object.values(person)); // ['Lee', 25]
console.log(Object.entries(person)); // [['name', 'Lee'], ['age', 25]]

Object.entries(person).forEach(([key, value]) => {
  console.log(key, value); // name Lee, age 25
});
```
