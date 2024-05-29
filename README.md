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
- [Trailing Closures](#trailing-closures)
- [Memory Management](#memory-managment)
- [Classes, Structures, and Enums](#classes-structures-and-enums)
  - [Declaration and Naming](#declaration-and-naming)
  - [Properties](#properties)
  - [Methods](#methods)
- [Error Handling](#error-handling)
- [Import Statements](#import-statements)

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

<img width="417" alt="Screenshot 2024-05-01 at 16 56 45" src="https://github.com/JSC-TBC-Bank/swift-style-guide/assets/72808071/75612c72-96ae-43c3-88d9-19abf084aa7e"> <br />


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

Property wrappers should be written as attributes on top of the objects or methods.

* naming methods using Target-Action version of the Command pattern, ([Target-Action](https://swift.org/documentation/api-design-guidelines/(https://developer.apple.com/library/archive/documentation/General/Conceptual/CocoaEncyclopedia/Target-Action/Target-Action.html)) is the Command Pattern part of Apple’s MVC). the UI element (e.g. a button) is the invoker, the action/message passed to/method called on the object is the command and the target is the receiver. In this case, a command should be something like `undo`, `popBackward`, or `scrollToEndOfDocument`.
  
(calling your action didTapContinueButton() sounds like a delegate method. We don’t expect a call from some very specific source like in a delegate pattern. Instead we get a command to interpret and execute. And this command may not be invoked. Or may be invoked multiple times. By different senders, we don’t even know who will be triggering the action.)

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
func continue() { *** }
func onContinue() { *** }
func continueTap() { *** }
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
* Using `get`, `fetch`, `remove`, `update` prefixes are preferable and sometimes unavoidable when declaring delegate methods for service calls.

**Recommended ✅**

```swift
protocol RateWorkerProtocol {
    func fetchRateInfo() -> [String: Int]
    func getConversionDetails(for: RequestModel) -> [DetailsModel]
    func removeFavoriteRate(model: RateModel)
}

Class RateWorker: RateWorkerProtocol {
    func fetchRateInfo() -> [String: Int] { *** }

    func getConversionDetails(for model: RequestModel) -> [DetailsModel] { *** }

    func removeFavoriteRate(model: RateModel) { *** }
}
```

**Not Recommended ❌**

```swift
protocol RateWorker {
    func rateInfo() -> [String: Int]
    func conversionDetails(for: RequestModel) -> [DetailsModel]
    func favoriteRate(model: RateModel)
}

Class DefaultRateWorker: RateWorker {
    func rateInfo()  -> [String: Int] { *** }

    func conversionDetails(for model: RequestModel) -> [DetailsModel] { *** }

    func favoriteRate(model: RateModel) { *** }
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

## Function Calls

Match the format of function declarations when calling them. If a call fits on one line, keep it that way and if the call site needs to be wrapped, each parameter should be placed on a new line.

**Recommended ✅**

```swift
let perimeter = calculatePerimeterOfRectangle(
    first: 3,
    second: 6,
    third: 3,
    forth: 6
)
```

**Not Recommended ❌**

```swift
let perimeter = calculatePerimeterOfRectangle(first: 3, second: 6, third: 3, forth: 6)
```

## Trailing Closures

If a function call has multiple closure arguments, then none are called using trailing closure syntax; all are labeled and nested inside the argument list’s parentheses.

**Recommended ✅**
```swift
UIView.animate(
  withDuration: 0.5,
  animations: {
    // ...
  },
  completion: { finished in
    // ...
  })
```

**Not Recommended ❌**

```swift
UIView.animate(
  withDuration: 0.5,
  animations: {
    // ...
  }) { finished in
    // ...
  }
```

If a function has a single closure argument and it is the final argument, then it is always called using trailing closure syntax and when a function called with trailing closure syntax takes no other arguments, empty parentheses `()` after the function name are never present.

**Recommended ✅**

```swift
Timer.scheduledTimer(timeInterval: 30, repeats: false) { timer in
  print("Timer done!")
}

let squares = [1, 2, 3].map { $0 * $0 }
```

**Not Recommended ❌**

```swift
Timer.scheduledTimer(timeInterval: 30, repeats: false, block: { timer in
  print("Timer done!")
})

let squares = [1, 2, 3].map({ $0 * $0 })
```
Functions should not be overloaded such that two overloads differ only by the name of their trailing closure argument. Doing so prevents using trailing closure syntax—when the label is not present, a call to the function with a trailing closure is ambiguous

**Not Recommended ❌**

```swift
func greet(enthusiastically nameProvider: () -> String) {
  print("Hello, \(nameProvider())! It's a pleasure to see you!")
}

func greet(apathetically nameProvider: () -> String) {
  print("Oh, look. It's \(nameProvider()).")
}

greet { "John" }  // error: ambiguous use of 'greet'
```

This example is fixed by differentiating some part of the function name other than the closure argument—in this case, the base name

**Recommended ✅**

```swift
func greetEnthusiastically(_ nameProvider: () -> String) {
  print("Hello, \(nameProvider())! It's a pleasure to see you!")
}

func greetApathetically(_ nameProvider: () -> String) {
  print("Oh, look. It's \(nameProvider()).")
}

greetEnthusiastically { "John" }
greetApathetically { "not John" }
```

For single-expression closures where the context is clear, use implicit returns:

**Recommended ✅**

```swift
attendeeList.sort { a, b in
  a > b
}
```

Chained methods using trailing closures should be clear and easy to read in context. Decisions on spacing, line breaks, and when to use named versus anonymous arguments is left to the discretion of the author. Examples:

**Recommended ✅**

```swift
// Prefer this approach in if or guard checks 
let value = numbers.map { $0 * 2 }.filter { $0 % 3 == 0 }.index(of: 90)

let value = numbers
  .map { $0 * 2 }
  .filter { $0 > 50 }
  .map { $0 + 10 }
```

Favor Void return types over () in closure declarations. If you must specify a Void return type in a function declaration, use Void rather than () to improve readability.

**Recommended ✅**

```swift
func method(completion: () -> Void) {
  ...
}
```

**Not Recommended ❌**

```swift
func method(completion: () -> ()) {
  ...
}
```

Name unused closure parameters as underscores. Makes it obvious which parameters are used and which are unused.

**Recommended ✅**

```swift
someAsyncThing() { _, _, argument3 in
  print(argument3)
}
```

**Not Recommended ❌**

```swift
someAsyncThing() { argument1, argument2, argument3 in
  print(argument3)
}
```

Closures should have a single space or newline inside each brace. Trailing closures should additionally have a single space or newline outside each brace.

**Recommended ✅**

```swift
let evenSquares = numbers.filter { $0.isMultiple(of: 2) }.map { $0 * $0 }

let evenSquares = numbers.filter({ $0.isMultiple(of: 2) }).map({ $0 * $0 })

let evenSquares = numbers
  .filter {
    $0.isMultiple(of: 2)
  }
  .map {
    $0 * $0
  }
```

**Not Recommended ❌**

```swift
let evenSquares = numbers.filter{$0.isMultiple(of: 2)}.map{  $0 * $0  }

let evenSquares = numbers.filter( { $0.isMultiple(of: 2) } ).map( { $0 * $0 } )

let evenSquares = numbers
  .filter{
    $0.isMultiple(of: 2)
  }
  .map{
    $0 * $0
  }
```

Omit Void return types from closure expressions

**Recommended ✅**

```swift
someAsyncThing() { argument in
  ...
}
```

**Not Recommended ❌**

```swift
someAsyncThing() { argument -> Void in
  ...
}
```

Prefer trailing closure syntax for closure arguments with no parameter name.

**Recommended ✅**

```swift
planets.map { $0.name }

// since this closure has a parameter name
planets.first(where: { $0.isGasGiant })

// ALSO FINE. Trailing closure syntax is still permitted for closures
// with parameter names. However, consider using non-trailing syntax
// in cases where the parameter name is semantically meaningful.
planets.first { $0.isGasGiant }
```

**Not Recommended ❌**

```swift
planets.map({ $0.name })
```

**Captures**

Avoid using unowned captures. Instead prefer safer alternatives like weak captures, or capturing variables directly.

**Not Recommended ❌**

```swift
// WRONG: Crashes if `self` has been deallocated when closures are called.
final class SpaceshipNavigationService {
  let spaceship: Spaceship
  let planet: Planet
  
  func colonizePlanet() {
    spaceship.travel(to: planet, onArrival: { [unowned self] in
      planet.colonize()
    })
  }
  
  func exploreSystem() {
    spaceship.travel(to: planet, nextDestination: { [unowned self] in
      planet.moons?.first
    })
  }
}
```
weak captures are safer because they require the author to explicitly handle the case where the referenced object no longer exists.

**Recommended ✅**

```swift
final class SpaceshipNavigationService {
  let spaceship: Spaceship
  let planet: Planet
  
  func colonizePlanet() {
    spaceship.travel(to: planet, onArrival: { [weak self] in
      guard let self else { return }
      planet.colonize()
    })
  }
  
  func exploreSystem() {
    spaceship.travel(to: planet, nextDestination: { [weak self] in
      guard let self else { return nil }
      return planet.moons?.first
    })
  }
}
```

Alternatively, consider directly capturing the variables that are used in the closure. This lets you avoid having to handle the case where self is nil, since you don't even need to reference self: But bare in mind that using this approach might cause memory leaks in some cases so use it wisely.

**MIGHT BE USEFUL IN SOME CASES**

```swift
final class SpaceshipNavigationService {
  let spaceship: Spaceship
  let planet: Planet
  
  func colonizePlanet() {
    spaceship.travel(to: planet, onArrival: { [planet] in
      planet.colonize()
    })
  }
  
  func exploreSystem() {
    spaceship.travel(to: planet, nextDestination: { [planet] in
      planet.moons?.first
    })
  }
}
```

## Import Statements

Import only the modules a source file requires. For example, don't import UIKit when importing Foundation will suffice. Likewise, don't import Foundation if you must import UIKit.

**Recommended ✅**

```swift
import UIKit

var view: UIView
var baseURLs: [URL]
```

**Not Recommended ❌**

```swift
import UIKit
import Foundation

var view: UIView
var baseURLs: [URL]
```
