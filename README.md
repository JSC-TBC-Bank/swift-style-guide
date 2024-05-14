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

Implicitly unwrapped optionals are inherently unsafe and should be avoided whenever possible. Instead, use non-optional values or normal optionals. It’s safer and more reliable, even if you think a value will never be `nil`. Even in situations where you feel confident that a property will never be `nil`, it's better to prioritize safety and consistency.

❗️The only time implicitly unwrapped optionals are used is with `@IBOutlets`, where their lifetimes are tied to the UI lifecycle rather than strictly to the owning object.


**Recommended ✅**

```swift
class SomeViewController: UIViewController {

  @IBOutlet private weak var saveButton: UIButton!
  
}
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

if let userGreetingName = greetingName, let inputGreetingText = greetingText {
     // ... do something with unwrapped values
}
```

**Not Recommended ❌**

```swift
private var greetingName: String?
private var greetingText: String?

if let actualGreetingName = greetingName, let unwrappedGreetingText = greetingText {
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


When multiple optionals are unwrapped either with `guard` or `if let`, minimize nesting by using the compound version when possible.


**Recommended ✅**

```swift
if let username, let password, let passcode {
    // ... do something with unwrapped values
} else {
    fatalError("Authentication Failed")
}
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


In the compound version, which can't be fit on one line, place the `guard` / `if` on its own line, then indent each condition on a separate line.


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
// ... do something with values
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
