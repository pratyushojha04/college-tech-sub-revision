# Structural Design Patterns
Structural design patterns deal with object composition and aim to simplify the structure of complex systems by identifying relationships between entities.

---

## **Part I: Adapter, Bridge, Composite**

### 1. **Adapter Pattern**
- **Purpose**: Converts the interface of a class into another interface clients expect. It allows incompatible interfaces to work together.
- **Types**:
  1. **Class Adapter**: Uses inheritance to adapt one class to another.
  2. **Object Adapter**: Uses composition to delegate requests to an adaptee object.
- **Participants**:
  - **Target**: Defines the domain-specific interface used by the client.
  - **Adaptee**: An existing class with an incompatible interface.
  - **Adapter**: Bridges the target and adaptee.
- **Example**:
  - **Real-World**: A power adapter converts a plug to fit into a different type of socket.
  - **Code**: Adapting a legacy API to work with a modern application.

#### **Code Example (Object Adapter in Python):**
```python
class Adaptee:
    def specific_request(self):
        return "Adaptee's specific request."

class Target:
    def request(self):
        pass

class Adapter(Target):
    def __init__(self, adaptee):
        self.adaptee = adaptee

    def request(self):
        return self.adaptee.specific_request()

adaptee = Adaptee()
adapter = Adapter(adaptee)
print(adapter.request())
```

---

### 2. **Bridge Pattern**
- **Purpose**: Decouples an abstraction from its implementation, allowing the two to vary independently.
- **Key Concepts**:
  - **Abstraction**: Defines the interface and maintains a reference to an implementer.
  - **Implementor**: Defines the interface for implementation classes.
  - **Concrete Implementor**: Provides specific implementations of the implementor interface.
- **Advantages**:
  - Improves extensibility.
  - Hides implementation details from the client.
- **Example**:
  - **Real-World**: A remote control (abstraction) operates different types of devices (implementor).
  - **Code**: Separating UI components from back-end logic.

#### **Code Example:**
```python
class Implementor:
    def operation(self):
        pass

class ConcreteImplementorA(Implementor):
    def operation(self):
        return "ConcreteImplementorA operation."

class ConcreteImplementorB(Implementor):
    def operation(self):
        return "ConcreteImplementorB operation."

class Abstraction:
    def __init__(self, implementor):
        self.implementor = implementor

    def operation(self):
        return self.implementor.operation()

implementor_a = ConcreteImplementorA()
abstraction = Abstraction(implementor_a)
print(abstraction.operation())
```

---

### 3. **Composite Pattern**
- **Purpose**: Composes objects into tree structures to represent part-whole hierarchies. Allows clients to treat individual objects and compositions uniformly.
- **Participants**:
  - **Component**: Declares the interface for objects in the composition.
  - **Leaf**: Represents leaf objects with no children.
  - **Composite**: Stores child components and implements child-related operations.
- **Example**:
  - **Real-World**: A file system where folders contain files or other folders.
  - **Code**: Building a menu system in a graphical user interface.

#### **Code Example:**
```python
class Component:
    def operation(self):
        pass

class Leaf(Component):
    def __init__(self, name):
        self.name = name

    def operation(self):
        return self.name

class Composite(Component):
    def __init__(self):
        self.children = []

    def add(self, component):
        self.children.append(component)

    def operation(self):
        results = [child.operation() for child in self.children]
        return ", ".join(results)

leaf1 = Leaf("Leaf 1")
leaf2 = Leaf("Leaf 2")
composite = Composite()
composite.add(leaf1)
composite.add(leaf2)
print(composite.operation())
```

---

## **Part II: Decorator, Façade, Flyweight, Proxy**

### 1. **Decorator Pattern**
- **Purpose**: Adds new responsibilities to objects dynamically without modifying their structure.
- **Key Concepts**:
  - **Component**: Defines the interface for objects.
  - **Concrete Component**: The base object being decorated.
  - **Decorator**: Wraps the component and adds responsibilities.
- **Example**:
  - **Real-World**: Adding layers to a cake.
  - **Code**: Adding features to a graphical UI element.

#### **Code Example:**
```python
class Component:
    def operation(self):
        pass

class ConcreteComponent(Component):
    def operation(self):
        return "Concrete Component"

class Decorator(Component):
    def __init__(self, component):
        self.component = component

    def operation(self):
        return self.component.operation()

class ConcreteDecoratorA(Decorator):
    def operation(self):
        return f"ConcreteDecoratorA({self.component.operation()})"

component = ConcreteComponent()
decorator = ConcreteDecoratorA(component)
print(decorator.operation())
```

---

### 2. **Façade Pattern**
- **Purpose**: Provides a simplified interface to a larger body of code.
- **Key Concepts**:
  - **Facade**: Simplifies access to the subsystem.
  - **Subsystem Classes**: Implement the complex subsystem logic.
- **Example**:
  - **Real-World**: A universal remote control simplifies operating multiple devices.
  - **Code**: A single API endpoint aggregating multiple back-end services.

