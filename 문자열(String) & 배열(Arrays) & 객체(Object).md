# 문자열(String) & 배열(Arrays) & 객체(Objects)

## 문자열 관련 메서드

- includes

  - 특정 문자열의 존재 여부를 **참/거짓**으로 반환

  - ```javascript
    const str = 'github ljm0850'
    
    str.includes('github')		// true
    str.includes('https')		// false
    ```

- split

  - 문자열을 토큰 기준으로 나눈 **배열** 반환

  - 인자가 없으면 기존 문자열을 배열에 담아 반환

  - ```javascript
    const str = 'github ljm'
    
    str.split()		// ['github ljm']
    str.split('')	// ['g','i','t','h','u','b',' ','l','j','m']
    str.split(' ')	// ['github', 'ljm']
    ```

- replace

  - 해당 문자열을 대상 문자열로 **교체**하여 반환

  - ```javascript
    const str = 'github ljm javascript'
    
    str.replace(' ', '-')		// 'github-ljm javascript'
    str.replaceAll(' ','-')		// 'github-ljm-javascript'
    ```

- trim

  - 문자열의 좌우 공백을 제거하여 반환

  - ```javascript
    const str = '   ljm   '
    str.trim()			// 'ljm'
    str.trimStart()		// 'ljm   '
    str.trimEnd()		// '   ljm'
    ```


---

## 배열(Arrays)

- 키와 속성들을 담고 있는 **참조 타입의 객체(object)**

  - key = index 인 객체

  - ```javascript
    a = [1,2,3]
    b = [1,2,3]
    a === b		// false
    a == b		// false
    ```

- 순서를 보장

- 대괄호`[]`를 이용하여 생성, 0을 포함한 **양의 정수** 인덱스로 특정 값에 접근 가능

  - 배열의 길이는 array.length 형태로 접근

  - ```javascript
    const numbers = [1,2,3,4,5]
    console.log(numbers[0])		// 1
    console.log(numbers[numbers.length-1])	//5
    console.log(numbers[-1])	// undefined
    ```

#### Spread operator

```javascript
const arr1 = [1,2,3]
const arr2 = [0, ...arr1, 4]
console.log(arr2)	// [0, 1, 2, 3, 4]
```







### 주요 메서드-기본 배열 조작

#### reverse

- 원본 배열의 요소들의 순서를 반대로 정렬

- ```javascript
  const numbers = [1,2,3]
  numbers.reverse()
  console.log(numbers)	//[3,2,1]
  ```

#### push & pop

- 배열 가장 뒤에 요소를 추가&제거

- ```javascript
  const numbers = [1,2,3]
  
  numbers.push(4)
  console.log(numbers)	// [1,2,3,4]
  
  numbers.pop()
  console.log(numbers)	// [1,2,3]
  ```

#### unshift & shift

- 배열 가장 앞에 요소를 추가&제거

- ```javascript
  const numbers = [1,2,3]
  
  numbers.unshift(0)
  console.log(numbers)	// [0,1,2,3]
  
  numbers.shift()
  console.log(numbers)	// [1,2,3]
  ```

#### includes

- 배열에 특정 값이 존재하는지 판별 true/false

- ```javascript
  const numbers = [1,2,3]
  console.log(numbers.includes(1))		// true
  console.log(numbers.includes(4))		// false
  ```

#### indexOf

- 배열에 특정 값이 존재하는지 판별 후 인덱스 반환

- 요소가 없을 경우 -1 반환

- ```javascript
  const numbers = [1,2,3]
  let test
  
  test = numbers.indexOf(3)
  console.log(test)			//2
  
  test = numbers.indexOf(4)
  console.log(test)			// -1
  ```

#### join

- 배열의 모든 요소를 구분자를 이용하여 연결, dafault = 쉼표

- ```javascript
  const numbers = [1,2,3]
  let test
  
  test = numbers.join()
  console.log(test)			// 1,2,3
  
  test = numbers.join('')
  console.log(test)			// 123
  ```

  

### 주요 메서드-Array Helper Methods

- 배열을 순회하며 특정 로직을 수행
- 매서드 호출 시 인자로 callback 함수를 받음
  - callback 함수: 함수의 내부에서 실행될 목적으로 인자를 넘겨받는 함수

#### forEach

- ```javascript
  array.forEach((element,index, array) => {
      ~~
  })
  ```

