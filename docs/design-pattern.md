# 📄 Design Pattern - Cheatsheet

## Singleton

```javascript
class Singleton {
  static instance;
  static getInstance() {
    if (!this.instance) {
      this.instance = new Singleton();
    }
    return this.instance;
  }
}
```

```javascript
class Singleton {
  static instance;

  static getInstance() {
    if (!this.instance) {
      this.instance = new Singleton();
    }
    return this.instance;
  }
}

const singleton1 = Singleton.getInstance();
const singleton2 = Singleton.getInstance();
console.log(singleton1 === singleton2); // true
```

| **Use Case**                                                                      | **Cons**                                                            |
|-----------------------------------------------------------------------------------|---------------------------------------------------------------------|
| When a single instance of a class is needed across the application.               | Challenging to unit test, issues in multithreading.                 |

## Factory Method

```javascript
class ProductFactory {
  createProduct(type) {
    if (type === 'A') return new ProductA();
    if (type === 'B') return new ProductB();
  }
}
```

```javascript
class ProductA {
  create() {
    console.log('Product A created');
  }
}

class ProductB {
  create() {
    console.log('Product B created');
  }
}

class ProductFactory {
  createProduct(type) {
    if (type === 'A') return new ProductA();
    if (type === 'B') return new ProductB();
  }
}

const factory = new ProductFactory();
const productA = factory.createProduct('A');
productA.create(); // Product A created

const productB = factory.createProduct('B');
productB.create(); // Product B created

```

| **Use Case**                                                                      | **Cons**                                                            |
|-----------------------------------------------------------------------------------|---------------------------------------------------------------------|
| When a class cannot anticipate the class of objects it must create.               | Increases complexity with an extra class for each product.          |

## Abstract Factory

```javascript
class GUIFactory {
  createButton();
  createCheckbox();
}
class WinFactory extends GUIFactory {
  createButton() { return new WinButton(); }
  createCheckbox() { return new WinCheckbox(); }
}
```

```javascript
class Button {
  paint() {}
}

class WinButton extends Button {
  paint() {
    console.log('Rendering a button in a Windows style');
  }
}

class MacButton extends Button {
  paint() {
    console.log('Rendering a button in a macOS style');
  }
}

class Checkbox {
  paint() {}
}

class WinCheckbox extends Checkbox {
  paint() {
    console.log('Rendering a checkbox in a Windows style');
  }
}

class MacCheckbox extends Checkbox {
  paint() {
    console.log('Rendering a checkbox in a macOS style');
  }
}

class GUIFactory {
  createButton() {}
  createCheckbox() {}
}

class WinFactory extends GUIFactory {
  createButton() {
    return new WinButton();
  }
  createCheckbox() {
    return new WinCheckbox();
  }
}

class MacFactory extends GUIFactory {
  createButton() {
    return new MacButton();
  }
  createCheckbox() {
    return new MacCheckbox();
  }
}

function getFactory(osType) {
  if (osType === 'Windows') {
    return new WinFactory();
  } else if (osType === 'macOS') {
    return new MacFactory();
  }
}

const factory = getFactory('Windows');
const button = factory.createButton();
button.paint(); // Rendering a button in a Windows style
const checkbox = factory.createCheckbox();
checkbox.paint(); // Rendering a checkbox in a Windows style

```

| **Use Case**                                                                      | **Cons**                                                            |
|-----------------------------------------------------------------------------------|---------------------------------------------------------------------|
| When families of related objects need to be created without specifying their concrete classes. | Difficult to extend with new product families.                      |

## Builder

```javascript
class ProductBuilder {
  setPartA(value) { this.partA = value; return this; }
  setPartB(value) { this.partB = value; return this; }
  build() { return new Product(this); }
}
```

```javascript
class Product {
  constructor(builder) {
    this.partA = builder.partA;
    this.partB = builder.partB;
  }
}

class ProductBuilder {
  setPartA(value) {
    this.partA = value;
    return this;
  }
  setPartB(value) {
    this.partB = value;
    return this;
  }
  build() {
    return new Product(this);
  }
}

const builder = new ProductBuilder();
const product = builder.setPartA('Value A').setPartB('Value B').build();
console.log(product);
```

