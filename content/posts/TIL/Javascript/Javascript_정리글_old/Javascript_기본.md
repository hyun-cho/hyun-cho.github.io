# __JavaScript 개요__

- 가벼운 인터프리터 또는 JIT 컴파일 프로그래밍 언어
- __일급 함수__ 를 지원, [예제](JavaScript\technique\일급함수.md)
  - 함수를 다른 변수와 동일하게 다루는 언어는 일급 함수를 가졌다고 표현
- 웹 페이지의 스크립트 언어

- 프로토타입 기반의 동적 다중 패러다임 스크립트 언어
  - 인터프리터 형태의 자바가 아니다!

- 다양한 프로그래밍 스타일 지원

  - 객체지향형
  - 명령형
  - 선언형(함수형 프로그래밍)

- Javascript가 DOM 모양을 바꿀 수 있다

<br><br>

# Java와 Javascript의 차이점

|Javascript|Java|
|---|---|
|객체 지향. 객체의 형 간에 차이 없음. 프로토타입 메커니즘을 통한 상속, 그리고 속성과 메서드는 어떤 객체든 동적으로 추가될 수 있음.|클래스 기반. 객체는 클래스 계층구조를 통한 모든 상속과 함께 클래스와 인스턴스로 나뉨. 클래스와 인스턴스는 동적으로 추가된 속성이나 메소드를 가질 수 없음.|
|변수 자료형이 선언되지 않음(dynamic typing, loosely typed).|변수 자료형은 반드시 선언되어야 함(정적 형지정, static typing).|
|하드 디스크에 자동으로 작성 불가.|하드 디스크에 자동으로 작성 가능.|

<br><br>

# 문법과 자료형

- 대소문자 구별, UniCode 문자셋 이용

- `;` (세미콜론)으로 명령문 구분
  - 한줄에 하나의 명령문은 생략가능 (별로 좋은 습관은 x)

## __주석__

```javascript
// 한 줄 주석

/*
 *  여러 줄 주석
 * /

중첩된 주석은 사용 x
```

## __변수__

- 선언에는 3가지 종류.
  - var
    - 변수를 선언, 추가로 동시에 값을 초기화

  - let
    - 블록 범위(scope) 지역 변수를 선언, 동시에 값을 초기화

  - const
    - 블록 범위 읽기 전용 상수를 선언

- 선언되지 않은 변수는 `undefined` 값을 가진다.

- `undefined`값은 Boolean 변수형에서 false이다.

- string 에서는 `NaN`으로 변환

- null 값은 수치에서는 0으로, Boolean에서는 false로 동작

## __변수 범위__

- var은 기본적으로 범위는 블록이 아닌 함수 단위
- let 선언은 다른 언어의 지역번수와 동일

```javascript
if (true) {
    var x = 5;
    let y = 5;
}
console.log(x); // 5
console.log(y); // ReferenceError: y is not defined
```

## __변수 호이스팅__

- javascript의 좋은점(?)
- 나중에 선언된 변수를 참조할 수 있다는 개념
- 이를 위해서 var변수는 블록의 최상단으로 올리고, let은 위치가 상관없다.
- 함수의 경우에는 선언이 호출보다 위에 있어야 한다.

- 변수의 선언 > 함수의 선언 > 변수의 할당 순으로 이루어진다.

- __사실 호이스팅이 일어나지 않도록 코드를 작성하는게 낫다__

```javascript
/**
 * Example 1
 */
console.log(x === undefined); // logs "true"
var x = 3;


/**
 * Example 2
 */
// undefined 값을 반환함.
var myvar = "my value";

(function() {
  console.log(myvar); // undefined
  var myvar = "local value";
})();
```

## 데이터 형

- 원시 데이터 형
  - Boolean
  - null
  - undefined
  - Number
  - String
  - Symbol
  - Object

- 숫자와 문자의 자동변환에 대해서

