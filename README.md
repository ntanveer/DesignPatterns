# A guide to using Design Patterns in C# and JavaScript

## Creational

- [Abstract Factory](#abstract)
- [Builder](#builder)
- [Factory Method](#factory)
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
- [Mediator](#mediator)
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

ctor.prototype.getResults = function() {
    return this;
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

## Factory
Factory pattern encapsualtes the object instantiation logic for objects with common interface. It differs from other creational patterns becuase it does not explicity require a constructor. A factory provices generic interface to create objects based on the type of factory object.

**C#**

```c#
public interface IPeople
{
    string GetName();
}

public class Villagers : IPeople
{
    public string GetName()
    {
        return "Village Person";
    }
}

public class CityPeople : IPeople
{
    public string GetName()
    {
        return "City Person";
    }
}

public enum PeopleType
{
    Rural,
    Urban
}

/// <summary>
/// Implementation of Factory - Used to create objects
/// </summary>
public class Factory
{
    public IPeople GetPeople(PeopleType type)
    {
        switch (type)
        {
            case PeopleType.Rural:
                return new Villagers();
            case PeopleType.Urban:
                return new CityPeople();
            default:
                throw new NotSupportedException()
        }
    }
}
```
**JavaScript**

```js

function Car(options) {
  this.doors = options.doors || 4;
  this.color = options.color || "silver";
}
 
function Truck(options){
  this.wheelSize = options.wheelSize || "large";
  this.color = options.color || "blue";
}
 
 
function VehicleFactory() {}
VehicleFactory.prototype.vehicleClass = Car;
VehicleFactory.prototype.createVehicle = function ( options ) {
  switch(options.vehicleType){
    case "car":
      this.vehicleClass = Car;
      break;
    case "truck":
      this.vehicleClass = Truck;
      break;
  }
  return new this.vehicleClass( options );
};
 
var carFactory = new VehicleFactory();
var car = carFactory.createVehicle( {
            vehicleType: "car",
            color: "yellow",
            doors: 6 } );
 
// Outputs: true
console.log( car instanceof Car );
 
// Outputs: Car object of color "yellow", doors: 6 in a "brand new" state
console.log( car );

var movingTruck = carFactory.createVehicle( {
                      vehicleType: "truck",
                      color: "red",
                      wheelSize: "small" } );
 
// Outputs: true
console.log( movingTruck instanceof Truck );
 
// Outputs: Truck object of color "red"
// and a "small" wheelSize
console.log( movingTruck );

```

## Mediator
Mediator pattern is used to avoid explicitly referring same object and recuding communication from "many-to-many" to "many-to-one". It can be helpful to reconcile differences between values of the object held by different owners. This pattern can be used to pinpoint dependencies and promote decoupled obects with smaller, resuable components.
The downside of using this pattern is that it can introduce a single point of failure making it difficult to identify the actual problem. Mediators should only be used when communication channel is accross multiple features. If it is used for mediating values within the same feature then the communication back and forth becomes cumbersome and can result in a performance hit.

**C#**
```c#
class Program
{
    static void Main(string[] args)
    {
        //list of participants
        IColleague<string> colleagueA = new ConcreteColleague<string>("ColleagueA");
        IColleague<string> colleagueB = new ConcreteColleague<string>("ColleagueB");
        IColleague<string> colleagueC = new ConcreteColleague<string>("ColleagueC");
        IColleague<string> colleagueD = new ConcreteColleague<string>("ColleagueD");

        //first mediator
        IMediator<string> mediator1 = new ConcreteMediator<string>();
        //participants registers to the mediator
        mediator1.Register(colleagueA);
        mediator1.Register(colleagueB);
        mediator1.Register(colleagueC);
        //participantA sends out a message
        colleagueA.SendMessage(mediator1, "MessageX from ColleagueA");

        //second mediator
        IMediator<string> mediator2 = new ConcreteMediator<string>();
        //participants registers to the mediator
        mediator2.Register(colleagueB);
        mediator2.Register(colleagueD);
        //participantB sends out a message
        colleagueB.SendMessage(mediator2, "MessageY from ColleagueB");
    }
}

public interface IColleague<t>
{
    void SendMessage(IMediator<t> mediator, T message);

    void ReceiveMessage(T message);
}

public class ConcreteColleague<t> : IColleague<t>
{
    private string name;

    public ConcreteColleague(string name)
    {
        this.name = name;
    }

    void IColleague<t>.SendMessage(IMediator<t> mediator, T message)
    {
        mediator.DistributeMessage(this, message);
    }

    void IColleague<t>.ReceiveMessage(T message)
    {
        Console.WriteLine(this.name + " received " + message.ToString());
    }
}


public interface IMediator<t>
{
    List<icolleague><t>> ColleagueList { get; }

    void DistributeMessage(IColleague<t> sender, T message);

    void Register(IColleague<t> colleague);
}


public class ConcreteMediator<t> : IMediator<t>
{
    private List<icolleague><t>> colleagueList = new List<icolleague><t>>();

    List<icolleague><t>> IMediator<t>.ColleagueList
    {
        get { return colleagueList; }
    }

    void IMediator<t>.Register(IColleague<t> colleague)
    {
        colleagueList.Add(colleague);
    }

    void IMediator<t>.DistributeMessage(IColleague<t> sender, T message)
    {
        foreach (IColleague<t> c in colleagueList)
            if (c != sender)    //don't need to send message to sender
                c.ReceiveMessage(message);
    }
}
```
**JavaScript**
```js

var Mediator = ( function( window, undefined ) {

	function Mediator() {
		this._topics = {};
	}

	Mediator.prototype.subscribe = function mediatorSubscribe( topic, callback ) {
		if( ! this._topics.hasOwnProperty( topic ) ) {
			this._topics[ topic ] = [];
		}

		this._topics[ topic ].push( callback );
		return true;
	};

	Mediator.prototype.unsubscribe = function mediatorUnsubscrive( topic, callback ) {
		if( ! this._topics.hasOwnProperty( topic ) ) {
			return false;
		}

		for( var i = 0, len = this._topics[ topic ].length; i < len; i++ ) {
			if( this._topics[ topic ][ i ] === callback ) {
				this._topics[ topic ].splice( i, 1 );
				return true;
			}
		}

		return false;
	};

	Mediator.prototype.publish = function mediatorPublish() {
		var args = Array.prototype.slice.call( arguments );
		var topic = args.shift();

		if( ! this._topics.hasOwnProperty( topic ) ) {
			return false;
		}

		for( var i = 0, len = this._topics[ topic ].length; i < len; i++ ) {
			this._topics[ topic ][ i ].apply( undefined, args );
		}
		return true;
	};

	return Mediator;

} )( window );

// example subscriber function
var Subscriber = function ExampleSubscriber( myVariable ) {
  console.log( myVariable );
};

// example usages
var myMediator = new Mediator();
myMediator.subscribe( 'some event', Subscriber );
myMediator.publish( 'some event', 'foo bar' ); // console logs "foo bar"
```

