## 3장 자바스크립트 개발 환경과 실행 환경

- 자바스크립트의 실행 환경
  - 브라우저
    - 목적: HTML, CSS, JS를 실행해 웹 페이지를 브라우저 화면에 렌더링
    - ECMAScript와 클라이언트 사이드 Web API 지원
  - Node.js
    - 목적: 브라우저 외부에서 JS 실행 환경을 제공
    - ECMAScript와 Node.js 고유의 API 지원
  - Web API 구분의 필요성 → Web API는 브라우저 환경에서만 유효하기에 Node.js에선 실행 불가함을 알기
- 대표 웹 브라우저 크롬의 개발자 도구
  - Elements, Console, Sources, Network, Application 패널로 다양한 기능 제공
  - Console 패널은 구현단계 중 실행 결과를 보다 간편하게 확인 가능 + REPL 환경으로 사용 가능
- 브라우저에서 JS 실행 방법
  - HTML 파일 내부에 `<script>` 태그 이용
- Node.js
  - Node.js: 브라우저 이외 환경에서도 JS를 동작할 수 있는 자바스크립트의 실행 환경
  - npm: 자바스크립트 패키지 매니저
- Node.js의 REPL

  - Node.js는 Read Eval Print Loop 제공 → 간단한 JS 코드를 실행 후 결과 확인까지 가능

- 코드 에디터와 확장 플러그인
  - 코드 에디터: **코드 자동 완성, 문법 오류 감지, 디버깅, Git 연동** 등 다양하고 편리한 기능 활용 가능
  - Code Runner 확장 플러그인: 단축키를 사용해 편리하게 현재 표시 중인 JS 파일을 실행 가능
  - Live Server 확장 플러그인: 가상 서버를 기동하여 소스코드의 수정 사항까지 자동 반영

---

## Q&A

- NPM 이랑 YARN 차이