```javascript
"37" - 7 // 30
"37" + 7 // 377

문자열 > 숫자의 변환은 다음을 사용을 권장
(+"1.1") + (+"1.1") //2.2
```

## 리터럴

- 배열

```javascript
var coffees = ['French", ...]
```

- Boolean

```javascript
true or false
```

- 부동 소수점

```javascript
3.1415926
-.123456789
-3.1E+12
.1e-23
```

- 정수

```javascript
0, 117 및 -345 (10진수)
015, 0001 및 -0o77 (8진수)
0x1123, 0x00111 및 -0xF1A7 (16진수)
0b11, 0b0011 및 -0b11 (2진수)
```

- 객체

```javascript

var car = { myCar: "Saturn", getCar: carTypes("Honda"), special: sales };

console.log(car.myCar);   // Saturn
console.log(car.getCar);  // Honda
console.log(car.special); // Toyota
```

- 정규식

```javascript
var re = /ab+c/;
```

정규식 같은 경우 다음의 [링크](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)에 자세히

- 문자열

```javascript
"foo"
'bar'

ES2015
var name = "foo", time = "today";
`Hello ${name}, how ar you ${time}`

```

문쟈열 리터럴을 자동으로 임시 문자열 객체로 변환, 메소드 호출 뒤 폐기  

<br><br>

# 흐름 제어와 오류 처리

- 블록 범위 : `{ }`
- 조건문

  `if ... else ...`
- 거짓으로 취급하는 값
  - false
  - undefined
  - null
  - 0
  - NaN
  - the empty string ("")

  `switch ... case 1: ... case 2: ...`

- 예외처리문

  `throw expression;`  
  `try ... catch ... finally ...`  

- `finally`구문의 내용이 `try`, `catch`구문과 상관없이 호출되기 때문에, try catch 구문에 return 구문이 있어도, finally 구문에 return문이 있다면, finally의 return이 반환값이 됩니다.

## Promises

- ECMAScript2015에서의 변화점, 지연된 흐름과 비동기식의 연산은 제어할 수 있는 [`Promise`](JavaScript\technique\Promise.md) 객체에 대한 지원을 함

- 자세한건 [Promise](JavaScript\technique\Promise.md) 항목 참조

# 루프와 반복

- for
- do ... while
- while
- 레이블 문  
  - label :
  - break 등등 에서 활용할 수는 있으나 글쎄.. C언어의 goto문 같아서 안쓰는게 좋아보인다.
- break
- continue
- for ... in
  - 객체의 열거 속성을 통해 지정된 변수를 반복한다.
  - javascript의 Object?

```javascript
let obj = {
  'key1': 'value1',
  'key2': 'value2'
}
for (var i in obj) {
  console.log(i, obj[i]) //key, value
}
```

- for ... of
  - 반복 가능한 객체를 통해 반복하는 루프 (배열, Map, Set, 인수 객체 등등)

```javascript
let arr = [3, 5, 7];
arr.foo = "hello";

for (let i in arr) {
   console.log(i); // logs "0", "1", "2", "foo"
}

for (let i of arr) {
   console.log(i); // logs "3", "5", "7"
}
```

# 함수

- 함수 선언

```javascript
function square(number) {
  return number * number;
}
```

- 기본 자료형인 매개변수(parameter)는 값으로 함수에 전달(call by value)
- 매개변수로 배열이나 사용자 정의 객체 등 기본 자료형이 아닌 경우 함수 밖에서도 값이 변경됩니다. (call by reference)
- 함수를 함수표현식을 통해 변수로 표현가능

```javascript
var square = funtion(number) {
  return number * number;
}
```

- 함수는 범위 내에 (함수 영역 등) 있어야 하며, 호이스팅이 일어난다.
  - 함수 호이스팅은 함수표현식에서는 작동하지 않는다!!

