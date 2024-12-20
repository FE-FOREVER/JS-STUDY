## 10장 객체 리터럴

### 객체

- 0개 이상의 프로퍼티로 구성된 집합
- 프로퍼티와 메서드로 구성된 집합체

### 객체 리터럴

- 가장 일반적이고 간단한 객체 생성 방법
- 객체를 생성하기 위한 표기법

### 프로퍼티

- 키와 값으로 구분
  - 프로퍼티 키: 빈 문자열 포함 모든 문자열 or 심벌 값 사용 가능
  - 프로퍼티 값: JS에서 사용할 수 있는 모든 값
- 프로퍼티 값의 갱신 가능
- 프로퍼티 동적 생성 가능
- 프로퍼티 삭제 가능

### 메서드

- 객체 내 프로퍼티의 값으로 사용되는 함수

### 프로퍼티 접근 방식

- 마침표 표기법
  - `객체로 평가되는 표현식.프로퍼티 키`
- 대괄호 표기법
  - `객체로 평가되는 표현식[프로퍼티 키]`
  - 프로퍼티 키는 반드시 따옴표로 감싼 문자열 형태일 것

### ES6에서 추가된 객체 리터럴의 확장 기능

- 프로퍼티 축약 표현
- 계산된 프로퍼티 이름
- 메서드 축약 표현

## 어려웠던 내용

- 객체에 존재하지 않는 프로퍼티에 접근하면 undefined를 반환한다.
  → 이 때 ReferenceError가 발생하지 않으므로 주의!
- 프로퍼티 키가 식별자 **네이밍 규칙을 준수하지 않는 이름**이면 `반드시 대괄호 표기법`을 사용해야한다.
- 프로퍼티 키가 숫자로 이루어진 문자열인 경우 따옴표를 생략할 수 있다.

## 궁금한 점

- 축약형 메서드일 때, 일반 프로퍼티 함수로 작성할 때의 차이점이 어떤 게 있을까? → 26장
- 프로퍼티 값의 메서드랑 클래스 메서드 차이? (객체 리터럴 / 클래스 객체)
