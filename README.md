# Introduction to Swift Programming

Welcome! In this workshop you'll deepen your understanding of Swift's core features, loops and ranges, optionals, collections, closures, generics, protocols, error handling and parsing, by tackling a series of increasingly challenging exercises. We'll be using SwiftFiddle (no local install needed), but you can also apply these in Xcode Playgrounds or a Swift Package.

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

## Exercise 7: Property Wrappers and Custom Attributes  
**Goals:** `@propertyWrapper`, computed properties and custom validation  

1. Create a property wrapper `@Clamped` that constrains values to a range:  
   ```swift
   @propertyWrapper
   struct Clamped<T: Comparable> {
     private var value: T
     private let range: ClosedRange<T>
     
     var wrappedValue: T {
       get { value }
       set { value = max(range.lowerBound, min(range.upperBound, newValue)) }
     }
     
     init(wrappedValue: T, _ range: ClosedRange<T>) {
       self.range = range
       self.value = max(range.lowerBound, min(range.upperBound, wrappedValue))
     }
   }
   ```
2. Create a `Player` struct with `@Clamped(0...100) var health: Int` and `@Clamped(1...99) var level: Int`.  
3. Test that values outside ranges are automatically clamped.  
4. **Bonus:** Create `@Capitalized` property wrapper that automatically capitalizes string values.  

---

## Exercise 8: Memory Management and Reference Cycles  
**Goals:** ARC, `weak`/`unowned` references, and avoiding memory leaks  

1. Create classes `Company` and `Employee` with a potential retain cycle:  
   ```swift
   class Company {
     let name: String
     var employees: [Employee] = []
     init(name: String) { self.name = name }
     deinit { print("\(name) company deallocated") }
   }
   
   class Employee {
     let name: String
     var company: Company?
     init(name: String) { self.name = name }
     deinit { print("\(name) employee deallocated") }
   }
   ```
2. Demonstrate the retain cycle by creating instances and observe they don't deallocate.  
3. Fix using `weak var company: Company?` in `Employee`.  
4. Create a `Manager` class that has `unowned let company: Company` and explain when to use `unowned` vs `weak`.  
5. **Bonus:** Create a closure-based scenario with `[weak self]` capture lists.  

---

## Exercise 9: Advanced Protocol Features and Type Erasure  
**Goals:** associated types, `Self` requirements, type erasure with `AnySequence`  

1. Define a protocol with associated types:  
   ```swift
   protocol Container {
     associatedtype Item
     var count: Int { get }
     mutating func append(_ item: Item)
     subscript(i: Int) -> Item { get }
   }
   ```
2. Implement `Container` for a generic `Stack<T>` struct.  
3. Create a protocol with `Self` requirements:  
   ```swift
   protocol Copyable {
     func copy() -> Self
   }
   ```
4. Try to store different `Container` conforming types in an array - observe the compiler error.  
5. Create type erasure with `AnyContainer<Item>` to solve this:  
   ```swift
   struct AnyContainer<Item>: Container {
     // Implementation using closure-based type erasure
   }
   ```

---

## Exercise 10: KeyPaths and Dynamic Programming  
**Goals:** `KeyPath`, `WritableKeyPath`, dynamic property access and KVO-style patterns  

1. Define a `User` struct with properties `name`, `email`, `age`.  
2. Create functions that work with KeyPaths:  
   ```swift
   func getValue<T, V>(from object: T, keyPath: KeyPath<T, V>) -> V
   func setValue<T, V>(on object: inout T, keyPath: WritableKeyPath<T, V>, to value: V)
   ```
3. Create a generic `Observer<T>` class that can watch changes to specific properties:  
   ```swift
   class Observer<T> {
     private var object: T
     private var observers: [PartialKeyPath<T>: (T) -> Void] = [:]
     
     func observe<V>(_ keyPath: KeyPath<T, V>, onChange: @escaping (V) -> Void)
     func update(_ newObject: T)
   }
   ```
4. **Bonus:** Implement a simple "data binding" system using KeyPaths.  

---

## Exercise 11: Result Builders and DSL Creation  
**Goals:** `@resultBuilder`, creating domain-specific languages, and advanced Swift meta-programming  

1. Create a simple HTML builder using result builders:  
   ```swift
   @resultBuilder
   struct HTMLBuilder {
     static func buildBlock(_ components: HTMLElement...) -> [HTMLElement]
     static func buildOptional(_ component: [HTMLElement]?) -> [HTMLElement]
     static func buildEither(first component: [HTMLElement]) -> [HTMLElement]
     static func buildEither(second component: [HTMLElement]) -> [HTMLElement]
   }
   
   protocol HTMLElement {
     var html: String { get }
   }
   ```
2. Implement `div`, `p`, `h1` structs conforming to `HTMLElement`.  
3. Create a function that uses the builder:  
   ```swift
   func html(@HTMLBuilder content: () -> [HTMLElement]) -> String
   ```
4. Test with nested structures:  
   ```swift
   let page = html {
     h1("Welcome")
     div {
       p("This is a paragraph")
       p("This is another paragraph")
     }
   }
   ```
5. **Bonus:** Add support for attributes and conditional rendering with `buildOptional`.  

---

## Exercise 12: Protocols & generics  
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

## Exercise 13: Error Handling & decodable  
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

By the end of these exercises you'll have practiced:

- Swift's loop constructs, ranges and strides  
- Function signatures, default and variadic parameters and optional unwrapping  
- String and collection APIs, closures and higher-order functions  
- **Property wrappers and custom attributes for data validation**  
- **Memory management with ARC, weak/strong references and retain cycles**  
- **Advanced protocol features, associated types and type erasure**  
- **KeyPaths for dynamic property access and meta-programming**  
- **Result builders for creating domain-specific languages**  
- Protocols, generics and error handling with `Codable`  
- Building a small parser/evaluator with enums, recursion and robust error handling  

Happy coding!
