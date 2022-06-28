# What's a design pattern?
- **Design patterns** are **typical solutions** to **common problems** in software design. 
- Each pattern is like a blueprint that you can customize to solve a particular design problem in your code.
- The pattern is not a specific piece of code, but a general concept for solving a particular problem.
- Patterns are not algorithms. Algorithms are always the same, but you can use patterns differently based on the codebase.
- Algorithms are like a recipe: they tell you what to do step by step. But a pattern shows you the features and results and it's up to you to create a recipe.

# Why should I learn patterns?
- Design patterns are a toolkit of **tried and tested solutions** to common problems in software design.
- Design patterns define a common language that you and your teammates can use to communicate more efficiently.

# Classification of design patterns
- Creational patterns
	- Factory method
	- Builder
	- Singleton
- Structural patterns
	- Adaptor
	- Decorator
- Behavioral patterns
	- Chain of responsibility
	- Observer
	- Strategy
	- Template method

# Creational patterns
These patterns provide various object creation mechanisms, which increase flexibility and reuse of existing code.
## Singleton
**Singleton** is a creational design pattern that lets you ensure that a class has only one instance, while providing a global access point to this instance.

**Example**: *Database*
```
static function getInstance(){
	if (instance == null){
		instance = new self;
	}
	return instance;
}
```

|Pros|Cons|
|---|---|
|You can be sure that a class has only a single instance.|Violates the Single Responsibility Principle. The pattern solves two problems at the time.|
|You gain a global access point to that instance.|The Singleton pattern can mask bad design, for instance, when the components of the program know too much about each other.|
|The singleton object is initialized only when it’s requested for the first time.|The pattern requires special treatment in a multithreaded environment so that multiple threads won’t create a singleton object several times.|
||It may be difficult to unit test the client code of the Singleton because many test frameworks rely on inheritance when producing mock objects. Since the constructor of the singleton class is private and overriding static methods is impossible in most languages, you will need to think of a creative way to mock the singleton. Or just don’t write the tests. Or don’t use the Singleton pattern.|

## Factory Method
**Factory Method** is a creational design pattern that provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created.
This pattern suggests that you replace direct object construction calls (using the `new` operator) with calls to a special _factory_ method.

We have a base class called _factory_ with a `abstract function` called _createProduct_. Now every _subfactory_ that implements this _factory_, must have a _createProduct_ that return an `interface` of _Product_.

**Example**: *Ship* and *Truck*

|Pros|Cons|
|---|---|
|You avoid tight coupling between the creator and the concrete products.| The code may become more complicated since you need to introduce a lot of new subclasses to implement the pattern. The best case scenario is when you’re introducing the pattern into an existing hierarchy of creator classes.|
|Single Responsibility Principle. You can move the product creation code into one place in the program, making the code easier to support.||
|Open/Closed Principle. You can introduce new types of products into the program without breaking existing client code.||



## Builder
**Builder** is a creational design pattern that lets you construct complex objects step by step. The pattern allows you to produce different types and representations of an object using the same construction code.
**Example**: *Car* and *Manual*
|Pros|Cons|
|---|---|
|You can construct objects step-by-step, defer construction steps or run steps recursively.|The overall complexity of the code increases since the pattern requires creating multiple new classes.|
|No need to define booleans as function's parameters: `Pizza(int size, boolean cheese, boolean pepperoni)`.||
|You can reuse the same construction code when building various representations of products.||
|Single Responsibility Principle. You can isolate complex construction code from the business logic of the product.||

# Structural patterns
These patterns explain how to assemble objects and classes into larger structures while keeping these structures flexible and efficient.

## Adaptor
**Adapter** is a structural design pattern that allows objects with incompatible interfaces to collaborate.
This is a special object that converts the interface of one object so that another object can understand it.
**Example**: _XML_ to _JSON_

