# generator & async/await

## 제너레이터(ES6)

- 코드 블록의 실행을 일시 중지 했다가, 필요한 시점에 재개할 수 있는 특수한 함수
- **함수 호출자**에게 함수 실행의 **제어권을 양도** 가능
  - 함수 제어권을 함수가 독점하는 것이 아닌, **함수 호출자**에게 **양도(yield)**할 수 있음
- 함수 호출자와 함수의 **상태를 주고받을 수 있음**
  - 일반 함수는 호출시 매개변수를 통해 값이 I/O 되고, 결과값을 반환(함수 동작 도중엔 값 전달이 없음)
  - 함수 호출자와 양방향으로 함수의 상태를 주고받을 수 있음
- 제너레이터 함수를 호출하면 **제너레이터 객체를 반환**
  - 일반 함수는 코드 실행 후 값을 반환
  - 제너레이터 함수 호출시 함수 코드를 실행하는 것이 아니라 **이터러블 & 이터레이터 & 제너레이터 객체 반환**

### 제너레이터 함수 정의

```javascript
// 제너레이터 함수 선언
function* genDecFunc() {
    yield 1;
};
// 제너레이터 함수 표현
const genExpFunc = function* () {
    yield 1;
};
// 제너레이터 메서드
const obj = {
    * genObjMethod() {
        yield 1;
    }
};
// 제너레이터 클래스 메서드
class MyClass {
    * genClsMethod() {
        yield 1;
    }
}
```

- 제너레이터 함수는 arrow function 사용 불가
- 제너레이터 함수는 new 연산자로 생성자 함수로서 호출 불가



### 제너레이터 객체

- `Symbol.iterator`메서드를 상속받는 이터러블 객체
- `value, done`프로퍼티를 갖는, `next` 메서드를 소유하는 이터레이터
- 추가로 `return, throw`메서드를 가짐

- **`next`**메서드 호출시 **`yield`**표현식까지 코드 블록을 실행, yield된 값을 value 프로퍼티 값으로, false를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체 반환
- **`return`** 메서드 호출시 인수로 전달받은 값을 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체 반환 (강제 종료)

```javascript
function* getFunc() {
    try {
        yield 1;
        yield 2;
        yield 3;   
    } catch (e) {
        console.error(e);
    }
}
const generator = getFunc();
console.log(generator.next()); // {value: 1,done: false}
console.log(generator.return('End')); // {value: 'End', done: true}
```

- `throw` 메서드 호출시 인수로 전달받은 에러를 발생

```javascript
function* getFunc() {
    try {
        yield 1;
        yield 2;
        yield 3;   
    } catch (e) {
        console.error(e);
    }
}
const generator = getFunc();
console.log(generator.next()); // {value: 1,done: false}
console.log(generator.throw('err')); // 에러발생 => catch 작동, 기존 done이 false라면 {value: undefined, done:true} 반환
```



### 일시 중지와 재개

- `yield` 키워드와 `next` 메서드를 통해 실행을 중지 재개 가능

```javascript
function* getFunc() {
    try {
        yield 1;
        yield 2;
        yield 3;   
    } catch (e) {
        console.error(e);
    }
}
const generator = getFunc();
console.log(generator.next()); // {value: 1,done: false}
console.log(generator.next()); // {value: 2,done: false}
console.log(generator.next()); // {value: 3,done: false}
console.log(generator.next()); // {value: undefined,done: true}
```

- 이터레이터의 next 메서드와 달리 제너레이터 객체의 next 메서드는 인수를 전달할 수 있음
  - 전달한 인수는 제너레이터 함수의 yield 표현실을 할당받는 변수에 할당됨

```javascript
function* getFunc() {
    const x = yield 1;
    const y = yield (x+10);
    return x+y;
}
const generator = geFunc(0);
let res = generator.next(); // {value:1, done:false}, x에 값이 할당되지 않은 상태
res = generator.next(10); // {value:20, done:false}, 이 때 x에 10이 할당되고, x+10의 결과인 20이 value인 객체가 반환
res = generator.next(20); // {value:30, done:true}, 이 때 y에 20이 할당됨, x+y의 결과인 30이 value인 객체가 반환
```

- 이를 이용한 것이 async



## async / await

- 프로젝트에서 사용 경험이 있기에, 간략하게 서술함



- 제너레이터를 활용하면 비동기 처리를 동기 처리처럼 동작하게 할 수 있지만, 코드가 복잡하여 ES8에서 도입
- 프로미스를 기반으로 동작, `then/catch/finally` 후속 처리 메서드에 콜백 함수를 전달해서 비동기 처리 결과를 후속 처리할 필요 없이 동기 처리처럼 프로미스를 사용 가능
- async 함수는 언제나 **프로미스**를 반환

### await

- await 키워드는 반드시 프로미스 앞에서 사용

- 프로미스가 settled 상태(비동기 처리가 수행된 상태)가 될 때 까지 대기, 이후 프로미스가 resolve한 처리 결과를 반환

```javascript
// const fetch = require('node-fetch') // node환경에서 window api인 fetch를 사용하기 위한 코드

const getUserName = async id => {
    const res = await fetch(`url/info/${id}`);
    const {name} = await res.json();
    console.log(name)
}
```



