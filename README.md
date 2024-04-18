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
class URLFinder { *** }
```

**Clarity**

* Keep clarity. try to be both clear and as brief as possible, without losing any clarity.
* don’t abbreviate the names of things. Spell them out, even if they’re long. Always examine a use case to make sure it reads clearly within its given context.
* Include type information in constant or variable names when it is not obvious otherwise.

**Recommended ✅**

```swift
var defaultBackgroundColor = UIColor()

func selectionDestination() { *** }

func insertObject(at) { *** }
```

**Not Recommended ❌**

```swift
var defaultBkgdColor = UIColor()

func selDest() { *** }

func insert(at) { *** }
```

**File**

The name of a file should make it clear what the contents are. For example, if there is a single class named ImageParser then the file should be called ImageParser.swift. If you have a view controller, along with some helper types, then using the name of the view controller will likely be the best choice. A good heuristic to use is that if it’s ever unclear what to name a file, it may be worth splitting that file up.

<img width="441" alt="Screenshot 2024-04-17 at 18 34 30" src="https://github.com/JSC-TBC-Bank/swift-style-guide/assets/72808071/c6e7581b-2e33-4746-8c4b-4d4dd9f5ec24"> <br />

**Object Types**

* Names should be descriptive and unambiguous.
* All constants that are instance-independent should be static.
* When naming method arguments, make sure that the function can be read easily to understand the purpose of each argument.

**Recommended ✅**

```swift
class RoundAnimatingButton: UIButton {
  static let buttonPadding: CGFloat = 20.0

  func startAnimating(duration: NSTimeInterval) { *** }
}
```

**Not Recommended ❌**

```swift
class CustomButton: UIButton {
  static let btnPadding: CGFloat = 20.0

  func startAnimate(for: NSTimeInterval) { *** }
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

Property wrappers should be written as attributes on top of the classes.

**Recommended ✅**

```swift
@MainActor
class UpdateViewModel {
    func updateData() { *** }
}

class AlbumViewModel {
  @MainActor
    var images: [UIImage]?
    var pages: [Int]?
}

@objc
func didTapContinueButton() { *** }
```

**Not Recommended ❌**

```swift
@MainActor class UpdateViewModel {
    func updateData() { *** }
}

class AlbumViewModel {
    @MainActor var images: [UIImage]?
    var pages: [Int]?
}

@objc func didTapContinueButton() { *** }
```

**Access Modifiers**

Encapsulation improves readability and maintainability of your code.

* Specify access modifiers only when they are needed and required by the compiler.
* You can declare property names considering their accessing scope.
* the access modifier should always be presented first in the list before any other modifiers.
* For `internal` types and functions, do not explicitly specify the access modifier since all entities in Swift are `internal` by default.
* Declare classes with `final` keyword to prevent them from being subclassed.
* Classes and methods marked as `open` or `public` can be subclassed and overridden respectively out of their defining module.
* Encapsulate outlets and actions as `private` to limit their accessibility.

**Recommended ✅**

```swift
public final class UserProfile {
  @IBOutlet private weak var profileImageView: UIImageView!

  public var username: String?
  private(set) var email: String?
  private let internalUserID: Int
  fileprivate let settings: [String: Any]

  public func updateEmail(newEmail: String) {
    email = newEmail
  }
}
```

**Not Recommended ❌**

```swift
class UserProfile {
  @IBOutlet weak private var ProfileImageView: UIImageView!

  var username: String?
  private (set) var email: String?
  public var internalUserID: Int
  internal var settings: [String: Any]

  func updateEmail(newEmail: String)() {
    email = newEmail
  }
}
```

**Protocol**

* A protocol should be named as nouns if they describe what something is doing (e.g. `Collection`) [Apple's API Design Guidelines](https://swift.org/documentation/api-design-guidelines/).
* A protocol that describes a capability can be named using the suffixes `able`, `ible`, or `ing` (e.g. `Equatable`, `ProgressReporting`).
* Delegate method naming should express exact data handling functionality that will be processed under the private implementation.

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

* A protocol Implementation can be named with prefix `Default`, this kind of naming will distinguish types between protocol and class/structure.
* Avoid using `get`, `fetch` or `parse` prefixes when declaring delegate methods.

**Recommended ✅**

```swift
protocol RateWorker {
    func rateInfo()  -> [String: Int]
    func conversionDetails(model: RequestModel)
}

Class DefaultRateWorker: RateWorker {
    func rateInfo()  -> [String: Int] { *** }

    func conversionDetails(model: RequestModel) { *** }
}
```

**Not Recommended ❌**

```swift
protocol RateWorkerProtocol {
    func fetchRateInfo() -> [String: Int]
    func getConversionDetails(model: RequestModel)
}

Class RateWorker: RateWorkerProtocol {
    func fetchRateInfo()  -> [String: Int] { *** }

    func getConversionDetails(model: RequestModel) { *** }
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