#### **Code Example:**
```python
class SubsystemA:
    def operation_a(self):
        return "Subsystem A operation"

class SubsystemB:
    def operation_b(self):
        return "Subsystem B operation"

class Facade:
    def __init__(self):
        self.subsystem_a = SubsystemA()
        self.subsystem_b = SubsystemB()

    def operation(self):
        return f"{self.subsystem_a.operation_a()} + {self.subsystem_b.operation_b()}"

facade = Facade()
print(facade.operation())
```

---

### 3. **Flyweight Pattern**
- **Purpose**: Reduces memory usage by sharing objects instead of creating new ones for every request.
- **Key Concepts**:
  - **Intrinsic State**: Shared among objects.
  - **Extrinsic State**: Context-specific and not shared.
- **Example**:
  - **Real-World**: Reusing character glyphs in a text editor.
  - **Code**: Caching database connections or objects.

#### **Code Example:**
```python
class Flyweight:
    def __init__(self, shared_state):
        self.shared_state = shared_state

    def operation(self, unique_state):
        return f"Shared: {self.shared_state}, Unique: {unique_state}"

class FlyweightFactory:
    def __init__(self):
        self.flyweights = {}

    def get_flyweight(self, shared_state):
        if shared_state not in self.flyweights:
            self.flyweights[shared_state] = Flyweight(shared_state)
        return self.flyweights[shared_state]

factory = FlyweightFactory()
flyweight = factory.get_flyweight("State A")
print(flyweight.operation("Unique 1"))
```

---

### 4. **Proxy Pattern**
- **Purpose**: Provides a placeholder for another object to control access, add functionality, or reduce overhead.
- **Types**:
  - **Virtual Proxy**: Defers object creation.
  - **Remote Proxy**: Represents objects in remote systems.
  - **Protection Proxy**: Controls access based on permissions.
- **Example**:
  - **Real-World**: A bank ATM acting as a proxy to a bank server.
  - **Code**: Lazy-loading objects or restricting access.

#### **Code Example:**
```python
class RealSubject:
    def request(self):
        return "RealSubject request."

class Proxy:
    def __init__(self):
        self.real_subject = RealSubject()

    def request(self):
        return f"Proxy: {self.real_subject.request()}"

proxy = Proxy()
print(proxy.request())
```

---

## 50 Important Interview Questions with Answers

### Adapter Pattern
1. **What is the main purpose of the Adapter pattern?**
   - It allows incompatible interfaces to work together by converting one interface to another expected by the client.

2. **How does the Adapter pattern differ from the Façade pattern?**
   - Adapter focuses on converting interfaces, while Façade simplifies and unifies complex subsystems.

3. **What are the two types of Adapters?**
   - Class Adapter (uses inheritance) and Object Adapter (uses composition).

4. **What is an example use case for the Adapter pattern?**
   - Integrating legacy APIs with a modern system.

5. **Can Adapter pattern be used to implement multiple inheritance?**
   - Yes, in some programming languages it can simulate multiple inheritance.

### Bridge Pattern
6. **What problem does the Bridge pattern solve?**
   - It decouples abstraction from implementation, allowing both to vary independently.

7. **What are the key participants in the Bridge pattern?**
   - Abstraction, Implementor, Refined Abstraction, Concrete Implementor.

8. **Give a real-world example of the Bridge pattern.**
   - A remote control (abstraction) that can operate TVs or sound systems (implementors).

9. **How does the Bridge pattern improve extensibility?**
   - By separating abstraction and implementation, you can extend them independently.

10. **What is the difference between the Bridge and Adapter patterns?**
    - Bridge decouples abstraction from implementation, while Adapter makes two incompatible interfaces work together.

### Composite Pattern
11. **What is the Composite pattern used for?**
    - To represent part-whole hierarchies and allow clients to treat individual objects and composites uniformly.

12. **What are the main participants of the Composite pattern?**
    - Component, Leaf, and Composite.

13. **What is an example use case of the Composite pattern?**
    - A file system where folders (composites) contain files or other folders.

14. **How does the Composite pattern handle recursion?**
    - The Composite object can contain other Composite objects or Leaf nodes, enabling recursive structures.

15. **How does the Composite pattern support Open/Closed Principle?**
    - By allowing new types of components without modifying existing code.

### Decorator Pattern
16. **What is the main purpose of the Decorator pattern?**
    - To dynamically add new behavior or responsibilities to an object without modifying its structure.

17. **How does the Decorator pattern support single responsibility principle?**
    - By allowing responsibilities to be divided into smaller, reusable classes.

18. **What are the main participants of the Decorator pattern?**
    - Component, Concrete Component, Decorator, Concrete Decorator.

19. **Give a real-world example of the Decorator pattern.**
    - Adding toppings to a pizza order.

20. **Can Decorators be chained together?**
    - Yes, multiple decorators can be applied in succession.

### Façade Pattern
21. **What is the main purpose of the Façade pattern?**
    - To provide a simplified interface to a complex subsystem.

22. **How does the Façade pattern promote loose coupling?**

