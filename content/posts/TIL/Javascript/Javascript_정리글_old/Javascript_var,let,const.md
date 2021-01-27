# Javascript의 선언에 대해서

## let

- 블록 유효 범위 `{ }`를 갖는 지역 변수를 선언, 선언과 동시에 임의의 값으로 초기화

- 변수가 선언 된 블록, 구문 또는 표현식 내에서만 유효한 변수를 선언
  - `var`키워드가 블록 범위를 무시하고, 전역 변수나 함수지역 변수로 선언되는 것과 다른점

### 왜 "let"을 사용해야 하는가? (의미적으로?)

- [참조링크](https://stackoverflow.com/questions/37916940/why-was-the-name-let-chosen-for-block-scoped-variable-declarations-in-javascri)

- 수학적으로 let ~ 하도록 하자. 라는 구문에서 가져온것같다.

- 이와 같은 행동을 하는 구문이 javascript에서는 `var`이므로, 블록 범위의 `let`이 탄생하였다는 이야기

```javascript
var x = 'global';
let y = 'global';
console.log(this.x); // "global" 전역 객체의 속성 x를 생성
console.log(this.y); // undefined 전역 객체의 속성 y를 생성하지 않음
```

- ~~초기화 전 참조될 경우 `var`은 호이스티잉 일어나 `undefined`가 되는 반면, `let`은 호이스팅이 일어나지 않아 `ReferenceError`가 발생합니다.~~
- 라고 생각했는데, 공식문서에서 나 같은 사람을 많이 봤는지 대놓고 아니라고 써놨다..
  - `let`들의 정의가 평가되기 전까지 초기화가 되지 않는 다는 의미로 봐야할 것 같다.

- 생성자와 함께 사용해 클로저를 사용하지 않고 비공개 변수를 만들고 접근 할 수 있다.
  - 클로저 개념과 함께 알아보도록..

## const

- 블록 범위 내의 상수

- 상수 이므로, 상수 초기자(initializer)가 필요함

- 대문자를 사용하는게 관습

- 대부분 상수의 성질과 같음

- 오브젝트의 키는 보호되지 않음

```javascript
const MY_OBJECT = {'key': 'value'};
MY_OBJECT.key = 'otherValue'; // 오브젝트를 변경할 수 없게 하려면 Object.freeze() 를 사용해야 합니다
```

## var

- 변수를 선언하고, 선택적으로 초기화 할 수 있다.

- 어디에 선언되어 있던 변수들은 코드가 실행되기 전에 처리가 된다.

- var로 선언된 변수의 번위는 현재 실행 문맥이다.
  - 여기서 실행 문맥이란, 할수 영역 혹은 함수의 외부에 전역으로 선언된 변수일 수 있다.

- 값 할당은 전역변수처럼 생성된다.

- 기본적으로 `undefined`로 초기화된다.

## var 호이스팅

- var 선언문을 함수 또는 전역 코드의 최상단에 이동하는 것 과 같은 행동

- 이해하기 위한 전역변수와 외부 함수 범위

```javascript
var x = 0;  // x는 전역으로 선언되었고, 0으로 할당됩니다.

console.log(typeof z); // undefined, z는 아직 존재하지 않습니다.

function a() { // a 함수를 호출했을 때,
  var y = 2;   // y는 함수 a에서 지역변수로 선언되었으며, 2로 할당됩니다.

  console.log(x, y);   // 0 2

  function b() {       // b 함수를 호출하였을때,
    x = 3;  // 존재하는 전역 x값에 3을 할당, 새로운 전역 var 변수를 만들지 않습니다.
    y = 4;  // 존재하는 외부 y값에 4를 할당, 새로운 전역 var 변수를 만들지 않습니다.
    z = 5;  // 새로운 전역 z 변수를 생성하고 5를 할당 합니다.
  }         // (strict mode에서는 ReferenceError를 출력합니다.)

  b();     // 호출되는 b는 전역 변수로 z를 생성합니다.
  console.log(x, y, z);  // 3 4 5
}

a();                   // 호출되는 a는 또한 b를 호출합니다.
console.log(x, z);     // 3 5
console.log(typeof y); // undefined y는 function a에서 지역 변수입니다.
```
