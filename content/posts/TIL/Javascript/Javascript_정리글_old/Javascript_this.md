# __THIS__

- javascript 에서 this는 다른 언어에서의 this와 약간 다름.

- 대부분 this 값은 함수를 호출한 방법에 의해 결정된다.

- 실행중에는 할당으로 설정 할 수 없고, 함수를 호출 할 때 마다 다를 수 있다.

- this 값을 설정 할 수 있는 bind 메서드가 존재

- 화살표 함수를 통해 this의 바인딩을 제공하지 않을 수 있다.

- 전역 실핼 문맥(global execution context)에서 `this`는 strict mode 여부와 관계없이 전역 객체를 참조한다.

```javascript
// 웹 브라우저에서는 window 객체가 전역 객체
console.log(this === window); // true

a = 37;
console.log(window.a); // 37

this.b = "MDN";
console.log(window.b)  // "MDN"
console.log(b)         // "MDN"
```

- 함수 문맥(functional context)에서는 함수를 호출한 방법에 의해 좌우된다.

- 단순 호출

```javascript
function f1() {
  return this;
}

// 브라우저
f1() === window; // true

// Node.js
f1() === global; // true

function f2(){
  "use strict"; // 엄격 모드 참고
  return this;
}

f2() === undefined; // true
```

- `call()`과 `apply()`를 통해 this를 임의적으로 정해줄 수 있다.

```javascript
function bar() {
  console.log(Object.prototype.toString.call(this));
}

bar.call(7);     // [object Number]
bar.call('foo'); // [object String]
bar.call(undefined); // [object global]
```

- `bind()`를 통해서 this의 값을 영구적으로 고정시킬 수 있다. 이는 한번만 동작한다.

```javascript
function f() {
  return this.a;
}

var g = f.bind({a: 'azerty'});
console.log(g()); // azerty

var h = g.bind({a: 'yoo'}); // bind는 한 번만 동작함!
console.log(h()); // azerty

var o = {a: 37, f: f, g: g, h: h};
console.log(o.a, o.f(), o.g(), o.h()); // 37, 37, azerty, azerty
```

- 화살표 함수는 자신을 감싼 정적 범위(lexical context)를 뜻하며, 새로운 this를 할당하지 않는다.
  - call, bind, apply등의 호출을 무시한다.

```javascript
var globalObject = this;
var foo = (() => this);
console.log(foo() === globalObject); // true

// 객체로서 메서드 호출
var obj = {func: foo};
console.log(obj.func() === globalObject); // true

// call을 사용한 this 설정 시도
console.log(foo.call(obj) === globalObject); // true

// bind를 사용한 this 설정 시도
foo = foo.bind(obj);
console.log(foo() === globalObject); // true
```

- 객체의 프로토타입 체인에서의 `this`
  - 객체의 프토토타입 체인 위에 존재하면, this값은 극 객체가 메서드를 가진 것 마냥 설정됨

```javascript
var o = {
  f:function() { return this.a + this.b; }
};
var p = Object.create(o);
p.a = 1;
p.b = 4;

console.log(p.f()); // 5 f에서의 this는 p를 뜻한다
```

- 접근자와 설정자의 this
  - 함수를 접근자와 설정자에서 호출하더라도 동일, 접근하거나 설정하는 속성을 가진 객체로 묶임

```javascript
function sum() {
  return this.a + this.b + this.c;
}

var o = {
  a: 1,
  b: 2,
  c: 3,
  get average() {
    return (this.a + this.b + this.c) / 3;
  }
};

Object.defineProperty(o, 'sum', {
    get: sum, enumerable: true, configurable: true});

console.log(o.average, o.sum); // 2, 6
```

- new 키워드와 함께 생성자로 사용하면 this는 새로 생긴 객체에 묶임
  - `new` 키워드를 더 공부해보자

- DOM 이벤트 처리기로서 사용하면 this는 이벤트를 발사한 요소로 설정
  - target과 currentTarget의 차이점은 이벤트 버블링에 따라, target은 최하위 요소를 반환, currentTarget은 이벤트가 바인딩된 요소를 반환

```javascript
// 처리기로 호출하면 관련 객체를 파랗게 만듦
function bluify(e) {
  // 언제나 true
  console.log(this === e.currentTarget);
  // currentTarget과 target이 같은 객체면 true
  console.log(this === e.target);
  this.style.backgroundColor = '#A5D9F3';
}

// 문서 내 모든 요소의 목록
var elements = document.getElementsByTagName('*');

// 어떤 요소를 클릭하면 파랗게 변하도록
// bluify를 클릭 처리기로 등록
for (var i = 0; i < elements.length; i++) {
  elements[i].addEventListener('click', bluify, false);
}
```

- 인라인 이벤트 처리기로 사용하면, this는 처리기를 배치한 DOM 요소로 설정

```html
<button onclick="alert(this.tagName.toLowerCase());">
  this 표시
</button>

위의 경고창은 button을 보여준다. 바깥쪽 코드만 this를 이런식으로 설정

<button onclick="alert((function() { return this; })());">
  내부 this 표시
</button>

위와 같이 내부 함수의 this는 정해지지 않았으므로 전역/window 객체를 반환.
```
