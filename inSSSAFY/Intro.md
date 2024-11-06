# JavaScript

- 브라우저 화면을 동적으로 만드는데 사용



## Browser

- URL로 웹을 탐색하며 서버와 통신, HTML 문서나 파일을 출력하는 GUI 기반의 소프트웨어
- Chrome, Firefox, Edge, Opera, Safari ...



### DOM(Document Object Model) 조작

- 문서(HTML,XML) 조작
- 문서를 객체로 구조화, 구조화된 구성 요소를 하나의 객체로 취급하여 다룸
  - key를 이용하여 접근
- 개발자 도구 console창
  - `document.title =  'DOM 조작'`

#### 대략적인 구조

- document 

  - html
    - head
      - title
    - body
      - h1
      - a

  

#### 파싱(Parsing)

- 구문 분석, 해석
- 브라우저가 문자열을 해석하여 DOM Tree로 만드는 과정



### BOM(Browser Object Model) 조작

- navigator, screen, location, frames, history, XHR

- 자바스크립트가 브라우저와 소통하기 위한 모델
- 브라우저 창이나 프레임을 추상화해서 프로그래밍적으로 제어 가능하게 하는 역할
- 개발자 도구 console 창 자체



### JavaScript Core (ECMAScript)

- Data Structure(Object,Array), Conditional Expression, Iteration

- BOM & DOM 을 조작하기 위한 명령어

#### ECMAScript

- ECMA
  - 정보 통신에 대한 표준을 제정하는 비영리 표준화 기구

- ECMAScript는 ECMA에서 ECMA-262* 규격에 따라 정의한 언어

  - ECMA-262* : 범용적인 목적의 프로그래밍 언어에 대한 명세

  

## JS 코딩 스타일

### 세미콜론(semicolon)

- `
- 선택적으로 사용 가능
  - 세미콜론이 없으면 ASI(Automatic Semicolon Insertion)에 의해 자동 삽입



### 코딩 스타일

- 일관성이 제일 중요
- 가이드
  - Airbnb Javascript Style Guide
  - Google Javascrpt Style Guide
  - standardjs

