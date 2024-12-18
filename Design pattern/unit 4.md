# Behavioral Design Patterns

Behavioral design patterns are concerned with the interaction and responsibilities of objects. These patterns help define how objects communicate and interact in a flexible and reusable way.

---

## Part I: Behavioral Patterns

### 1. **Chain of Responsibility Pattern**

#### Overview:
The Chain of Responsibility pattern allows a request to be passed along a chain of handlers until it is processed. Each handler decides either to process the request or to pass it to the next handler in the chain.

#### Key Concepts:
- Decouples the sender and receiver of a request.
- Provides multiple objects a chance to handle the request.
- Handlers form a chain.

#### Structure:
1. **Handler (Abstract Class/Interface):** Defines the interface for handling requests and references the next handler in the chain.
2. **ConcreteHandler:** Implements the handler interface and processes the request or forwards it.
3. **Client:** Initiates the request and attaches handlers.

#### Example in Python:
```python
class Handler:
    def __init__(self, successor=None):
        self.successor = successor

    def handle_request(self, request):
        if self.successor:
            return self.successor.handle_request(request)

class ConcreteHandlerA(Handler):
    def handle_request(self, request):
        if request == "A":
            return "Handled by A"
        return super().handle_request(request)

class ConcreteHandlerB(Handler):
    def handle_request(self, request):
        if request == "B":
            return "Handled by B"
        return super().handle_request(request)

# Client setup
handler_chain = ConcreteHandlerA(ConcreteHandlerB())
print(handler_chain.handle_request("A"))  # Output: Handled by A
print(handler_chain.handle_request("B"))  # Output: Handled by B
```

---

### 2. **Command Pattern**

#### Overview:
The Command pattern encapsulates a request as an object, thereby allowing parameterization of clients with queues, requests, and operations.

#### Key Concepts:
- Decouples the sender and receiver of requests.
- Enables undo and redo functionality.
- Supports queuing and logging of requests.

#### Structure:
1. **Command (Interface):** Declares the execution method.
2. **ConcreteCommand:** Implements the command interface and calls the receiver's action.
3. **Receiver:** Performs the request's action.
4. **Invoker:** Stores and executes commands.
5. **Client:** Configures the commands.

#### Example in Python:
```python
class Command:
    def execute(self):
        pass

class Light:
    def turn_on(self):
        print("Light is ON")

    def turn_off(self):
        print("Light is OFF")

class LightOnCommand(Command):
    def __init__(self, light):
        self.light = light

    def execute(self):
        self.light.turn_on()

class LightOffCommand(Command):
    def __init__(self, light):
        self.light = light

    def execute(self):
        self.light.turn_off()

class RemoteControl:
    def __init__(self):
        self.command = None

    def set_command(self, command):
        self.command = command

    def press_button(self):
        self.command.execute()

# Client setup
light = Light()
on_command = LightOnCommand(light)
off_command = LightOffCommand(light)

remote = RemoteControl()
remote.set_command(on_command)
remote.press_button()  # Output: Light is ON
remote.set_command(off_command)
remote.press_button()  # Output: Light is OFF
```

---

### 3. **Interpreter Pattern**

#### Overview:
The Interpreter pattern defines a grammatical representation for a language and an interpreter to interpret sentences in the language.

#### Key Concepts:
- Useful for building interpreters or parsers.
- Requires a grammar.
- Often used for simple languages or configuration files.

#### Structure:
1. **AbstractExpression:** Declares the interpretation method.
2. **TerminalExpression:** Implements an interpretation method for terminal symbols.
3. **NonTerminalExpression:** Implements interpretation for non-terminal symbols.
4. **Context:** Contains information needed during interpretation.

#### Example in Python:
```python
class Expression:
    def interpret(self):
        pass

class Number(Expression):
    def __init__(self, value):
        self.value = value

    def interpret(self):
        return self.value

class Add(Expression):
    def __init__(self, left, right):
        self.left = left
        self.right = right

    def interpret(self):
        return self.left.interpret() + self.right.interpret()

# Client setup
expr = Add(Number(5), Number(3))
print(expr.interpret())  # Output: 8
```

---

### 4. **Iterator Pattern**

#### Overview:
The Iterator pattern provides a way to access elements of a collection sequentially without exposing the underlying representation.

#### Key Concepts:
- Decouples collection traversal from the collection.
- Supports multiple traversals.
- Implements a consistent interface for iteration.