| **Use Case**                                                                      | **Cons**                                                            |
|-----------------------------------------------------------------------------------|---------------------------------------------------------------------|
| When constructing complex objects step by step is required.                       | Increased complexity with the necessity of a builder class.         |

## Prototype

```javascript
class Prototype {
  clone() { return Object.assign({}, this); }
}
```

```javascript
class Prototype {
  constructor(name) {
    this.name = name;
  }

  clone() {
    return new Prototype(this.name);
  }
}

const original = new Prototype('Original');
const copy = original.clone();
console.log(copy.name); // Original
```

| **Use Case**                                                                      | **Cons**                                                            |
|-----------------------------------------------------------------------------------|---------------------------------------------------------------------|
| When instances of a class can have one of only a few different combinations of state. | Cloning complex objects with circular references can be challenging |

## Adapter

```javascript
class OldSystem {
  oldMethod() {}
}
class Adapter {
  constructor(oldSystem) { this.oldSystem = oldSystem; }
  newMethod() { this.oldSystem.oldMethod(); }
}
```

```javascript
class OldSystem {
  oldMethod() {
    console.log('Old system method');
  }
}

class Adapter {
  constructor(oldSystem) {
    this.oldSystem = oldSystem;
  }

  newMethod() {
    this.oldSystem.oldMethod();
  }
}

const oldSystem = new OldSystem();
const adapter = new Adapter(oldSystem);
adapter.newMethod(); // Old system method
```

| **Use Case**                                                                      | **Cons**                                                            |
|-----------------------------------------------------------------------------------|---------------------------------------------------------------------|
| When the interface of an existing class needs to be adapted to another interface.  | Can lead to excessive use of objects.                               |

## Bridge

```javascript
class Abstraction {
  constructor(implementor) { this.implementor = implementor; }
  operation() { this.implementor.operationImpl(); }
}
```

```javascript
class Implementor {
  operationImpl() {}
}

class ConcreteImplementorA extends Implementor {
  operationImpl() {
    console.log('Concrete Implementor A');
  }
}

class ConcreteImplementorB extends Implementor {
  operationImpl() {
    console.log('Concrete Implementor B');
  }
}

class Abstraction {
  constructor(implementor) {
    this.implementor = implementor;
  }

  operation() {
    this.implementor.operationImpl();
  }
}

const implementorA = new ConcreteImplementorA();
const abstractionA = new Abstraction(implementorA);
abstractionA.operation(); // Concrete Implementor A

const implementorB = new ConcreteImplementorB();
const abstractionB = new Abstraction(implementorB);
abstractionB.operation(); // Concrete Implementor B

```

| **Use Case**                                                                      | **Cons**                                                            |
|-----------------------------------------------------------------------------------|---------------------------------------------------------------------|
| When you need to separate an object’s abstraction from its implementation.        | Can increase complexity with two layers of abstraction.             |

## Composite

```javascript
class Component {
  operation() {}
}
class Composite extends Component {
  constructor() { this.children = []; }
  add(child) { this.children.push(child); }
  operation() { this.children.forEach(child => child.operation()); }
}
```

```javascript
class Component {
  operation() {}
}

class Leaf extends Component {
  operation() {
    console.log('Leaf operation');
  }
}

class Composite extends Component {
  constructor() {
    super();
    this.children = [];
  }

  add(child) {
    this.children.push(child);
  }

  operation() {
    this.children.forEach(child => child.operation());
  }
}

const leaf1 = new Leaf();
const leaf2 = new Leaf();
const composite = new Composite();
composite.add(leaf1);
composite.add(leaf2);
composite.operation();
// Leaf operation
// Leaf operation

```

| **Use Case**                                                                      | **Cons**                                                            |
|-----------------------------------------------------------------------------------|---------------------------------------------------------------------|
| When objects need to be composed into tree structures to represent part-whole hierarchies. | Can make the system overly general.                                |

## Decorator

