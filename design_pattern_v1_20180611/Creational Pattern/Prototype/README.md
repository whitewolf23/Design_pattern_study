# 프로토 타입 디자인 패턴

- 자바스크립트의 프로토타입 상속을 이용
- 높은 성능을 필요로 하는 상황에서 객체르 만들기 위해 사용
- 다른 어플리케이션에서 사용할 객체를 만들기 위해 광범위한 데이터베이스 연산 수행 필요

#prototype 프로퍼티

- 자바스크립트 모든 함수는 prototype 프로퍼티를 갖는다.
- 해당 프로퍼티는 참조 타입의 인스턴스가 가져야할 프로퍼티와 메서드를 갖고 있는 객체
- 객체 인스턴스 전체에서 공유되는 특징.
- 이 프로퍼티를 이용해서 객체지향애 '상속'과 비슷하게 다른 객체의 메소드나 프로퍼티를 이용가능.

#장점

- 객체를 만들때 똑같은 코드를 반복하는 것을 방지

#단점

```js
var peopleProto = function() {};

peopleProto.prototype.age = 0;
peopleProto.prototype.name = "no name";
peopleProto.prototype.city = "no city";

peopleProto.prototype.printPerson = function() {
  console.log(this.name + this.age + this.city);
};

var person1 = new peopleProto();
person1.name = "john";
person1.age = "20";
person1.city = "NewWork";

person1.printPerson();
```

#dynamic prototype pattern

```js
var peopleProto = function(name, age, city) {
  this.age = age;
  this.name = name;
  this.state = state;

  if (typeof this.printPerson !== "function") {
    peopleProto.prototype.printPerson = function() {
      console.log(this.name + this.age + this.city);
    };
  }
};

var person1 = new peopleProto();
person1.name = "john";
person1.age = "20";
person1.city = "NewWork";

person1.printPerson();
```

#어떻게 동작?

- 함수가 생성될때 마다 prototype 프로퍼티 역시 특정 규칙에 따라 생성
- 자동으로 constructor 프로퍼티를 갖는다.
- constructor 프로퍼티는 prototype 이 프로퍼티 로써 소속된 함수를 지칭