#### Structure:
1. **Iterator:** Defines the interface for iteration.
2. **ConcreteIterator:** Implements the iterator.
3. **Aggregate:** Defines the interface for creating iterators.
4. **ConcreteAggregate:** Implements the aggregate interface.

#### Example in Python:
```python
class Iterator:
    def __init__(self, collection):
        self.collection = collection
        self.index = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self.index < len(self.collection):
            value = self.collection[self.index]
            self.index += 1
            return value
        else:
            raise StopIteration

# Client setup
items = [1, 2, 3, 4]
iterator = Iterator(items)
for item in iterator:
    print(item)  # Output: 1, 2, 3, 4
```

---

## Part II: Behavioral Patterns

### 1. **Mediator Pattern**

#### Overview:
The Mediator pattern defines an object that encapsulates how a set of objects interact, promoting loose coupling.

#### Key Concepts:
- Centralizes communication.
- Avoids direct communication between objects.
- Simplifies object collaboration.

#### Structure:
1. **Mediator (Interface):** Declares communication methods.
2. **ConcreteMediator:** Implements coordination.
3. **Colleague:** Communicates via the mediator.

#### Example:
```python
class Mediator:
    def notify(self, sender, event):
        pass

class ConcreteMediator(Mediator):
    def __init__(self, component1, component2):
        self.component1 = component1
        self.component1.mediator = self
        self.component2 = component2
        self.component2.mediator = self

    def notify(self, sender, event):
        if event == "A":
            print("Mediator reacts to A and triggers B.")
            self.component2.do_b()
        elif event == "B":
            print("Mediator reacts to B and triggers A.")
            self.component1.do_a()

class BaseComponent:
    def __init__(self):
        self.mediator = None

class Component1(BaseComponent):
    def do_a(self):
        print("Component1 does A.")
        self.mediator.notify(self, "A")

class Component2(BaseComponent):
    def do_b(self):
        print("Component2 does B.")
        self.mediator.notify(self, "B")

# Client setup
c1 = Component1()
c2 = Component2()
mediator = ConcreteMediator(c1, c2)

c1.do_a()  # Output: Component1 does A. Mediator reacts to A and triggers B.
c2.do_b()  # Output: Component2 does B. Mediator reacts to B and triggers A.
```

---

### 2. **Memento Pattern**

#### Overview:
The Memento pattern captures an object’s state and allows restoring it without violating encapsulation.

#### Key Concepts:
- Provides undo functionality.
- Separates state storage from the object itself.

#### Structure:
1. **Memento:** Stores the state.
2. **Originator:** Creates and restores mementos.
3. **Caretaker:** Manages mementos.

#### Example:
```python
class Memento:
    def __init__(self, state):
        self.state = state

class Originator:
    def __init__(self):
        self.state = None

    def save_state(self):
        return Memento(self.state)

    def restore_state(self, memento):
        self.state = memento.state

class Caretaker:
    def __init__(self):
        self.mementos = []

    def backup(self, memento):
        self.mementos.append(memento)

    def undo(self, originator):
        if self.mementos:
            originator.restore_state(self.mementos.pop())

# Client setup
originator = Originator()
caretaker = Caretaker()

originator.state = "State1"
caretaker.backup(originator.save_state())

originator.state = "State2"
originator.state = "State3"
caretaker.undo(originator)
print(originator.state)  # Output: State1
```

---

### 3. **Observer Pattern**

#### Overview:
The Observer pattern defines a dependency between objects so that when one object changes state, all dependents are notified.

#### Key Concepts:
- Promotes loose coupling.
- Implements a publish/subscribe model.

#### Structure:
1. **Subject:** Maintains observers and notifies them.
2. **Observer:** Receives updates.
3. **ConcreteSubject:** Implements state change.

#### Example:
```python
class Subject:
    def __init__(self):
        self._observers = []

    def attach(self, observer):
        self._observers.append(observer)

    def detach(self, observer):
        self._observers.remove(observer)

    def notify(self):
        for observer in self._observers:
            observer.update(self)

class Observer:
    def update(self, subject):
        pass

class ConcreteObserver(Observer):
    def update(self, subject):
        print("Observer updated with state:", subject.state)

# Client setup
subject = Subject()
observer = ConcreteObserver()
subject.attach(observer)

subject.state = "New State"
subject.notify()  # Output: Observer updated with state: New State
```

---

## 50 Important Interview Questions and Answers

### Chain of Responsibility Pattern

