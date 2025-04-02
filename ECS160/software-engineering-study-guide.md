# Software Engineering Final Exam Study Guide

## 1. Object-Oriented Concepts

### Core Principles
- **Encapsulation**: Bundling data and methods that operate on that data within a single unit (class)
  - Example: A `BankAccount` class encapsulates balance data and withdrawal/deposit methods
  - Benefits: Information hiding, controlled access to data

- **Inheritance**: Mechanism where a new class inherits properties and behaviors from an existing class
  - Example: `SavingsAccount` inherits from `BankAccount`
  - Types: Single, multiple (in some languages), multilevel, hierarchical

- **Polymorphism**: Ability to present the same interface for different underlying forms
  - **Method Overriding**: Redefining a method in a subclass
  - **Method Overloading**: Multiple methods with the same name but different parameters
  - Example: `shape.draw()` behavior changing based on whether shape is a Circle or Square

- **Abstraction**: Simplifying complex systems by modeling classes based on essential properties
  - Abstract classes and interfaces
  - Example: `Vehicle` as an abstract base class for `Car` and `Motorcycle`

### OO Design Concepts
- **Classes vs Objects**: Classes are templates/blueprints; objects are instances
- **Constructors and Destructors**: Special methods for initialization and cleanup
- **Access Modifiers**: public, private, protected, package/internal
- **Static vs Instance Members**: Class-level vs object-level elements
- **Composition vs Inheritance**: "has-a" vs "is-a" relationships
- **Interfaces and Abstract Classes**: Contracts for implementation

### Advanced OO Concepts
- **SOLID Principles**:
  - Single Responsibility Principle
  - Open/Closed Principle
  - Liskov Substitution Principle
  - Interface Segregation Principle
  - Dependency Inversion Principle
- **Association, Aggregation, and Composition**: Different relationships between objects

## 2. Reflective Programming

### Basic Concepts
- **Definition**: Ability of a program to examine and modify its structure and behavior at runtime
- **Introspection vs Intercession**: Examining vs modifying program elements

### Reflection Mechanisms
- **Type Introspection**: Examining metadata about classes, interfaces, fields, and methods
- **Dynamic Class Loading**: Loading classes at runtime
- **Method Discovery and Invocation**: Finding and calling methods dynamically
- **Field Access**: Reading and writing to fields dynamically
- **Annotation Processing**: Reading annotations at runtime

### Dynamic Proxies
- **Definition**: Objects that intercept method calls to another object
- **Use Cases**:
  - Aspect-oriented programming
  - Remote method invocation
  - Lazy loading
  - Logging/monitoring
  - Transaction management
