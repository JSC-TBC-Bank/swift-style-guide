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

## Optionals

### Declaring Optionals

Declare optional constants/variables and function return types as optional to convey a non-error result that is either a value or the absence of a value (`nil`).

When creating Models of API responses (JSON data), always declare properties as optionals to ensure handling situations where certain properties may not have a value in the received data. 

Define an optional variable that does not have a default value without explicitly assigning it to `nil`.


**Recommended ✅**

```swift
private var currency: String?
```

**Not Recommended ❌**

```swift
private var currency: String? = nil
```


### Unwrapping Optionals

#### 1. Force Unwrapping

Implicitly unwrapped optionals are inherently unsafe and should be avoided whenever possible. Instead, use non-optional values or normal optionals. Even in situations where you feel confident that a property will never be `nil`, it's better to prioritize safety and consistency.

❗️The only time implicitly unwrapped optionals are used is with `@IBOutlets`, where their lifetimes are tied to the UI lifecycle rather than strictly to the owning object.


**Recommended ✅**

```swift
class SomeViewController: UIViewController {

  @IBOutlet private weak var saveButton: UIButton!
  
}
```


**`weak` VS `unowned`**

It’s advisable to prefer the use of `weak` over `unowned`. Implicit unwrapping should be avoided whenever possible, as `unowned` is essentially a `weak` property that is implicitly unwrapped.

**Recommended ✅**

```swift
private weak var parentViewController: UIViewController?
```

**Not Recommended ❌**

```swift
private weak var parentViewController: UIViewController!

private unowned var parentViewController: UIViewController
```


#### 2. Checking for `nil`

When there's a need to verify the presence of a value in an optional without using it, preferring direct checking against nil over optional binding is clearer and more explicit.


**Recommended ✅**

```swift
if userId != nil {
  // ... do something 
}
```

**Not Recommended ❌**

```swift
if let _ = userId {
  // ... do something 
}
```


#### 3. Optional Binding

Use conditional unwrap with optional binding to access the optional’s value if it does exist and to make that value available as a temporary constant or variable. 

If there's no need to reference the original optional constant or variable after accessing the value it contains, the existing name can be shadowed using the shorthand syntax.


**Recommended ✅**

```swift
private var greetingName: String? = "Stranger"

if let greetingName {
    print("Hello, \(greetingName)!")
}
```

**Not Recommended ❌**

```swift
private var greetingName: String? = "Stranger"

if let greetingName = greetingName {
    print("Hello, \(greetingName)!")
}
```


Otherwise, it's advisable to introduce new names, but avoid using names like `unwrappedView` or `actualLabel` and so on.


**Recommended ✅**

```swift
private var greetingName: String?
private var greetingText: String?

if 
let userGreetingName = greetingName, 
let inputGreetingText = greetingText {
     // ... do something with unwrapped values
}
```

**Not Recommended ❌**

```swift
private var greetingName: String?
private var greetingText: String?

if 
let actualGreetingName = greetingName, 
let unwrappedGreetingText = greetingText {
   // ... do something with unwrapped values
}
```


When unwrapping optionals, prefer `guard` statements as opposed to `if` statements to decrease the amount of nested indentation in the code.


**Recommended ✅**

```swift
guard let creditSale else { return }
requestCredit(using: creditSale)
enjoyShopping()
```

**Not Recommended ❌**

```swift
if let creditSale {
    requestCredit(using: creditSale)
    enjoyShopping()
}
```

**Not Recommended ❌**

```swift
if creditSale == nil { return }
requestCredit(using: creditSale)
enjoyShopping()
```


When multiple optionals are unwrapped either with `guard` or `if` statement, minimize nesting by using the compound version when possible.

Additionally, for better readability and debugging, place the `guard` / `if` on its own line, and then indent each condition on a separate line.


**Recommended ✅**

```swift
guard 
  let username,
  let email,
  let password,
  let passcode,
  let securityAnswer
else {
  fatalError("Authentication Failed")
}
// ... do something with unwrapped values
```

**Not Recommended ❌**

```swift
if let username {
    if let password {
        if let passcode {
            // ... do something with unwrapped values
        } else {
            fatalError("Authentication Failed")
        }
    } else {
        fatalError("Authentication Failed")
    }
} else {
    fatalError("Authentication Failed")
}
```


### Providing Fallback Value

Use the nil-coalescing operator as a shorthand method to provide a fallback value when unwrapping an optional, instead of using the ternary conditional operator and forced unwrapping.

**Recommended ✅**

```swift
private let depositDefaultName = "My Piggy"
private var userDefinedDepositName: String?

private var depositNameToDisplay = userDefinedDepositName ?? depositDefaultName
```

**Not Recommended ❌**

```swift
private let depositDefaultName = "My Piggy"
private var userDefinedDepositName: String?

private var depositNameToDisplay = userDefinedDepositName != nil ? userDefinedDepositName! : depositDefaultName
```
