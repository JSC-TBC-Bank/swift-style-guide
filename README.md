# TBC Swift Style Guide

TBC Swift Style Guide is your go-to resource for mastering Swift syntax conventions, enhancing readability, and ensuring consistency across your projects. It's a dynamic reference shaped by industry best practices and community feedback. Our goal is to streamline your coding experience, making it easier to collaborate and maintain codebases. Dive in to discover tips, tricks, and guidelines that will elevate your Swift coding style and clarity.

## Table of Contents

- [Naming Conventions](#naming-conventions)
- [Formatting Guidelines](#formatting-guidelines)
  - [Indentation](#indentation)
  - [Line Length](#line-length)
  - [Braces and Parentheses](#braces-and-parentheses)
  - [Whitespace](#whitespace)
- [Comments and Documentation](#comments-and-documentation)
- [Types](#types)
  - [Constants](#constants)
  - [Static Methods and Variable Type Properties](#static-methods-and-variable-type-properties)
  - [Optionals](#optionals)
  - [Lazy Initialization](#lazy-initialization)
  - [Type Inference](#type-inference)
  - [Syntactic Sugar](#syntactic-sugar)
- [Control Flow](#control-flow)
  - [If Statements](#if-statements)
  - [Switch Statements](#switch-statements)
  - [Loops](#loops)
- [Function Declarations](#function-declarations)
- [Function Calls](#function-calls)
- [Closures](#closures)
- [Memory Management](#memory-managment)
- [Classes, Structures, and Enums](#classes-structures-and-enums)
  - [Declaration and Naming](#declaration-and-naming)
  - [Properties](#properties)
  - [Methods](#methods)
- [Error Handling](#error-handling)
- [Imports and Module Organization](#imports-and-module-organization)

## Naming Conventions
When naming any type, variable, method or object, there are two main question that you should ask to yourself.
* What purpose does it have?
* What kind of data it should hold?

These two questions will not only help you name correctly but also help other developers when they have to reuse or refactor your code.

* Use `PascalCase` for type names (e.g. struct, enum, class, generics, typealias, associatedtype, etc.).
* Use `camelCase` (initial lowercase letter) for function, method, property, constant, variable, argument names, enum cases, etc.
* Acronyms should be written in caps, except at the start of a name, which needs to start with lowercase, in this case use all lowercase for the acronym.

```swift
// "HTML" is at the start of a constant name, so we use lowercase "html"
let htmlBodyContent: String = "<p>Hello, World!</p>"
// Prefer using ID to Id
let profileID: Int = 1
// Prefer URLFinder to UrlFinder
class URLFinder {
}
```

**Clarity**

* Keep clarity. try to be both clear and as brief as possible, without losing any clarity.
* don’t abbreviate the names of things. Spell them out, even if they’re long. Always examine a use case to make sure it reads clearly within its given context.
* Include type information in constant or variable names when it is not obvious otherwise.

**Recommended ✅**

```swift
var defaultBackgroundColor = UIColor()

func selectionDestination() {}

func insertObject(at) {}
```

**Not Recommended ❌**

```swift
var defaultBkgdColor = UIColor()

func selDest() {}

func insert(at) {}
```

**File**

The name of a file should make it clear what the contents are. For example, if there is a single class named ImageParser then the file should be called ImageParser.swift. If you have a view controller, along with some helper types, then using the name of the view controller will likely be the best choice. A good heuristic to use is that if it’s ever unclear what to name a file, it may be worth splitting that file up.

<img width="466" alt="Screenshot 2024-04-16 at 15 16 45" src="https://github.com/JSC-TBC-Bank/swift-style-guide/assets/72808071/3da1595a-9dad-4c2b-a309-48e5b35683ee"> <br />

**Object Types**

* Names should be descriptive and unambiguous.
* All constants that are instance-independent should be static.
* When naming method arguments, make sure that the function can be read easily to understand the purpose of each argument.

**Recommended ✅**

```swift
class RoundAnimatingButton: UIButton {
  static let buttonPadding: CGFloat = 20.0

  func startAnimating(duration: NSTimeInterval) { }
}
```

**Not Recommended ❌**

```swift
class CustomButton: UIButton {
  static let btnPadding: CGFloat = 20.0
  func startAnimate(for: NSTimeInterval) { }
 }
```

Initializer arguments that correspond directly to a stored property have the same name as the property. Explicit self. is used during assignment to disambiguate them.

**Recommended ✅**

```swift
public struct Person {
  public let name: String
  public let phoneNumber: String

  public init(name: String, phoneNumber: String) {
    self.name = name
    self.phoneNumber = phoneNumber
  }
}
```

**Not Recommended ❌**

```swift
public struct Person {
  public let name: String
  public let phoneNumber: String

  public init(name otherName: String, phoneNumber otherPhoneNumber: String) {
    name = otherName
    phoneNumber = otherPhoneNumber
  }
}
```

**Protocol**

* A protocol should be named as nouns if they describe what something is doing (e.g. `Collection`)
* A protocol that describes a capability should be named using the suffixes `able`, `ible`, or `ing` (e.g. `Equatable`, `ProgressReporting`).”
* You can also add a `Protocol` suffix to the protocol's name if you want to distinguish it from a Swift class or struct.
* Delegate method naming should express exact data handling that will be processed under the private implementation.

**Recommended ✅**

```swift
protocol Loadable {
    associatedtype Result
    func loadData() throws -> Result
    func cancelTask(result: Result) 
}
```

**Not Recommended ❌**

```swift
protocol Load {
    associatedtype Result
    func load() throws -> Result
    func cancel(Result)
}
```

* Protocol Implementation can be named with suffix `Source`, this kind of naming will distinguish types between protocol and class/structure.

**Recommended ✅**

```swift
protocol RateWorker {
    func rateInfoFetched()  -> [String: Int]
}

Class RateWorkerSource: RateWorker {
    func rateInfoFetched()  -> [String: Int] {
    }
}
```

**Not Recommended ❌**

```swift
protocol RateWorkerProtocol {
    func rateInfoFetched() -> [String: Int]
}

Class RateWorker: RateWorkerProtocol {
    func rateInfoFetched()  -> [String: Int] {
    }
}
```

## Comments and Documentation

Comments should be used to explain the reasoning behind particular sections of code. It's essential to keep them accurate and pertinent, updating or removing them as necessary to reflect any changes in the codebase. Avoid embedding extensive comments within the code itself, except for those required for documentation purposes. Instead, opt for Int- or triple-slash comments over C-style `/* ... */` comments for clarity and consistency.

## Function Declarations

Keep short function declarations on one line including the opening brace:

```swift
func generateRandomDigit(numberOfDigits: Int) -> Int {
  // generate random digit
}
```
For functions with long signatures or more than two parameters, put each parameter on a new line and add an extra indent on subsequent lines:

**Recommended ✅**

```swift
func calculatePerimeterOfRectangle(
    first: Int,
    second: Int,
    third: Int,
    forth: Int
) -> Int {
    // calculate code goes here
}
```

**Not Recommended ❌**

```swift
func calculatePerimeterOfRectangle(first: Int, second: Int, third: Int, forth: Int) -> Int {
  // calculate code goes here
}
```
