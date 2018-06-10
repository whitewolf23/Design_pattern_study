#싱글톤 패턴

- 전역으로 단 하나만의 객체를 두고 사용하는 방식
- 특정 클래스의 인스턴스를 오직 하나만 유지
- 새로운 객체를 만들면 실제로 이 객체는 다른 어떤 객체와도 같지 않다 .
- 전체 시스템에서 하나의 인스턴스만 존재하도록 보장하는 객체 생성 패턴
- 객체 리터럴도 싱글톤 패턴
- 하나의 객체 인스턴스만 존재 해야할때 사용
- 최초 한번만 메모리를 할당하고, 그 메모리에 인스턴스를 만들어 사용한다.

#장점

- 고정된 메모리 영역을 얻으면서 한번의 new 로 인스턴스를 사용하기 때문에 메모리 낭비 방지
- 싱글톤으로 만들어진 클래스의 인스턴스는 전역 인스턴스이기 때문에 다른 클래스와에 데이터 공유하기 쉽다.
- 캐시처럼 모듈간 데이터 공유 용도로 최적

#단점

- 싱글톤 인스턴스가 너무 많은 일을 하거나 많은 데이터를 공유시킬 경우, 다른 클래스의 인스턴스들 간의 결합도가 높아져 "개방 - 패쇄 원칙에 위배"

## 객체지향 설계원칙(a.k.a SOLID 원칙)

Single Responsibility Principle (단일 책임 원칙)
Open/Closed Principle (개방/폐쇄 원칙)
Liskov Substitution Principle (리스코프 치환 원칙)
Interface Segregation Principle (인터페이스 분리 원칙)
Dependency Inversion Principle (의존성 역전 원칙)

# 객체 리터럴(Object Literal) : Object 객체의 구조를 정의 하고 생성

```js
var singletonObj = {
  a: "값",
  b: function() {}
};
```

# EX1

```js
var obj = {
  myprop: "my value"
};
var obj2 = {
  myprop: "my value"
};
obj === obj2; // false
obj == obj2; // false
```

- 자바스크립트에서 객체들은 동일한 객체가 아니고서는 절대로 같을 수 없다. 완전히 같은 멤버를 가지는 똑같은 객체를 만들더라도, 이전에 만들어진 객체와 동일 하지는 않다.

- 객체 리터럴을 이용해 객체를 생성할 때마다 사실은 싱글톤을 만드는 것과 같으며, 싱글톤을 위해 별도의 문법이 존재하지는 않는다.

```js
var singleton = (function() {
  var instance;
  var a = "hello";
  function initiate() {
    return {
      a: a,
      b: function() {
        alert(a);
      }
    };
  }
  return {
    getInstance: function() {
      if (!instance) {
        instance = initiate();
      }
      return instance;
    }
  };
})();
var first = singleton.getInstance();
var second = singleton.getInstance();
console.log(first === second); // true;
```

- IIFE 로 비공개 변수를 가질수 있게 만들어, initiate() 함수내 return 안에 부분이 싱글톤 객체
- getInstance 함수 호출 -> 내부적으로 initiate 함수 호출 -> inistance 에 이전에 객체 내용이 저장 되면서 동시 반환
- 만약 여러번 호출시, initiate 를 거치지 않고 바로 반환 하기 때문에, 동일한 객체가 나옴

#즉시실행 함수표현(IIFE)

```js
(function() {
 // private 함수또는 변수들을 선언하세요.

 return {
// public 함수 또는 변수를 선언하세요.
}

}();
```