```javascript
class Component {
  operation() {}
}
class Decorator extends Component {
  constructor(component) { this.component = component; }
  operation() { this.component.operation(); }
}
```

```javascript
class Component {
  operation() {}
}

class ConcreteComponent extends Component {
  operation() {
    console.log('ConcreteComponent operation');
  }
}

class Decorator extends Component {
  constructor(component) {
    super();
    this.component = component;
  }

  operation() {
    this.component.operation();
  }
}

class ConcreteDecoratorA extends Decorator {
  operation() {
    super.operation();
    console.log('ConcreteDecoratorA operation');
  }
}

const component = new ConcreteComponent();
const decorator = new ConcreteDecoratorA(component);
decorator.operation();
// ConcreteComponent operation
// ConcreteDecoratorA operation

```

| **Use Case**                                                                      | **Cons**                                                            |
|-----------------------------------------------------------------------------------|---------------------------------------------------------------------|
| When behavior should be added to objects dynamically.                             | Can lead to a large number of small classes.                        |

## Facade

```javascript
class Facade {
  operation() {
    subsystem1.operation();
    subsystem2.operation();
  }
}
```

```javascript
class Subsystem1 {
  operation() {
    console.log('Subsystem1 operation');
  }
}

class Subsystem2 {
  operation() {
    console.log('Subsystem2 operation');
  }
}

class Facade {
  constructor() {
    this.subsystem1 = new Subsystem1();
    this.subsystem2 = new Subsystem2();
  }

  operation() {
    this.subsystem1.operation();
    this.subsystem2.operation();
  }
}

const facade = new Facade();
facade.operation();
// Subsystem1 operation
// Subsystem2 operation

```

| **Use Case**                                                                      | **Cons**                                                            |
|-----------------------------------------------------------------------------------|---------------------------------------------------------------------|
| When a simple interface to a complex subsystem is needed.                         | Can become a god object that knows too much or does too much.       |

## Flyweight

```javascript
class Flyweight {
  constructor(sharedState) { this.sharedState = sharedState; }
  operation(uniqueState) {}
}
class FlyweightFactory {
  getFlyweight(sharedState) {
    if (!this.flyweights[sharedState]) {
      this.flyweights[sharedState] = new Flyweight(sharedState);
    }
    return this.flyweights[sharedState];
  }
}
```

```javascript
class Flyweight {
  constructor(sharedState) {
    this.sharedState = sharedState;
  }

  operation(uniqueState) {
    console.log(`Flyweight: shared=${this.sharedState}, unique=${uniqueState}`);
  }
}

class FlyweightFactory {
  constructor() {
    this.flyweights = {};
  }

  getFlyweight(sharedState) {
    if (!this.flyweights[sharedState]) {
      this.flyweights[sharedState] = new Flyweight(sharedState);
    }
    return this.flyweights[sharedState];
  }
}

const factory = new FlyweightFactory();
const flyweight1 = factory.getFlyweight('shared');
flyweight1.operation('unique1');
const flyweight2 = factory.getFlyweight('shared');
flyweight2.operation('unique2');
// Flyweight: shared=shared, unique=unique1
// Flyweight: shared=shared, unique=unique2

```

| **Use Case**                                                                      | **Cons**                                                            |
|-----------------------------------------------------------------------------------|---------------------------------------------------------------------|
| When a large number of similar objects are needed.                                | Complex to implement, needs careful management of shared state.     |

## Proxy

```javascript
class Proxy {
  constructor(realSubject) { this.realSubject = realSubject; }
  request() { this.realSubject.request(); }
}
```

```javascript
class RealSubject {
  request() {
    console.log('RealSubject request');
  }
}

class Proxy {
  constructor(realSubject) {
    this.realSubject = realSubject;
  }

  request() {
    console.log('Proxy request');
    this.realSubject.request();
  }
}

const realSubject = new RealSubject();
const proxy = new Proxy(realSubject);
proxy.request();
// Proxy request
// RealSubject request

```

| **Use Case**                                                                      | **Cons**                                                            |
|-----------------------------------------------------------------------------------|---------------------------------------------------------------------|
| When a placeholder or proxy for another object is required to control access.     | Can introduce latency.                                              |

