# Swift - How Is Memory Management Handled in iOS?

#####By Korel Hayrullah

##### [GitHub]( https://github.com/korelhayrullah)

##### [LinkedIn](https://www.linkedin.com/in/korel-chairoula-238882121)

In Swift programming language, there is a system called Automatic Reference Counting (ARC) and automatically manages the memory the application uses. It traces the application and manages the class instances in which frees up the memory that are no longer needed. Remember that, structures and enumerations are value types and ARC only applies to reference types which are instances of classes. However, there are some cases where the ARC needs to know the relationship between these instances of classes in order to manage the memory without leaks.



### The working logic of Automatic Reference Counting (ARC)

Every time a class is instantiated the ARC allocates a block of memory to hold that instance in memory and whenever that instance is no longer needed it deallocates that block of memory and frees up the memory space for another uses. However, there are some cases where a class instance may have a property referencing to another class instance. In such cases, this reference is called a **strong reference** and when this reference gets deallocated, it may cause the application to crash. ARC makes sure that all the referencing class instance's relationsips are deallocated before deallocating the referencing instance. In such cases, when the relationship between instances are not handled carefuly, memory leaks occur and are not good for the application's life cycle. 



Example:

```swift
import Foundation

class Car {
  let model: String
  
  init(model: String) {
    self.model = model
    print("A new car with the model \(self.model) is created.")
  }
  
  deinit {
    print("The car with the model \(self.model) is destroyed.")
  }
}


var newCar: Car? = Car(model: "BMW")
// prints A new car with the model BMW is created.
newCar = nil
// prints The car with the model BMW is destroyed.

```

In the example above, a new car with the model "BMW" is created and held in an optional `var` called `newCar`  in order to make it nillable. Whenever the optional is set to `nil` the deinitializer of the car class gets triggered and `newCar` referencing to the car instance gets freed up by the ARC. However, in the example below, if we add another reference to that car and set `newCar` to `nil`, ARC won't deinitialize that instance since there is another variable holding a strong reference to that car which is `carReference1`. As soon as, `carReference1` is set to `nil` the deinitializer gets called and ARC deinitializes the car instance with the model "BMW".

```swift
import Foundation

class Car {
  let model: String
  
  init(model: String) {
    self.model = model
    print("A new car with the model \(self.model) is created.")
  }
  
  deinit {
    print("The car with the model \(self.model) is destroyed.")
  }
}


var newCar: Car? = Car(model: "BMW")
// prints A new car with the model BMW is created.
var carReference1: Car? = newCar

newCar = nil
// prints nothing

carReference1 = nil
// prints The car with the model BMW is destroyed.
```



**Benefitted from:**

*<https://docs.swift.org/swift-book/LanguageGuide/AutomaticReferenceCounting.html>*