---
title: "iOS Questions"
date: 2018-12-19T19:12:00+11:00
draft: false
ads: true
categories:
  - Article
tags:
  - ios
  - programming
  - development
  - interview
  - swift
---

# iOS Questions

**How could you setup live rendering?**

The attribute `@IBDesignable` lets Interface Builder perform live updates on a particular view.

**Difference between synchronous and asynchronous tasks?**

- _synchronous_: blocks running thread until the task has completed
- _asynchronous_: completes a task in background and can notify you when complete

**What are B-Trees?**

B-trees are search trees that provide an ordered key-value store with excellent performance characteristics. In principle, each node maintains a sorted array of its own elements, and another array for its children.

**What is made up of `NSError` object?**

A string domain, an integer code, and a user infor dictionary. Domain identifies what categories of errors this error is coming from.

**What is `enum`?**

Enumeration is a type that basically contains a group of related values in same umbrella.

**What is bounding box?**

Bounding box is a geometry term, it refers to the smallest area that can contain a given set of points.

**Why don't we use `strong` for `enum` property in ObjC?**

`enum` is like a primitive value type which isn't object, so we don't specify `strong` or `weak`.

**What is `@synthesize` in ObjC?**

Synthesize generates getter and setter methods for property, there are a few special cases:

- readwrite property with custom getter and setter
  when providing **both** a getter and setter custom implementation, the property won't be automatically synthesized
- readonly property with custom getter
  when providing a custom getter for a readonly property, this won't be automatically synthesized
- `@dynamic`
  when using `@dynamic propertyName`, the property won't be automatically synthesized
- properties declared in a `@protocol`
  when comforming to a protocol, any property the protocol defines won't be automatically synthesized
- properties declared in a category
- overriden property
  when you override a property of a superclass, you must explicitly synthesize it

**What is `@dynamic` in ObjC?**

`@dynamic` tells the compiler that getter and setter are implemented somewhere else, examples like subclasses of `NSManagedObject`.

**Why do we use `synchronized`?**

`synchronized` guarantees that only one thread can be executing the code in the block at any given time.

**What is the difference between `strong`, `weak`, `readonly` and `copy`?**

- `strong`: the reference count will be increased and the reference to it will be maintained through the life of the object
- `weak`: referencing an object without increasing its reference count, often used when creating a parent child relationship
- `readonly`: a property can be read but not modified
- `copy`: the value of an object is copied when it's assigned, prevents its value from being modified

**What is Dynamic Dispatch?**

Dynamic Dispatch is the process of selecting which implementation
of a polymorphic operation that’s a method or a function to call at run time. This means, that when we wanna invoke our methods like object method. but Swift does not default to dynamic dispatch.

**What is code coverage?**

Code coverage is a metric that helps us to measure the value of our unit tests.

**What is completion handler?**

It's a handler (usually block or closure) that handles the result when a task has finishes, and the task is often asynchronous.

**What is the difference between frame and bounds?**

They both specifies a rectangle in terms of origin and size, but frame is relative to a view's superview, and bounds is relative to its own coordination system.

**What is responder chain?**

A hierarchy of objects that have the opportunity to respond to events received.

**What is Regular Expression?**

RegEx specifies a pattern that describes a string, and can be used to check whether a string matches the pattern.

**What is operator overloading?**

Operator overloading is to define or change how existing operators behave with types.

**What is platform limitation of tvOS?**

1. no browser support, this means the app can't link out to a web browser for things like OAuth or social media sites
2. cannot explicitly use local storage
3. app bundle cannot exceeed 4GB

**What is ABI?**

ABI stands for Application Binary Interface, at runtime, Swift program binaries interact with other libraries through an ABI, it defines many low level details for binary. ABI stability means locking down the ABI to the point that future compiler versions can produce binaries conforming to the stable ABI. It enables binary compatibility between applications and libraries compiled with different Swift versions.

**Why is design pattern important?**

Design patterns are reusable solutions to common problems in software design. They are templates designed to help you write code that's easy to understand and reuse.

