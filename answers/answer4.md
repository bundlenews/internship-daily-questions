# Swift - What is the difference between weak and strong?

##### By Korel Hayrullah

##### [GitHub]( https://github.com/korelhayrullah)

##### [LinkedIn](https://www.linkedin.com/in/korel-chairoula-238882121)

### Strong reference cycles between class instances

Before we jump into **weak** we need to know how **strong** references occur between classes. The  Automatic Reference Counter (ARC) can automatically deinitialize the class instances for us, however, when two classes have a strong reference to each other, the ARC gets fooled and can not deinitialize the instances. This happens because the reference counter for the referencing class instance never reaches to zero strong references. Therefore, in order to make the ARC work we need to define a `weak` or `unowned` reference type in order to break this strong relationsip in which we will explain them afterwards.

An example how to class instances can keep strong references to each other:

```swift
import Foundation

class Person {
  var name: String
  var pet: Pet?
  
  init(name: String) {
    self.name = name
    print("A new person -> \(name) intialized")
  }
  
  deinit {
    print("Person -> \(name) deinitalized")
  }
}

class Pet {
  var type: String
  var name: String
  var owner: Person?
  
  init(name: String, type: String) {
    self.name = name
    self.type = type
    print("A new pet -> \(name) intialized")
  }
  
  deinit {
    print("Pet -> \(name) deintialized")
  }
}

var person: Person?
var pet: Pet?

person = Person(name: "Korel Hayrullah")
// prints A new person -> Korel Hayrullah intialized
pet = Pet(name: "Jack", type: "Dog")
// prints A new pet -> Jack intialized

// setting the pet to the owner and the person to the pet to acquire a strong reference
person?.pet = pet
pet?.owner = person


person = nil
pet = nil
// prints nothing
```

As you can see in the example above the deinitializers of the classes aren't getting triggered and that is because of the strong releationship of the instances.

### Weak references

We define a weak reference with the `weak` keyword. The `weak` keyword allows us to break the strong relationship and whenever the referencing instance gets deallocated the ARC automatically sets its value to `nil`. Therefore, the `weak` keyword needs to be set as an optional variable in order to set it to `nil` in runtime. If we slightly change the example above we can break the strong reference with adding the `weak` keyword in front of the `var pet: Pet?` in the `Person` class. 

```swift
import Foundation

class Person {
  var name: String
  weak var pet: Pet?
  
  init(name: String) {
    self.name = name
    print("A new person -> \(name) initialized")
  }
  
  deinit {
    print("Person -> \(name) deinitialized")
  }
}

class Pet {
  var type: String
  var name: String
  var owner: Person?
  
  init(name: String, type: String) {
    self.name = name
    self.type = type
    print("A new pet -> \(name) initialized")
  }
  
  deinit {
    print("Pet -> \(name) deinitialized")
  }
}

var person: Person?
var pet: Pet?

person = Person(name: "Korel Hayrullah")
// prints A new person -> Korel Hayrullah initialized
pet = Pet(name: "Jack", type: "Dog")
// prints A new pet -> Jack initialized
person?.pet = pet
pet?.owner = person

pet = nil
// prints Pet -> Jack deinitialized
person = nil
// prints Person -> Korel Hayrullah deinitialized
```

### Unowned references

We define an unowned reference with the `unowned` keyword. Unowned is used when the referencing instance has the same or longer lifetime. That is, the `unowned` is defined as a non-optional value since it is not set to `nil` unlike `weak` references. Keep in mind that if the `unowned` declared reference needs to be accessed after being deinitialization, it will cause a runtime error. Example: 

```swift
import Foundation

class Person {
  var name: String
  var identityCard: IdentityCard?
  
  init(name: String) {
    self.name = name
    print("The person -> \(name) initialized")
  }
  
  deinit {
    print("The person -> \(name) deinitialized")
  }
}

class IdentityCard {
  var name: String
  var idNumber: UInt64
  unowned var owner: Person
  
  init(idNumber: UInt64, owner: Person) {
    self.name = owner.name
    self.idNumber = idNumber
    self.owner = owner
    
    print("The unique id card for person \(name) and identification number \(idNumber) initialized")
  }
  
  deinit {
    print("The unique id card for person \(name) and identification number \(idNumber) deinitialized")
  }
}


var person: Person?

person = Person(name: "Korel Hayrullah")
// prints
// The person -> Korel Hayrullah initialized
person!.identityCard = IdentityCard(idNumber: 1234567890, owner: person!)
// prints
// The unique id card for person Korel Hayrullah and identification number 1234567890 initialized

person = nil
// prints 
// The person -> Korel Hayrullah deinitialized
// The unique id card for person Korel Hayrullah and identification number 10239019203 deinitialized
```



### Closures and resolving strong reference cycles 

As mentioned above about strong reference cycles, in cases where there is a closure assigned to a property, its body captures the instance. When the body accesses the instance class itself (`self`), a strong reference cycle is being established. This happens because of closures being references like classes. When strong references are established, as mentioned above we need to break that reference using `weak` or `unowned`.  Let's see an example without breaking the strong relationship, causing a memory leak.

```swift
import Foundation

class PhoneNumber {
  var countryCode: Int
  var phoneNumber: Int
  
  
  lazy var completePhoneNumber: () -> String = {
    return "+\(self.countryCode)\(self.phoneNumber)"
  }
  
  init(countryCode: Int, phoneNumber: Int) {
    self.countryCode = countryCode
    self.phoneNumber = phoneNumber
    print("Phonenumber initialized.")
  }
  
  deinit {
    print("Phonenumber deinitialized.")
  }
}

var phonenumber: PhoneNumber?

phonenumber = PhoneNumber(countryCode: 90, phoneNumber: 555_555_55_55)
// prints Phonenumber initialized.

print(phonenumber!.completePhoneNumber())
// prints +905555555555

phonenumber = nil
// prints nothing
```

As can be seen, when `phonenumber` is set to `nil` the `phonenumber` instance doesn't gets deinitialized and that is because of the strong reference cycle of the closure in that class instance. To solve that, `weak` or `unowned` keywords are added in the [capture list](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#ID544). However, in these cases, we need to be sure how to use these keywords. Use `weak` whenever the captured value might be `nil` at some time and remind that using `weak` allows us to check its existence before use since it is an optinal value. On the other side, the keyword `unowned` is used whenever you are sure that the captured value will have a value that is not nil. We used `unowned` in our case, since we know that the captured instance will never have a `nil` value during its lifetime.   

```swift
import Foundation

class PhoneNumber {
  var countryCode: Int
  var phoneNumber: Int
  
  
  lazy var completePhoneNumber: () -> String = { [unowned self] in
    return "+\(self.countryCode)\(self.phoneNumber)"
  }
  
  init(countryCode: Int, phoneNumber: Int) {
    self.countryCode = countryCode
    self.phoneNumber = phoneNumber
    print("Phonenumber initialized.")
  }
  
  deinit {
    print("Phonenumber deinitialized.")
  }
}

var phonenumber: PhoneNumber?

phonenumber = PhoneNumber(countryCode: 90, phoneNumber: 555_555_55_55)
// prints Phonenumber initialized.

print(phonenumber!.completePhoneNumber())
// prints +905555555555

phonenumber = nil
// prints Phonenumber deinitialized.


```

Now, when we set `phonenumber` to `nil` you can see that the deinitializer of that class instance is going to work and the memory will be deallocated.



*Benefitted from:*

*<https://docs.swift.org/swift-book/LanguageGuide/AutomaticReferenceCounting.html>*

*<https://www.raywenderlich.com/959-arc-and-memory-management-in-swift>*