1. **What is the Chain of Responsibility pattern?**
   - It is a behavioral design pattern that allows a request to pass through a chain of handlers until it is processed. Each handler either handles the request or passes it to the next handler.

2. **What are the key components of the Chain of Responsibility pattern?**
   - Handler (abstract interface or class), ConcreteHandler (specific implementations of the handler), and Client (initiates the request).

3. **What is the primary benefit of using the Chain of Responsibility pattern?**
   - It promotes loose coupling by separating the sender and receiver of a request, allowing flexibility in the chain of handlers.

4. **Give a real-world example of the Chain of Responsibility pattern.**
   - Help desks in organizations: A request is passed from Level 1 support to Level 2 and then to Level 3 if unresolved.

5. **What are the drawbacks of the Chain of Responsibility pattern?**
   - It may introduce performance issues if the chain is long, as every request needs to traverse multiple handlers.

### Command Pattern

6. **What is the Command pattern?**
   - The Command pattern encapsulates a request as an object, allowing the client to parameterize and queue requests.

7. **What are the key components of the Command pattern?**
   - Command, ConcreteCommand, Receiver, Invoker, and Client.

8. **What are the advantages of the Command pattern?**
   - It promotes loose coupling, supports undo/redo operations, and allows request queuing and logging.

9. **How is the Command pattern used in real-world scenarios?**
   - GUI buttons, where each button click is a command that encapsulates an action.

10. **Can the Command pattern be used for transactions?**
    - Yes, the Command pattern is often used for implementing transactional systems

Here are 30 important questions and answers based on the behavioral design patterns you mentioned:

### **Behavioral Patterns Part I:**

#### 1. **What is the Chain of Responsibility Pattern?**
   **Answer:** The Chain of Responsibility Pattern is a behavioral design pattern that allows passing a request along a chain of handlers. Each handler processes the request or passes it to the next handler in the chain. This decouples sender and receiver by giving multiple objects a chance to handle the request.

#### 2. **What are the main components of the Chain of Responsibility Pattern?**
   **Answer:** 
   - **Handler**: Defines an interface for handling requests.
   - **ConcreteHandler**: Handles requests it is responsible for or passes the request to the next handler in the chain.
   - **Client**: Sends requests to the handler chain.

#### 3. **How does the Chain of Responsibility Pattern improve flexibility?**
   **Answer:** It allows adding or removing handlers dynamically at runtime, which helps in creating a more flexible and maintainable system where behavior can be changed without modifying existing code.

#### 4. **What is the Command Pattern?**
   **Answer:** The Command Pattern is a behavioral design pattern where requests are encapsulated as objects. This allows the client to issue requests without knowing the details of the operation being executed.

#### 5. **What are the key components of the Command Pattern?**
   **Answer:**
   - **Command**: Defines the interface for executing an operation.
   - **ConcreteCommand**: Implements the Command interface and defines the binding between a receiver and an action.
   - **Invoker**: Asks the command to execute the request.
   - **Receiver**: Knows how to perform the operation to satisfy a request.

#### 6. **What are the benefits of using the Command Pattern?**
   **Answer:** The Command Pattern decouples the sender of a request from the object that performs the action, enables undo/redo functionality, and supports transactional behavior.

#### 7. **What is the Interpreter Pattern?**
   **Answer:** The Interpreter Pattern is a behavioral design pattern that defines a grammatical representation for a language and an interpreter to interpret sentences in the language.

#### 8. **What are the components of the Interpreter Pattern?**
   **Answer:**
   - **AbstractExpression**: Declares an abstract method for interpreting the context.
   - **TerminalExpression**: Implements the interpretation rule for a terminal expression.
   - **NonTerminalExpression**: Represents the rules for non-terminal expressions.
   - **Context**: Holds information that is needed for interpretation.

#### 9. **When should the Interpreter Pattern be used?**
   **Answer:** It is useful when designing a simple language or expression evaluator where expressions can be parsed and interpreted at runtime.

#### 10. **What is the Iterator Pattern?**
   **Answer:** The Iterator Pattern is a behavioral design pattern that provides a way to access elements of a collection sequentially without exposing the underlying representation of the collection.

#### 11. **What are the components of the Iterator Pattern?**
   **Answer:**
   - **Iterator**: Defines methods for iterating over elements.
   - **ConcreteIterator**: Implements the Iterator interface and keeps track of the current position in the collection.
   - **Aggregate**: Defines a method for returning an iterator.
   - **ConcreteAggregate**: Implements the Aggregate interface and returns an iterator.

