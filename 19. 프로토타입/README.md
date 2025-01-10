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