**What is Singleton pattern?**

The singleton pattern ensures that only one instance exists for a given class and that there's a global access point to that instance. It usually uses `lazy` loading to create the single instance when it's needed for the first time.

**What is Facade pattern?**

The facade pattern provides a single interface to a complex subsystem. Instead of exposing the user to a set of of classes and their APIs, only a simple unified API is exposed.

**What is Decorator pattern?**

The decorator pattern dynamically adds behaviours and responsibilities to an object without modifying its code. It's an alternative to subclassing where you modify a class's behavior by wrapping it with another object.

In ObjC two common implementations are Category and Delegation, in Swift there are Extension and Delegation.

**What is Adapter pattern?**

An adapter allows classes with incompatible interfaces to work together, it wraps itself around an object and expose a standard interface to interact with that object.

Protocol A makes request, B is a class that makes special request, adapter implements A and wraps a B instance, its request implementation would call B's special request.

**What is Observer pattern?**

In observer pattern, one object notifies other objects of any state changes.

**What is Memento pattern?**

In memento pattern, your stuff is saved somewhere external, later when needed, externalised state can be restored without violating encapsulation.
???

**What is VIPER?**

View, Interactor, Presenter, Entity and Router:

- `View`: UI related operations, similar to View in MVVM, user interaction is passed to Presenter.
- `Presenter`: similar to ViewModel in MVVM, responds to user interaction, if needed, it asks Interactor to alter data model
- `Router`: Screen transition and UI components switching
- `Interactor`: Data processing, including network request, data storage and modelling
- `Entity`: Data model

**What is JSON/PLIST limits?**

Can't use complex query to filter, can be slow, need serialize or deserialize it when used, not thread-safe.

**What is SQLite limits?**

Need to define schemas of tables and relations between tables, have to manually write query to fetch data, and need to map result to model

**How many APIs are there for battery-efficient location tracking?**

- significant location changes
- region monitoring
- visit events

**Swift's main advantage?**

- optionals makes application crash-resistant
- built-in error handling
- closures
- faster compared to other languages
- type-safe
- pattern matching

**What is Generics?**

Generics create code that does not need to specify underlying data type, so the code can be flexible and re-usable, code can be written without duplication and can express its intent in a clear and more abstract manner.

**Explain `lazy` in Swift?**

The initial value of a `lazy` property is calculated only when its called for the first time.

**What is `defer` in Swift?**

`defer` specifies a block of code that will be executed when execution is leaving current scope.

**How to pass data between view controllers?**

1. Segue: in `prepareForSegue` method
2. Delegate
3. Setting variable directly

**What is concurrency?**

Concurrency is dividing up the execution paths of the program so that they can possibly run at the same time

**What is GCD?**

GCD is a library that provides a low-level and object-based API to run tasks concurrently while managing threads behind the scenes.

- **Dispatch Queues**: responsible for executing tasks in first-in, first-out order
- **Serial Dispatch Queue**: runs tasks one at a time
- **Concurrent Dispatch Queue**: runs as many tasks as it can without waiting for the started tasks to finish
- **Main Dispatch Queue**: a globally available queue that executes tasks on main thread

**Explain `NSOperation`, `NSOperationQueue` and `NSBlockOperation`**

- `NSOperation`: adds a little extra overhead compared to GCD, but it can have dependency among various operations and support re-use, cancel or suspend.
- `NSOperationQueue`: it allows a pool of threads to be created and used to execute `NSOperation`s in parallel.
- `NSBlockOperation`: a concrete subclass of `NSOperation` that manages concurrent execution of one or more blocks, the operation is considered finished only when all blocks have finished executing.

**Explain KVC and KVO?**

**KVC**: Key-Value Coding, it's a mechanism that lets you access an object's properties using strings at runtime than having to know the property name at development time
**KVO**: Key-Value Observing, it let you be notified when properties of observed object changes value

**Explain Swift's pattern matching techniques?**

[Link][https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar_wildcard-pattern]