```javascript
console.log(square);   // square는 초기값으로 undefined를 가지고 호이스트된다.
console.log(square(5));  // TypeError: square는 함수가 아니다.
square = function (n) {
  return n * n;
}
```

## 함수의 범위

- 함수 내에서 정의된 변수는 함수의 범위에서만 정의 된다.
- 전역함수는 모든 전역 변수를 액세스 할 수 있다.
- 자기 자신을 참조하기 위한 방법

```javascript
var foo = function bar() {
  //statement
}
foo(), bar(), arguments.callee();
```

## 중첩된 함수와 클로저

- 함수 내에 함수를 끼워 넣을 수 있다.
- 중첩 된 함수는 외부 함수와 별개이며 클로저를 형성한다.
- 클로저 : 그 변수를 결합하는 환경을 자유롭게 변수와 함께 가질 수 있는 표현
  - 내부 함수는 외부 함수의 명령문에서만 액세스 가능
  - 내부 함수는 클로저를 형성
    - 외부 함수는 내부 함수의 인수와 변수를 사용 할 수 없는 반면
    - 내부 함수는 외부 함수의 인수와 변수를 사용 가능

```javascript
function outside(x) {
  function inside(y) {
    return x + y;
  }
  return inside;
}
fn_inside = outside(3); // Think of it like: give me a function that adds 3 to whatever you give it
result = fn_inside(5); // returns 8

result1 = outside(3)(5); // returns 8
```

- 여기서 외부 함수의 인수 x가 보존되는 것을 볼 수 있다.
- 메모리는 그 무엇도 내부 함수에 접근하지 않을 때 (unreachable) 해제된다.

- 클로저에 더 자세히는 [여기](JavaScript\technique\클로저.md)서

## argument 객체

- `argument[i]`
- 보통 함수에 정의된 개수보다 많은 인수를 넘겨주며 함수를 호출 가능
- 배열과 비슷한 형태
- ~~이런식으로 함수 오버로딩이 가능하다니..~~

```javascript
function myConcat(separator) {
   var result = ""; // 리스트를 초기화한다
   var i;
   // arguments를 이용하여 반복한다
   for (i = 1; i < arguments.length; i++) {
      result += arguments[i] + separator;
   }
   return result;
}
// returns "red, orange, blue, "
myConcat(", ", "red", "orange", "blue");
```

- argument는 실제 배열은 아니라고 한다.
  - index로 색인이 가능해 비슷 할 뿐 배열의 모든 메소드를 가지진 않는다.

## 디폴트 매개변수

- 파라미터에 값은 기본적으로 `undefined`가 전달된다.
- 파라미터 기본값을 넣어주면, 디폴트 파라미터가 바뀌게 된다.

## 나머지 매개변수

- 불확실한 개수의 인수를 나타낼 수 있다.

```javascript
function multiply(multiplier, ...theArgs) {
  return theArgs.map(x => multiplier * x);
}

var arr = multiply(2, 1, 2, 3);
console.log(arr); // [2, 4, 6]
```

## 화살표 함수

- 함수 표현식과 비교적 짧은 문법을 가짐.
- 사전적으로 this 값을 묶음
- 언제나 익명 함수!!  
- 더 짧은 함수가 환영받음

```javascript
var a = [
  "Hydrogen",
  "Helium",
  "Lithium",
  "Beryl­lium"
];

var a2 = a.map(function(s){ return s.length });
console.log(a2); // logs [8, 6, 7, 9]
var a3 = a.map( s => s.length );
console.log(a3); // logs [8, 6, 7, 9]
```

- 사전적 __this__
- 다른 언어에서의 this처럼 사용할 수 있게 됨
- javascript에서의 this의 동작은 [여기](JavaScript\technique\this.md)서 공부  

# 표현식과 연산자

## 연산자 종류

- 다른 언어와 다른 점? 위주로 정리해보자

### 할당 연산자

- `=`를 사용하여 값을 할당
- `*=`, `+=`등 사용가능
- `**`를 사용하여 거듭제곱 사용

