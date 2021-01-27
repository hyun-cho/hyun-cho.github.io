# 일급함수란?

함수를 다른 변수와 동일하게 다루는 언어는 일급 함수를 가졌다고 표현

## 변수에 함수 할당

```javascript
const foo = function() {
   console.log("foobar");
}
// 변수를 사용해 호출
foo();
```

## 함수를 인자로 전달

다른 함수에 인자로 전달된 함수를 _콜백 함수__ 라고 한다.

```javascript
function sayHello() {
   return "Hello, ";
}
function greeting(helloMessage, name) {
  console.log(helloMessage() + name);
}
// `sayHello`를 `greeting` 함수에 인자로 전달
greeting(sayHello, "JavaScript!");
```

## 함수 반환

함수를 반환하는 함수를 __고차함수__ 라고 부른다.
```javascript
function sayHello() {
   return function() {
      console.log("Hello!");
   }
}
```

고차함수는 다음과 같은 방법을 활용해 호출

```javascript
const sayHello = function() {
   return function() {
      console.log("Hello!");
   }
}
const myFunc = sayHello();
myFunc();
sayHello()();
// 두 방식이 같음
```