- **Tuple**:
- **Type-casting**:
- **Wildcard patterns**:
- **Optional patterns**:
- **Enumeration case patterns**:
- **Expression patterns**:

**What is `guard`?**

A `guard` statement is used to transfer program control out of a scope if one or more conditions aren’t met.

**Explain method swizzling in Swift?**

Swizzling can replace the implementation of a method with a different one at _runtime_, by changing the mapping between a specific `#selector(method)` and the function that contains its implementation. The class containing the method to be swizzled must extend `NSObject`, and the methods must have dynamic attributes.

**What is the difference between escaping and non-escaping closures?**

non-escaping closure is executed only inside the scope of a function, whereas escaping closures may be executed outside of the function's closure, for example, completion of asynchronous tasks.

**Explain `[weak self]` and `[unowned self]`**

`[weak self]` is used when `self` might be deallocated when the closure is executed, and you need to handle this; `[unowned self]` is used when it's certain that `self` won't be deallocated when closure is executed, it cannot be optional.

**Explain `#keyPath()`**

`class KeyPath<Root, Value>: PartialKeyPath<Root>`

a key path from a specific root type to a specific value type

**What are the major debugging improvements in XCode 8?**

- **View Debugger**: visualize UI layouts and see constraints at runtime, XCode 8 introduced warning for constraint conflicts
- **Thread Sanitizer**: runtime tool that alerts you to threading issues - potentially race conditions
- **Memory Graph Debugger**: visualization of memory graph at a point in time and flags leaks

**What is TDD?**

Test Driven Development

**What does `final` do in Swift?**

Prevents class or method from being overridden

**Differences between `open`, `public`?**

- **open** allows other modules to use and inherit the class, `open` class members can be used as well as overridden.
- **public** only allows other modules to use the public classes and members, public classes cannot be subclassed, nor public member can be overridden.

**Differences between `internal`, `fileprivate`, `private`, and `public private(set)`?**

- `internal` entities can be used within any source file from defining module but not in any file outside
- `file-private` entities can only be used by its own defining source file
- `private` entities can be used only by the enclosing declaration and extensions of the declaration **in the same file**
- default access level is `internal`

**What is BDD?**

Business Driven Development, test cases can be read by non-engineers

**What is forced unwrapping?**

When we define a variable as `optional`, then to gt the value from it, we will have to `unwrap` it, forced unwrap is putting an exclamation mark at the end of this variable.

**What is bitcode?**

Bitcode refers to the type of code: "LLVM Bitcode" that is sent to iTunes Connect, this allows Apple to use certain calculations to re-optimize apps further and this can be done without uploading a new build.

**Slicing** is the process of Apple optimizing app for user's specific device
**App Thinning** is the combination of slicing, bitcode and on-demand resources

**What is `inout`?**

`inout` parameter means if function modifies the parameter as local variable, the passed-in parameter is also modified, it's like `inout` means reference type whereas value type if without `inout`.

**What is nil coalescing?**

`nil-coalescing` operator unwraps an optional if it contains a value, or returns a default value on the right of the operator.

**What is ternary operator?**

Ternary operator operates on three targets, if the first expression is true, it evaluates and returns the value of second expression, otherwise it evaluates and returns the value of the third expression.

**What is subscripts?**

Subscripts are shortcuts for accessing the member elements of a collection, list or sequence, or a custom type that supports subscripts.

**What is DispatchGroup?**

`DispatchGroup` allows for aggregate synchronization of work, you can use them to submit multiple different work items and track when they all complete.

**Explain the difference between atomic and nonatomic synthesized properties?**

- `atomic`: ensures integrity of setter and getter, safer than `nonatomic`, any thread access atomic object can get the correct object
- `nonatomic`: doesn't ensure integrity of setter and getter, might be in problem state when different threads trying to access the same object, faster than `atomic`

**Explain polymorphism?**

Polymorphism allows the expression of some sort of contract, with potentially many types implementing the contract in different ways, each according to their own purpose.

**What are the major purposes of frameworks?**

- code encapsulation
- code modularity
- code reuse