## Chain of Responsibility

```javascript
class Handler {
  setNext(handler) { this.next = handler; return handler; }
  handle(request) {
    if (this.next) { return this.next.handle(request); }
    return null;
  }
}
```

```javascript
class Handler {
  setNext(handler) {
    this.next = handler;
    return handler;
  }

  handle(request) {
    if (this.next) {
      return this.next.handle(request);
    }
    return null;
  }
}

class ConcreteHandler1 extends Handler {
  handle(request) {
    if (request === 'request1') {
      console.log('ConcreteHandler1 handled request1');
    } else {
      super.handle(request);
    }
  }
}

class ConcreteHandler2 extends Handler {
  handle(request) {
    if (request === 'request2') {
      console.log('ConcreteHandler2 handled request2');
    } else {
      super.handle(request);
    }
  }
}

const handler1 = new ConcreteHandler1();
const handler2 = new ConcreteHandler2();
handler1.setNext(handler2);

handler1.handle('request1'); // ConcreteHandler1 handled request1
handler1.handle('request2'); // ConcreteHandler2 handled request2

```

| **Use Case**                                                                      | **Cons**                                                            |
|-----------------------------------------------------------------------------------|---------------------------------------------------------------------|
| When multiple objects can handle a request, but the handler isn’t known beforehand. | Hard to observe the flow of the request.                     |

## Command

```javascript
class Command {
  execute() {}
}
class Invoker {
  setCommand(command) { this.command = command; }
  executeCommand() { this.command.execute(); }
}
```

```javascript
class Command {
  execute() {}
}

class ConcreteCommand extends Command {
  constructor(receiver) {
    super();
    this.receiver = receiver;
  }

  execute() {
    this.receiver.action();
  }
}

class Receiver {
  action() {
    console.log('Receiver action');
  }
}

class Invoker {
  setCommand(command) {
    this.command = command;
  }

  executeCommand() {
    this.command.execute();
  }
}

const receiver = new Receiver();
const command = new ConcreteCommand(receiver);
const invoker = new Invoker();
invoker.setCommand(command);
invoker.executeCommand(); // Receiver action

```

| **Use Case**                                                                      | **Cons**                                                            |
|-----------------------------------------------------------------------------------|---------------------------------------------------------------------|
| When parameterize objects with operations is required.                          | Can result in a large number of command classes.                    |

## Interpreter

```javascript
class Expression {
  interpret(context) {}
}
class TerminalExpression extends Expression {
  interpret(context) {
    // implementation
  }
}
```

```javascript
class Expression {
  interpret(context) {}
}

class TerminalExpression extends Expression {
  constructor(value) {
    super();
    this.value = value;
  }

  interpret(context) {
    return context.includes(this.value);
  }
}

class OrExpression extends Expression {
  constructor(expr1, expr2) {
    super();
    this.expr1 = expr1;
    this.expr2 = expr2;
  }

  interpret(context) {
    return this.expr1.interpret(context) || this.expr2.interpret(context);
  }
}

const expr1 = new TerminalExpression('Hello');
const expr2 = new TerminalExpression('World');
const orExpr = new OrExpression(expr1, expr2);

const context = 'Hello everyone';
console.log(orExpr.interpret(context)); // true

```

| **Use Case**                                                                      | **Cons**                                                            |
|-----------------------------------------------------------------------------------|---------------------------------------------------------------------|
| When the grammar of a language needs to be interpreted.                           | Complex grammars can be hard to manage and slow to execute.         |

## Iterator

```javascript
class Iterator {
  next() {}
  hasNext() {}
}
class Aggregate {
  createIterator() { return new Iterator(); }
}
```

```javascript
class Iterator {
  constructor(collection) {
    this.collection = collection;
    this.index = 0;
  }

  next() {
    return this.collection[this.index++];
  }

  hasNext() {
    return this.index < this.collection.length;
  }
}

class Aggregate {
  constructor() {
    this.items = [];
  }

  add(item) {
    this.items.push(item);
  }

  createIterator() {
    return new Iterator(this.items);
  }
}

const aggregate = new Aggregate();
aggregate.add('Item 1');
aggregate.add('Item 2');

const iterator = aggregate.createIterator();
while (iterator.hasNext()) {
  console.log(iterator.next());
}
// Item 1
// Item 2

```

