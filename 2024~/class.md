# Class

- 클래스와 생성자 함수의 차이
  - 클래스는 **new** 연산자 없이 호출시 에러 발생, 생성자 함수는 일반 함수로서 호출됨
  - 클래스는 상속을 지원하는 **extends**와 **super** 키워드를 제공
  - 클래스는 호이스팅이 발생하지 않는 것 처럼 동작
  - 클래스는 모든 코드에 암묵적으로 **strict mode**가 지정되어 실행되며 해제 불가
  - 클래스의 constructor, 프로토타입 메서드, 정적 메서드는 모두 프로퍼티 어트리뷰트 **[[Enumerable]]의 값이 false** (열거 불가)

## 정의

```javascript
class Person {};
const Person = class {};
const Person = class MyClass {};
```

- 클래스는 함수, 일급 객체이기에 표현식으로 정의 가능

```javascript
// 클래스 선언문
class Person {
    // 생성자
    constructor(name){
      this.name = name;
    }
    // 프로토타입 메서드
    sayHi() {
      console.log(`Hi my name is ${this.name}`);
    }
    // 정적 메서드
    static sayHello() {
      console.log('Hello');
    }
  }

  const me = new Person('Lee'); // 인스턴스 생성
  console.log(me.name); // 인스턴스의 프로퍼티 참조
  me.sayHi(); // 프로토타입 메서드 호출
  Person.sayHello(); // 정적 메서드 호출, Math.max() 같은 형식
```



## 메서드

- constructor (생성자)
- 프로토타입 메서드
- 정적 메서드



### constructor

- 인스턴스를 생성하고 초기화 하기 위한 메서드

```javascript
class Person {
  constructor(name){
    this.name = name;
    
    // return this; 가 암묵적으로 되어있음, return 객체를 지정하면 오버라이딩 되는 느낌
    // return 100; 과 같이 원시값의 반환은 무시되고 this가 반환됨
  }
}

console.log(typeof Person); // function
```

### 프로토타입 메서드

```javascript
// 생성자함수
function Person(name) {
  this.name = name;
}
Person.prototype.sayHi = function ()  {
  console.log(`Hi My name is ${this.name}`);
};

const me = new Person('lee');
me.sayHi();

// ES6 class
class Person {
    constructor(name) {
      this.name = name;
    }
    sayHi() {
      console.log(`Hi My name is ${this.name}`);
    }
  }
const me = new Person('lee');
me.sayHi()
```

### 정적 메서드

- 인스턴스를 생성하지 않아도 호출할 수 있는 메서드

```javascript
// 생성자 함수
function Person(name) {
  this.name = name;
}
// 정적 메서드
Person.sayHi = function() {
  console.log('Hi');
}
Person.sayHi();
```

```javascript
class Person {
    constructor(name) {
      this.name = name;
    }
    static sayHi() {
      console.log('Hi');
    }
  }
```



### 프로토타입 메서드와 정적 메서드의 차이

- 정적 메서드와 프로토타입 메서드는 자신이 속해 있는 프로토타입 체인이 다르다.
- 정적 메서드는 클래스로 호출, 프로토타입 메서드는 인스턴스로 호출
- 정적 메서드는 인스턴스 프로퍼티를 참조할 수 없지만, 프로토타입 메서드는 인스턴스 프로퍼티 참조 가능

```javascript
class Square {
    // 정적 메서드
    static area(width, height) {
      return width * height;
    }
}
console.log(Square.area(10,10))
```

```javascript
class Square {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }
  // 프로토타입 메서드
  area() {
    return this.width * this.height;
  }
}

const square = new Square(10,10);
console.log(square.area()); // 100
```

- 프로토타입 메서드는 인스턴스로 호출해야 하므로, 메서드 내부의 this는 메서드를 호출한 객체(인스턴스)에 바인딩 됨
- 정적 메서드는 클래스로 호출해야 하므로, this가 클래스를 가리킴



## 프로퍼티

### 인스턴스 프로퍼티

```javascript
class Person {
    constructor(name) {
        // 인스턴스 프로퍼티
        this.name = name; // public
    }
}

const me = new Person('lee');
console.log(me); // Person {name: 'lee'}
```

