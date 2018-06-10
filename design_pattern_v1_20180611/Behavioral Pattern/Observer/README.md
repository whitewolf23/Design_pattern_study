#옵저버 패턴

- 클라이언트 측 자바스크립트 프로그래밍에서 널리 사용되는 패턴이다.(MVC)
- 이 패턴의 주요 목적은 객체간의 결합도를 낮추는 것이다.
- 여러개의 class 를 작성하였을때, 각 class 들간의 연관성을 유지하기 위해 결합도를 높이는 대신 개체의 상태를 관찰하는 방법
- 객체의 값 변화를 알고 싶을 때 사용
  이를 위해 getter, setter 를 사용해 원하는 속성 래핑

#장점

- Subject 클래스와 Observer 클래스가 서로 독립적으로 변경 가능 -> 독립적 사용

#단점

- 데이터 값이 어떻게 변경되었는지 명시적으로 표시하지 않는 이상, 동일한 변경작업을 반복 수행하는 사태가 발생 할 수도 있음.

```js
var Subject = function() {
  this.observers = [];

  return {
    subscribeObserver: function(observer) {
      this.observers.push(observer);
    },
    unsubscribeObserver: function(observer) {
      var index = this.observers.indexOf(observer);
      if (index > -1) {
        this.observers.splice(index, 1);
      }
    },
    notifyObserver: function(observer) {
      var index = this.observers.indexOf(observer);
      if (index > -1) {
        this.observers[index].notify(index);
      }
    },
    notifyAllObservers: function() {
      for (var i = 0; i < this.observers.length; i++) {
        this.observers[i].notify(i);
      }
    }
  };
};

var Observer = function() {
  return {
    notify: function(index) {
      console.log("Observer " + index + " is notified!");
    }
  };
};

var subject = new Subject();

var observer1 = new Observer();
var observer2 = new Observer();
var observer3 = new Observer();
var observer4 = new Observer();

subject.subscribeObserver(observer1);
subject.subscribeObserver(observer2);
subject.subscribeObserver(observer3);
subject.subscribeObserver(observer4);

subject.notifyObserver(observer2); // Observer 2 is notified!

subject.notifyAllObservers();
// Observer 1 is notified!
// Observer 2 is notified!
// Observer 3 is notified!
// Observer 4 is notified!
```
