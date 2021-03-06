JavaScript Design Patterns
================================================================================

Ports of Gang of Four design patterns in modern JavaScript, which have been ported from [CoffeeScript Design Patterns](https://github.com/dustinboston/coffeescript-design-patterns) with [Decaffeinate](https://decaffeinate-project.org/repl/), [CoffeeScript](http://coffeescript.org/), and [repl.it](https://repl.it/).

:warning: *Important:* Still in development, YMMV

* [Creational Patterns](#creational-patterns)
  * [Abstract Factory](#abstract-factory)
  * [Builder](#builder)
  * [Factory Method](#factory-method)
  * [Prototype](#prototype)
  * [Singleton](#singleton)
* [Structural Patterns](#structural-patterns)
  * [Adapter](#adapter)
  * [Bridge](#bridge)
  * [Composite](#composite)
  * [Decorator](#decorator)
  * [Facade](#facade)
  * [Flyweight](#flyweight)
  * [Proxy](#proxy)
* [Behavioral Patterns](#behavioral-patterns)
  * [Chain of Responsibility](#chain-of-responsibility)
  * [Command](#command)
  * [Interpreter](#interpreter)\*\*
  * [Iterator](#iterator)\*\*
  * [Mediator](#mediator)\*\*
  * [Memento](#memento)\* \*\*
  * [Observer](#observer)\*\*
  * [State](#state)\* \*\*
  * [Strategy](#strategy)\*\*
  * [Template Method](#template-method)\*\*
  * [Visitor](#visitor)\* \*\*
* [Foundational Classes](#foundational-classes)
  * [List Class](#list-class)\*\*

\* Unfinished pattern<br />
\*\* Unfinished port

Creational Patterns
================================================================================

Abstract Factory
--------------------------------------------------------------------------------

```js
class AbstractProductA {
  constructor(arg) {
    console.log(arg);
  }
}

class AbstractProductB {
  constructor(arg) {
    console.log(arg);
  }
}

class ConcreteProductA1 extends AbstractProductA {}
class ConcreteProductA2 extends AbstractProductA {}
class ConcreteProductB1 extends AbstractProductB {}
class ConcreteProductB2 extends AbstractProductB {}

class AbstractFactory {
  createProductA() {}
  createProductB() {}
}

class ConcreteFactory1 extends AbstractFactory {
  createProductA() {
    return new ConcreteProductA1('ConcreteProductA1');
  }
  createProductB() {
    return new ConcreteProductB1('ConcreteProductB1');
  }
}

class ConcreteFactory2 extends AbstractFactory {
  createProductA() {
    return new ConcreteProductA2('ConcreteProductA2');
  }
  createProductB() {
    return new ConcreteProductB2('ConcreteProductB2');
  }
}

class Client {
  constructor(factory) {
    this.abstractProductA = factory.createProductA();
    this.abstractProductB = factory.createProductB();
  }
}

class Example {
  static run() {
    let client2;
    const factory1 = new ConcreteFactory1();
    const client1 = new Client(factory1);
    const factory2 = new ConcreteFactory2();
    client2 = new Client(factory2);
  }
}

Example.run();
```

Builder
--------------------------------------------------------------------------------

```js
class Director {
  constructor(builder) {
    this.builder = builder;
  }
  construct(structure) {
    for (let i = 0; i < structure.length; i++) {
      const obj = structure[i];
      console.log(obj);
      this.builder.buildPart(obj);
    }
  }
}

class Product {
  constructor() {
    this.result = '';
  }
  append(obj) {
    this.result += obj;
  }
  get() {
    return this.result;
  }
}

class Builder {
  buildPart() {}
}

class ConcreteBuilder extends Builder {
  constructor() {
    super();
    this.product = new Product();
  }
  buildPart(obj) {
    this.product.append(obj.toUpperCase());
  }
  getResult() {
    return this.product;
  }
}

class Client {
  static run() {
    const concreteBuilder = new ConcreteBuilder();
    const director = new Director(concreteBuilder);
    director.construct('ohai');
    const result = concreteBuilder.getResult();
    alert(result.get());
  }
}

Client.run();
```

Factory Method
--------------------------------------------------------------------------------

```js
class Product {}
class ConcreteProduct1 extends Product {}
class ConcreteProduct2 extends Product {}

class Creator {
  factoryMethod() {}
  operation() {
    const product = this.factoryMethod();
  }
}

class ConcreteCreator extends Creator {
  factoryMethod(id) {
    switch (id) {
      case id === 1: return new ConcreteProduct1();
      case id === 2: return new ConcreteProduct2();
    }
  }
}

class Example {
  static run() {
    const creator = new ConcreteCreator();
    console.log(creator.factoryMethod(1));
    console.log(creator.factoryMethod(2));
    console.log(creator.factoryMethod(3));
  }
}

Example.run();
```

Prototype
--------------------------------------------------------------------------------

```js
class Client {
  constructor() {}
  operation(prototype) {
    return prototype.clone();
  }
}

class Prototype {
  clone() {
    return Object.create(this);
  }

  setProperty(property) {
    this.property = property;
  }
  
  logProperty() {
    console.log(this.property || '-');
  }
}

class ConcretePrototype1 extends Prototype {}

class ConcretePrototype2 extends Prototype {}

class Example {
  static run() {
    const client = new Client();
    const cp1 = new ConcretePrototype1();
    const cp1Prototype = client.operation(cp1);

    cp1.setProperty('original1');
    cp1Prototype.setProperty('clone1');
    cp1.logProperty();
    cp1Prototype.logProperty();
  }
}

Example.run();
```

Singleton
--------------------------------------------------------------------------------

```js
class Singleton {
  static getInstance() {
    return this._instance || (this._instance = new this(...arguments));
  }
}

Singleton._instance = null;

// Example implementation
class ExampleClass extends Singleton {
  
  constructor(arg1, arg2) {
    super();
    console.log(arg1, arg2);
  }
  
  set(key, val) {
    this.properties[key] = val;
  }

  get(key) {
    return this.properties[key];
  }
}

ExampleClass.prototype.properties = {};

class Example {
  static run() {
    example = ExampleClass.getInstance('arg1', 'arg2');
    example.set('Singleton', 'This is a singleton value');
    console.log(example.get('Singleton'));
  }
}

Example.run();
```

Structural Patterns
================================================================================

Adapter
--------------------------------------------------------------------------------

```js
class Target {
  request() {
    return console.log('Not fired');
  }
}

class Adaptee {
  specificRequest() {
    console.log('Specific request');
  }
}

class Adapter extends Target {
  constructor(adaptee) {
    super()
    this.adaptee = adaptee;
  }
  request() {
    this.adaptee.specificRequest();
  }
}

class Client {
  static run() {
    const adaptee = new Adaptee();
    const adapter = new Adapter(adaptee);
    adapter.request();
  }
}

Client.run();
```

Bridge
--------------------------------------------------------------------------------

```js
class Abstraction {
  constructor(imp) {
    this.imp = imp;
  }
  operation() {
    this.imp.operationImp();
  }
}

class RefinedAbstraction extends Abstraction {}

class Implementor {
  operationImp() {}
}

class ConcreteImplementorA extends Implementor {
  operationImp() {
    console.log('ConcreteImplementorA::operationImp');
  }
}

class ConcreteImplementorB extends Implementor {
  operationImp() {
    console.log('ConcreteImplementorB::operationImp');
  }
}

class Client {
  static run() {
    const concreteImplementorA = new ConcreteImplementorA();
    const refinedAbstractionA = new RefinedAbstraction(concreteImplementorA);
    refinedAbstractionA.operation();

    const concreteImplementorB = new ConcreteImplementorB();
    const refinedAbstractionB = new RefinedAbstraction(concreteImplementorB);
    refinedAbstractionB.operation();
  }
}

Client.run();
```

Composite
--------------------------------------------------------------------------------

```js
class Component {
  constructor() {
    this.list = [];
  }
  getComposite() {}
  operation() {}
  add(component) {}
}

class Leaf extends Component {}

class Composite extends Component {
  add(component) {
    this.list.push(component);
  }
  operation() {
    console.log(this.list);
  }
  getComposite() {
    return this;
  }
}

class Client {
  static run() {
    // Create a Composite object and add a Leaf
    const composite = new Composite();
    const leaf = new Leaf();
    composite.add(leaf);
    composite.operation();
    
    // Add a Composite to the Composite
    const composite2 = new Composite();
    composite.add(composite2);
    composite.operation();
  }
}

Client.run();
```

Decorator
--------------------------------------------------------------------------------

```js
class Component {
  add(key, val) {
    this.props[key] = val;
  }
  get() {
    return this.props;
  }
  process() {}
}
Component.prototype.props = {};

class ConcreteComponent extends Component {
  process() {}
}

class Decorator extends Component {
  constructor(component) {
    super();
    this.component = component;
  }
  process() {
    this.component.process();
  }
}

class ConcreteDecoratorA extends Decorator {
  process() {
    this.component.add('concreteDecoratorAProcess', true);
    super.process();
  }
}

class ConcreteDecoratorB extends Decorator {
  process() {
    this.component.add('concreteDecoratorBProcess', true);
    super.process();
  }
}

class Example {
  static run() {
    const cmpt = new ConcreteDecoratorA(new ConcreteDecoratorB(new ConcreteComponent()));
    cmpt.process();
    console.log(cmpt.get());
  }
}

Example.run();
```

Facade
--------------------------------------------------------------------------------

```js
class Subsystem1 {
  constructor() {
    console.log('subsystem1');
  }
}

class Subsystem2 {
  constructor() {
    console.log('subsystem2');
  }
}

class Facade {
  request() {
    const s1 = new Subsystem1();
    const s2 = new Subsystem2();
  }
}

class Client {
  static run() {
    const facade = new Facade();
    facade.request();
  }
}

Client.run();
```

Flyweight
--------------------------------------------------------------------------------

```js
class FlyweightFactory {
  constructor() {
    this.flyweights = {};
  }

  getFlyweight(key) {
    if (!this.flyweights[key]) {
      this.flyweights[key] = new ConcreteFlyweight(key);
    }
    return this.flyweights[key];
  }
}

class Flyweight {
  constructor(key) {
    this.key = key;
  }
  operation(extrinsicState) {}
}

class ConcreteFlyweight extends Flyweight {
  operation(extrinsicState) {
    this.intrinsicState += `${extrinsicState} `;
    console.log(this.intrinsicState);
  }
}
ConcreteFlyweight.prototype.intrinsicState = '';

class UnsharedConcreteFlyweight extends Flyweight {}
UnsharedConcreteFlyweight.prototype.allState = null;

class Client {
  static run() {
    const factory = new FlyweightFactory();
    const foo = factory.getFlyweight('foo');
    const bar = factory.getFlyweight('bar');
    const baz = factory.getFlyweight('baz');
    const qux = factory.getFlyweight('foo');
    foo.operation('red');
    bar.operation('green');
    baz.operation('blue');
    qux.operation('black');
  }
}
 
Client.run();
```

Proxy
--------------------------------------------------------------------------------

```js
class Subject {
  request() {}
}

class RealSubject extends Subject {
  request() {
    return console.log('Real subject');
  }
}

class Proxy extends Subject {
  request() {
    if (!this.realSubject) {
      this.realSubject = new RealSubject();
    }
    this.realSubject.request();
  }
}

class Client {
  static run() {
    const proxy = new Proxy();
    proxy.request();
  }
}

Client.run();
```

Behavioral Patterns
================================================================================

Chain of Responsibility
--------------------------------------------------------------------------------

```js
class Handler {
  constructor(kind, successor) {
    this.kind = kind;
    this.successor = successor;
  }
  handleRequest() {
    if (this.successor) {
      this.successor.handleRequest();
    }
  }
}

class ConcreteHandler1 extends Handler {
  handleRequest() {
    console.log(`${this.kind} handled`);
  }
}

class ConcreteHandler2 extends Handler {}

class Client {
  static run() {
    const concreteHandler1 = new ConcreteHandler1('foo');
    const concreteHandler2 = new ConcreteHandler2('bar', concreteHandler1);
    concreteHandler2.handleRequest();
  }
}

Client.run();
```

Command
--------------------------------------------------------------------------------

```js
class Command {
  execute() {}
}

class Invoker {
  execute(command) {
    command.execute();
  }
}

class Receiver {
  action1() { return console.log('action1'); }
  action2() { return console.log('action2'); }
}

class ConcreteCommandA extends Command {
  constructor(receiver) {
    super();
    this.receiver = receiver;
  }
  execute() {
    return this.receiver.action1();
  }
}

class ConcreteCommandB extends Command {
  constructor(receiver) {
    super();
    this.receiver = receiver;
  }
  execute() {
    return this.receiver.action2();
  }
}

class Client {
  static run(action) {
    const receiver = new Receiver();
    const ccmdA = new ConcreteCommandA(receiver);
    const ccmdB = new ConcreteCommandB(receiver);
    const invoker = new Invoker();
    if (action === 1) { invoker.execute(ccmdA); }
    if (action === 2) { return invoker.execute(ccmdB); }
  }
}

class Example {
  static run() {
    Client.run(1);
    Client.run(2);
  }
}

Example.run();
```

Interpreter
--------------------------------------------------------------------------------

```js
class Context {
  constructor(name) {
    this.name = name;
  }
  getName() {
    return this.name;
  }
}

class AbstractExpression {
  constructor() {
    this.expressions = [];
  }
  interpret(context) {
    this.context = context;
  }
}

class TerminalExpression extends AbstractExpression {
  interpret(context) {
    this.context = context;
    return console.log(`Terminal expression for ${this.context.getName()}`);
  }
}

class NonterminalExpression extends AbstractExpression {
  addExpression(expression) {
    return this.expressions.push(expression);
  }

  interpret(context) {
    this.context = context;
    console.log(`Nonterminal expression for ${this.context.getName()}`);
    return Array.from(this.expressions).map((expression) =>
      expression.interpret(this.context));
  }
}

class Client {
  static run() {
    let context = new Context('*le context');
    let root = new NonterminalExpression();
    root.addExpression(new TerminalExpression());
    root.addExpression(new TerminalExpression());
    return root.interpret(context);
  }
};

Client.run();
```

Iterator
--------------------------------------------------------------------------------

```js
class Iterator {
  first() {}
  next() {}
  isDone() {}
  currentItem() {}
}

class ConcreteIterator extends Iterator {
  constructor(list1) {
    this.list = list1;
    this.current = 0;
  }
  first() {
    return this.current = 0;
  }
  next() {
    return this.current += 1;
  }
  isDone() {
    return this.current >= this.list.count();
  }
  currentItem() {
    if (this.isDone()) { throw new Error('IteratorOutOfBounds'); }
    return this.list.get(this.current);
  }
}
    
class Aggregate {
  createIterator() {}
}

class ConcreteAggregate extends Aggregate {
  createIterator(items) {
    let list = new List();
    for (let key in items) {
      let val = items[key];
      val.__POINTER__ = key;
      list.append(val);
    }
    return new ConcreteIterator(list);
  }
}

class Client {
  static run() {
    let aggregate = new ConcreteAggregate();
    let iterator = aggregate.createIterator(items);

    return (() => {
      let result = [];
      while (!iterator.isDone()) {
        let current = iterator.currentItem();
        // Do something with the item here...
        result.push(iterator.next());
      }
      return result;
    })();
  }
};

Client.run();
```

Mediator
--------------------------------------------------------------------------------

```js
class Colleague {
  constructor(mediator) {
    this.mediator = mediator;
  }
  changed() {
    return this.mediator.colleagueChanged(this);
  }
}

class ConcreteColleague1 extends Colleague {
  event() {
    return this.changed();
  }
}

class ConcreteColleague2 extends Colleague {
  event() {
    return this.changed();
  }
}

class Mediator {
  colleagueChanged(colleague) {}
}

class ConcreteMediator extends Mediator {
  createColleagues() {
    this.colleague1 = new ConcreteColleague1(this);
    this.colleague2 = new ConcreteColleague2(this);
    this.colleague1.event();
    return this.colleague2.event();
  }

  colleagueChanged(colleague) {
    if (colleague instanceof ConcreteColleague1) {
      return console.log('colleague1 changed');
    } else if (colleague instanceof ConcreteColleague2) {
      return console.log('colleague2 changed');
    }
  }
}

class Example {
  static run() {
    let m = new ConcreteMediator();
    return m.createColleagues();
  }
};

Example.run();
```

Memento
--------------------------------------------------------------------------------

```js
class Memento {
  constructor(state) {
    this.state = state;
  }
  getState() { return this.state; }
}

class Originator {
  constructor(state) {
    this.state = state;
  }

  setMemento(memento) {
    this.state = memento.getState();
  }

  createMemento() {
    return new Memento(this.state);
  }

  _changeState(state) {
    this.state = state;
  }
  _showState() {
    return console.log(this.state);
  }
}

class Caretaker {
  constructor(originator) {
    this.originator = originator;
  }

  doCommand() {
    this.state = this.originator.createMemento();
    return this.originator.setMemento(this.state);
  }

  undoCommand() {
    return this.originator.setMemento(this.state);
  }
}

class Client {
  static run() {
    let originator = new Originator('foo');
    let caretaker = new Caretaker(originator);
    originator._showState();

    caretaker.doCommand();

    originator._changeState('bar');
    originator._showState();

    caretaker.undoCommand();
    return originator._showState();
  }
};

Client.run();
```

Observer
--------------------------------------------------------------------------------

```js
class Subject {
  constructor() {
    this.counter = 0;
    this.observers = new List();
  }
  attach(o) {
    o.__POINTER__ = this.counter;
    this.observers.append(o);
    return this.counter += 1;
  }
  detach(o) {
    return this.observers.remove(o);
  }
  notify() {
    let i = new ConcreteIterator(this.observers);
    return (() => {
      let result = [];
      while (!i.isDone()) {
        i.currentItem().update(this);
        result.push(i.next());
      }
      return result;
    })();
  }
};

class Observer {
  update(theChangedSubject) {}
}

class ConcreteSubject extends Subject {}

class ConcreteObserver extends Observer {
  update(theChangedSubject) {
    return console.log('Updated');
  }
}

class Example {
  static run() {
    let subject = new ConcreteSubject();
    let observer = new ConcreteObserver();
    subject.attach(observer); 
    return subject.notify();
  }
};

Example.run();
```

State
--------------------------------------------------------------------------------

Strategy
--------------------------------------------------------------------------------

```js
class Context {
  constructor(strategy) {
    this.strategy = strategy;
  }
  contextInterface() {
    return this.strategy.algorithmInterface();
  }
};

class Strategy {
  algorithmInterface() {}
}

class ConcreteStrategyA extends Strategy {
  algorithmInterface() {
    return console.log('ConcreteStrategyA');
  }
}

class ConcreteStrategyB extends Strategy {
  algorithmInterface() {
    return console.log('ConcreteStrategyB');
  }
}

class ConcreteStrategyC extends Strategy {
  algorithmInterface() {
    return console.log('ConcreteStrategyC');
  }
}

class Example {
  static run() {
    let resultC;
    let context = new Context(new ConcreteStrategyA());
    let resultA = context.contextInterface;

    context = new Context(new ConcreteStrategyB());
    let resultB = context.contextInterface;

    context = new Context(new ConcreteStrategyC());
    return resultC = context.contextInterface;
  }
};

Example.run();
```

Template Method
--------------------------------------------------------------------------------

```js
class AbstractClass {
  templateMethod() {
    this.primitiveOperation1();
    return this.primitiveOperation2();
  }

  primitiveOperation1() {}
  primitiveOperation2() {}
}

class ConcreteClass extends AbstractClass {
  primitiveOperation1() {
    return console.log('primitiveOperation1');
  }
  primitiveOperation2() {
    return console.log('primitiveOperation2');
  }
}

// Static Template Method
class StaticAbstractClass {
  static templateMethod() {
    let cls = new (this)();
    cls.primitiveOperation1();
    return cls.primitiveOperation2();
  }
  primitiveOperation1() {}
  primitiveOperation2() {}
}

class StaticConcreteClass extends StaticAbstractClass {
  primitiveOperation1() {
    return console.log('primitiveOperation1');
  }
  primitiveOperation2() {
    return console.log('primitiveOperation2');
  }
}

class Example {
  static run() {
    let concrete = new ConcreteClass();
    concrete.templateMethod();

    return StaticConcreteClass.templateMethod();
  }
};

Example.run();
```

Visitor
--------------------------------------------------------------------------------

Foundational Classes
================================================================================

List Class
--------------------------------------------------------------------------------

```js
class List {
  constructor() {
    // A list of pointers
    this.items = [];
    
    // Objects passed in by pointer
    this.objects = {};
  }

  // Returns the number of objects in the list
  count() {
    return this.items.length;
  }

  // Returns the object at the given length
  get(index) {
    return this.objects[this.items[index]];
  }
    
  // Returns the first object in the list
  first() {
    return this.objects[this.items[0]];
  }

  // Returns the last object in the list
  last() {
    return this.objects[this.items[this.items.length - 1]];
  }

  // Adds the argument to the list, making it the last item
  append(item) {
    let pointer = item.__POINTER__;
    this.items.push(pointer);
    return this.objects[pointer] = item;
  }

  // Removes the given element from the list.
  remove(item) {
    let pointer = item.__POINTER__;
    delete this.objects[pointer];
    let index = Array.from(this.items).includes(pointer);
    // delete @items[index] if index isnt -1
    return this.items.splice(index, 1);
  }

  // Removes the last element from the list
  removeLast() {
    return this.remove(this.last);
  }

  // Removes the first element from the list
  removeFirst() {
    return this.remove(this.first);
  }

  // Removes all elements from the list
  removeAll() {
    this.items = [];
    return this.objects = {};
  }
}

let list = new List();
list.append({__POINTER__: 'uniqueid', other: 'properties'});
```
