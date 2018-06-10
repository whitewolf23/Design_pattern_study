# 프록시 패턴

- 대리자, 스파이
- 사용자가 원하는 행동을 하기 전에 한번 거쳐가는 단계
- 실제 인스턴스의 인터페이스를 미러링하여 값비싼 객체의 생성,사용을 제어
  브라우져 마우스 이동 창크기 조정 등의 비싼작업에 사용-

- 일반적으로 다른 어떤 클래스의 인터페이스로 동작하는 클래스
- 내부적으로 실제의 객체에 접근할때 호출 되는 wrapper 혹은 대리 객체이다.
- 다른 객체 와의 연동을 관장하는 하나의 객체로 구성
  사용자가 다른 문제를 일으키지 않고 실체를 더 효율적으로 사용할 수 있게 해준다.

#장점

- 사용자로 부터 최적화와 관련 내용을 감추어서 코드를 단순하게 해준다.

#단점

- 매번 인스턴스를 만들어 사용되면, 초기화 하는데 시간이 오래걸려 성능 저하를 일으킬 수 있음.

#EX1

```js
function PhoneBook() {
  this.dictionary = {
    이승민: "01012341234",
    이현섭: "01023456789",
    오유근: "01077777777"
  };
}
PhoneBook.prototype.get = function(name, callback) {
  var self = this;
  setTimeout(function() {
    callback(self.dictionary[name]);
  }, 3000);
};
```

```js
function PhoneBookProxy() {
  var phoneBook = new PhoneBook();
  var viewCount = 0;
  return {
    get: function(name, callback) {
      viewCount++;
      phoneBook.get(name, callback);
    },
    getViewCount: function() {
      return viewCount;
    }
  };
}
```

```js
function PhoneBookProxy() {
  var phoneBook = new PhoneBook();
  var viewCount = 0;
  var cache = {};
  return {
    get: function(name, callback) {
      viewCount++;
      if (cache[name]) {
        callback(cache[name]);
      } else {
        phoneBook.get(name, function(number) {
          cache[name] = number;
          callback(number);
        });
      }
    },
    getViewCount: function() {
      return viewCount;
    }
  };
}
```

#EX2

```js
var Praetorian = (function() {
  function Praetorian() {}
  Praetorian.prototype.report = function(fact) {
    console.log("황제에게 " + fact + "을 보고드립니다!");
  };
  Praetorian.prototype.assassinate = function(target) {
    console.log(target + " 암살 명령을 받았습니다");
  };
  return Praetorian;
})();
```

```js
var PraetorianProxy = (function() {
  function PraetorianProxy(master) {
    this.master = master;
    this.praetorian = new Praetorian();
  }
  PraetorianProxy.prototype.report = function(fact) {
    var lie = "거짓";
    console.log(this.master + "에게 " + fact + "을 보고드립니다");
    this.praetorian.report(lie);
  };
  PraetorianProxy.prototype.assassinate = function(target) {
    console.log("더 이상 " + target + "을 암살하지 않습니다");
    this.praetorian.assassinate("Galba");
  };
  return PraetorianProxy;
})();
```

```js
var praetorian = new PraetorianProxy("Otho");
praetorian.report("사실"); // Otho에게 사실을 보고드립니다. 황제에게 거짓을 보고드립니다.
praetorian.assassinate("Otho"); // 더 이상 Otho을 암살하지 않습니다. Galba 암살 명령을 받았습니다.
```

#EX3

```js
class BarrelCalculator {
  calculateNumberNeeded(volume) {
    return Math.ceil(volume / 357);
  }
}

class DragonBarrelCalculator {
  calculateNumberNeeded(volume) {
    if (this._barrelCalculator == null) {
      this._barrelCalculator = new BarrelCalculator();
      return this._barrelCalculator.calculateNumberNeeded(volume * 0.77);
    }
  }
}
```

#EX4

```js
class RealImage {
  constructor(filename) {
    this.filename = filename;
    this.loadImageFromDisk();
  }
  loadImageFromDisk() {
    console.log(`Loading ${this.filename}`);
  }
  displayImage() {
    console.log(`Displaying ${this.filename}`);
  }
}

class ProxyImage {
  constructor(filename) {
    this.filename = filename;
  }
  displayImage() {
    const image = new RealImage(this.filename);
    image.displayImage();
  }
}
const img1 = new ProxyImage("HiRes_10MB_Photo1.png");
const img2 = new ProxyImage("HiRes_30MB_Photo2.png");
img1.displayImage();
img2.displayImage();
```
