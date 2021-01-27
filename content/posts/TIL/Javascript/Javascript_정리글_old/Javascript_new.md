# new

- new 연산자는 사용자 정의 객체 타입 또는 내장 객체 타입의 인스턴스 생성

```javascript
function Car(make, model, year) {
  this.make = make;
  this.model = model;
  this.year = year;
}

const car1 = new Car('Eagle', 'Talon TSi', 1993);

console.log(car1.make);
```

- 사용자 정의 객체를 생성하는 데는 두 단계가 필요
  1. 함수를 장성하여 객체 타입을 정의
  1. `new`연산자로 객체의 인스턴스를 생성

- new Foo(...) 가 실행 될 때 다음과 같은 일이 발생
  1. Foo.prototype을 상속하는 새로운 객체가 하나 생성
  1. 명시된 인자 그리고 새롭게 생성된 객체에 바인드된 this와 함께 생성자 함수 Foo가 호출된다.
  1. new Foo는 new Foo()와 동일, 인자가 명시되지 않으면 인자 없이 Foo 객체 생성\
  1. 생성자 함수에 의해 리턴된 객체는 전체 new 호출 결과가 됨
  1. 만약 생성자 함수가 명시적으로 객체를 리턴하지 않으면, 첫 번째 단계에서 생성된 객체가 대신 사용 -> 일반적으로 return하지 않고, override 할 경우에

- 정의된 객체에 속성 추가 가능

- 모든 객체에 속성을 추가하려면 Function.prototype 속성을 사용해 이전에 정의된 객체 타입에 공유 속성을 추가 가능
