# Set & Map

## Set

- 중복되지 않는 유일한 값들의 집합
  - 대체적으로 Python set과 비슷
- 모든 값을 요소로 저장 가능(객체, 배열, 함수 ...)
- 순서에 의미가 없으나, 이터러블한 객체
  - 인덱스를 갖지 않음
  - `for of`순회 가능하며 스프레드 문법(`...set`)과 배열 디스트럭처링 가능(`const [a, ...rest] = set;`)
    - 순서에 의미는 없으나, 순회하는 순서는 요소가 추가된 순서를 따름

```javascript
const set = new Set();
console.log(set); // Set(0) {size: 0}

const set1 = new Set([1,2,3,3])
console.log(set1);  // Set(3) {1, 2, 3}

const set2 = new Set('hello')
console.log(set2) // Set(4) {'h', 'e', 'l', 'o'}

// 중복 제거 활용
const uniq = array => array.filter((v,i,self)=>self.indexOf(v)===i);
const uniq = array => [...new Set(array)];
```

- `Set.prototype.size` : 요소 개수 확인

  - ```javascript
    const set = new Set([1,2,3]);
    set.size = 10; // 무시됨
    console.log(set.size); // 3
    ```

- `Set.prototype.add`

  - ```javascript
    const set = new Set();
    set.add(1).add(2).add(3);
    ```

  - `NaN`, `(+0, -0)` 을 같다고 평가

- `Set.prototype.has`: 요소 존재 여부 확인
- `Set.prototype.delete`: 요소 삭제 후 성공시 true 반환, 없더라도 에러는 발생하지 않고 false반환
  - 값은 반환하기에 연속적으로 호출 불가 (`set.delete(1).delete(2)` 불가)

- `Set.prototype.clear`: 초기화

### 집합 연산

```javascript
const setA = new Set([1,2,3,4]);
const setB = new Set([2,3,4,5]);

// 교집합: Set.prototype.intersection
const setC = setA.intersection(setB);
console.log(setC); //Set(3) {2, 3, 4}

// 합집합: Set.prototype.union
const setD = setA.union(setB);
console.log(setD); // Set(5) {1, 2, 3, 4, 5}

// 차집합: Set.prototype.difference
const setE = setA.difference(setB);
console.log(setE); // Set(1) {1}

// 부분집합 확인: Set.prototype.isSuperset
const setF = new Set([1,2,3]);
console.log(setF.isSubsetOf(setA)); // true
console.log(setF.isSubsetOf(setB)); // false
```



## Map

- 키와 값의 쌍으로 이루어진 컬렉션
- 객체와의 차이점
  - 객체를 포함한 **모든 값**을 **키**로 사용 가능
  - 이터러블 => `for of`, `...스프레드` 등 가능
  - `Object.keys(obj).length` => `map.size`

```javascript
const map = new Map();
console.log(map); // Map(0) {size: 0}

const map1 = new Map([['key1','value1'],['key2','value2']]);
console.log(map1) //Map(2) {'key1' => 'value1', 'key2' => 'value2'}

const map2 = new Map([['key1','value1'],['key1','value2']]);
console.log(map2) // Map(1) {'key1' => 'value2'} , 같은 키가 있을 경우 덮어씌워짐
```

- 이터러블을 인수로 전달받아 Map 객체를 생성, 인수로 전달되는 이터러블은 키와 값의 쌍으로 이루어진 요소로 구성되어야 함

```javascript
// 요소 추가
const map = new Map();
map.set('key1','value1').set('key2','value2'); // set 메서드는 Map객체를 반환 => set 메서드 연속 호출 가능

// 조회
console.log(map.get('key1')) // value1

// 존재 확인
console.log(map.has('key1')) // true

// 삭제, boolean 반환
map.delete('key1') // true
map.delete('key3') // false

// 초기화
map.clear();
```

### 순회

- `Map.prototype.forEach`

  - `현재 순회 중인 값, 현재 순회 중인 키, Map 객체 자체(this)` 3개를 인수로 받음

  - ```javascript
    const lee = {name:'Lee'};
    const kim = {name:'Kim'};
    const map = new Map([[lee,'BE'],[kim,'FE']]);
    map.forEach((v,k,map)=>console.log(v,k,map))
    // BE {name: 'Lee'} Map(2) {{…} => 'BE', {…} => 'FE'}
    // FE {name: 'Kim'} Map(2) {{…} => 'BE', {…} => 'FE'}
    ```

- `for of`

  - ```javascript
    const lee = {name:'Lee'};
    const kim = {name:'Kim'};
    const map = new Map([[lee,'BE'],[kim,'FE']]);
    
    for (const items of map){console.log(items)}
    // [{…}, 'BE']
    // [{…}, 'FE']
    
    console.log([...map]); // [Array(2), Array(2)]
    const [a,b] = map;
    console.log(a) // [{…}, 'BE']
    ```

- `Map.prototype.keys`: 키를 값으로 갖는 이터러블 & 이터레이터인 객체 반환

- `Map.prototype.values`: 값을 값으로 갖는 이터러블 & 이터레이터인 객체 반환

- `Map.prototype.entries`: python의 dict.items와 비슷, 요소키와 요소값을 값으로 갖는 이터러블 & 이터레이터 객체 반환