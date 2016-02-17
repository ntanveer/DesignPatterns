# DesignPatterns in C# and JavaScript

## Creational

- [Abstract Factory](#abstract)
- Builder
- Factory Method
- Object Pool
- Prototype
- Singleton

## Structural

- Adapter
- Bridge
- Composite
- Decorator
- Facade
- Flyweight
- Private Class Data
- Proxy

## Behavioral

- Chain of Responsibility
- Command
- Interpreter
- Iterator
- Mediator
- Memento
- Null Object
- Observer
- State
- Strategy
- Template Method
- Visitor
- Monad Pattern / Promises

## Concurrency

## Abstract
**C#**
**JavaScript**
```js
var __hasProp = {}.hasOwnProperty,
  __extends = function(child, parent) { 
    for (var key in parent) { 
      if (__hasProp.call(parent, key)) 
        child[key] = parent[key]; 
    } 
    function ctor() { 
    this.constructor = child; 
    } 
    ctor.prototype = parent.prototype; 
    child.prototype = new ctor(); 
    child.__super__ = parent.prototype; 
    return child; 
  };

var Fruit = (function() {
  function Fruit() {
    console.log("New fruit");
  }
  return Fruit;
})();

var Apple = (function(_super) {
  __extends(Apple, _super);
  function Apple() {
    console.log("New apple");
    Apple.__super__.constructor.apply(this, arguments);
  }
  return Apple;
})(Fruit);

var apple = new Apple();
```
