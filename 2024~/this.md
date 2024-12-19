# this

|           함수 호출 방식           |           this 바인딩            |
| :--------------------------------: | :------------------------------: |
|           일반 함수 호출           |            정역 객체             |
|            메서드 호출             |       메서드를 호출한 객체       |
|          생성자 함수 호출          | 생성자 함수가 만들어 낼 인스턴스 |
| Function.prototype.call/apply/bind |    첫번째 인수로 전달한 객체     |



- Java나 C++같은 클래스 기반 언어에서 this는 클래스가 생성하는 인스턴스를 가르킴
- JS의 경우 함수가 호출되는 방식에 따라 this에 바인딩될 값이 동적으로 결정
  - strict mode 또한 this 바인딩에 영향을 줌

```javascript
console.log(this); // 브라우저에선 window

function square(number) {
    // 일반 함수 내부에서 this는 전역 객체
    console.log(this); // window
    return number * number
}

const person = {
    name: 'lee',
    getName() {
        // 메서드 내부에선 this는 메서드를 호출한 객체
        console.log(this); // {name: 'lee', getName:f}
        return this.name;
    }
}

function Person(name){
    this.name = name
    // 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스
    console.log(this);
}
const me = new Person('Lee') // Person {name: 'Lee'}
```

- this는 객체의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수, 일반적으로 객체의 메서드 내부 또는 생성자 함수 내부에서만 의미가 있음
  - strict mode가 적용된 일반 함수 내부의 this에는 undefined가 바인딩 됨



## 함수 호출 방식에 따른 this

```javascript
const foo = function () {
    console.log(this);
}
// 일반함수 호출
foo(); // window

// 메서드 호출
const obj = { foo };
obj.foo(); // obj

// 생성자 함수 호출
new foo(); // foo {}

// Function.prototype.apply/call/bind 메서드
const bar = { name:'bar'};
foo.call(bar); // bar
foo.apply(bar); // bar
foo.bind(bar)(); // bar
```

### 일반 함수 호출

- 기본적으로 this에는 전역 객체가 바인딩

```javascript
function foo() {
    console.log(this); // 브라우저 기준 window, strict모드시 undefined
    function bar() {
        console.log(this) // window // undefined
    }
    bar();
}
foo();
```

```javascript
var v = 2; // v는 전역 객체의 프로퍼티
let value = 1; // 전역 객체의 프로퍼티가 아님

const obj = {
    value:100,
    foo(){
        console.log(this); // {value:100, foo:f}
        console.log(this.value); // 100
        
        function bar() {
            console.log(this); // window
            console.log(this.value); // undefined
            console.log(this.v); // 2
        }
        bar();
        
        setTimeout(function (){
            // obj의 this가 bind되어 100 출력, bind 하지 않은 경우 this는 window.value
            console.log(this.value);
        }.bind(this),100);
        
        // 화살표 함수는 상위 스코프의 this를 가리킴
        setTimeout(()=>console.log(this.value),100); //100
    }
}
obj.foo();
```



### 메서드 호출

- 메서드 내부의 this는 메서드를 **호출한 객체**가 바인딩 됨

```javascript
const person = {
    name: 'lee',
    getName() {
        return this.name;
    }
}
console.log(person.getName()); // lee

const anotherPerson = {
    name: 'god',
}
anotherPerson.getName = person.getName;
console.log(anotherPerson.getName()); // god

const getName = person.getName;
console.log(getName()); // '', 브라우저 기준 window.name을 선언 안했기에 
```

```javascript
function Person(name){
    this.name = name;
}
Person.prototype.getName = function (){
    return this.name;
}

Person.prototype.name = 'kim'
const me = new Person('lee');

console.log(me.getName());	// lee
console.log(Person.prototype.getName()) // kim
```

