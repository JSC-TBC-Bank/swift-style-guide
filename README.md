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
## Error Handling

**Basic do-catch block**

Prioritize using custom error types over NSError unless you specifically require interoperability with Objective-C frameworks. 

**Recommended ✅**

```swift
enum NetworkError: Error {
    case noInternet
    case serverError
}

func fetchData() throws {
    if !isConnectedToInternet {
        throw NetworkError.noInternet
    }

    // Fetch data from the server
}

do {
    try fetchData()

    // Data fetched successfully
} catch {
    // Handle errors
    print("An unexpected error occurred.")
}
```

**Not Recommended ❌**

```swift
func fetchData() throws {
    if !isConnectedToInternet {
        throw NSError(domain: "my error domain", code: 35, userInfo: [:])
    }

    // Fetch data from the server
}

do { 
    try fetchData()

    // Data fetched successfully
} catch {
    // Handle errors
    print("An unexpected error occurred.")
}
```

**Pattern matching**

When catching an error with a parameter, you should use "let" after the "catch" keyword

**Recommended ✅**

```swift
do {
    let result = try processData()

    print(result)
} catch let DataError.invalidData(message) {
    // Handling the error
    print("Data error occurred with message: \(message)")
}
```

**Not Recommended ❌**

```swift
do {
    let result = try processData()

    print(result)
} catch DataError.invalidData(let message) {
    // Handling the error
    print("Data error occurred with message: \(message)")
}
```

**Variable**

When catching a general error with a parameter and you don't want to declare a different name for the error variable, you omit the variable name after the "catch" keyword

**Recommended ✅**

```swift
do {
    let result = try processData()

    print(result)
} catch {
    // Handling the error
    print("Error \(error)")
}
```

**Not Recommended ❌**

```swift
do {
    let result = try processData()

    print(result)
} catch let error {
    // Handling the error
    print("Error \(error)")
}
```

Start throw/try from new line for clarity, consistency, and better error isolation.

**Recommended ✅**

```swift
func fetchData() throws {
    if !isConnectedToInternet {
        throw NetworkError.noInternet
    }

    // Fetch data from the server
}
```

**Not Recommended ❌**

```swift
func fetchData() throws {
    if !isConnectedToInternet { throw NetworkError.noInternet }

    // Fetch data from the server
}
```

**Recommended ✅**

```swift
do {
    try fetchData()

    // Data fetched successfully
} catch {
    // Handle errors
    print("An unexpected error occurred.")
}
```

**Not Recommended ❌**

```swift
do { try fetchData() }
catch {
    // Handle errors
    print("An unexpected error occurred.")
}
```

**Handling specific errors**

It's preferable to handle distinct errors separately rather than within a general catch block.

**Recommended ✅**

```swift
do {
    try fetchData()
    // Data fetched successfully
} catch NetworkError.noInternet {
    // Handle the no internet error
    print("No internet connection.")
} catch NetworkError.serverError {
    // Handle the server error
    print("Server error occurred.")
} catch {
    // Handle other errors
    print("An unexpected error occurred.")
}
```

**Not Recommended ❌**

```swift
do {
    try fetchData()
    // Data fetched successfully
} catch {
    // Handle other errors
    print("An unexpected error occurred.")
}
```

**Optionals**

When expecting a value from a throwable function, sometimes using try? instead of a do-catch block is better.

**Recommended ✅**
```swift
func divide(_ dividend: Int, by divisor: Int) throws -> Int {
    guard divisor != 0 else {
        throw MathError.divisionByZero
    }

    return dividend / divisor
}

// Using try? for optional value
let result = try? divide(10, by: 2)

if let value = result {
    print("Result of division: \(value)")
} else {
    print("Division by zero or invalid input occurred")
}
```

**Not Recommended ❌**

```swift
func divide(_ dividend: Int, by divisor: Int) throws -> Int {
    guard divisor != 0 else {
        throw MathError.divisionByZero
    }

    return dividend / divisor
}

// Using do-catch block for error handling
do {
    let result = try divide(10, by: 2)
    print("Result of division: \(result)")
} catch MathError.divisionByZero {
    print("Division by zero occurred")
} catch {
    print("An unexpected error occurred: \(error)")
}
```

**Propagating errors**

When expecting a value from a throwable function, sometimes using try? instead of a do-catch block is better.

**Recommended ✅**
```swift
func fetchDataFromDatabase() throws -> [String] {
    // Simulating a function that fetches data from a database
    let success = Bool.random()
    if success {
        return ["Data 1", "Data 2", "Data 3"]
    } else {
        // Throw queryError if data retrieval fails
        throw DatabaseError.queryError
    }
}

// Function propagating the error to the caller
func processData() throws {
    let data = try fetchDataFromDatabase()
    print("Data retrieved from database: \(data)")
}

// Example usage
do {
    try processData()
} catch DatabaseError.queryError {
    print("Database query error occurred.")
} catch {
    print("An unexpected error occurred: \(error)")
}
```

**Not Recommended ❌**

```swift
// Function propagating the error to the caller
func processData() throws -> [String] {
    let data: [String]

    // Simulating a function that fetches data from a database
    let success = Bool.random()
    if success {
        data = ["Data 1", "Data 2", "Data 3"]
    } else {
        // Throw queryError if data retrieval fails
        throw DatabaseError.queryError
    }

    print("Data retrieved from database: \(data)")

    return data
}

// Example usage
do {
    try processData()
} catch DatabaseError.queryError {
    print("Database query error occurred.")
} catch {
    print("An unexpected error occurred: \(error)")
}
```

**Defer**

In some cases, using defer is better because it ensures consistent execution, regardless of the error handling path.

**Recommended ✅**
```swift
func processFile() throws {
    let file = openFile()

    defer {
        closeFile(file)
    }

    guard file.mimeType == "image/jpeg" else {
        throw FileError.unexpectedFileMIMEType
    }

    guard file.size > 0 else {
        throw FileError.invalidFileData
    }

    // File process
}
```

**Not Recommended ❌**

```swift
func processFile() throws {
    let file = openFile()

    guard file.mimeType == "image/jpeg" else {
        closeFile(file)

        throw FileError.unexpectedFileMIMEType
    }

    guard file.size > 0 else {
        closeFile(file)

        throw FileError.invalidFileData
    }

    // File process
}
```

**Error handling with async/await**

Keyword 'try' must precede 'await' keyword.

**Recommended ✅**
```swift
try await Task.sleep(for: .seconds(5))
```

**Not Recommended ❌**

```swift
await try Task.sleep(for: .seconds(5))
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