- ```javascript
  const foods = ['참치','닭','돼지','소','양']
  
  foods.forEach((food, index) => {
      console.log(food, index)
  })
  // 참치 0  닭 1 ~~
  ```

- 배열의 각 요소에 대해 callback 함수를 한 번씩 실행

- **반환 값 없음**

- 매개변수

  - element : 배열의 요소
  - index : 배열 요소의 인덱스
  - array : 배열 그자체

##### 배열 순회 비교

- for loop
  - 모든 브라우저 지원
  - 인덱스를 활용하여 배열의 요소에 접근
  - break, continue 사용 가능
- for of
  - 일부 오래된 브라우저 지원x
  - 인덱스 없이 배열의 요소에 접근
  - break, continue 사용 가능
- forEach
  - 대부분의 브라우저 지원
  - break, continue 사용 불가
  - Airbnb Style Guide 권장 방식



#### map

- ```javascript
  array.map((element, index, array) =>{
   	~~   
  })
  ```

- ```javascript
  const numbers = [1,2,3]
  
  const newNumbers = numbers.map((num) => {
      return num +2
  })
  console.log(newNumbers)		// [3,4,5]
  ```

- 배열의 각 요소에 대해 콜백 함수를 한 번씩 실행

- callback 함수의 **반환 값**을 요소로 하는 새로운 배열 반환



#### filter

- ```javascript
  array.filter((element, index, array) => {
      ~~
  })
  ```

- ```javascript
  const numbers = [1,2,3]
  const newNumbers = numbers.filter((num,index)=>{
      return num %2
  })
  console.log(newNumbers)		//[1,3]
  ```

- callback 함수의 **반환 값이 참인 요소**들만 모아 새로운 배열 반환



#### reduce

- ```javascript
  array.reduce((acc, element, index, array) =>{
      ~~
  }, initialValue)
  ```

- ```javascript
  const numbers = [1,2,3]
  const result = numbers.reduce((acc, num) => {
      return acc + num
  },5)
  console.log(result)		// 11
  // initialValue 미작성시 6
  ```

- 배열의 각 요소에 대해 콜백 함수를 한 번씩 실행

- callback 함수의 반환 값들을 하나의 값(acc)에 누적 후 반환
- 매개변수
  - acc : 이전 callback 함수의 반환 값이 누적되는 변수
  - initialValue : 최초 함수 호출시 acc에 할당되는 값, default는 배열의 첫 번째 값
    - 빈 배열의 경우 initialValue값이 없을 경우 에러 발생



#### find

- ```javascript
  array.find((element, index, array) => {
      ~~
  })
  ```

- ```javascript
  const info = [
      { name: 'ljm', language: 'Python'},
      { name: 'ljm0850', language: 'JavaScript'},
  ]
  
  const result = info.find((info) => {
      return info.name === 'ljm0850'
  })
  console.log(result)
  // {name: 'ljm0850', language: 'JavaScript'}
  ```

- 배열의 각 요소에 대해 콜백 함수를 한 번씩 실행

- callback 함수의 반환 값이 참이면 해당 요소를 반환후 종료
  - 첫번째 요소만 반환이 됨
- 찾는 값이 배열에 없으면 undefined 반환



#### some

- ```javascript
  array.some((element, index, array) => {
      ~~
  })
  ```

- ```javascript
  const numbers = [1,3,5]
  const evenNum = numbers.some((num)=>{
      return num%2 === 0
  })
  console.log(evenNum)		// false
  ```

- 배열의 요소 중 **한개라도** 함수(조건)을 통과하면 true을 반환
- 빈 배열의 경우 **항상 false** 반환



#### every

- ```javascript
  array.every((element,index,array) => {
      ~~
  })
  ```

- ```javascript
  const numbers = [1,3,5]
  const oddNum = numbers.every((num)=>{
      return num % 2
  })
  console.log(oddNum)		//true
  ```

- 배열의 **모든 요소**가 함수(조건)을 통과하면 true을 반환
- 빈 배열의 경우 **항상 true** 반환

---

## 객체(Objects)

```javascript
const info = {
    language : 'JavaScript',
    'today date' : {
        year : '2022',
        month : 4,
    },
}
console.log(info.language)		// JavaScript
console.log(info['today date'])	// {year: '2022', month: 4}
```