#### 구조분해

- 배열이나 객체의 구조를 반영해 배열이나 객체에서 데이터를 추출 할 수 있다.  

```javascript
var foo = ['one', 'two', 'three'];

// 구조 분해를 활용하지 않은 경우
var one   = foo[0];
var two   = foo[1];
var three = foo[2];

// 구조 분해를 활용한 경우
var [one, two, three] = foo;
```

## 비교 연산자

- 기존의 비교 연산자가 `==`인 것에 비해 javascript에서 엄격한 비교 연산자인 `===`를 제공한다.

```javascript
var var1 = 3;
var var2 = 4;

3 == var1 // true
"3" == var1 // true

3 === var1 // true
"3" == var1 // false
```

## 산술 연산자

- 산술 연산의 경우 부동 소수점이 기본이다.

```javascript
1 / 2; // 0.5
1 / 2 == 1.0 / 2.0; // true
```

## 논리 연산자

- expr1을 true로 변환할 수 있는 경우 expr2을 반환하고, 그렇지 않으면 expr1을 반환합니다.

```javascript
특이한 경우
var a5 = "Cat" && "Dog";    // t && t returns Dog
var a6 = false && "Cat";    // f && t returns false
var a7 = "Cat" && false;    // t && f returns false

var o5 = "Cat" || "Dog";    // t || t returns Cat
var o6 = false || "Cat";    // f || t returns Cat
var o7 = "Cat" || false;    // t || f returns Cat
```

### IN 연산자

- Object의 속성의 이름, 또는 배열의 인덱스를 나타내는 숫자, 또는 Object의 key

### instanceof 연산자

- 명시된 객체가 명시된 객체형인 경우 true를 반환

```javascript
var theDay = new Date(1995, 12, 17);
if (theDay instanceof Date) {
  // statements to execute
}
```

### __this__

- 현재 객체를 참조하는 데 this 키워드 사용 가능
- `this["propertyName"]`, `this.propertyName`으로 접근가능

# 숫자와 날짜

## 숫자

### Number 객체, Math 객체

- [링크](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Numbers_and_dates)

```javascript
Number.parseFloat("0.5");
Number.parseInt("123");
```

### Date 객체

- 날짜 데이터 타입이 없지만, Date 객체를 사용하여 날짜와 시간을 처리 가능

```javascript
var today = new Date();
var endYear = new Date(1995, 11, 31, 23, 59, 59, 999); // Set day and month
endYear.setFullYear(today.getFullYear()); // Set year to this year
var msPerDay = 24 * 60 * 60 * 1000; // Number of milliseconds per day
var daysLeft = (endYear.getTime() - today.getTime()) / msPerDay;
var daysLeft = Math.round(daysLeft); //returns days left in the year
```

# 문자열

- 문자열 리터럴 - 16비트 부호 없는 정수값(UTF-16 code units)의 집합?

## 문자열 리터럴

- `\xA9` : 16진수
- `\u00A9` : 유니코드

- .length 속성 사용가능

- 다양한 메서드 [여기](https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Text_formatting)서 확인 가능

- 기본적으로 `''`, `""` 사용

```javascript
// `를 사용하여 다중 선 문자열 또는 syntactic sugar 구문 사용가능
var a = 5;
var b = 10;
console.log(`Fifteen is ${a + b} and\nnot ${2 * a + b}.`);
```

# 배열 객체

## 배열의 특성

- 배열에 양의 정수가 아닌 값을 줄 격우, 배열의 요소가 대신 배열로 대변되는 객체의 속성이 생성

```javascript
var arr = [];
arr[3.4] = 'Oranges';
console.log(arr.length);                // 0
console.log(arr.hasOwnProperty(3.4));   // true
```

- 배열 또한 객체이기때문에, `[]`를 사용해서 Property에 접근 가능하다.

- length property를 임의로 변경 가능하며 벗어나는 범위의 내용은 삭제된다.

### 반복처리

- 일반적인 방법

```javascript
var colors = ['red', 'green', 'blue'];
for (var i = 0; i < colors.length; i++) {
  console.log(colors[i]);
}
```

- DOM nods의 효율적인 방법

```javascript
var divs = document.getElementsByTagName('div');
for (var i = 0, div; div = divs[i]; i++) {
  /* Process div in some way */
}
```

- forEach() 메서드를 활용한 방법

```javascript
var colors = ['red', 'green', 'blue'];
colors.forEach(function(color) {
  console.log(color); // red green blue
});