| **Use Case**                                                                      | **Cons**                                                            |
|-----------------------------------------------------------------------------------|---------------------------------------------------------------------|
| When sequential access to elements of a collection is needed without exposing its underlying representation. | Might not be optimal for large collections.            |

## Mediator

```javascript
class Mediator {
  notify(sender, event) {}
}
class Component {
  constructor(mediator) { this.mediator = mediator; }
}
```

```javascript
class Mediator {
  notify(sender, event) {
    if (event === 'A') {
      console.log('Mediator reacts on A and triggers the following operations:');
      this.component2.doC();
    }
    if (event === 'D') {
      console.log('Mediator reacts on D and triggers the following operations:');
      this.component1.doB();
      this.component2.doC();
    }
  }
}

class Component1 {
  constructor(mediator) {
    this.mediator = mediator;
  }

  doA() {
    console.log('Component 1 does A.');
    this.mediator.notify(this, 'A');
  }

  doB() {
    console.log('Component 1 does B.');
    this.mediator.notify(this, 'B');
  }
}

class Component2 {
  constructor(mediator) {
    this.mediator = mediator;
  }

  doC() {
    console.log('Component 2 does C.');
    this.mediator.notify(this, 'C');
  }

  doD() {
    console.log('Component 2 does D.');
    this.mediator.notify(this, 'D');
  }
}

const mediator = new Mediator();
const component1 = new Component1(mediator);
const component2 = new Component2(mediator);

mediator.component1 = component1;
mediator.component2 = component2;

component1.doA();
// Component 1 does A.
// Mediator reacts on A and triggers the following operations:
// Component 2 does C.

component2.doD();
// Component 2 does D.
// Mediator reacts on D and triggers the following operations:
// Component 1 does B.
// Component 2 does C.

```

| **Use Case**                                                                      | **Cons**                                                            |
|-----------------------------------------------------------------------------------|---------------------------------------------------------------------|
| When reducing the complexity of communication between multiple objects is needed. | Can become a complex god object.                                    |

## Memento

```javascript
class Memento {
  constructor(state) { this.state = state; }
  getState() { return this.state; }
}
class Originator {
  setState(state) { this.state = state; }
  createMemento() { return new Memento(this.state); }
  restore(memento) { this.state = memento.getState(); }
}
```

```javascript
class Memento {
  constructor(state) {
    this.state = state;
  }

  getState() {
    return this.state;
  }
}

class Originator {
  setState(state) {
    console.log(`Originator: Setting state to ${state}`);
    this.state = state;
  }

  saveStateToMemento() {
    console.log(`Originator: Saving state to Memento`);
    return new Memento(this.state);
  }

  getStateFromMemento(memento) {
    this.state = memento.getState();
    console.log(`Originator: State after restoring from Memento: ${this.state}`);
  }
}

class Caretaker {
  constructor() {
    this.mementoList = [];
  }

  add(state) {
    this.mementoList.push(state);
  }

  get(index) {
    return this.mementoList[index];
  }
}

const originator = new Originator();
const caretaker = new Caretaker();

originator.setState("State #1");
originator.setState("State #2");
caretaker.add(originator.saveStateToMemento());

originator.setState("State #3");
caretaker.add(originator.saveStateToMemento());

originator.setState("State #4");

console.log(`Current State: ${originator.state}`);

originator.getStateFromMemento(caretaker.get(0));
originator.getStateFromMemento(caretaker.get(1));

```

| **Use Case**                                                                      | **Cons**                                                            |
|-----------------------------------------------------------------------------------|---------------------------------------------------------------------|
| When capturing and restoring an object’s internal state is required.              | Can lead to high memory usage.                                      |

## Observer

