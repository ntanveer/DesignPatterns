# DesignPatterns in C# and JavaScript

## Creational

- [Abstract Factory](#abstract)
- [Builder](#builder)
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
Abstract factory pattern is useful when creating objects which have a common iterface. It will facilitate the creation of related or dependent objects and facilitate polymorphism.
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
## Builder
The builder pattern helps in reducing the exponential list of constructors and remove this anti-pattern. Instead of using multiple constructors, a builder receives each initialization parameter step by step and then returns the resulting constructed object at once. 
The Builder and Abstract Factory patterns are similar in that they both look at construction at an abstract level. However, the Builder pattern is concerned with how a single object is made upby the different factories, whereas the Abstract Factory pattern is concerned with what products are made. The Builder pattern abstracts the algorithm for construction by including the concept of a director. The director is responsible for itemizing the steps and calls on builders to fulfill them. Directors do not have to conform to an interface.

**C#**
```c#
public class Car
{
    public Car()
    {
    }

    public int Wheels { get; set; }
    public string Colour { get; set; }
}

public interface ICarBuilder
{
    void SetColour([NotNull]string colour);
    void SetWheels([NotNull]int count);

    Car GetResult();
}

public class CarBuilder : ICarBuilder
{
    private Car _car;

    public CarBuilder()
    {
        this._car = new Car();
    }

    public void SetColour(string colour)
    {
        this._car.Colour = colour;
    }

    public void SetWheels(int count)
    {
        this._car.Wheels = count;
    }

    public Car GetResult()
    {
        return this._car;
    }
}

public class CarBuildDirector
{
    public Car Construct()
    {
        CarBuilder builder = new CarBuilder();

        builder.SetColour("Red");
        builder.SetWheels(4);

        return builder.GetResult();
    }
}
```
**JS**
```js
var carBuilder = function() {
    this.colour = 'red';
    this.wheels = 4;
};

ctor.prototype.setColour = function (colour) {
    this.colour = colour;
};

ctor.prototype.setWheels = function (wheels) {
    this.wheels = wheels;
};

return ctor;
    
function Director() {
    this.assembleCar = function(builder) {
        var car = builder.getResults;
        
        return {
            car: car
        }
    }
}
  
var director = new Director(),
    carBuilder = new carBuilder();

director.assembleCar(carBuilder);
```
