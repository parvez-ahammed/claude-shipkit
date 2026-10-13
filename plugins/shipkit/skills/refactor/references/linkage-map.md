# Master linkage map

Symptom -> candidate refactoring(s) and/or candidate pattern(s) -> spoke that scores it.
The orchestrator uses this to decide which spoke to invoke for a given symptom. The pattern
spoke always has final say via its scored preconditions; this table only nominates
candidates.

Covers all 22 smells `find-code-smells` triages (plus Incomplete Library Class), and every
refactoring named here has an entry in `apply-refactoring`. Keep the three in sync: if a
smell or refactoring is added to a spoke, add its row here.

## Smell -> refactoring (fix the smell directly)

| Smell | Group | Candidate refactorings | Spoke |
|-------|-------|------------------------|-------|
| Long Method | Bloater | Extract Method, Replace Temp with Query, Decompose Conditional | apply-refactoring |
| Large Class | Bloater | Extract Class, Extract Subclass, Extract Interface | apply-refactoring |
| Long Parameter List | Bloater | Introduce Parameter Object, Preserve Whole Object, Replace Parameter with Method Call | apply-refactoring (+ behavioral if flag params select behavior: Strategy) |
| Primitive Obsession | Bloater | Replace Primitive with Object, Replace Type Code with Subclasses / State/Strategy | apply-refactoring (+ behavioral when the type code drives behavior) |
| Data Clumps | Bloater | Extract Class, Introduce Parameter Object | apply-refactoring |
| Switch Statements | OO-Abuser | Replace Conditional with Polymorphism, Replace Type Code with Subclasses | apply-refactoring + creational/behavioral |
| Temporary Field | OO-Abuser | Extract Class, Introduce Null Object | apply-refactoring |
| Refused Bequest | OO-Abuser | Replace Inheritance with Delegation, Push Down Method/Field | apply-refactoring |
| Alternative Classes with Different Interfaces | OO-Abuser | Rename Method, Move Method, Extract Superclass | apply-refactoring (+ structural: Adapter if a class cannot be changed) |
| Divergent Change | Change-Preventer | Extract Class (split responsibilities) | apply-refactoring |
| Shotgun Surgery | Change-Preventer | Move Method/Field, Inline Class | apply-refactoring |
| Parallel Inheritance Hierarchies | Change-Preventer | Move Method/Field to collapse hierarchies | apply-refactoring |
| Comments (as deodorant) | Dispensable | Extract Method, Rename, Introduce Assertion | apply-refactoring |
| Duplicate Code | Dispensable | Extract Method, Pull Up Method, Form Template Method | apply-refactoring + behavioral |
| Dead Code | Dispensable | Delete; Inline | apply-refactoring |
| Speculative Generality | Dispensable | Collapse Hierarchy, Inline Class, Remove Parameter | apply-refactoring |
| Lazy Class | Dispensable | Inline Class, Collapse Hierarchy | apply-refactoring |
| Data Class | Dispensable | Encapsulate Field/Collection, Remove Setting Method, Move Method | apply-refactoring |
| Feature Envy | Coupler | Move Method, Extract Method | apply-refactoring |
| Inappropriate Intimacy | Coupler | Move Method/Field, Change Bidirectional Association to Unidirectional, Replace Inheritance with Delegation | apply-refactoring |
| Message Chains | Coupler | Hide Delegate, Extract Method | apply-refactoring + structural (Facade) |
| Middle Man | Coupler | Remove Middle Man, Inline Method, Replace Delegation with Inheritance | apply-refactoring (+ structural if the indirection is a deliberate Proxy/Decorator/Facade) |
| Incomplete Library Class | Other | Introduce Foreign Method / Local Extension | apply-refactoring (+ structural: Adapter) |

## Symptom -> candidate design pattern (when refactoring alone is not enough)

### Creational (object construction)

| Symptom | Candidate pattern | Spoke |
|---------|-------------------|-------|
| constructor arity high / telescoping ctors / many optional params | Builder | pick-creational-pattern |
| `new ConcreteClass` scattered; subclass decides which type to make | Factory Method | pick-creational-pattern |
| families of related objects must be created together and stay consistent | Abstract Factory | pick-creational-pattern |
| object expensive to build, needs many similar copies | Prototype | pick-creational-pattern |
| hand-rolled global single instance | Singleton (often anti; verify) | pick-creational-pattern |

### Structural (object composition / interfaces)

| Symptom | Candidate pattern | Spoke |
|---------|-------------------|-------|
| incompatible interface needs to fit an expected one | Adapter | pick-structural-pattern |
| abstraction and implementation vary independently (combinatorial subclasses) | Bridge | pick-structural-pattern |
| tree of part-whole objects treated uniformly | Composite | pick-structural-pattern |
| add responsibilities to one object at runtime without subclass explosion | Decorator | pick-structural-pattern |
| complex subsystem needs a simple entry point; Message Chains | Facade | pick-structural-pattern |
| many similar objects blow memory; shared intrinsic state | Flyweight | pick-structural-pattern |
| need to control / defer / guard access to an object | Proxy | pick-structural-pattern |

### Behavioral (object interaction / responsibility)

| Symptom | Candidate pattern | Spoke |
|---------|-------------------|-------|
| switch/if selecting interchangeable algorithms at runtime | Strategy | pick-behavioral-pattern |
| object behavior changes with internal state; state-machine if/switch | State | pick-behavioral-pattern |
| request needs to be queued, logged, undone, parameterized | Command | pick-behavioral-pattern |
| one-to-many change notification / event subscription | Observer | pick-behavioral-pattern |
| request handled by one of a chain of handlers | Chain of Responsibility | pick-behavioral-pattern |
| algorithm skeleton fixed, steps vary by subclass; Duplicate Code in siblings | Template Method | pick-behavioral-pattern |
| operation added across a class hierarchy without editing the classes | Visitor | pick-behavioral-pattern |
| many objects communicate in a tangle (n-to-n) | Mediator | pick-behavioral-pattern |
| traverse a collection without exposing its structure | Iterator | pick-behavioral-pattern |
| capture and restore object state (undo) | Memento | pick-behavioral-pattern |

## Disambiguation notes

- Strategy vs State: Strategy's variants are chosen by the client and rarely change each
  other; State's variants drive transitions between themselves. Distinguishing precondition:
  "do the variants decide the next variant?" yes -> State, no -> Strategy.
- Factory Method vs Abstract Factory: one product vs a family of related products.
- Decorator vs Proxy: Decorator adds behavior; Proxy controls access. Same shape, different
  intent.
- Adapter vs Facade: Adapter makes one interface fit a required one; Facade simplifies a
  whole subsystem.
- Template Method vs Strategy: inheritance with a fixed skeleton vs composition with injected
  variation; runtime choice or unwanted inheritance -> Strategy.
- Chain of Responsibility vs Decorator: may a link end the request? yes -> Chain.
- Observer vs Mediator: one-to-many notification vs centralized many-to-many coordination.
