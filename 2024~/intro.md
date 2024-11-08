# intro

- 잡다한 지식을 기록



## Node.js

- 구글 V8 자바스크립트 엔진으로 빌드된 자바스크립트 **런타임 환경**
- 브라우저의 자바스크립트 엔진에서만 동작하던 js를 브라우저 이외에서 동작하게 만듬
- FE / BE에서 같은 언어를 사용할 수 있다는 자점

- **비동기 I/O** 지원하며 **단일 스레드 이벤트 루프** 기반 동작
  - **단일 스레드**: 자바스크립트는 하나의 스레드에서 동작하기 때문에, 기본적으로 하나의 작업만 처리. 그러나 비동기 작업은 이벤트 루프를 통해 관리하여 병렬적으로 실행되는 것처럼 처리 가능
  - **이벤트 루프(Event Loop)**: 이벤트 루프는 대기 중인 비동기 작업을 순차적으로 확인하여, **완료된 작업을 콜백 형태로 자바스크립트 코드에 전달**
- 데이터를 실시간 처리하기 위해 I/O가 빈번하게 발생하는 **SPA**에 적합
  - SPA는 CBD(component based development)방법론을 기반으로 함
- CPU 사용율이 높은 애플리케이션에는 권장하지 않음

### 차이

- 브라우저는 HTML, CSS, JS를 실행해 웹페이지를 브라우저 화면에 렌더링 하는 것이 목적
- Node.js는 브라우저 외부에서 실행 환경을 제공하는 것이 목적

- ECMAScript를 모두 실행할 수 있지만, DOM API와 같이 추가로 제공되는 기능은 호환되지 않음
  - ex) 브라우저 알림창을 띄우는 alert 함수



## ECMAScript

- 자바스크립트의 기본 사양을  정의하는 표준
- JS는 ECMAScript + 브라우저가 별도로 지원하는 Web API(DOM, BOM, Canvas, ...)를 포함하는 개념