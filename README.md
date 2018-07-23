# Tips and tricks for iOS developers

# Contribution guidelines
* Clearly explain the given problem & solution
* Write a [short code snippet](https://help.github.com/articles/creating-and-highlighting-code-blocks/) demonstrating the solution
* Make sure the problem is generic in nature, i.e. not copy pasted from a private project
* Create a pull request and assign it to [fajdof](https://github.com/fajdof), [tamaramilisa](https://github.com/tamaramilisa), or [d-srd](https://github.com/d-srd)

## Optionals

### Use `Optional.map` and `Optional.flatMap` to avoid unnecessary unwrapping

This:

```swift
let foo: Int? = 6
let bar: Double?

if let foo = foo {
    bar = Double(foo)
} else {
    bar = nil
}
```

becomes this:

```swift
let foo: Int? = 6
let bar = foo.map(Double.init)
```

This:

```swift
let foo: String? = "4"
let bar: Int?

if let foo = foo {
    bar = Int(foo)
} else {
    bar = nil
}
```

becomes this:

```swift
let foo: String? = "4"
let bar = foo.flatMap(Int.init)
```

The difference between `Optional.map` and `Optional.flatMap` lies in their return types. These are their respective function signatures:
`public func map<U>(_ transform: (Wrapped) throws -> U) rethrows -> U?`
`public func flatMap<U>(_ transform: (Wrapped) throws -> U?) rethrows -> U?`

Using `Optional.map` on a function which returns an `Optional` (e.g. `Double.init(_ text: StringProtocol)`) will make it a double `Optional`. For example:

The type of bar here will be `Double??`

```swift
let foo: String? = "4"
let bar = foo.map(Double.init)
```

The type of bar here will be `Double?`

```swift
let foo: String? = "4"
let bar = foo.flatMap(Double.init)
```

### Use `Collection.compactMap` to transform a collection of `Optional<T>` to a collection of `T`

```swift
// foo has a type of Array<Optional<Int>>, i.e. [Int?]
let foo = [5, 4, nil, 2, nil, nil]

// this works, but it's inefficient — O(n^2) — not to mention ugly
// bar has a type of [Int]
let bar = foo.filter { $0 != nil }.map { $0! }

// clean, efficient
// baz has a type of [Int]
let baz = foo.compactMap { $0 }
```