```javascript
class Observer {
  update(subject) {}
}
class Subject {
  attach(observer) { this.observers.push(observer); }
  notify() { this.observers.forEach(observer => observer.update(this)); }
}
```

```javascript
class Subject {
  constructor() {
    this.observers = [];
  }

  attach(observer) {
    this.observers.push(observer);
  }

  detach(observer) {
    this.observers = this.observers.filter(obs => obs !== observer);
  }

  notify() {
    this.observers.forEach(observer => observer.update());
  }
}

class Observer {
  constructor(name) {
    this.name = name;
  }

  update() {
    console.log(`${this.name} has been notified`);
  }
}

const subject = new Subject();

const observer1 = new Observer('Observer 1');
const observer2 = new Observer('Observer 2');

subject.attach(observer1);
subject.attach(observer2);

subject.notify();
// Observer 1 has been notified
// Observer 2 has been notified

```

| **Use Case**                                                                      | **Cons**                                                            |
|-----------------------------------------------------------------------------------|---------------------------------------------------------------------|
| When an object needs to notify multiple observers about state changes.            | Can lead to memory leaks with improper management.                  |

## State

```javascript
class State {
  handle(context) {}
}
class Context {
  setState(state) { this.state = state; }
  request() { this.state.handle(this); }
}
```

```javascript
class State {
  handle(context) {}
}

class ConcreteStateA extends State {
  handle(context) {
    console.log('ConcreteStateA handles request.');
    context.state = new ConcreteStateB();
  }
}

class ConcreteStateB extends State {
  handle(context) {
    console.log('ConcreteStateB handles request.');
    context.state = new ConcreteStateA();
  }
}

class Context {
  constructor() {
    this.state = new ConcreteStateA();
  }

  request() {
    this.state.handle(this);
  }
}

const context = new Context();
context.request(); // ConcreteStateA handles request.
context.request(); // ConcreteStateB handles request.
context.request(); // ConcreteStateA handles request.

```

| **Use Case**                                                                      | **Cons**                                                            |
|-----------------------------------------------------------------------------------|---------------------------------------------------------------------|
| When an object’s behavior depends on its state.                                   | Can increase the number of classes and complexity.                  |

## Strategy

```javascript
class Strategy {
  execute() {}
}
class Context {
  setStrategy(strategy) { this.strategy = strategy; }
  executeStrategy() { this.strategy.execute(); }
}
```

```javascript
class Strategy {
  execute() {}
}

class ConcreteStrategyA extends Strategy {
  execute() {
    console.log('Strategy A');
  }
}

class ConcreteStrategyB extends Strategy {
  execute() {
    console.log('Strategy B');
  }
}

class Context {
  setStrategy(strategy) {
    this.strategy = strategy;
  }

  executeStrategy() {
    this.strategy.execute();
  }
}

const context = new Context();
context.setStrategy(new ConcreteStrategyA());
context.executeStrategy(); // Strategy A

context.setStrategy(new ConcreteStrategyB());
context.executeStrategy(); // Strategy B

```

| **Use Case**                                                                      | **Cons**                                                            |
|-----------------------------------------------------------------------------------|---------------------------------------------------------------------|
| When different algorithms can be used interchangeably.                            | Clients must be aware of different strategies.                      |

## Template Method

```javascript
class AbstractClass {
  templateMethod() {
    this.step1();
    this.step2();
  }
  step1() {}
  step2() {}
}
class ConcreteClass extends AbstractClass {
  step1() {
    // implementation
  }
  step2() {
    // implementation
  }
}
```

