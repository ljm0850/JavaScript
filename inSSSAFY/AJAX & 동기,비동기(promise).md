# AJAX & 동기식,비동기식

## AJAX

- Asynchronous JavaScript And XML (비동기식 JavaScript와 XML)
- `XMLHttpRequest()`를 활용하여 서버와 통신
- 페이지 전체를 새로고침 하지 않아도 수행되는 **비동기성**
  - 전체 페이지가 아닌 일부분만 업데이트 가능
  - 서버로부터 데이터를 받고 작업을 수행 가능



### XMLHttpRequest()

- 서버와 상호작용하기 위해 사용
- 새로고침 없이 데이터 받기 가능
- XML뿐만 아니라 모든 종류의 데이터를 받아올 수 있음

```javascript
const request = new XMLHttpRequest()
const URL = '~~~'
request.open('GET', URL)
```



#### Axios

- 브라우저를 위한 Promise(뒤에 서술) 기반의 클라이언트

- 브라우저 내장 객체인 XHR를 활용하여 AJAX 요청 처리보다 편리하게 도움을 줌

  - 간편한 라이브러리 제공



## Asynchronous JavaScript

### 동기식

- 순차적, 직렬적 Task 수행
- 요청을 보낼 경우 응답을 받을때 까지 대기(blocking)
  - JavaScript가 single threaded라
    - 이벤트를 처리하기 위해 Web API에서 이벤트를 처리
    - 처리가 끝난 이벤트는 que형식으로 들어와 모든 요청이 끝난 뒤 FIFO로 이벤트 발생

### 비동기식

- 병렬적 Task 수행
- 요청을 보낸 후 응답을 기다리지 않고 다음 동작 수행(non-blocking)
- single thread로 비동기 처리
  - multi thread를 이용하여 여러 작업을 처리 하는 개념이 아님

```javascript
console.log('start')
setTimeout(function ljm() {
    console.log('ljm')
},3000)		// 3000 ms 지연
console.log('end')
// start
// end
// ljm
```



#### Threads

- 프로그램이 작업을 완료하기 위해 사용할 수 있는 단일 프로세스

- 각 Thread는 한번에 한개의 작업 수행

  

### Concurrency model(동시성 모델)

#### 전체 구조

- Call Stack
  - 요청이 들어올 때마다 해당 요청을 순차적으로 처리하는 Stack 형태 구조
- Web API(Browser API)
  - 브라우저에서 제공하는 API
  - `setTimeout(), DOM event, AJAX 데이터 응답` 과 같은 시간이 소요되는 일을 처리
- Task Queue(Event Queue, Message Queue)
  - 비동기 처리된 callback함수(Web API에서 처리한 함수)가 대기하는 Queue 형태의 자료 구조
  - main thread가 끝난 후 실행
- Event Loop
  - Call Stack이 비어 있는지 확인
  - 비어있으면 Task Queue에서 대기중인 callback함수를 Call stack으로 push



#### 비동기의 문제점

- 이벤트처리 순서대로 callback함수가 작동하다 보니 코드의 실행 순서가 불명확

  - Async callbacks

    - 백그라운드에서 실행을 시작할 함수를 호출할 떄 인자로 지정된 함수

  - promise-style 

    

### Callback Function

- 다른 함수에 인자로 전달된 함수
- 외부 함수 내에서 호출되어 작업을 진행
- 비동기 작접이 완료된 후 코드 실행을 계속하는데 사용되는 경우 비동기 콜백(asynchronous callback)

- JavaScript의 함수가 일급 객체라서 가능
  - 다른 객체에 적용할 수 있는 연산을 모두 지원
    - 인자로 넘기기
    - 함수의 반환값으로 사용
    - 변수에 할당 가능

#### Async callbacks

- 백그라운드 코드 실행이 끝나면 callback 함수를 호출하여 다음 작업을 실행하게 할 수 있음

```javascript
function callback(ljm){
    return function() {
        anotherFunction(ljm) ~~
    }
}
```

- 과하게 사용시 코드가 안으로 들어가면서 가독성이 떨어짐
  - Promise callbacks 사용



### Promise

```javascript
// axios CDN
<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
  <script>
    const API_URI = 'API 주소'

    function changeImg() {
      axios.get(API_URI)
        .then(response => response.data.message) 
        .then(response => document.querySelector('img').src = `${response}`)
        .catch(error => console.log(error))
        .finally(function () {
          console.log('changeImg가 끝났습니다')
      })
    }
```

```javascript
work1().then(function(result1) {
    return work2(result1)
})
.then(function(result2){
    console.log(result2)
})
```



#### Promise object

- 비동기 작업의 최종 완료 또는 실패를 나타내는 객체
- `.then()` 
  - 이전 작업이 성공시 수행할 작업을 나타내는 callback 함수
  - callback 함수는 이전 작업의 성공 결과를 인자로 전달받음(return값)
    - 연쇄적인 작업 수행 가능(chaining)
    - 위 예시에서 2개의 response는 각자 다른 정보가 할당되어 있음(2번째 response는 1번째에서 response.data.message와 동일)
- `.catch()`
  - .then 작업이 한개라도 실패시 동작
  - 실패로 인해 생성된 객체를 catch 블록 안에서 사용 가능
- `.finally(callback)`
  - Promise 객체를 반환
  - 결과와 상관없이 지정된 callbcak 함수가 실행
  - 어떠한 인자도 전달받지 않음



#### Promise.resolve()

- Promise.resolve(value) 메서드는 주어진 값으로 이행하는 Promise.then 객체 반환(https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise/resolve)



#### Promise.all()

- `()`안의 순회 가능한 객체(array...)에 주어진 모든 프로미스가 이행된 뒤 이행하는 Promise를 반환

- 주어진 프로미스중 하나가 거부할 경우 같은 이유로 Promise.all이 거부됨

```javascript
const promise1 = Promise.resolve(123);
const promise2 = 45;
const promise3 = new Promise((resovle,reject)=>{
    setTimeout(resolve,100,'foo');
});

Promise.all([promise1, promise2, promise3]).then((valutes)=>{
	console.log(values)
})
// Array[123,45,"foo"]
```





### async & await

- 비동기 코드를 작성하는 새로운 방법
  - ECAMAScript 2017 에서 등장
- Promise와 동작 방식이 동일
  - then chaining을 제거
  - 더 쉽게 읽고 표현할 수 있도록 설계

```javascript
async function changeImg() {
      const uri = await axios.get(API_URI)
      const response1 = uri.data.message 
      // 함수 앞에 async, 비동기 코드 앞에 await 
```

```javascript
const target = ['a','b','c','d','e']

async function asc(){

  for(const item of target){
    const res = await request(item) // request는 또 다른 async 함수
    console.log(res)
  }
}
```

