의존 관계 한 객체가 다른 객체에 의존
연관 관계 한 객체가 다른 객체를 객체 변수로 가지고 있음
- 집합관계 약하게 결합
- 합성관계 ( 라이프 사이클이 같음) 강하계 연결

```java

class MainMovieListener {
    MovieFinder movieFinder;
    MovieListener() {
        movieFinder = new KoreanMovieFinder();
    }
}

```

- MovieListener는 2개의 클래스에 연관 , 직접적으로는 KoreanMovieFinder

구상체인 클래스에 강결합이 되어버림 (new 때문)

```java
// DI 사용
class MainMovieListener {
    MovieFinder movieFinder;
    MovieListener(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }
}
```

DI는 의존성을 주입
- 직접 구상체를 생성해서 강결합으로 만드는게 아닌, 클라이언트에게 의존성 주입 책임을 위임 > 해당 모듈레벨에서는 약결합 상태

- 책임이 위로 올라감, 인터페이스에 의존

더 상위 객체가 있다면, (어딘가의 최상위 객체) 그 곳에서는 new 강결합이 일어나게 된다.

## IoC

DI를 끝까지 가보자  
하나의 객체에 모든 의존성을 주입(폭탄 강결합)  

결국 프로그래머가 어플리케이션의 객체 생성 제어권을 가지고 있기 때문 >> 라이브러리  

제어권을 다른 객체에게? > DI를 관리하는 가상의 개념을 생성  
개념(Content) -> Type -> interface or class


## instance factor

객체 생성 팩토리로 이름에 맞는 적절한 객체 생성을 담당  

## DependencyInjector

객체 에 알맞은 객체를 의존성 주입을 해준다.  

## 위 두 가지 개념을 합친것이 IoC 컨테이너

의존성을 가진 객체들을 관리 > oop에 배반? > xml로 바뀜  
현재는 annotation

제어의 역전  

객체 생성곽 관계 설정의 제어권을 가상의 컨테이너에 넘겨줌  

메타정보 수정만으로 변경 가능

더 이상 실행되지 않을 이벤트들도(예시에서 load의 경우 한번만 실행) 명시적으로 이벤트를 제거해줘야 하나요?