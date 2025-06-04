# Introduction to Swift Programming

Welcome! In this workshop you’ll deepen your understanding of Swift’s core features, loops and ranges, optionals, collections, closures, generics, protocols, error handling and parsing, by tackling a series of increasingly challenging exercises. We’ll be using SwiftFiddle (no local install needed), but you can also apply these in Xcode Playgrounds or a Swift Package.

Official docs:  
- The Swift Programming Language (Guide): https://docs.swift.org/swift-book/documentation/the-swift-programming-language/guidedtour/
- Swift Standard Library Reference: https://developer.apple.com/documentation/swift/swift-standard-library  

---

## Exercise 1: Number sequences and loops  
**Goals:** `for-in`, `stride(from:to:by:)`, `while`/`repeat-while`  

1. Print 1…100 using a standard `for i in 1...100` loop.  
2. Print only even numbers in 0…100 with `stride(from:to:by:)`.  
3. Rewrite (1) using `while` and then `repeat-while`.  

---

## Exercise 2: Functions, parameters and optionals  
**Goals:** function signatures, default arguments, variadics and optional handling  

1. Write `func add(_ a: Int, _ b: Int) -> Int`.  
2. Add a third parameter with a default value, e.g. `func add(_ a: Int, _ b: Int, _ c: Int = 0)`.  
3. Write `func sum(_ numbers: Int...) -> Int` using variadic parameters.  
4. Write `func divide(_ a: Int, by b: Int) -> Int?` returning `nil` on division by zero, and use `guard let` to unwrap.  

---

## Exercise 3: String manipulation and palindromes  
**Goals:** `String` APIs, `Character` iteration and `Optional` chaining  

1. Implement `func reverse(_ s: String) -> String`.  
2. Implement `func isPalindrome(_ s: String) -> Bool`, ignore case and non-alphanumeric characters.  
3. Use `s.lowercased()`, `s.filter { ... }`, and `String(s.reversed())`.  

---

## Exercise 4: Collections, arrays, sets and Dictionaries  
**Goals:** initialization, de-duplication, sorting and transformations  

1. Given an array `[Int]`, return a sorted array of unique elements.  
2. From an array of names `[String]`, produce a count dictionary `[String: Int]`.  
3. Use `map`, `filter`, `reduce`, `sorted(by:)` and `Set`.  

---

## Exercise 5: Word frequency with dictionaries  
**Goals:** text processing, regex, dictionaries and `NSRegularExpression` or `String` APIs  

1. Write `func wordFrequency(in text: String) -> [String: Int]`.  
2. Downcase the text, strip punctuation via `text.replacingOccurrences(of:pattern:options:)` (use regex), split on whitespace, then tally.  
3. Return the top 5 most frequent words sorted descending.  

---

## Exercise 6: Closures and higher-order functions  
**Goals:** closure syntax, capturing and `map`/`filter`/`reduce` with custom types  

1. Define `struct Person { let name: String; let age: Int }` and an array of people.  
2. Sort by age using a closure: `people.sorted { $0.age < $1.age }`.  
3. Use `filter` to find all adults (`age >= 18`), then `map` their names, then `reduce` to a single comma-separated string.  

---

## Exercise 7: Protocols & generics  
**Goals:** protocol-oriented programming, associated types and generic functions  

1. Define protocol  
   ```swift
   protocol Shape {
     func area() -> Double
   }
   ```  
2. Create `struct Circle`, `struct Rectangle` conforming to `Shape`.  
3. Write generic `func totalArea<S: Shape>(_ shapes: [S]) -> Double` summing areas.  
4. Define protocol `NamedShape` with a `name` property and extend it.  

---

## Exercise 8: Error Handling & decodable  
**Goals:** `Error` protocols, `do-try-catch`, `Codable` and `JSONDecoder`  

1. Define `enum JSONError: Error { case decodingFailed }`.  
2. Given JSON string  
   ```json
   {"id": 1, "title": "Hello", "completed": false}
   ```  
   define `struct Todo: Codable { ... }` and decode with `JSONDecoder`.  
3. Handle errors via `do { ... } catch { ... }`, mapping failures to `JSONError`.  

---

## Final Task: Arithmetic expression parser and evaluator  
**Goals:** enums, recursion, string parsing, precedence and error handling  

1. Define  
   ```swift
   enum Expr {
     case number(Double)
     case add(Expr, Expr)
     case subtract(Expr, Expr)
     case multiply(Expr, Expr)
     case divide(Expr, Expr)
   }
   enum ParseError: Error { case invalidSyntax, divisionByZero }
   ```  
2. Write `func parse(_ input: String) -> Result<Expr, ParseError>` implementing a **recursive-descent parser** that handles parentheses and operator precedence ((+, -, *, /)).  
3. Write `func evaluate(_ expr: Expr) -> Result<Double, ParseError>` that:  
   - Recursively computes each node.  
   - Returns `.failure(.divisionByZero)` if division by zero is encountered.  
4. Test your parser/evaluator with expressions like  
   - `"3 + 5 * (2 - 4) / 2"` = Result \(-2.0\)  
   - `"10 / (5 - 5)"` = `.failure(.divisionByZero)`  
5. Extend to support unary minus and exponentiation (^).  

---

By the end of these exercises you’ll have practiced:

- Swift’s loop constructs, ranges and strides  
- Function signatures, default and variadic parameters and optional unwrapping  
- String and collection APIs, closures and higher-order functions  
- Protocols, generics and error handling with `Codable`  
- Building a small parser/evaluator with enums, recursion and robust error handling  

Happy coding!