```javascript
class AbstractClass {
  templateMethod() {
    this.baseOperation1();
    this.requiredOperation1();
    this.baseOperation2();
    this.hook1();
    this.requiredOperation2();
    this.baseOperation3();
    this.hook2();
  }

  baseOperation1() {
    console.log('AbstractClass says: I am doing the bulk of the work');
  }

  baseOperation2() {
    console.log('AbstractClass says: But I let subclasses override some operations');
  }

  baseOperation3() {
    console.log('AbstractClass says: But I am doing the bulk of the work anyway');
  }

  requiredOperation1() {}
  requiredOperation2() {}

  hook1() {}
  hook2() {}
}

class ConcreteClass1 extends AbstractClass {
  requiredOperation1() {
    console.log('ConcreteClass1 says: Implemented Operation1');
  }

  requiredOperation2() {
    console.log('ConcreteClass1 says: Implemented Operation2');
  }
}

class ConcreteClass2 extends AbstractClass {
  requiredOperation1() {
    console.log('ConcreteClass2 says: Implemented Operation1');
  }

  requiredOperation2() {
    console.log('ConcreteClass2 says: Implemented Operation2');
  }

  hook1() {
    console.log('ConcreteClass2 says: Overridden Hook1');
  }
}

const concreteClass1 = new ConcreteClass1();
concreteClass1.templateMethod();
// AbstractClass says: I am doing the bulk of the work
// ConcreteClass1 says: Implemented Operation1
// AbstractClass says: But I let subclasses override some operations
// AbstractClass says: But I am doing the bulk of the work anyway
// ConcreteClass1 says: Implemented Operation2

const concreteClass2 = new ConcreteClass2();
concreteClass2.templateMethod();
// AbstractClass says: I am doing the bulk of the work
// ConcreteClass2 says: Implemented Operation1
// AbstractClass says: But I let subclasses override some operations
// ConcreteClass2 says: Overridden Hook1
// AbstractClass says: But I am doing the bulk of the work anyway
// ConcreteClass2 says: Implemented Operation2

```

| **Use Case**                                                                      | **Cons**                                                            |
|-----------------------------------------------------------------------------------|---------------------------------------------------------------------|
| When defining the skeleton of an algorithm in a method, deferring some steps to subclasses is needed. | Can lead to code duplication if subclasses do not reuse the shared code. |

## Visitor

```javascript
class Visitor {
  visit(element) {}
}
class Element {
  accept(visitor) { visitor.visit(this); }
}
```

```javascript
// Visitor interface
class Visitor {
  visitConcreteElementA(element) {}
  visitConcreteElementB(element) {}
}

// ConcreteVisitor1 that implements Visitor
class ConcreteVisitor1 extends Visitor {
  visitConcreteElementA(element) {
    console.log(`${element.constructor.name} is visited by ConcreteVisitor1`);
  }

  visitConcreteElementB(element) {
    console.log(`${element.constructor.name} is visited by ConcreteVisitor1`);
  }
}

// ConcreteVisitor2 that implements Visitor
class ConcreteVisitor2 extends Visitor {
  visitConcreteElementA(element) {
    console.log(`${element.constructor.name} is visited by ConcreteVisitor2`);
  }

  visitConcreteElementB(element) {
    console.log(`${element.constructor.name} is visited by ConcreteVisitor2`);
  }
}

// Element interface
class Element {
  accept(visitor) {}
}

// ConcreteElementA that implements Element
class ConcreteElementA extends Element {
  accept(visitor) {
    visitor.visitConcreteElementA(this);
  }

  operationA() {
    console.log('ConcreteElementA operation');
  }
}

// ConcreteElementB that implements Element
class ConcreteElementB extends Element {
  accept(visitor) {
    visitor.visitConcreteElementB(this);
  }

  operationB() {
    console.log('ConcreteElementB operation');
  }
}

// Client code
const elements = [new ConcreteElementA(), new ConcreteElementB()];
const visitor1 = new ConcreteVisitor1();
const visitor2 = new ConcreteVisitor2();

elements.forEach(element => {
  element.accept(visitor1);
  element.accept(visitor2);
});

// Output:
// ConcreteElementA is visited by ConcreteVisitor1
// ConcreteElementB is visited by ConcreteVisitor1
// ConcreteElementA is visited by ConcreteVisitor2
// ConcreteElementB is visited by ConcreteVisitor2

```

| **Use Case**                                                                      | **Cons**                                                            |
|-----------------------------------------------------------------------------------|---------------------------------------------------------------------|
| When performing operations on elements of an object structure without changing the classes on which it operates. | Adding new element classes can be difficult.             |

[Home](https://avi-nash-s.github.io/handbook)
