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
```c#
interface ILuxury
{
    string Name();
}
interface ISports
{
    string Name();
}

class Lexus : ILuxury
{
    public string Name()
    {
        return "Lexus";
    }
}

class Ferrari : ISports
{
    public string Name()
    {
        return "Ferrari";
    }
}

interface ICarFactory
{
    ILuxury GetLuxury();
    ISports GetSports();
}

class FerrariFactory : ICarFactory
{
    public ILuxury GetLuxury()
    {
        throw new Exception("Cannot Make Luxury...");
    }
    
    public ISports GetSports()
    {
        return new Ferrari();
    }
}

class MazdaFactory : ICarFactory
{
    public ILuxury GetLuxury()
    {
        return new Mazda();
    }
    
    public ISports GetSports()
    {
        throw new Exception("Cannot Make Sports...");
    }
}

switch (cars)
{
    case MANUFACTURERS.FERRARI:
        factory = new FerrariFactory();
        break;
    case MANUFACTURERS.MAZDA:
        factory = new MazdaFactory();
        break;
}
```

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

var Car = (function() {
  function Car() {
    console.log("New Car");
  }
  return Fruit;
})();

var Mazda = (function(_super) {
  __extends(Mazda, _super);
  function Mazda() {
    console.log("New Mazda");
    Apple.__super__.constructor.apply(this, arguments);
  }
  return Mazda;
})(Car);

var mazda = new Mazda();
```