color.forEach(color => console.log(color));
```

- forEach 구문에 대해서, 정의 되지 않은 요소는 생략됨.
  - undefined라도, 배열 안에서 정의 되었다면 생략되지 않음

- `for ... in` 속성은 사용하지 않는게 좋음. 왜냐면 배열 속성뿐만아니라 객체 속성까지 나타내기 때문

### 유용한 메서드들

- `concat()` : 두 배열을 합쳐 새로운 배열을 만듬 (python의 extend?)
- `join(delimiter = ",")` : 배열의 모든 요소 사이에 delimiter를 넣은 문자열 반환
- `push()` : 맨 뒤에 요소를 추가, 길이 반환
- `pop()` : 마지막 요소 삭제, 제거된 요소 반환
- `shift()` : 첫번째 요소 삭제, 제거된 요소 반환
- `unshift()` : 하나 혹은 그 이상의 요소를 배열의 앞쪽에 추가, 길이 반환
- `slice(start_index, upto_index)` : 배열의 특정 부분을 추출해 새로운 배열 반환, sub_array 만들때 씀
- `splice(index, count_to_remove, addElement1, addElement2, ...)` : 주어진 인덱스 요소를 포함해 count_to_remove만큼 삭제하고, 추가 요소로 바꿈.  
  - 요소를 삭제할 때 주로 사용  
- `reverse()` : 배열을 뒤집는다.
- `sort()` : 배열의 요소를 제자리에 정렬하고 배열에 대한 참조 반환
  - compare 함수를 파라미터로 넘겨줄 수 있다.
- `indexOf(searchElement, [, fromIndex])`, lastIndexOf(searchElement, [, fromIndex])` : 배열에서 searchElement를 찾는다. 앞에서 뒤에서 차이
- `forEach(callback[, thisObject])` : 반복적으로 주어진 콜백 호출
- `map(callback[, thisObject])` : 배열의 모든 요소에 대해 콜백함수를 실행하고 콜백함수의 실행결과를 새로운 배열에 담아 반환
- `filter(callback[, thisObject])` : 콜백함수가 true를 반환하는 요소만 새로운 배열에 담음
- `every(callback[, thisObject])` : 콜백함수가 모든 항목에 대해 true를 반환해야만 true를 반환
- `some(callback[, thisObject])` : 모든 요소에 대해서 콜백함수를 실행하고 하나의 요소라도 콜백함수가 ture면, some() 메서드의 결과는 true다.
- `reduce(callback[, thisObject]), reduceRight(callback[, thisObject])` : 배열내의 요소를 하나의 요소로 줄이기 위해 firstvalue, secondvalue를 받아서 실행.
  - 예시

```javascript
var a = [10, 20, 30];
var total = a.reduce(function(first, second) { return first + second; }, 0);
console.log(total) // Prints 60
```

### NodeList와 배열의 차이

- `element.childNodes`, `document.querySelectorAll()`에서 반환되는 노드의 콜렉션
- forEach()등을 사용가능하다.
- Array.from()을 통해 Array로 변환 가능

- childNodes는 라이브 콜렉션으로 DOM의 변경 사항을 실시간으로 콜렉션에 반영한다.

- querySelectorAll()의 반환값은 정적 콜렉션으로, DOM을 변경해도 콜렉션 내용에 영향을 주지 않는다.

