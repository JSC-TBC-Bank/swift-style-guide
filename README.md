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

## Type Inference

Swift's type inference allows the compiler to deduce the type of a variable or expression automatically, making code more concise and readable. However, over-reliance on type inference can lead to ambiguous code, reducing readability and maintainability. Additionally, certain practices can affect compiler performance. The following guidelines illustrate best practices and common pitfalls with examples.

### Variable Declaration

Using type inference can make variable declarations more concise, but it's important to ensure the type remains clear and to consider compiler performance.

**Recommended ✅**

For initializing numbers or strings, use untyped literals when possible.

```swift
let message = "Hello, world"
let count = 42
```

If the type doesn't match the default type, specify the type explicitly.

```swift
let value: Decimal = 12.25
```

**Not Recommended ❌**

Avoid using type inference when the type is not clear from the context.

```swift
let data = getDataFromServer()
```

In this case, explicitly specifying the type improves clarity.

```swift
let data: [String: Any] = getDataFromServer()
```

### Constructable Initialization

When initializing a constructable (struct or class), always use an explicitly typed declaration. This is especially important when there's no clear type on the left-hand side.

**Recommended ✅**

```swift
let value: MyConstructible = MyConstructible()
```

This ensures the compiler clearly understands the type, improving both performance and readability.

**Not Recommended ❌**

```swift
let value = MyConstructible()
```

This can lead to ambiguity and potential performance issues.

### Function Return Types

Swift can infer the return type of a function, which can reduce redundancy. However, in complex functions, it's better to specify the return type explicitly.

**Recommended ✅**

```swift
func square(of number: Int) -> Int {
    number * number
}
```

Here, the return type `Int` is clear from the function's context and the provided return type annotation.

**Not Recommended ❌**

```swift
func process(input: String) -> [String: Any] {
    // Complex processing logic
    ...
    return result
}
```

Always specify the return type in complex functions to improve readability.

### Type Aliases

Using type aliases with type inference can simplify code, but they should be clear and not overly complex.

**Recommended ✅**

```swift
typealias CompletionHandler = (Bool) -> Void

func fetchData(completion: CompletionHandler) {
    // Fetch data logic
    completion(true)
}
```

Here, `CompletionHandler` is clear and concise.

**Not Recommended ❌**

```swift
typealias DataHandler = ([String: Any], Error?) -> Void

func getData(handler: DataHandler) {
    // Fetch data logic
    handler(["key": "value"], nil)
}
```

In this case, breaking down the type alias into more descriptive components can improve readability.

### Dictionary and Array Initialization

When initializing collections, type inference can be useful, but it's important to ensure clarity and consider compiler performance. Use untyped literals for simple collections and be explicit with empty and nested collections.

**Recommended ✅**

For simple dictionaries and arrays, using untyped literals improves performance and readability.

```swift
let simpleDictionary = ["one": 1, "two": 2, "three": 3]
let mixedArray = [1, nil, 1.0, Decimal(1)]
```

For empty collections, explicitly specify the type to avoid ambiguity.

```swift
var numbers: [Int] = []
var strings: [String] = []
```

For nested collections, specify the type explicitly on the left-hand side to improve clarity and compiler performance.

```swift
let nestedDictionary: [String: [String: [String: Int]]] = [
    "level1": [
        "level2": [
            "key": 1
        ]
    ]
]

let nestedArray: [[Any]] = [
    [1, nil, 1.0, Decimal(1)],
    ["a", "b", "c"]
]
```

**Not Recommended ❌**

Avoid using type inference for nested collections and empty collections, as it can lead to performance issues and reduced readability.

```swift
let complexDictionary = [
    "level1": [
        "level2": [
            "key": 1
        ]
    ]
]

let complexArray = [
    [1, nil, 1.0, Decimal(1)],
    ["a", "b", "c"]
]

var ambiguousNumbers = []
```
