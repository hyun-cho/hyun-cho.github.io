# 클로저

- Javascript의 강력한 기능 중 하나
- 함수의 중첩이 일어나고, 내부 함수가 외부 함수 안에서 정의된 모든 변수와 함수들은 안전하게 접근 할 수 있다.
- 하지만 외부 함수는 내부 함수 안의 변수와 함수에 접근 X
- 일종의 캡슐화 기능 제공

- 간단한 예시

```javascript
var pet = function(name) {   // 외부 함수는 'name'이라 불리는 변수를 정의합니다.
  var getName = function() {
    return name;             // 내부 함수는 외부 함수의 'name' 변수에 접근합니다.
  }
  return getName;            // 내부 함수를 리턴함으로써, 외부 범위에 노출됩니다.
},
myPet = pet("Vivie");
myPet();                     // "Vivie"로 리턴합니다.
```

- 더 자세한 예시

```javascript
var createPet = function(name) {
  var sex;
  
  return {
    setName: function(newName) {
      name = newName;
    },
    getName: function() {
      return name;
    },
    getSex: function() {
      return sex;
    },
    setSex: function(newSex) {
      if(typeof newSex == "string" && (newSex.toLowerCase() == "male" || newSex.toLowerCase() == "female")) {
        sex = newSex;
      }
    }
  }
}

var pet = createPet("Vivie");
pet.getName();                  // Vivie

pet.setName("Oliver");
pet.setSex("male");
pet.getSex();                   // male
pet.getName();                  // Oliver
```

- ~~언뜻 보면 class 객체를 구현한 것 같다..~~
- `name`이란 변수에 메소드를 통한 접은은 되나, 직접적인 접근은 불가능하다!

- 내부 함수에서 외부함수와 같은 이름을 사용하면, 다시 외부 함수의 변수에 접근 할 방법이 없어진다.
