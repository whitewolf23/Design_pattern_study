# 반복자

- 객체의 내부구조가 복잡하더라도 개별 속성에 쉽게 접근하기 위한 패턴
- 단순히 배열이나 객체를 제외한 인스턴스에도 내부 요소를 반복 가능하도록 인터페이스를 구현하는 패턴.
  보통은 next 메서드를 만들어 다음 요소를 꺼내는 동작을 실행하고, 더 꺼낼 요소가 없을 시 별도의 에러를 일으키기도 하고, Done 등의 getter 로 모든 요소를 꺼냈는지도 구현할 수 있다.

# 장점

- 집합 객체 내의 코드가 변경되어도 순회하는 코드에 영향이 없음
- 순회방법을 임의로 정의 가능

# 단점

- 너무 복잡한 구조면, 적용이 불가능하다. -> 찾아보니 단점이 별로 없음, 개인적 의견

# EX1

```js
var Beehives = (function() {
  function Beehives(hiveList) {
    this.hiveList = hiveList;
    this.index = 0;
  }
  Beehives.prototype.next = function() {
    console.log(this.hiveList[this.index++] + "에서 꿀을 걷습니다");
  };
  Beehives.prototype.done = function() {
    return this.hiveList.length === this.index;
  };
  return Beehives;
})();
```

```js
var beehives = new Beehives([
  "hive1",
  "hive2",
  "hive3",
  "hive4",
  "hive5",
  "hive6",
  "hive7",
  "hive8",
  "hive9"
]);
beehives.next(); // hive1에서 꿀을 걷습니다
beehives.next(); // hive2에서 꿀을 걷습니다
beehives.next(); // hive3에서 꿀을 걷습니다
beehives.done(); // false
beehives.next(); // hive4에서 꿀을 걷습니다
beehives.next(); // hive5에서 꿀을 걷습니다
beehives.next(); // hive6에서 꿀을 걷습니다
beehives.next(); // hive7에서 꿀을 걷습니다
beehives.done(); // false
beehives.next(); // hive8에서 꿀을 걷습니다
beehives.next(); // hive9에서 꿀을 걷습니다
beehives.done(); // true
```

```js
while (!beehives.done()) {
  beehives.next();
}
```

# EX2

```js
var element;
while ((element = agg.next())) {
  // ...
  console.log(element);
}
```

```js
var agg = (function() {
  var index = 0,
    data = [1, 2, 3, 4, 5],
    length = data.length;

  return {
    next: function() {
      var element;
      if (!this.hasNext()) {
        return null;
      }
      element = data[index];
      index += 1;
      return element;
    },
    hasNext: function() {
      return index < length;
    },
    rewind: function() {
      index = 0;
    },
    current: function() {
      return data[index];
    }
  };
})();
```
