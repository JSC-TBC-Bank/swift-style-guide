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
  - [Ternary Operator](#ternary-operator)
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
# Control Flow

# If Statements

When using control flow statement `if`, the first continuation line should be after the control flow keyword. If have more than one continuation line, write them on a new line. The open brace `{` should be next to the last  continuation line.

**Recommended ✅**

```swift
if firstBooleanValue,
   secondBooleanValue,
   thirdBooleanValue {
    // do something
}
```
**Not Recommended ❌**

```swift
if firstBooleanValue,
   secondBooleanValue,
   thirdBooleanValue 
{
    // do something
}
```

```swift
if
    firstBooleanValue,
    secondBooleanValue,
    thirdBooleanValue
{
    // do something
}
```

If have `else if` block write first continuation line after it.

**Recommended ✅**

```swift
if firstBooleanValue {
    // do something
} else if secondBooleanValue {
    // do something
}
```
**Not Recommended ❌**

```swift
if firstBooleanValue {
    // do something
} else if
    secondBooleanValue {
    // do something
}
```

Minimize `if` statement nesting by using the compound version when possible.

**Recommended ✅**

```swift
if firstBooleanValue,
   secondBooleanValue,
   thirdBooleanValue {
    // do something
}
```

**Not Recommended ❌**

```swift
if firstBooleanValue {
    if secondBooleanValue {
        if thirdBooleanValue {
            // do something
        }
    }
}
```

# Switch Statements

When use switch with `enums ` with associated values, write let before case.

```swift
enum PaymentMethod {
    case creditCard(number: String, expirationDate: String, cvv: String)
    case paypal(email: String)
    case applePay
}

let paymentMethod = PaymentMethod.applePay
```

**Recommended ✅**

```swift
switch paymentMethod {
case let .creditCard(number, expirationDate, cvv):
    () // Do something...
case let .paypal(email):
    () // Do something...
case .applePay:
    () // Do something...
}
```

**Not Recommended ❌**

```swift
switch paymentMethod {
case .creditCard(number: let number, expirationDate: let expirationDate, cvv: let cvv):
    () // Do something...
case .paypal(email: let email):
    () // Do something...
case .applePay:
    () // Do something...
}
```

```swift
switch paymentMethod {
case .creditCard(let number, let expirationDate, let cvv):
    () // Do something...
case .paypal(let email):
    () // Do something...
case .applePay:
    () // Do something...
}
```

When using switch to set the value of a variable. 

**Recommended ✅**

```swift
let spelledOut = switch Int.random(in: 0...3) {
case 0:
    "Zero"
case 1:
    "One"
case 2:
    "Two"
case 3:
    "Three"
default:
    "Out of range"
}
```

**Not Recommended ❌**

```swift
let spelledOut: String

switch Int.random(in: 0...3) {
case 0:
    spelledOut = "Zero"
case 1:
    spelledOut = "One"
case 2:
    spelledOut = "Two"
case 3:
    spelledOut = "Three"
default:
    spelledOut = "Out of range"
}
```

# Ternary Operator

Use Ternary Operator to increase code clarity. Write it in one line. If conditions are too long and complex, put them into separate variables.

**Recommended ✅**

```swift
let operation = "add"
let fifty = 50
let twenty = 20
let sum = "The sum of \(fifty) and \(twenty) is \(fifty + twenty)."
let difference = "The difference between \(fifty) and \(twenty) is \(fifty - twenty)."
let text = operation == "add" ? sum : difference
```

**Not Recommended ❌**

```swift
let operation = "add"
let fifty = 50
let twenty = 20
let text = operation == "add" ? "The sum of \(fifty) and \(twenty) is \(fifty + twenty)." : "The difference between \(fifty) and \(twenty) is \(fifty - twenty)."
```

```swift
let operation = "add"
let fifty = 50
let twenty = 20
let text = operation == "add" ?
"The sum of \(fifty) and \(twenty) is \(fifty + twenty)." :
"The difference between \(fifty) and \(twenty) is \(fifty - twenty)."
```