- Private, public, protected 키워드와 같은 접근 제한자를 지원하지 않음(only public) **ES6 당시**

  - **ES2022**이후로 **private 필드 지원**(# 문자 활용)

  - ```javascript
    class Example {
      #privateField = "This is private";
    
      constructor(value) {
        this.#privateField = value; // 클래스 내부에서만 접근 가능
      }
    
      getPrivateField() {
        return this.#privateField;
      }
    }
    
    const example = new Example("Private Value");
    console.log(example.getPrivateField()); // 출력: Private Value
    console.log(example.#privateField);     // SyntaxError: Private field '#privateField' must be declared in an enclosing class
    ```

  

## Class field declarations & private

- 클래스 내부에서 필드를 선언하고 초기화하는 문법

```javascript
// 기존 방식
class OldClass {
  constructor() {
    this.name = "Old Way";	// public 선언
  }
}
// 새 방식
class NewClass {
  name = "New Way"; // public 선언
}
```

```javascript
// 초기화
class MyClass {
  name = "Default";

  constructor(name) {
    if (name) {
      this.name = name;
    }
  }
}

const obj = new MyClass("Custom Name");
console.log(obj.name); // "Custom Name"
```



### private

```javascript
class MyClass {
  #secret = "Hidden Value"; // private 필드 선언

  getSecret() {
    return this.#secret; // 클래스 내부에서 접근 가능
  }
}

const obj = new MyClass();
console.log(obj.getSecret()); // "Hidden Value"
console.log(obj.#secret); // SyntaxError: Private field '#secret' must be declared in an enclosing class
```

- private로 선언된 것은 **상속되지 않음**

```javascript
class Parent {
  #privateField = "Parent's Private";

  getPrivateField() {
    return this.#privateField;
  }
}

class Child extends Parent {
  constructor() {
    super();
    console.log(this.#privateField); // SyntaxError: Private field '#privateField' must be declared in an enclosing class
  }
}
```



### 정적 메서드, static 필드

```javascript
class MyClass {
  static counter = 0; // static 필드 선언
  
  static increment() {
    this.counter++;
  }
}

console.log(MyClass.counter); // 0
MyClass.increment();
console.log(MyClass.counter); // 1
```

```javascript
// private static
class MyClass {
  static #privateStatic = "Private Static";

  static getPrivateStatic() {
    return this.#privateStatic;
  }
}

console.log(MyClass.getPrivateStatic()); // "Private Static"
console.log(MyClass.#privateStatic); // SyntaxError: Private field '#privateStatic' must be declared in an enclosing class
```



## 상속을 통한 클래스 확장

- 클래스와 생성자 함수는 유사하지만, 클래스는 상속을 통해 기존 클래스를 확장할 수 있는 문법이 제공됨
  - **extends**

```javascript
class Animal {
    // 클래스 필드 사용
    age = 0;
    weight = 0;
    constructor(age,weight) {
        if (age){this.age = age}
        if (weight){this.weight = weight}
        // this.age = age;
        // this.weight = weight;
    }
    eat(){return 'eat'} // 프로토타입 메서드
    move(){return 'move'}
}

// Animal을 상속받은 Bird 클래스
class Bird extends Animal {
    fly() {return 'fly'}
}
const bird = new Bird(1,5);

console.log(bird); // Bird {age: 1, weight:5}
console.log(bird instanceof Bird) // true
console.log(bird instanceof Animal) // true
console.log(bird.eat()) // eat
console.log(bird.move()) // move
console.log(bird.fly()) // fly
```

```javascript
// 동적 확장 방법 - ? 활용
class Derived extends (condition? Base1:Base2){}
```

### constructor

```javascript
constructor(...args) {super(...args)}
```

- constructor 생략시 위 코드가 암묵적으로 정의됨

### super

- super 호출시 super클래스의 constructor를 호출
- super를 참조하면 super클래스의 메서드 호출 가능

```javascript
class Base {
    a = null;
    b = null;
    constructor(a,b) {
        if (a){this.a = a};
        if (b){this.b = b};
    }
    say(){ return `a:${this.a}, b:${this.b}`}
}

class Derived extends Base {
    c = null;
    constructor(a,b,c){
        super(a,b); // constructor를 추가 정의시 반드시 호출해야함
        if (c){this.c = c}; // super 호출 전까지 this 참조 불가, 서브클래스는 인스턴스 생성을 super에 위임
    }
    say() { return `${super.say()} c:${this.c}`}
}

const derived = new Derived(1,2,3);
console.log(derived); // Derived {a:1, b:2, c:3}
console.log(derived.say()); // a:1, b:2, c:3
```

- super 참조가 동작하기 위해서는 super를 참조하고 있는 메서드(Derived의 say)가 바인딩되어 있는 객체(Derived의 프로토타입)의 프로토타입(Base의 프로토타입)을 찾을 수 있어야함

- 이를 위해 메서드는 내부슬롯 `[[HomeObject]]`를 가지며, 자신을 바인딩하고 있는 객체를 가리킴

  - ES6의 축약표현으로 정의된 함수만이 `[[HomeObject]]`를 가짐

  - ```javascript
    const obj = {
      foo() {}, // foo는 축약표현
      bar : function () {}
    }
    ```

  - 