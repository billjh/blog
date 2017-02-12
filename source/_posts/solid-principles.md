---
title: SOLID Principles
date: 2017-2-9
tags:
- programming
- oop
- design pattern
---

A refresher of the five basic principles behind maintainable and extensible object-oriented programs.

<!-- more -->

### SOLID

SOLID principles are something you must heard of when you take any programming classes that teach you objects and classes. They are abstract but highly valuable, because they are summarized from the best practices in the industry.

Violation of these principles in your work is an indication of having code smells and may results in un-maintainability of your codebase. Now, let's roughly go through the SOLID principles one-by-one.

### Single Responsibility Principle
A class should have a single responsibility (i.e. only one reason to change). If your classes have too many responsibility, consider to break them apart into different classes with clearer intentions.

### Open-closed Principle
Software entities should be open for extension, but closed for modification. In other words, a class can allow its behavior to be extended without modification to its code, usually through _inheritance_.

### Liskov Substitution Principle
This principle states that an object of subtype can _substitute_ its supertype objects. It defines a _strong behavior subtyping_ relation, which is useful when reasoning about the design of class hierarchies. In details, it imposes some requirements on subtype signatures:
- Contravariance of _method arguments_ in the subtype.
- Covariance of _return types_ in the subtype.
- _Exceptions_ thrown by subtype methods must be subtypes of the exceptions thrown by supertype methods.

It also leads to restrictions on subtype behavior conditions:
- _Preconditions_ cannot be strengthened in a subtype.
- _Postconditions_ cannot be weakened in a subtype.
- _Invariants_ of the supertype must be preserved in a subtype.
- History constraint. Subtype should not introduce methods that may mutate the states in which are immutable in supertype.

### Interface Segregation Principle
Many client-specific interfaces are better than one general-purpose interface. This is encouraged to keep systems decoupled and easier to refactor and change.

### Dependency Inversion Principle
One should "depend upon abstraction, [not] concretion". The dependency inversion principle states that: high-level modules should not depend on concrete implementation of low-level modules; instead, interactions between high-level modules and low-level modules are treated as abstractions, which leads to decoupling.

In practices of implementing such pattern, a high-level module defines interfaces of the needed interactions from its lower-level modules. And low-level modules implement those interfaces. It is often used with techniques/patterns like dependency injection.


_References:_
1. <https://en.wikipedia.org/wiki/SOLID_(object-oriented_design)>
2. https://en.wikipedia.org/wiki/Single_responsibility_principle
3. https://en.wikipedia.org/wiki/Open/closed_principle
4. https://en.wikipedia.org/wiki/Liskov_substitution_principle
5. https://en.wikipedia.org/wiki/Interface_segregation_principle
6. https://en.wikipedia.org/wiki/Dependency_inversion_principle