- **Implementation**: Using language-specific proxy mechanisms (e.g., Java's `Proxy` class)

### Advantages and Disadvantages
- **Advantages**: Flexibility, extensibility, reduced code duplication
- **Disadvantages**: Performance overhead, type safety concerns, complexity

### Examples in Different Languages
- **Java**: java.lang.reflect package, java.lang.Class
- **C#**: System.Reflection namespace
- **Python**: inspect module, getattr/setattr functions
- **JavaScript**: Reflect API, Proxy objects

## 3. Software Architecture

### HTTP Methods
- **GET**: Retrieve data, idempotent
- **POST**: Create new resources, not idempotent
- **PUT**: Update/replace resources, idempotent
- **DELETE**: Remove resources, idempotent
- **PATCH**: Partial updates, not always idempotent
- **HEAD**: Like GET but returns only headers, no body
- **OPTIONS**: Describes communication options for a resource

### Architectural Styles
- **Monolithic Architecture**:
  - Single, unified codebase
  - Pros: Simplicity, easier testing, deployment
  - Cons: Scalability issues, technology lock-in, larger deployment units

- **Microservice Architecture**:
  - Independently deployable, loosely coupled services
  - Pros: Independent scaling, technology diversity, fault isolation
  - Cons: Distributed system complexity, inter-service communication overhead
  - Patterns: API Gateway, Service Discovery, Circuit Breaker

### Communication Patterns
- **RPC (Remote Procedure Call)**:
  - Makes remote procedure calls appear local
  - Examples: gRPC, XML-RPC, Java RMI
  - Characteristics: Synchronous, often uses IDL (Interface Definition Language)

- **Message Queues (MQ)**:
  - Asynchronous message passing
  - Examples: RabbitMQ, Apache Kafka, ActiveMQ
  - Patterns: Point-to-point, request-reply, competing consumers

- **Publish/Subscribe (Pub/Sub)**:
  - Publishers send messages without knowledge of subscribers
  - Subscribers express interest in topics/events
  - Examples: Kafka topics, MQTT, Redis pub/sub
  - Characteristics: Decoupling, scalability, event-driven design

### Architectural Patterns
- **Layered Architecture**: Presentation, business logic, data access
- **Client-Server**: Split functionality between consumers and providers
- **MVC/MVVM/MVP**: Separating concerns in user interfaces
- **Event-Driven Architecture**: Systems react to events
- **Serverless Architecture**: Function-as-a-Service (FaaS)

## 4. Design Patterns

### Creational Patterns
1. **Singleton**: Ensures a class has only one instance
   - Example: Database connection pool, logger
   
2. **Factory Method**: Creates objects without specifying exact class
   - Example: UI element creation based on configuration

3. **Abstract Factory**: Creates families of related objects
   - Example: Creating UI elements for different platforms

4. **Builder**: Separates object construction from its representation
   - Example: StringBuilder, complex object construction

5. **Prototype**: Creates objects by cloning an existing object
   - Example: Cloning a fully configured object as a starting template

### Structural Patterns
6. **Adapter**: Allows incompatible interfaces to work together
   - Example: Legacy system integration

7. **Decorator**: Adds behavior to objects dynamically
   - Example: Adding buffering to I/O streams

8. **Composite**: Treats individual objects and compositions uniformly
   - Example: File systems (files and directories)

9. **Proxy**: Provides a substitute or placeholder for another object
   - Example: Virtual proxies, protection proxies, remote proxies

### Behavioral Patterns
10. **Observer**: Defines one-to-many dependency between objects
    - Example: Event handling systems, MVC (Model-View-Controller)

11. **Strategy**: Defines a family of algorithms and makes them interchangeable
    - Example: Different sorting algorithms, payment methods

12. **Command**: Encapsulates a request as an object
    - Example: Undo/redo functionality, job queues

### Additional Patterns (Beyond the 12 Core Patterns)
- **Dependency Injection**: Inversion of control for better testability
- **Repository**: Abstraction layer between domain and data mapping layers
- **Unit of Work**: Maintains list of objects affected by a transaction

## 5. Software Security

### Spatial and Temporal Safety
- **Spatial Safety**: Protection against spatial memory errors
  - Buffer overflows
  - Out-of-bounds array access
  - Null pointer dereferences
  - Techniques: Bounds checking, memory protection

- **Temporal Safety**: Protection against timing-related memory errors
  - Use-after-free
  - Double-free
  - Dangling pointer issues
  - Techniques: Garbage collection, ownership systems

### Smart Pointers
- **Types and Implementation**:
  - **Unique pointers**: Exclusive ownership (C++: `std::unique_ptr`)
  - **Shared pointers**: Reference counting (C++: `std::shared_ptr`)
  - **Weak pointers**: Non-owning references to shared objects (C++: `std::weak_ptr`)

- **Benefits**:
  - Automatic memory management
  - Prevention of memory leaks
  - Clear ownership semantics
  - Exception safety

### Garbage Collection (GC)
- **Approaches**:
  - **Mark-and-sweep**: Marks reachable objects, sweeps unmarked
  - **Reference counting**: Counts references to objects
  - **Generational**: Divides heap into generations (young/old)
  - **Concurrent**: Runs alongside program execution

- **Considerations**:
  - Memory overhead
  - CPU overhead
  - Pause times
  - Deterministic vs non-deterministic finalization

### Other Security Concepts
- **Input Validation and Sanitization**: Preventing injection attacks
- **Authentication and Authorization**: Verifying identity and permissions
- **Secure Communication**: Encryption, TLS/SSL
- **Principle of Least Privilege**: Minimizing access rights
- **Secure Coding Practices**: Avoiding common vulnerabilities (OWASP Top 10)

## 6. Testing

### Unit Testing
- **Definition**: Testing individual components in isolation
- **Characteristics**:
  - Fast execution
  - Independent tests
  - Repeatable results
  - Self-validating
  - Timely creation

- **Frameworks**:
  - JUnit (Java)
  - NUnit/xUnit (C#)
  - pytest (Python)
  - Jest (JavaScript)

- **Best Practices**:
  - Arrange-Act-Assert pattern
  - Test naming conventions
  - Test coverage metrics
  - Small, focused tests

### Mock Testing
- **Definition**: Using substitutes for dependencies in unit tests

- **Types of Test Doubles**:
  - **Mocks**: Objects pre-programmed with expectations
  - **Stubs**: Provide canned answers to calls
  - **Fakes**: Working implementations but not suitable for production
  - **Spies**: Record calls for later verification
  - **Dummies**: Passed around but never used

- **Frameworks**:
  - Mockito (Java)
  - Moq (C#)
  - unittest.mock (Python)
  - Sinon.js (JavaScript)

- **Techniques**:
  - Dependency injection
  - Behavior verification
  - State verification

### Fuzz Testing
- **Definition**: Automated testing technique that feeds random/unexpected data

- **Types**:
  - **Generation-based**: Creates inputs from scratch
  - **Mutation-based**: Modifies existing valid inputs
  - **Evolutionary**: Uses feedback to guide mutations

- **Applications**:
  - File format parsing
  - Network protocol implementations
  - API interfaces
  - User input handling

- **Tools**:
  - American Fuzzy Lop (AFL)
  - LibFuzzer
  - LLVM's libFuzzer
  - Peach Fuzzer

### Testing Pyramid
- **Unit Tests**: Numerous, fast, isolated
- **Integration Tests**: Component interactions
- **System Tests**: End-to-end functionality
- **Acceptance Tests**: User requirements

### Test-Driven Development (TDD)
- Red (failing test) → Green (passing implementation) → Refactor
- Benefits: Better design, regression protection, documentation

## Additional Study Tips

- **Focus on Relationships**: Understand how these concepts interconnect
- **Code Examples**: Practice implementing key patterns and concepts
- **Real-World Applications**: Think about where you'd use specific patterns
- **Trade-offs**: Know the advantages and disadvantages of different approaches
- **Terminology**: Be familiar with the precise definitions and terms
