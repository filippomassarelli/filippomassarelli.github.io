---
layout: post
tags: [software-design, book, wip]
title: '[WIP] My notes on "Practical OOD" by Sandi Metz'
---

## Chapter 3. Managing Dependencies

### Writing loosely coupled code

#### Isolate Dependencies
Even before being DRY requires it, try to remove external dependencies and encapsulate them in methods of their own. Strive to have complex methods only depend to messages sent to _self_.

#### Remove argument-order dependencies
Use keyword arguments in the `initialize` method to avoid depending on positional arguments. This also allows us to add or remove initialization arguments and defaults to the class definition without worrying about side effects. The added verbosity is paid off by removing the risk that future changes will cascade into dependents.
If you depend on a class that has ordered arguments and cannot change it (eg. you don't own it) then wrap its initialization with a single method that makes it require keyword arguments.

### Managing Dependency Direction

#### Choosing Dependency Direction
Classes vary in their likelihood of change, their level of abstraction, and their number of dependents. When _likelihood of change_ intersects with _number of dependents_ your present design decisions will have future repercussions that can be healthy or deadly for your application.
<figure>
 <img src="https://www.informit.com/content/images/chap3_9780321721334/elementLinks/03fig02.jpg" />
 <figcaption>Likelihood of change versus number of dependents - p.57</figcaption>
</figure> 

## Chapter 4. Creating Flexible Interfaces

### Understanding interfaces

### Defining Interfaces

#### Public Interfaces
Public interfaces are made of public methods that are expected to be invoked by others and reveal the primary responsibility of the class. They are well documented in the tests and should not change often.

#### Private Interfaces
Methods that  handle implementation details are not expected to be sent by other objects, they are free to change and thus are not safe to depend on.

#### Responsibilities, Dependencies, and Interfaces
When you divide your class' methods into public and private you are telling users of your class what the class responsibility is and which methods they can safely depend on.

### Finding the Public Interface

#### Constructing an Intention
When adding code to an existing codebase you are extending a design. When starting a new application however you are creating a design that will be exestended by others in the future. You should start with tests first but it is hard to write your first test without a clear idea of what you want to test. Start with a _use case_, the main classes are usually easy to spot because they form the nouns in the sentence describing the use case. Be wary not to construct your design around these domain objects because you will fall into the trap of coercing behaviour into them. Focus instead on the messages that need to pass between them as they will reveal objects that are just as necessary but far less obvious.

#### Sequence Diagrams
Before you start typing, you should form an intention about the objects _and_ the messages needed to satisfy this use case - sequence diagrams have been thought to aid in this process. They use Unified Model Language (UML) which is great for communicating object-oriented design. They're a lightweight way to acquire an intention about an interaction as they capture objects and the messages passing between them.

<figure>
 <img src="https://www.informit.com/content/images/rcch04_9780321721334/elementLinks/th04fig03.jpg" />
 <figcaption>Moe talks to trip and bicycle - p.69</figcaption>
</figure>

Should `Trip` be responsible for finding a bicycle? The value of the sequence diagram becomes apparent as it exposes the public interfaces of the objects, making it easier to identify if a receiver should be responsible for responding to a given message or not.

<figure>
 <img src="https://www.informit.com/content/images/rcch04_9780321721334/elementLinks/th04fig04.jpg" />
 <figcaption>Moe talks to trip and bicycle - p.69</figcaption>
</figure>

Sequence diagrams are conducive to focussing on messages. Instead of deciding on a class and then figuring out its responsibilities, you are now deciding on a message and figuring our where to send it.
Notice however that the resulting design brings a new set of issues: `Customer` now knows too much of how other objects should collaborate to provide a suitable trip. It is binding itself to an implementation that might change.

#### Asking for "What" instead of telling "How"
This allows for senders to relinquish responsibility to the receivers. Instead of directing all the needed actions, the senders can abstract away and ask for _what_ they want. The _how_ is doen will be orchestrated by the receiver's internals. This way, the public interface of the receiver becomes smaller and safer for senders to depend on because it is now decoupled from the implementation. 

#### Seeking context independence 

# Quotes
> "Verbosity exists at the intersection between the needs of the present and the uncertainty of the future" - p.50

> "Depend on things that change less often than you do" - p.55

> "Classes implement methods; some of those methods are intended to be used by others, and these methods make up its public interface." - p.63

> "Public methods should read like a description of responsibilities of the class" - p.64

> "You don't send messages because you have objects, you have objects because you send messages." - p.69

# Principles
## Dependency Injection
## Single Responsibility

