# Promise 객체
- 비동기 작업이 맞이할 미래의 완료 또는 실패와 그 결과 값을 나타낸다.

## 사용법
- 함수에 콜백을 전달하는 대신, 콜백을 첨부하는 방식의 객체

## 콜백과 비교한 장점
- Javascript Event Loop이 현재 실행중인 콜 스택을 완료하기 이전에는 절대 호출 되지 않는다.
- 비동기 작업이 성공하거나 실패한 뒤에 then()을 이요하여 추가적인 콜백이 일어날수있다.
- then()을 여러번 사용해 여러개의 콜백을 추가할 수 있다.
- 각각의 콜백은 주어진 순서대로 하나 하나 실행되게 된다.

- Promise의 가장 뛰어난 장점 중 하나는 Chaining이다.

## Chaining
- 하나나 두개 이상의 비동기 작업을 순차적으로 실행해야 할 때 사용
- 순차적으로 이전 단계의 비동기 작업이 성공하고, 그 결과값을 이용해 다음 비동기 작업을 실행
- Promise Chain을 통해 문제 해결  

- then 함수에서 새로운 Promise객체 반환

- 지옥의 콜백 피라미드(... ㅋㅋ)를 피할 수 있다.
```javascript
Callback
doSomething(function(result) {
  doSomethingElse(result, function(newResult) {
    doThirdThing(newResult, function(finalResult) {
      console.log('Got the final result: ' + finalResult);
    }, failureCallback);
  }, failureCallback);
}, failureCallback);

Promise_Chain
doSomething().then(function(result) {
  return doSomethingElse(result);
})
.then(function(newResult) {
  return doThirdThing(newResult);
})
.then(function(finalResult) {
  console.log('Got the final result: ' + finalResult);
})
.catch(failureCallback);
```

- 기존 콜백 API를 활용해서 Promise를 만들수도 있다.

- Composition, Timing, Nesting에 대해서 더 공부해 볼것 [링크](https://wiki.developer.mozilla.org/ko/docs/Web/JavaScript/Guide/Using_promises#Guarantees)

## Composition
- Promise Chaining이 순차적으로 Promise를 처리한다면, Composition(구성)은 메소드의 병렬처리를 위한 방식이다.
- Promise.all()은 모든 Promise 배열의 비동기 처리가 모두 종료되면 결과값을 배열로 반환한다.
- all()에서는 하나라도 reject 된다면, 결과는 reject된다.
- Promise.race()는 경주를하듯이 Promise 객체 중 가장 먼저 처리 된 Promise 결과가 반환된다.
- race()는 resolve나 reject와 상관없이 가장 빠른 녀석이 주인공이다.


# Promise의 상태
- Promise의 상태는 다음과 같다
  - Pending : 초기상태, fulfilled 되거나 rejected되지 않음
  - Fulfilled : 연산 수행 성공
  - Rejected : 연산 수행 실패
  - Settled : Promise가 fulfilled 이거나 rejected 이지만 pending은 아님


<p align="center">
    <br>
    <img src="https://mdn.mozillademos.org/files/8633/promises.png">
    <br>
</p>





# async 함수
- async 함수는 Promise 객체를 반환한다.
- resolve 상태면 then으로 처리, reject 상태면 catch에서 처리한다.

- Promise를 반환하는 코드를 다수 기술이 필요할 경우, await를 사용하여 읽기 쉽게 기술할 수 있다.
await는 resolve 상태의 값은 좌향에 바인딩 하고, reject 상태는 async의 catch로 전달된다.
```javascript
const delay = (time, value) => new Promise(resolve => {
  setTimeout(() => resolve(value), time);
});
```
```javascript
async function f() {
  const a = await delay(1000, 'a');
  const b = await delay(2000, 'b');
  return `${a}${b}`;
}

f().then(console.log) // ab
```
```javascript
async function f() {
  const a = await delay(1000, 'a');
  const b = await Promise.reject('에러 발생!');
  return `${a}${b}`;
}

f().catch(console.log) // 에러 발생!
```

- await를 사용된 함수에서 예외가 발생하면 catch로 전달된다.

```javascript
const g = () => Promise.resolve(die)
const f = async () => {
  return await g()
}

f().catch((err) => console.log('에러 발생!', err)) // 에러 발생!
```

- await를 사용하게 되면, 해당 값을 반환 되기전까지 async 내부 함수는 일시 중단이 될 수 있다.
  - ~~일시 중단을 위해서 await를 쓰는 줄 알았다.. 아니 사실 결과적으로 맞는것 같긴 하다.~~

- await 키워드는 async 함수에서만 사용 가능하다.