# 키 기반의 컬렉션

## Maps

- 요소의 삽입 순서대로 원소를 순회 [key, value]

### Object와 Map의 비교

- Object와 Map는 값에 키를 할당 할 수 있고, 그 키로 값을 얻을 수 있고, 키를 삭제할 수 있고, 어떤 키에 값이 존재하는지 확인할 수 있다는 점에서 비슷하다.

- 다음과 같은 차이점이 있다.

|   |Javascript|Java|
|---|-----|-----|
|의도치않은 키| Map은 명시적으로 제공한 키 외에는 어떤 키도 가지지 않음 | Object는 프로토 타입을 가지므로, 기본 키가 존재할 수 있다.|
|키 자료형| Map의 키는 함수, 객체 등을 포함한 모든 값이 가능 | Object의 키는 반드시 String 또는 Symbol|
|키 순서| Map의 키는 정렬됨, Map의 순회는 삽입순 | ~~Object의 키는 정렬되지 않는다.~~ 지금은 정렬된다고 한다.|
|크기| Map의 항목 수는 size를 통해 얻음 | 직접 알아내야함 |
|순회| 순회 가능 | 모든 키를 알아낸 후, 키의 배열을 통해 순회 |
|성능| 잦은 키-값 쌍의 추가와 제거에서 좋은 성능 | 잦은 키-값 쌍의 추가와 제거를 위한 최적화x |

### 주요 메소드

- `clear()` : 제거
- `delete(key)` : 특정 키와 값을 제거, 제거하기 전에 has(key)결과를 반환
- `entries()` : 객체 안의 모든 요소를 [key, value] 형태의 Array로 순서대로 가지고 있는 Iterator 객체 반환
- `forEach(callbackFn[, thisArg])` : Map 객체 안에 모든 key/value pair에 집어넣은 순서대로 callbackFn을 부름, thisArg 매개변수가 제공되면 이게 각 callback의 this값으로 사용
- `get(key)` : key에 대한 value 얻음, 없으면 `undefined`
- `has(key)` : Map 객체 안에 key/value 페어가 존재하는지 검사
- `keys()` : key만 넣은 Iterator 객체 반환
- `set(key, value)` : Map 객체에 key/value 페어를 집어넣음
- `values()` : value만 넣은 Iterator 객체 반환

## WeakMap

- Object만을 키로 허용하고, 값은 임의의 값을 사용하는 key/value 형태의 요소 집합

- WeakMap 내의 key는 약하게 유지되어, 다른 강한 키 참조가 없는경우, 그러면 모든 항목은 가비지 컬렉터에 의해 WeakMap에서 제거된다.

- 열거 불가(키 목록을 반환하는 메소드가 존재하지x)

- 사용해야하는 이유
  - javascript의 가비지 컬렉션은 참조되는, 도달 가능한 값을 메모리에 유지한다.
  - Map의 key로서 도달가능한 오브젝트들은 가비지컬렉션의 대상이 되지 않는다.
  - 이를 방지하기 위해 WeakMap을 사용

- 사용되는 곳
  - 추가 데이터
    - 부차적인 데이터를 저장할 곳이 필요할 때
    - `weakMap.set(john, "비밀문서");` 를 설정하면, john이 파기되면, 비밀문서도 파기됨.
  - 캐싱
    - 시간이 오래 걸리는 작업의 결과를 저장해 연산 시간과 비용을 절약해주는 기법
    - 동일한 함수를 여러 번 호출해야 할 때, 최초 호출 시 반환된 값을 어딘가에 저장해 놨다가, 다음에 함수를 호출하는 대신 저장된 값을 사용
    - 함수 연산결과 등을 WeakMap에 저장가능

