<!-- 생성자 패턴과 비슷 -->

# 팩토리 패턴

- 비슷한 객체를 생성하는 반복 작업을 수행한다.
- 구체적인 타입을 몰라도 객체를 생성할 수 있게 한다.
- 객체를 생성하기 위한 인터페이스를 정의하는데, 어떤 클래스의 인스턴스를 만들지는 서브클래스에서 결정하게 만든다.
- 즉 팩토리 메소드 패턴을 이용하면 클래스의 인스턴스를 만드는 일을 서브클래스에게 맡기는 것.

# 장점

- 객체 생성 코드를 한 객체 or 메소드에 집어넣으면, 코드에 중복되는 내용을 제거 가능, 나중에 관리할때도 한군데만 신경
- 인터페이스를 바탕으로 프로그래밍 가능해저 유연성과 확장성이 뛰어난 코드를 만듬

# 단점

- 서브 클래스를 만들어서 객체 생성 메소드의 행동을 변경 불가능

# EX1(추상팩토리 패턴)

```js
var peopleFactory = function(name, age, state) {
  var temp = {};

  temp.age = age;
  temp.name = name;
  temp.state = staet;

  temp.printPerson = function() {
    console.log(this.name + this.age + this.state);
  };

  return temp;
};

var person1 = peopleFactory("minsu", 29, "a");
var person2 = peopleFactory("dongsu", 27, "b");
var person3 = peopleFactory("drakejin", 26, "c");

person1.printPerson();
person2.printPerson();
person3.printPerson();
```

# EX2(팩토리메소드 패턴)

```js
class Car {
  constructor(type, color) {
    this.type = type;
    this.color = color;
  }
}
class CarFactoryMethod {
  produce(carName, color) {
    return new Car(carName, color);
  }
}
const cf = new CarFactoryMethod();
const bmw = cf.produce("Bmw", "White");
const ss = cf.produce("Audi", "Black");
```

# EX3(팩토리메소드 패턴 2)

```js
class Pizza {
  constructor() {
    this.price = 0;
  }
  getPrice() {
    return this.price;
  }
}
class HamAndMushroomPizza extends Pizza {
  constructor() {
    super();
    this.price = 8.5;
  }
}
class DeluxePizza extends Pizza {
  constructor() {
    super();
    this.price = 10.5;
  }
}
class DefaultPizza extends Pizza {
  constructor() {
    super();
    this.price = 6.5;
  }
}
class PizzaFactoryMethod {
  createPizza(type) {
    switch (type) {
      case "Ham and Mushroom":
        return new HamAndMushroomPizza();
      case "Deluxe":
        return new DeluxePizza();
      default:
        return new DefaultPizza();
    }
  }
}
const pfm = new PizzaFactoryMethod();
const hnm = pfm.createPizza("Ham and Mushroom");
const dlx = pfm.createPizza("Deluxe");
console.log(hnm.getPrice(), dlx.getPrice());
```
