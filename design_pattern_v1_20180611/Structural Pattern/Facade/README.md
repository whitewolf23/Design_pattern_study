#퍼사드 패턴

- 퍼사드 = 건물의 정면
- 복잡하고 세부적인 것들은 감추고 간단한 것만 보여줌
  다수의 서브 시스템을 사용하기 쉽게 서비스를 제공
- 클래스 라이브러리 같은 어떤 소프트웨어의 다른 커다란 코드 부분에 대한 간략화된 인터페이스를 제공하는 객체
- 어떠한 공통 작업들을 하나에 몰아서 그 안에서 처리하게 됨

#장점

- 시스템에 대한 통합된 인터페이스를 제공하고, 고수준 인터페이스를 정의 하기 때문에 서브시스템을 쉽게 사용 가능
- 즉, 인터페이스를 간단하게 바꾸어 간편함
- 서브 시스템에 대한 간단한 인터페이스 제공 가능

#단점

- 다른 구성요소에 대한 메소드 호출을 처리하기 위해 'Wrapper'라는 클래스를 더 만들어야 할 수도 있어,
  시스템 복잡해지며, 성능 저하를 일으킬 수 있다.

#EX1

```js
class Legion {
  supply() {}
  makeFormation() {}
  goForward() {}
  pullOutSword() {}
  runToEnemy() {}
  retreat() {}
}

class Galba {
  constructor() {
    this.legions = [];
    this.legions.push(new Legion(1));
    this.legions.push(new Legion(2));
    this.legions.push(new Legion(3));
  }

  march() {
    this.legions.forEach(function(legion) {
      legion.supply();
      legion.makeFormation();
      legion.goForward();
    });
  }

  attach() {
    this.legions.forEach(function(legion) {
      legion.makeFormation();
      legion.pullOutSword();
      legion.runToEnemy();
    });
  }

  halt() {
    this.legions.forEach(function(legion) {
      legion.halt();
    });
  }

  retreat() {
    this.legions.forEach(function(legion) {
      legion.retreat();
    });
  }
}
```

#EX2

```js
// 생성자 생성
var Mortgage = function(name) {
  this.name = name;
};

Mortgage.prototype = {
  applyFor: function(amount) {
    var result = "approved";
    if (!new Bank().verify(this.name, amount)) {
      result = "denied";
    }
    else if (!new Credit().get(this.name)) {
      result = "denied";
    }
    else if (!new Background().check(this.name)) {
      result = "denied";
  return this.name + "has benn" + result + "for a " + amount + "mortgage"
  }
};


var Bank = function() {
  // TODO
}
var Credit = function() {
  // TODO
}
var Background = function() {
  // TODO
}

function run() {
  var mortgage = new Mortgage("minsu");
  var result = mortgage.applyFor("$500")
  console.log(result)
}
```

#EX3

```js
class CPU {
  freeze() {}
  jump(position) {}
  execute() {}
}
class Memory {
  load(position, data) {}
}
class HardDrive {
  read(lba, size) {}
}

class FacadeComputer {
  startComputer() {
    const cpu = new CPU();
    const memory = new Memory();
    const hard = new HardDrive();
    cpu.freeze();
    memory.load(BOOT_ADDRESS, hard.read(BOOT_SECTOR, SECTOR_SIZE));
    cpu.jump(BOOT_ADDRESS);
    cpu.execute();
  }
}

const computer = new FacadeComputer();
computer.startComputer();
```
