# DOM 조작과 Event

## DOM, BOM

### DOM

- HTML, XML과 같은 문서를 다루기 위한 문서 프로그래밍 인터페이스
- 문서를 구조화, 구조화된 구성 요소를 하나의 객체로 취급하여 다루는 논리적 트리 모델
- 단순한 속성 접근, 메서드 활용 + 프로그래밍 언어적 틍성을 활용한 조작 가능
- 객체
  - window : DOM을 표현하는 창, 가장 최상위 객체(생략가능), 브라우저상 탭 한개에 해당
  - document : 페이지 컨텐츠의 Entry Point 역할
  - navigator, location, history, screen ...

### BOM

- Browser Object Model
- 자바스크립트가 브라우저와 소통하기 위한 모델
- 브라우저의 창이나 프레임을 프로그래밍적으로 제어하는 수단



### DOM 조작

- 조작순서
  - 선택(Select)
  - 변경(Manipulation)

#### 객체 상속 구조

1. EventTarget
   - Event Listener를 가질 수 있는 객체가 구현하는 DOM 인터 페이스
2. Node
   - 여러 DOM 타입들이 사ㅣㅇ속하는 인터페이스
3. Element
   - Document 안의 모든 객체가 상속하는 범용적인 인터페이스

4. Document
   - 브라우저가 불러온 웹 페이지
   - DOM 트리의 진입점(entry point) 역할을 수행
5. HTML Element
   - 모든 종류의 HTML 요소
   - 부모 element 속성 상속

#### DOM 선택 메서드

```javascript
const h1 = document.querySelector('h1')
const h2 = document.querySelector('#idName')
const liTags = document.querySelectorAll('.className')
```

- **document.querySelector(selector)**
  - 제일먼저 제공한 선택자와 일치하는 element 객체 반환(없음면 null)
  - 단일 element
  - id, class, tag선택자 등 다양하게 사용이 가능하여 선호
- **document.querySelectorAll(selector)**
  - 제공한 선택자와 일치하는 모든 element 선택하여 **NodeList**반환
  - id, class, tag선택자 등 다양하게 사용이 가능하여 선호

- getElementById(id)
  - 단일 element
- getElementsByTagName(name)
  - HTMLCollection
- getElementsByClassName(names)
  - HTMLCollection



##### HTMLCollection & NodeList

- HTMLCollection
  - name, id, index 속성으로 항목 접근
- NodeList
  - index로만 항목에 접근 가능
  - forEach와 같은 메서드 사용 가능
  - Static Collection으로 실시간 반영x
    - HTMLCollection의 경우 반복문 진행시 항목이 늘어날 경우 추가로 반복하는 문제 발생

##### Collection

- Live Collection
  - 문서가 바뀔 때 실시간 업데이트
  - DOM의 변경사항을 collenction에 바로 반영
  - HTMLCollection, 일반적인 NodeList에 해당
- Static Collection
  - DOM이 변경되어도 collection 내용에는 영향을 주지 않음
  - querySelectorAll()의 반환 NodeList는 static collection



#### DOM 변경 메서드(Creation & append)

```javascript
const ulTag = document.querySelector('ul')
const newLiTag = document.createElement('li')
newLiTag.innerText = '추가할 리스트 태그'
ulTag.append(newLiTag)
```

- document.createElement()
  - 작성한 태그 명의 HTML 요소를 생성하여 반환



- Element.append()
  - 특정 부모 Node의 자식 NodeList 중 마지막 자식 다음에 Node 객체나 DOMString을 삽입
  - 여러 개의 Node 객체, **DOMString**을 추가 가능
  - 반환값이 없음

- Node.appendChild()

  - 특정 부모 Node의 자식 NodeList중 새로운 Node를 마지막에 삽입
  - 한번에 한개의 Node 추가 가능

  - 추가된 Node 객체를 반환



#### DOM 변경 관련 속성(property)

- Node.innerText
  - Node객체와 그 자손 텍스트 컨텐츠(DOMString)를 표현
  - 태그 등이 함께 string으로 표시가 됨
- Element.innerHTML
  - element 내에 포함된 HTML 마크업을 반환
  - XSS 공격에 취약



#### DOM 삭제

- ChildNode.remove()
  - Node가 속한 트리에서 **해당 Node** 제거
- Node.removeChild()
  - DOM에서 **자식 Node를 제거**하고 제거된 Node 반환



#### DOM 속성 관련 메서드

- Element.setAttribute(name, value)
  - 지정된 요소의 값을 설정
  - 속성이 이미 존재하면 값을 갱신, 없을경우 추가
- Element.getAttribute(attributeName)
  - 해당 요소의 지정된 값(문자열) 반환
  - 인자(attributeName)는 값을 얻고자 하는 속성의 이름

------

## Event

- 활동, 상호작용의 발생을 알리기 위한 객체

`EventTarget.addEventListener()`

- 지정한 이벤트가 대상에 전달될 떄마다 호출할 함수를 설정
- 이벤트를 지원하는 모든 객체(Element, Document, Window...)가 대상

`target.addEventListener(type, listener)`

- type
  - 반응 할 이벤트 유형
- listener
  - 이벤트 발생시 알림을 받는 객체
  - EventListener 인터페이스 or JS function 객체(콜백함수)

```javascript
const btn = document.querySelector('button')
btn.addEventListener('click', function(event) {
    alert('클릭 이벤트 발생')
    console.log(event)
})
```

```javascript
const textInpuit = document.querySelector('#idName')
textInput.addEventListener('input', function (event) {
    const ptag = document.querySelector('#p태그 id를 넣어 p태그 밑에 추가할 예정')
    ptag.innerText = event.target.value		//event.target.value로 이벤트 값을 가져올수 있음
})
```



### Event 취소

- `event.preventDefault()`

- 현재 이벤트의 기본 동작을 중단
  - 취소 불가능한 이벤트도 있음