- 속성(property)의 집합, key-value 쌍으로 표현
- key는 문자열 타입만 가능
  - key 이름에 띄어쓰기 등의 구분자가 있을 경우 따옴표`''`'로 묶어서 표현

- value는 모든 타입 가능
- 객체 요소 접근은 .(dot) 혹은 대괄호[]로 가능
  - key이름에 구분자가 있을 경우 대괄호만 가능



### 객체와 메서드

```javascript
const me = {
    firstName: 'jm',
    lastName: 'l',
    fullName: this.firstName + this.lastname,		// this가 window를 의미함
    getFullName: function(){
        return this.firstName + this.lastName		// this가 me를 의미
    }
}
console.log(me.fullName)			// NaN
console.log(me.getFullName())		// jml
```

```javascript
const obj = {
    PI = 3.14,
    radiuses = [1,2,3],
    printArea: function(){
        this.radiuses.forEach(function(r){
            console.log(this.PI * r * r)		// 여기서의 this는 메스드에서 호출한 것이 아니라 window를 의미, bind를 이용하여 this = obj를 의미하게 설계 or arrowFunction 이용
        }.bind(this)
    }
}
```

- 메서드는 객체의 속성이 참조하는 함수
- 객체.메서드명()으로 호출 가능
- 매서드 내부에서는 this가 객체를 의미

#### this

- 실행 문맥에 따라 다른 대상을 지칭

- 기본적으로는 최상위 객체(window)
- class 내부의 생성자(constructor)함수
  - 생성되는 객체를 가리킴(python-self)
- 매서드(객채.메서드()로 호출하는 함수)
  - 해당 매서드가 소속된 객체를 가리킴

### 객체 관련 ES6 문법

#### 속성명 축약 : value 생략

```javascript
const books = ['JUSTICE','THE TYRANNY OF MERIT']
const magazines = ['TIME', 'Nature']
const bookshop = {
    books,//books:books,
    magazines//magazines:magazines,
}
```

- key와 value에 들어갈 변수 이름이 같으면 축약



#### 메서드명 축약 : function 생략

```javascript
const obj ={
    greeting(){//greeting = function(){
        
    }
}
```



#### 계산된 속성(computed property name)

```javascript
const key = 'books'
const value = ['팩트풀니스','보통의존재','우리가 보낸 가장 긴 밤']

const bookstore = {
    [key]: value,
}
console.log(bookstore)		// {books:Array(3)}
console.log(bookstore.books)	// ['팩트풀니스','보통의존재','우리가 보낸 가장 긴 밤']
```

- 객체 정의시 key의 이름을 표현식을 이용하여 동적으로 생성 가능



#### 구조 분해 할당(destructing assignment)

```javascript
const info ={
    name: 'ljm',
    github: 'github.com/ljm0850',
    blog: 'ljm0850.tistory.com/',
    
}
const { name } = info					// const name = info.name
const { blog, github } = info

console.log(blog)		// ljm0850.tistory.com/
```

- 배열, 객체를 분해하여 속성을 변수에 쉽게 할당 하는 방법



#### Spread operator(...)

```javascript
const obj = {b:2, c:3}
const newObj = {a:1, ...obj, e:5 }
console.log(newObj)		// {a: 1, b: 2, c: 3, e: 5}
```

- `...`을 사용하여 객체 내부에서 객체 전개 가능
- 얕은 복사에 활용 가능



### JSON (JavaScript Object Notation)

```javascript
const jsonData = JSON.stringify({
    name: 'ljm',
    language: 'JavaScript',
})
const parsedData = JSON.parse(jsonData)
console.log(parsedData)			//{name: 'ljm', language: 'JavaScript'} , string 타입
console.log(jsonData)			//{"name":"ljm","language":"JavaScript"}, object 타입
```

- key-value쌍의 형태로 데이터를 표기하는 언어 독립적 표준 포맷
- JS의 객체와 유사하지만, 문자열 타입
  - 객체로 사용하기 위해서는 parsing이 필수
- JSON.parse()
  - JSON => 객체
- JSON.stringify()
  - 객체 => JSON



-----

### lodash

- 모듈성, 성능 및 추가기능을 제공하는 JavaScript 유틸리티 라이브러리
- array, object 등 자료구조를 다룰 때 유용하고 간편한 함수 제공
  - reverse, sortBy, range, random, cloneDeep ...