|Pros|Cons|
|---|---|
|Single Responsibility Principle. You can separate the interface or data conversion code from the primary business logic of the program.|The overall complexity of the code increases because you need to introduce a set of new interfaces and classes. Sometimes it’s simpler just to change the service class so that it matches the rest of your code.|
|Open/Closed Principle. You can introduce new types of adapters into the program without breaking the existing client code, as long as they work with the adapters through the client interface.||

## Decorator
**Decorator** is a structural design pattern that lets you attach new behaviors to objects by placing these objects inside special wrapper objects that contain the behaviors.
The wrapper contains the same set of methods as the target and delegates to it all requests it receives. However, the wrapper may alter the result by doing something either before or after it passes the request to the target.
- *Inheritance* is static. You can’t alter the behavior of an existing object at runtime. You can use a class and create objects from it beforehand.
- Subclasses can have just **one** parent class
 
This pattern solves these problems by using _aggregation_ instead of _inheritance_.

**Example**: sending notifications via Email and now you want to send notifications to other channels as well. So you have to create many subclasses like these:
_sendFacebookAndEmail_, _sendSmsAndEmail_, sendFacebookAndSmsAndEmail_
Or you could use `booleans` as the parameters of _send_ function:
`function send(bool send_fb, bool send_sms, send_email){ ... }`
Or in decorator pattern:
```
stack = new Notifier();
if(facebookEnabled)
	stack = FacebookDecorator(stack);
if(smsEnabled)
	stack = SmsDecorator(stack);
stack.send();
```

|Pros|Cons|
|---|---|
|You can extend an object’s behavior without making a new subclass.|It’s hard to remove a specific wrapper from the wrappers stack.|
|You can add or remove responsibilities from an object at runtime.|It’s hard to implement a decorator in such a way that its behavior doesn’t depend on the order in the decorators stack.|
|You can combine several behaviors by wrapping an object into multiple decorators.|The initial configuration code of layers might look pretty ugly.|
|Single Responsibility Principle. You can divide a monolithic class that implements many possible variants of behavior into several smaller classes.|



# Behavioral patterns
These patterns are concerned with algorithms and the assignment of responsibilities between objects.

## Chain of responsibility
It lets you pass requests along a chain of handlers. Upon receiving a request, each handler decides either to process the request or to pass it to the next handler in the chain.

|Pros|Cons|
|---|---|
|You can control the order of request handling.|Some requests may end up unhandled.|
|Single Responsibility Principle. You can decouple classes that invoke operations from classes that perform operations.||
|Open/Closed Principle. You can introduce new handlers into the app without breaking the existing client code.||


## Observer
**Observer** (Pub/Sub) is a behavioral design pattern that lets you define a subscription mechanism to notify multiple objects about any events that happen to the object they’re observing.

|Pros|Cons|
|---|---|
|Open/Closed Principle. You can introduce new subscriber classes without having to change the publisher’s code (and vice versa if there’s a publisher interface).|Subscribers are notified in random order.|
You can establish relations between objects at runtime.||


## Strategy
**Strategy** is a behavioral design pattern that lets you define a family of algorithms, put each of them into a separate class, and make their objects interchangeable.
The Strategy pattern suggests that you take a class that does something specific in a lot of different ways and extract all of these algorithms into separate classes called _strategies_.

|Pros|Cons|
|---|---|
|You can swap algorithms used inside an object at runtime.|If you only have a couple of algorithms and they rarely change, there’s no real reason to overcomplicate the program with new classes and interfaces that come along with the pattern.|
|You can isolate the implementation details of an algorithm from the code that uses it.|Clients must be aware of the differences between strategies to be able to select a proper one.|
|You can replace inheritance with composition.|A lot of modern programming languages have functional type support that lets you implement different versions of an algorithm inside a set of anonymous functions. Then you could use these functions exactly as you’d have used the strategy objects, but without bloating your code with extra classes and interfaces.|
|Open/Closed Principle. You can introduce new strategies without having to change the context.||