**What is downcasting?**

- `as`: used for upcasting and type casting to bridged type
- `as?`: used for safe casting, returns `nil` if failed
- `as!`: used to force casting, crash if failed

**What are the `ReadingOptions` of `JSONSerialization`?**

- _mutableContainers_:
- _mutableLeaves_:
- _allowFragments_:

**What is the difference between `Any` and `AnyObject`?**

- _Any_ can represent an instance of any type at all, including function types and optional types
- _AnyObject_ can represent an instance of any class type

**What is `Hashable`?**

A type that can be hashed to produce an integer hash value.

The `Hashable` protocol inherits from `Equatable` protocol, which must also be satisfied.

- for `struct`: all its stored properties must conform to `Hashable`
- for `enum`: all its associated values must conform to `Hashable`, an `enum` without associated values has `Hashable` conformance even without declaration

**When do we use optional chaining vs `if let` or `guard`?**

Optional chaining is used when we do not really care if the operation fails.

**What is the difference between `nil` and `.None`?**

`nil` is just syntactic sugar for `.None`

**What is `availability` attributes?**

It indicates the code should only be called if running system meet the requirement specified.

```swift
@available(iOS 9.0, *)

@available(iOS, introduced: 9.0)
@available(OSX, introduced: 10.11)
// is replaced by
@available(iOS 9.0, OSX 10.11, *)

if #available(iOS 9.0, *) {}

```

**What is Protocol Extension?**

Protocol extension is to extend a protocol and provide default implementation of methods, so that classes that conform to the protocol will have the default implementation for free, and classes can selectively override protocol methods just like class inheritance. The benefit of this is that a class can have 0 or 1 superclass but protocols don't have such constraint.

**What is selector in Objc?**

Selectors are ObjC's internal representation of a method name.

**What is unwind segue?**

An unwide segue (sometimes called exit segue) can be used to navigate back through push, modal or popover segues, you can go back multiple steps in navigation hierarchy.

**What is the difference between iVar and `@property`**

iVar is like the backing variable for declared `@property`, `@synthesize`d property will have backing iVar created, `@property` is used with KVO, and can be overwritten with custom getter and setter.

**Why do we need to specify `self` to refer to a stored property or a method in asynchronous code?**

Since the code is dispatched to a background thread, we need to capture a reference to the correct object.

**What is intrinsic content size?**

The natural size for the receiving view, considering only properties of the view itself. Custom views typically have content that they display of which the layout system is unaware of, so it's important for such views to define its own `intrinsicContentSize` based on its content. If a custom view has no intrinsice content size for a given dimension, it can use `noIntrinsicMetric` for that dimension.

**What is deep linking?**

A technique that allows an app to be opened to a specific UI or resource, in response to some external event.

**What is optional binding?**

Optional binding checks whether an optional contains a value, if so, make the value available as a temporary constant or variable.

**Explain Codable in Swift 4**
See Notes

**What is Notification Center?**

A notification dispatch mechanism that enables the broadcast of information to registered observers.

Each running application has a `default` notification center, when an object adds itself as an observer, it specifies which notification it should receive, so an object may register itself a few times as observer for different notifications.

**Explain `Sequence` in Swift**

A type that provides sequential, iterated access to its elements. A sequence is a list of values that can be stepped through one at a time, the most common way to iterate over the elements of a sequence is to use `for-in` loop. This capability gives you access to a large number of operations that can be performed on any sequence, such like `contains(_:)`.

To add `Sequence` conformance to your own custom type, add a `makeIterator()` method and returns an iterator. Alternatively, if your type can act as its own iterator, implementing the requirements of `IteratorProtocol` and declaring conformance to both `Sequence` and `IteratorProtocol`.

```swift
struct Countdown: Sequence, IteratorProtocol {
    var count: Int

    mutating func next() -> Int? {
        guard count > 0 else {
            return nil
        }
        deter { count -= 1 }
        return count
    }
}
```

**Explain `zip(_:_:)`**

`zip` creates a sequence of pairs built out of two underlying sequences.