```javascript
//캐싱 예제
// 📁 cache.js
let cache = new WeakMap();

// 연산을 수행하고 그 결과를 위크맵에 저장합니다.
function process(obj) {
  if (!cache.has(obj)) {
    let result = /* 연산 수행 */ obj;

    cache.set(obj, result);
  }

  return cache.get(obj);
}

// 📁 main.js
let obj = {/* ... 객체 ... */};

let result1 = process(obj);
let result2 = process(obj);

// 객체가 쓸모없어지면 아래와 같이 null로 덮어씁니다.
obj = null;

// 이 예시에선 맵을 사용한 예시처럼 cache.size를 사용할 수 없습니다.
// 하지만 obj가 가비지 컬렉션의 대상이 되므로, 캐싱된 데이터 역시 메모리에서 삭제될 겁니다.
// 삭제가 진행되면 cache엔 그 어떤 요소도 남아있지 않을겁니다.
```

- 단점
  - 반복 작업이 불가능 하다는 점
  - 저장된 자료를 한번에 얻는게 불가능 하다.

## Sets

- 값들의 집합.
- 입력 순서에 따라 저장된 요소를 반복처리 가능
- 중복된 값을 허용하지 않는다.
- 전개 연산자를 통해 Array로 변환가능 `[...mySet]`

## WeakSet

- WeakMap과 같이 Object만 저장할 수 있다.
- 중복된 개체는 없고, 열거할 수 없다.

# Object (객체)

- 객체는 Property의 모음이다.
- Property는 이름(name)과 값(value)로 이루어진다.
- Property의 value가 함수일 수 있는데, 이 경우 메소드라 불린다.
- `.` 을 통해서 Property를 접근 가능
- ["Property"]를 통해서도 Property 접근 가능
- 빈칸이나 `-` 등의 문자는 대괄호 접근만 가능
- `for ... in`을 사용하여 객체의 열거 가능한 Property에 대해 접근 가능

## 메소드

- 객체의 Property 중 함수인 것
- 메소드에서 this를 사용하게되면, 메소드를 포함한 객체를 가리킨다.

### getter, setter

- get set 메소드를 설정 가능

```javascript
var o = {
  a: 7,
  get b() { return this.a + 1; },
  set c(x) { this.a = x / 2; }
};
//-----------
var o = { a:0 }

Object.defineProperties(o, {
    "b": { get: function () { return this.a + 1; } },
    "c": { set: function (x) { this.a = x / 2; } }
});

o.c = 10 // Runs the setter, which assigns 10 / 2 (5) to the 'a' property
console.log(o.b) // Runs the getter, which yields a + 1 or 6
```

- `delete` 연산자를 통해 Property 삭제 가능
- 객체 간 비교의 경우, 그 속성값을이 모두 같아도 equal이 될 수 없다.

```javascript
// 속성은 같지만 서로 별개인 두 변수와 두 객체
var fruit = {name: "apple"};
var fruitbear = {name: "apple"};

fruit == fruitbear // false 리턴
fruit === fruitbear // false 리턴
// 두 개의 변수이지만 하나의 객체
var fruit = {name: "apple"};
var fruitbear = fruit;  // fruit 객체 레퍼런스를 fruitbear 에 할당

// here fruit and fruitbear are pointing to same object
fruit == fruitbear // true 리턴
fruit === fruitbear // true 리턴
```

# 프로토타입 기반 언어

- javascript는 클래스 기반이 아닌 prototype에 기초한 객체 기반 언어
- 클래스 기반의 언어들은 두개의 구별되는 개념에 기반
  - 클래스 : 특정 객체군을 특징 짓는 모든 속성
  - 인스턴스 : 클래스를 기반으로 실체화된 것
- 프로토타임 기반의 언어들은 클래스와 인스턴스의 차이를 두지 않는다. - 객체를 가질 뿐
- 원형(prototype)의 객체 개념을 가짐
- 하나의 객체는 새로운 객체를 생성했을 때 초기 속성을 가질 수 있게 하는 형판(template)으로 사용.

## 하위 클래스와 상속