## Template method
**Template Method** is a behavioral design pattern that defines the skeleton of an algorithm in the superclass but lets subclasses override specific steps of the algorithm without changing its structure.

**Example**: We have the same steps for our gateways (_getToken_, _goToGateway_, _callback_). With template, we can add as many gateways as we want without breaking the Open/Closed principle.

|Pros|Cons|
|---|---|
|You can let clients override only certain parts of a large algorithm, making them less affected by changes that happen to other parts of the algorithm.|Some clients may be limited by the provided skeleton of an algorithm.|
|You can pull the duplicate code into a superclass.|You might violate the Liskov Substitution Principle by suppressing a default step implementation via a subclass.|
||Template methods tend to be harder to maintain the more steps they have.|

## Specification
It's where business rules can be recombined by chaining the business rules together using boolean logic.

**Example**:
```
var OverDue = new OverDueSpecification();
var NoticeSent = new NoticeSentSpecification();
var InCollection = new InCollectionSpecification();

// example of specification pattern logic chaining
var SendToCollection = OverDue.And(NoticeSent).And(InCollection.Not());

var InvoiceCollection = Service.GetInvoices();

foreach (var currentInvoice in InvoiceCollection) {
    if (SendToCollection.IsSatisfiedBy(currentInvoice))  {
        currentInvoice.SendToCollection();
    }
}
```

```
export interface ISpecification {
  isSatisfiedBy(candidate: unknown): boolean;
  and(other: ISpecification): ISpecification;
  andNot(other: ISpecification): ISpecification;
  or(other: ISpecification): ISpecification;
  orNot(other: ISpecification): ISpecification;
  not(): ISpecification;
}

export abstract class CompositeSpecification implements ISpecification {
  abstract isSatisfiedBy(candidate: unknown): boolean;

  and(other: ISpecification): ISpecification {
    return new AndSpecification(this, other);
  }

  andNot(other: ISpecification): ISpecification {
    return new AndNotSpecification(this, other);
  }

  or(other: ISpecification): ISpecification {
    return new OrSpecification(this, other);
  }

  orNot(other: ISpecification): ISpecification {
    return new OrNotSpecification(this, other);
  }

  not(): ISpecification {
    return new NotSpecification(this);
  }
}

export class AndSpecification extends CompositeSpecification {
  constructor(private leftCondition: ISpecification, private rightCondition: ISpecification) {
    super();
  }

  isSatisfiedBy(candidate: unknown): boolean {
    return this.leftCondition.isSatisfiedBy(candidate) && this.rightCondition.isSatisfiedBy(candidate);
  }
}

export class AndNotSpecification extends CompositeSpecification {
  constructor(private leftCondition: ISpecification, private rightCondition: ISpecification) {
    super();
  }

  isSatisfiedBy(candidate: unknown): boolean {
    return this.leftCondition.isSatisfiedBy(candidate) && this.rightCondition.isSatisfiedBy(candidate) !== true;
  }
}

export class OrSpecification extends CompositeSpecification {
  constructor(private leftCondition: ISpecification, private rightCondition: ISpecification) {
    super();
  }

  isSatisfiedBy(candidate: unknown): boolean {
    return this.leftCondition.isSatisfiedBy(candidate) || this.rightCondition.isSatisfiedBy(candidate);
  }
}

export class OrNotSpecification extends CompositeSpecification {
  constructor(private leftCondition: ISpecification, private rightCondition: ISpecification) {
    super();
  }

  isSatisfiedBy(candidate: unknown): boolean {
    return this.leftCondition.isSatisfiedBy(candidate) || this.rightCondition.isSatisfiedBy(candidate) !== true;
  }
}

export class NotSpecification extends CompositeSpecification {
  constructor(private wrapped: ISpecification) {
    super();
  }

  isSatisfiedBy(candidate: unknown): boolean {
    return !this.wrapped.isSatisfiedBy(candidate);
  }
}
```

---
**Sources:**
https://refactoring.guru
https://laracasts.com
https://wikipedia.com
