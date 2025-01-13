## **핵심 개념 간략 정리: 20장 strict mode**

### strict mode의 등장

- 의도치 않은 암묵적 전역과 같은 잠재적 오류를 발생 시키기 어려운 개발 환경 구성을 위함 ⇒ ES5부터 지원
  - 암묵전 전역: 선언하지 않고 사용한 변수가 전역 객의 프로퍼티로 동적 생성되는 것
- 자바스크립트 언어의 문법을 조금 더 엄격히 적용
- **ESLint**: strict mode와 유사하지만 더 강제성이 뛰어난 도구

### strict mode의 적용

- 전역 선두 or 함수 몸체 선두에 `'use strict';` 추가
- strict mode는 즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 가장 바람직
- 암묵적 전역, 식별자(변수, 함수, 매개변수) 삭제, 매개변수 이름 중복 등의 상황에서 에러 발생
- 일반 함수에서 this 바인딩 값, arguments 객체 갱신 등에 있어 엄격 모드일 때와 아닐 때의 차이점 존재

## 짚고 넘어가기

- [**20.5.2**](https://github.com/FE-FOREVER/JS-STUDY/blob/main/20.%20strict%20mode/%EC%A1%B0%EB%AF%BC%EC%A7%80.md#2052-%EB%B3%80%EC%88%98-%ED%95%A8%EC%88%98-%EB%A7%A4%EA%B0%9C%EB%B3%80%EC%88%98%EC%9D%98-%EC%82%AD%EC%A0%9C) - 엄격 모드에서 `delete` 연산은 왜 에러가 발생할까?
  - 엄격 모드에서 `delete` 는 객체의 프로퍼티를 삭제할 때만 사용 가능한 것
    - 예제에선 비정규화된 식별자(변수, 함수, 함수 매개변수 등)를 삭제하려고 했기에 에러가 발생
- [**20.6.1**](https://github.com/FE-FOREVER/JS-STUDY/blob/main/20.%20strict%20mode/%EC%A1%B0%EB%AF%BC%EC%A7%80.md#2061-%EC%9D%BC%EB%B0%98-%ED%95%A8%EC%88%98%EC%9D%98-this) - 일반 함수의 this가 strict mode가 아닌 경우엔 어떤 값이 바인딩됐는지 리마인드
  - 브라우저에선 window, Node.js에선 global 전역 객체
