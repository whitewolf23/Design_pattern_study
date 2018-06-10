# 데코레이터 패턴

- 객체를 바꾸지 않고 기능을 늘리는 패턴
- 런타임시 객체에 동적으로 부가기능을 추가할 수 있는 패턴

# 장점

- 객체의 동작을 동적으로 확장 가능하다
- 런타임 변경을 허용 -> 유연성 제공
- 적은 클래스를 정의하면서 여러 기능을 무한대로 혼합하여 사용할 수 있게 함
- 기본적으로 데이터에 첨가할 데이터가 일정치 않을때 효율적이다.

# 단점

- 너무 많은 객체들이 추가 될 수가 있어서, 필요이상으로 코드가 복잡해질 가능성이 있다.

# EX1

```js
class BasicArmor {
  CalculateDamageFromHit(hit) {
    return 1;
  }

  GetArmorIntegrity() {
    return 1;
  }
}

class ChainMail {
  constructor(decoratedArmor) {
    this.decoratedArmor = decoratedArmor;
  }

  CalculateDamageFromHit(hit) {
    hit.strength = hit.strength * 0.8;
    return this.decoratedArmor.CalculateDamageFromHit(hit);
  }

  GetArmorIntegrity() {
    return 0.9 * this.decoratedArmor.GetArmorIntegrity();
  }
}

var basicArmor = new BasicArmor();
var chainMail = new ChainMail(new BasicArmor());

console.log(chainMail.CalculateDamageFromHit({ strength: 5 })); // 4
console.log(chainMail.GetArmorIntegrity()); // 0.9
```

#EX2

```js
class Component {
  Operation() {}
}

class ConcreteComponent extends Component {
  constructor() {
    super();
    console.log("ConcreteComponent created");
  }
  Operation() {
    console.log("component operation");
  }
}

class Decorator extends Component {
  constructor(component) {
    super();
    this.component = component;
    console.log("Decorator created");
  }
  Operation() {
    this.component.Operation();
  }
}

class ConcreteDecoratorA extends Decorator {
  constructor(component, sign) {
    super(component);
    this.addedState = sign;
    console.log("ConcreteDecoratorA created");
  }
  Operation() {
    super.Operation();
    console.log(this.addedState);
  }
}

class ConcreteDecoratorB extends Decorator {
  constructor(component, sign) {
    super(component);
    this.addedState = sign;
    console.log("ConcreteDecoratorB created");
  }
  Operation() {
    super.Operation();
    console.log(this.addedState);
  }
  AddedBehavior() {
    this.Operation();
    console.log("decoratorB addedBehavior");
  }
}

const component = new ConcreteComponent();
const decoratorA = new ConcreteDecoratorA(component, "decoratorA added sign");
const decoratorB = new ConcreteDecoratorB(component, "decoratorB added sign");
component.Operation();
decoratorA.Operation();
decoratorB.AddedBehavior();
```
