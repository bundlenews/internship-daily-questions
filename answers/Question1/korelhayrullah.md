# Swift - Several Ways of Unwrapping Optionals

##### By Korel Hayrullah

##### [GitHub]( https://github.com/korelhayrullah)

##### [LinkedIn](https://www.linkedin.com/in/korel-chairoula-238882121)

### What is an Optional?

Optional values either contain some value or doesn't contain any value at all. In Swift programming language the use of optionals are convenient and makes the programming language flexible yet powerful.

To define an optional simply put a "?" or "!" after the type.

Example:

```swift
let someOptionalValueOfTypeInt: Int? = 5
```

This indicates that the variable ```someOptionalValueOfTypeInt: Int?``` may contain an integer, in which in this case it contains a 5, or may not containt any value at all which is ```nil```.

To better understand the use of optionals, think of a case where you have an application that lets you search the wheather forecast for a specific location and shows the relevant data according to the given location. As soon as you type a location for searching there is an API call that is done asynchronously and the data may not be shown due to lost internet connection, slow internet connection, corrupted data and etc. Using optionals for this case will let you know whether the relevant info has returned or not. So, the development phase becomes pretty easy and the neccessary actions can be taken in order to solve this problem.

A simple example case to comprehend:

```swift
struct Weather {
    let temperature: Int
    let day: String
}


let _weaklyForecastForIstanbul = getWeatherForecast(for: "Istanbul")

if let weaklyForecastForIstanbul = _weaklyForecastForIstanbul {
    // show the daily forecast in a UITableView ... 
} else {
    // the value returned nil
    // do other stuff needed
}

func getWeatherForecast(for: String) -> [Weather]? {
    // some API call ...
}
```

### 8 ways for unwrapping

##### Way 1 - Force Unwrapping

Example:

```swift
// Example
let optionalInt: Int? = 5
// let optionalInt: Int? = nil
print(optionalInt!)
```

**Is it safe? ** :-1:

**P.S. Should be avoided as much as possible.**

##### Way 2 - Implicitly Unwrapping

Example:

```swift
let optionalChar: Character! = "a"
print(optionalChar)
```

**Is it safe** :-1:

##### Way 3 - Safety Check

Example:

```swift
let optionalString: String? = "Value"
//let optionalString: String? = nil

if optionalString != nil {
  print(optionalString!)
} else {
  print("The variable named optionalString is nil!")
}
```

**Is it safe? ** :+1:

##### Way 4 - Optional Binding

Example: 

```swift
let optionalDouble: Double? = 54.4
//let optionalDouble: Double? = nil

if let unwrappedOptionalDouble = optionalDouble {
  print(unwrappedOptionalDouble)
} else {
  print("The variable named optionalDouble is nil!")
}
```

**Is it safe? ** :+1:

##### Way 5 - Guard

Example:

```swift
let optionalCGFloat: CGFloat? = 90.452
//let optionalCGFloat: CGFloat? = nil

guard let unwrappedOptionalCGFloat = optionalCGFloat else { return }
print(unwrappedOptionalCGFloat)
```

**Is it safe? ** :+1:

##### Way 6 - Nil Coalescing

Example:

```swift
let optionalFloat: Float? = 3.14
//let optionalFloat: Float? = nil
print(optionalFloat ?? 0.0)
```

**Is it safe? ** :+1:

##### Way 7 - Optional Chaining

Example:

```swift
struct Wallet {
  let cash: Double
  let creditCard: CreditCard?
}

struct CreditCard {
  let name: String
  let amount: Double
}

let myWallet = Wallet(cash: 50.50, creditCard: nil)

if let amountOfCreditCard = myWallet.creditCard?.amount {
  print(amountOfCreditCard)
} else {
  print("You don't own any credit card!")
}
```

**Is it safe? ** :+1:

##### Way 8 - Optional Pattern

Example:

```swift
let optionalNames: [String?] = ["Alice", nil, nil, "Peter", "Jack", nil, "Jasmine"]

// will print only the non nil values
for case .some(let name) in optionalNames {
  print(name)
}
// Outputs

// Alice
// Peter
// Jack
// Jasmine


let anotherOptional: Double? = 43.2

if case .some(let amount) = anotherOptional {
  print(amount)
}

if case let amount? = anotherOptional {
  print(amount)
}


// Another example with switch-case

// you can change the value to see different results
let everythingPossible: Any? = "I am a string!"

switch everythingPossible {
case .some(let value as String):
  print("The value \(value) is of type String.")
case .some(let value as Double):
  print("The value \(value) is of type Double.")
case .some(let value as Character):
  print("The value \(value) is of type Character.")
case .some(let value as Int):
  print("The value \(value) is of type Int.")
case .some(let value):
  print("The value \(value) can be everything :D")
case .none:
  print("The value is nil!")
}

// prints "The value I am a string! is of type String."
```

**Is it safe? ** :+1:

###### Note: Way 4 and 5 can also be type casted while safely-unwrapping the optional value.

Example:

```swift
let sentence = "I am a sentence."

// substrings is type of [String.SubSequence]
let substrings = sentence.split(separator: " ")

// lastWord is an optional (the last property returns an optional, see: https://developer.apple.com/documentation/swift/array/1689973-last)
let lastWord = substrings.last

// unwrappedLastWord is now casted as String and unwrapped

if let unwrappedLastWord = lastWord as? String {
  print(unwrappedLastWord)
}

guard let unwrappedLastWord = lastWord as? String else { return }
print(unwrappedLastWord)
```



*Benefitted from:*

*<http://www.figure.ink/blog/2015/12/6/matching-with-swifts-optional-pattern>*

*<https://swift.org/documentation/>*

*<https://gist.github.com/patricklynch/689525d466c4ada42b8e>*

*<https://medium.com/@CoderLyn/unwrapping-optionals-let-me-count-the-ways-db03b1dd5b0b>*

