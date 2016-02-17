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
Abstract factory pattern is useful when creating objects which have a common iterface. It will facilitate the creation of related or dependent objects.
**C#**
'''
interface IDumb
{
    string Name();
}
interface ISmart
{
    string Name();
}
'''

'''
class Asha : IDumb
{
    public string Name()
    {
        return "Asha";
    }
}

class Lumia : ISmart
{
    public string Name()
    {
        return "Lumia";
    }
}

class GalaxyS2 : ISmart
{
    public string Name()
    {
        return "GalaxyS2";
    }
}
'''

'''
interface IPhoneFactory
{
    ISmart GetSmart();
    IDumb GetDumb();
}
'''

'''
class NokiaFactory : IPhoneFactory
{
    public ISmart GetSmart()
    {
        return new Lumia();
    }

    public IDumb GetDumb()
    {
        return new Asha();
    }
}

'''

'''
switch (phoneType)
        {
            case MANUFACTURERS.SAMSUNG:
                factory = new SamsungFactory();
                break;
            case MANUFACTURERS.NOKIA:
                factory = new NokiaFactory();
                break;
        }
'''

**JavaScript**
There is no concept of classes or inheritance in JavaScript as such. This pattern can be replicated by using functions and extended objects.
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