```swift
let words = ["one", "two", "three", "four"]
let numbers = 1...4

for (word, number) in zip(words, numbers) {
    print("\(word): \(number)")
}

// prints "one: 1"
// prints "two: 2"
// prints "three: 3"
// prints "four: 4"
```

If the two sequences are different lengths, the resulting sequence is the same length as the shorter sequence:

```swift
let naturalNumbers = 1...Int.max
let zipped = Array(zip(words, naturalNumbers))
// zipped == [("one", 1), ("two", 2), ("three", 3), ("four", 4)]
```

**What is "app ID" and "bundle ID"?**

Before code signing app during distribution, XCode automatically prefixes the bundle ID with team ID - the unique character sequence issued by Apple to each development team - and stores the combined string as the app ID. Bundle ID unique identifies an application in Apple's ecosystem.

**Difference between `accessibilityIdentifier`, `accessibilityLabel` and `accessibilityValue`**

- `accessibilityLabel`: a succinct label that identifies the accessibility element, in a localized string.
- `accessibilityValue`: the value of the accessibility element, in a localized string. Used when an accessibility element has static label and dynamic value, for example, a text field for message may have "message" as its label and the actual text as its value.

**What's the difference between xib and storyboard?**

Both for creating views using interface builder, xib is for view or single view controller, storyboard support multiple view controllers with transition flow.

**What is `URLSession`?**

`URLSession` is responsible for sending and receiving HTTP requests, created via `URLSessionConfiguration`

**Difference between `WKWebView` and `UIWebView`**

`UIWebview`:

- part of `UIKit`, no need to import additional module
- can scale page to fit

`WKWebView`:

- need to import `WebKit` framework
- higher and more efficient performance
- runs outside of app's main process
- supports server-side authentication challenge
- `WKWebViewConfiguration` provides more custom options

**What is OAuth?**

OAuth is an open standard authorization protocol or framework that describes how unrelated servers and services can safely allow authenticated access to their assets without sharing the initial, related, single logon credential. One simple example is when you go to log onto a website, it offers a few ways to log on using another website's/service's logon.

**Explain `rethrows` vs `throws`**

- `throws`: used to indicate a function, method or initializer can throw an error
- `rethrows`: indicate a function or method could throw an error only if one of its function parameters throws an error, it's for functions that do not throw errors on their own, but only forward errors from their function parameters

**What is Safe area?**

The safe area of a view reflects the area not covered by navigation bars, tab bars, toolbars and other system component that overlaps a view controller's view. Additional insets can be specified via `additionalSafeAreaInsets`.

**What is the difference between `Array` and `NSArray`?**

`Array` is a struct, thus it's value type in Swift, `NSArray` is an immutable ObjectiveC class, it is reference type and bridged to `Array<AnyObject>` in Swift.

**Explain Semaphore in iOS?**

`DispatchSemaphore` provides an efficient implementation of a traditional counting semaphore, which can be used to control access to a resource across multiple execution contexts.

**What is LLVM, LLDB and Clang?**

The LLVM Project is a collection of modular and reusable compiler and toolchain technologies.
Clang is a C language front-end for LLVM.
LLDB builds on libraries provided by LLVM and Clang to provide a great native debugger.

**What is Dependency Inversion Principle?**

DI decouples classes from one another by explicitly providing dependencies for them, rather than having them create the dependencies themselves.

**Explain difference between `class` and `struct` and when do we use one over the other?**

Stack is used for static memory allocation and Heap for dynamic memory allocation, both stored in RAM. Allocation of stack is dealth with in compile-time, thus optimisation is possible.

_class:_

- reference type, object with identity
- slower on heap
- updated with logic
- internals can remain mutable even when declared with `let`
- can be inherited
- type conversion can check the type of instance at runtime
- can use `deinit`
- same instance can be referenced more than once

_struct:_

- value typed, used for object without identity, e.g. `Address`, copy on assignment
- faster on stack
- simple data store, safer for multi-threading
- immutable when declared with `let`
