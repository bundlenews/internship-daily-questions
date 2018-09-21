# Swift - What are closures and what are they used for? Are closures value or reference types?

##### By Korel Hayrullah

##### [GitHub]( https://github.com/korelhayrullah)

##### [LinkedIn](https://www.linkedin.com/in/korel-chairoula-238882121)

### What are closures?

A wrapped code block that can be used around in code can be defined as a closure. Constants and variables defined in that block can be captured by the closure, however, Swift programming language handles the memory management. Global and nested functions are special cases of closures. Closures allow us to write shorter and neater code without any incoherencies. 

A closure can be defined as follows:

```swift
{(paramters) -> return type in
 statements
}
```

######Examples of usage areas:

The sort function of swift uses the closures for sorting the values according to the need.

For example sorting numbers from the in ascending order is expressed as: 

```swift
var numbers: [Int] = [5, 2, 6, 1 , 9 ,8 ,2, 4, 10]

let sortedNumbersArray = numbers.sorted { (n1, n2) -> Bool in
  return n1 < n2
}

print(sortedNumbersArray)
// prints [1, 2, 4, 5, 6, 7, 8, 9, 10]
```

In the example above ```{n1, n2} -> Bool in return n1 < n2 }``` is a closure that determines whether the first numbers should be smaller than the second. Since Swift can infer types, making the syntax more clear, the n1 and n2 are automatically inferred as ```Int```. The above example is the exact same writing a function that returns our desired order.

```swift
import Foundation

var numbers: [Int] = [5, 2, 6, 1 , 9 ,8 ,2, 4, 10]

func incrementedOrder(_ n1: Int, _ n2: Int) -> Bool {
    return n1 < n2
}

let sortedNumbersArray = numbers.sorted {by: incrementedOrder }

print(sortedNumbersArray)
// prints [1, 2, 4, 5, 6, 7, 8, 9, 10]
```

However, writing closures makes it simpler to read and looking nifty :smile:

Swift programming language can infer the type from its context. As mentioned above, n1 and n2 are automatically inferred as ```Int``` and even more, the return type can also be omitted since the sort function expects something like ```(Int, Int) -> Bool```. So if we were going to rewrite the function above, it would be something like this doing the exact same job as the 2 examples above.

```swift
import Foundation

var numbers: [Int] = [5, 2, 6, 1 , 9 ,8 ,2, 4, 10]

let sortedNumbersArray = numbers.sorted(by: {n1, n2 in return n1 < n2})

print(sortedNumbersArray)
// prints [1, 2, 4, 5, 6, 7, 8, 9, 10]
```

The closure above can be shortened a little bit more :grimacing:. It is done by writing with shorthand argument names.

```swift
import Foundation

var numbers: [Int] = [5, 2, 6, 1 , 9 ,8 ,2, 4, 10]

let sortedNumbersArray = numbers.sorted(by: {$0 < $1})

print(sortedNumbersArray)
// prints [1, 2, 4, 5, 6, 7, 8, 9, 10]
```

The ```$0``` and ```$1``` refers to the first and second element of the array respectively which are inferred as ``` Int``` and the greater sign infers to the ```Bool``` value to be returned.

One last trick to show off. The above code can be simplified even further, thanks to the Swift programming language's [operator models](https://docs.swift.org/swift-book/LanguageGuide/AdvancedOperators.html#ID42). The < operator has the method defined for taking two parameters of any type (inferred by the language) and a return value which in our case it fits perfectly.

```swift
import Foundation

var numbers: [Int] = [5, 2, 6, 1 , 9 ,8 ,2, 4, 10]

let sortedNumbersArray = numbers.sorted(by: <)

print(sortedNumbersArray)
// prints [1, 2, 4, 5, 6, 7, 8, 9, 10]
```

:point_right: You can define the operators whatever you want to them to do such as in this case.

:exclamation: As the documentation states, omitting the types are highly discouraged for other readers of your code. It might look cleaner and a clever for the writer, however, for the reader it might not that simple to understand your code. However, in this case, since it is a simple sorting function and its purpose is known the omitting of the types can be done. :exclamation:

###Functions and closures are reference types

In the example below, inspired by the documentation, the code shows that closures are of type reference. The code below, makes an incrementer and has a nested function that takes no parameters and returns an ```Int```. The parameters for the nested function are captured from the outer block and there is no need to provide the ```total``` and ```amount``` for the function to operate. Going on, it creates a decrementer, for a decrement amount of 5. After 2 executions of the  ```decrementByOne``` the total value is returned is -10. Assigning the ```decrementByOne``` to another constant and calling it proves us that closures are reference types.

```swift
import Foundation

func makeDecrementer(forDecrement amount: Int) -> () -> Int {
  var total = 0
  func decrementer() -> Int {
    total -= amount
    return total
  }
  return decrementer
}

let decrementByOne = makeDecrementer(forDecrement: 5)

print(decrementByOne())
// prints -5
print(decrementByOne())
// prints -10

let anotherDecrementByOne = decrementByOne
print(anotherDecrementByOne())
// prints -15

```



Note that parameters of functions and closures are always constants. For example, in the code above the ```forDecrement amount: Int``` is a constant and cannot be changed. If the value ```amount``` needs to be changed you have to assign it to a ```var``` before use.

If you want to read more about closures and **trailing closures** and **capturing values** click [here](https://docs.swift.org/swift-book/LanguageGuide/Closures.html).

*Benefitted from:*

*<https://docs.swift.org/swift-book/LanguageGuide/Closures.html>*