#### 12. **How does the Iterator Pattern promote flexibility?**
   **Answer:** It allows collection classes to change their implementation without affecting the client code, as the client interacts only with the iterator.

---

### **Behavioral Patterns Part II:**

#### 13. **What is the Mediator Pattern?**
   **Answer:** The Mediator Pattern is a behavioral design pattern that centralizes communication between objects to promote loose coupling. Instead of communicating directly, objects interact via a mediator, which handles communication between them.

#### 14. **What are the key components of the Mediator Pattern?**
   **Answer:**
   - **Mediator**: Defines the interface for communication between colleagues.
   - **ConcreteMediator**: Implements the Mediator interface and coordinates communication between colleague objects.
   - **Colleague**: Objects that communicate through the mediator.

#### 15. **How does the Mediator Pattern help in reducing dependencies?**
   **Answer:** It eliminates direct dependencies between objects, making them easier to modify and maintain. The mediator handles the interaction, reducing the complexity of relationships.

#### 16. **What is the Memento Pattern?**
   **Answer:** The Memento Pattern is a behavioral design pattern that allows capturing and externalizing an object’s internal state so that it can be restored later without violating encapsulation.

#### 17. **What are the main components of the Memento Pattern?**
   **Answer:**
   - **Originator**: The object whose state is saved.
   - **Memento**: Stores the internal state of the originator.
   - **Caretaker**: Maintains the memento but doesn't modify it.

#### 18. **How does the Memento Pattern support undo functionality?**
   **Answer:** By storing snapshots of the object's state at different points in time, the Memento Pattern allows restoring a previous state, effectively supporting undo operations.

#### 19. **What is the Observer Pattern?**
   **Answer:** The Observer Pattern is a behavioral design pattern where an object (the subject) maintains a list of its dependent observers and notifies them of state changes, typically by calling one of their methods.

#### 20. **What are the components of the Observer Pattern?**
   **Answer:**
   - **Subject**: The object being observed.
   - **Observer**: Objects that need to be updated when the subject's state changes.
   - **ConcreteSubject**: Implements the Subject interface and notifies observers.
   - **ConcreteObserver**: Implements the Observer interface and updates itself when notified by the subject.

#### 21. **What are the advantages of the Observer Pattern?**
   **Answer:** It promotes loose coupling between the subject and observers, as the subject does not need to know about the specific observers. It's ideal for event-driven systems and for updating multiple views when the state of an object changes.

#### 22. **What is the role of a subject in the Observer Pattern?**
   **Answer:** The subject maintains a list of observers and is responsible for notifying them when its state changes. It is unaware of the specifics of each observer, allowing flexibility.

#### 23. **How does the Observer Pattern facilitate dynamic updates?**
   **Answer:** Observers are notified automatically when the subject’s state changes, ensuring that updates are propagated without the need for manual intervention.

#### 24. **How does the Mediator Pattern differ from the Observer Pattern?**
   **Answer:** In the Mediator Pattern, communication is centralized through a mediator, while in the Observer Pattern, the subject directly communicates with its observers. The Mediator acts as a hub for communication, whereas the Observer involves direct notifications between subject and observers.

#### 25. **When would you use the Command Pattern?**
   **Answer:** Use the Command Pattern when you need to decouple the sender and receiver of a request, support undo/redo functionality, or handle requests as objects (such as in GUI actions or transactional operations).

#### 26. **How does the Command Pattern help in implementing undo/redo functionality?**
   **Answer:** By encapsulating requests as objects, it allows storing these objects and undoing or redoing actions by re-executing or reversing the commands.

#### 27. **What is a key advantage of using the Iterator Pattern?**
   **Answer:** It allows traversal of a collection without exposing its internal structure. It provides a uniform interface for iterating over different types of collections.

#### 28. **What is the benefit of using the Memento Pattern in managing state?**
   **Answer:** It allows saving and restoring an object's state without exposing its internal details, thus supporting scenarios like undo functionality and state recovery.

#### 29. **How does the Chain of Responsibility Pattern improve code maintainability?**
   **Answer:** By allowing the responsibility of handling requests to be divided into multiple classes, it enhances maintainability, as new handlers can be added without altering existing code.

#### 30. **What are the key benefits of using the Observer Pattern in event-driven systems?**
   **Answer:** The Observer Pattern simplifies handling events and updates across multiple objects by decoupling the source of the event (subject) from the listeners (observers). It ensures that observers are automatically updated when the subject's state changes.