- 클래스 기반 언어에서는 클래스 정의를 통해 클래스 계층구조를 생성
  - 클래스를 정의 할 때 이미 존재하는 클래스의 하위 클래스를 새로운 클래스로 지정할 수 있다.

- 자바스크립트는 생성자 함수와 프로토타입 객체를 연결해 상속을 구현

## 속성의 추가 삭제

- 클래스 기반 언어에서는 컴파일 시점에 클래스를 생성한 후에, 컴파일 시점 혹은 실행 시 해당 클래스의 인스턴스 생성
- 자바스크립트에서는 실행시에 객체의 속성을 추가 혹은 삭제 가능
  - 객체군의 프로토타입으로 사용되는 객체에 속성을 추가하면, 프로토타입이 되는 객체들에도 새로운 속성이 추가

## 클래스 기반 언어와, 프로토타입 기반언어의 차이점 정리

|클래스 기반(자바) | 원형 기반(자바스크립트) |
|---|---|
|클래스와 인스턴스는 별개입니다.|모든 객체는 다른 객체로부터 상속을 받습니다.|
|클래스 정의를 가지고 클래스를 생성하고 생성자 메서드로 인스턴스를 생성합니다.|생성자 함수를 가지고 객체군을 정의 및 생성합니다.|
|new 연산자로 하나의 객체(인스턴스)를 생성합니다.|동일합니다.|
|이미 존재하는 클래스에 대한 하위 클래스를 정의함으로써 객체의 계층구조를 생성합니다.|하나의 객체를 생성자 함수와 결합된 프로토타입에 할당함으로써 객체의 계층구조를 생성 합니다.|
|클래스의 상속 구조에 따라 속성을 상속 받습니다.|프로토타입 체인에 따라  속성을 상속 받습니다.|
|클래스 정의는 모든 인스턴스의 모든 속성을 명시합니다. 실행시에 동적으로 속성을 추가할 수 없습니다.|생성자 함수 혹은 프로토타입은 초기 속성들을 명시합니다. 개별 객체 혹은 전체 객체군에 동적으로 속성을 추가 삭제할 수 있습니다.|

- javascript의 상속을 이해하기 위해 [다음 글](https://www.zerocho.com/category/JavaScript/post/573d812680f0b9102dc370b7) 참조하면 좋음

# Promise

- Promise에 관해서 [여기](JavaScript\technique\Promise.md)서 자세히

# Iterator

- 반복자는 시퀀스를 정의하고, 종료시의 반환값을 잠재적으로 정의하는 객체
- 두 개의 속성(value, done)을 반환하는 next() 메소드를 사용하여 객체의 Iterator protocol을 구현
- 마지막 값이 산출되었다면, done값은 true가 된다.
- `next()` 메소드를 반복적으로 호출하여 명시적으로 반복시킬 수 있다.


- next() 메소드를 호출함으로서 어떤 값이 소비되면, 생성자 함수는 yield 키워드를 만날 때 까지 생성된다.

```javascript
function* fibonacci(){
  var fn1 = 0;
  var fn2 = 1;
  while (true){
    var current = fn1;
    fn1 = fn2;
    fn2 = current + fn1;
    var reset = yield current;
    if (reset){
        fn1 = 0;
        fn2 = 1;
    }
  }
}

var sequence = fibonacci();
console.log(sequence.next().value);     // 0
console.log(sequence.next().value);     // 1
console.log(sequence.next().value);     // 1
console.log(sequence.next().value);     // 2
console.log(sequence.next().value);     // 3
console.log(sequence.next().value);     // 5
console.log(sequence.next().value);     // 8
console.log(sequence.next(true).value); // 0
console.log(sequence.next().value);     // 1
console.log(sequence.next().value);     // 1
console.log(sequence.next().value);     // 2
```

# Proxy

- 기본적인 동작의 새로운 행동을 정의

- `new Proxy(target, handler);`

- \