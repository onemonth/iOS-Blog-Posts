
##Swift Optionals

Optionals are a powerful feature of the Swift programming language, but they can be difficult to understand and use effectively. Let's review the essentials.

###At a high level, what are Optionals? 

Let's say a mutual friend is throwing a Halloween party. The invitation outlines the pertinent details (e.g. time and location) and among other things it says, "bring a beverage, costumes optional." Now if we wanted to represent this party and its guests in code, we might create a `Party` object and a `Guest` object, and we might translate the above details into properties on these classes. However, in order to accurately represent this Halloween party we want to express the fact that costumes are optional. We do this like so:

```Swift
class Party
{
    var time: NSDate
    var location: CGPoint
    var guests: Array<Guest>
    
    // ...
}

class Guest 
{
    var beverage: String // Each guest must have a beverage
    var costume: String? // Each guest can choose to come in costume or plain clothes (i.e. costumes optional)
    
    // ...
}
```

In Swift, the `?` is how we designate a variable as Optional. And this just means that it's okay for the variable to have a value (`costume = "Frankenstein"`) or to not have a value (`costume = nil`). In contrast, every guest must have a beverage, this is expressed by the absence of a `?`, and the compiler will enforce this for us.  

###A little background

Optionals work in tandem with variables. A variable is a construct that we use to hold onto a piece of data. At a minimum we usually give a variable a name, type and value.

```Swift
// Here we explicitly specify the type
var name: String = "Fred"

// But the type can also be inferred from the fact that we're assigning it a String value
var name = "Fred"
```

###vars and lets

Swift offers us two distinctly different kinds of variables: `vars` and `lets`. In order to understand Optionals we must first understand `vars` and `lets`.

The primary distinction between `vars` and `lets` is that `vars` are mutable and `lets` are immutable. In other words, we can change the value of a `var` as many times as we want, but once we set the value of a `let`, we can never change it. Let's look at some examples.

```Swift
// vars

var mutableName = "Fred"
println(mutableName) // "Fred"

mutableName = "Freddy"
println(mutableName) // "Freddy"

mutableName = mutableName + " Velvet"
println(mutableName) // "Freddy Velvet"

// lets

let immutableName = "Fred"
println(immutableName) // "Fred"

immutableName = "Freddy" // Compiler error

immutableName = immutableName + " Velvet" // Compiler error
```

Test these out in a [Swift Playground](http://www.objc.io/issue-16/rapid-prototyping-in-swift-playgrounds.html) and you'll see that the compiler enforces these mutability rules for us. And in doing so it forces us to be explicit about our intentions. If we used `vars` alone we'd be wandering into a lawless land. Mad Max territory. Variables that we intend to be immutable might unintentionally be mutated. But in this day and age we can elect to use `vars` and `lets` where appropriate, and proceed with confidence.

###Optionals

Optionals add a layer of complexity to `vars` and `lets`. They modify `vars` and `lets` to make a distinction between variables whose value can be either something or nothing, and variables whose value can be something but never nothing. In the context of Swift, nothing is expressed with `nil`, the absence of a value. Let's look at some examples.

```Swift
// Non-optional vars

var mutableName = "Fred"
println(mutableName) // "Fred"

mutableName = "Freddy"
println(mutableName) // "Freddy"

mutableName = mutableName + " Velvet"
println(mutableName) // "Freddy Velvet"

mutableName = nil // Compiler error

// Optional vars

var mutableName: String? = "Fred"
println(mutableName) // Optional("Fred")

mutableName = "Freddy"
println(mutableName) // Optional("Freddy")

mutableName = mutableName! + " Velvet"
println(mutableName) // Optional("Freddy Velvet")

mutableName = nil // nil

// Non-optional lets

let immutableName = "Fred"
println(immutableName) // "Fred"

immutableName = "Freddy" // Compiler error

immutableName = immutableName + " Velvet" // Compiler error

immutableName = nil // Compiler error

// Optional lets

let immutableName: String? = "Fred"
println(immutableName) // Optional("Fred")

immutableName = "Freddy" // Compiler error

immutableName = immutableName! + " Velvet" // Compiler error

immutableName = nil // Compiler error

let anotherName: String? = nil 
println(anotherName) // nil
```

Note the addition of a question mark to our variable declaration. The question mark signifies that the variable is an Optional, a variable that can be something (a value) or nothing (`nil`). Without the question mark these variables can never be `nil`. Note that declaring a variable as Optional does not effect its mutability. `vars` remain mutable and `lets` remain immutable.

Also note the use of an exclamation mark in select locations. The exclamation mark is a heavy-handed way to [unwrap](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/OptionalChaining.html) i.e. gain access to the value that an optional variable holds. 

###Unwrapping Optionals

Allowing variables to be either something or nothing injects risk into our programs. If we assume a variable is something and it turns out to be nothing, our program might behave unpredictably or worse yet crash. To mitigate this risk and avoid having to make assumptions, we want to know definitively whether a variable is something or nothing before working with it. Anyone who has worked with Objective-c is familiar with this upfront runtime check. 

```Objective-c
NSString *name = @"Alfie";

// ...

if (name == nil) {
    // Do something
} else {
    // Do something else
}
```

Swift Optionals allow this check to happen at compile-time. If we attempt to use an optional variable without [unwrapping](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/OptionalChaining.html) it, the compiler will throw an error instructing us to first unwrap the value. Which means that the compiler will never let us use an optional value without first being certain that it is something or nothing. 

We can safely unwrap optionals like this:

```Swift
var name: String? = "Alfie"
println(name) // Optional("Alfie")

name = name + " Hanssen" // Compiler Error

if let something = name {
    // The value is not nil, use it with confidence
    something = something + " Hanssen"
    println(something) // "Alfie Hanssen"
} else {
    // The value is nil
}
```

A heavy-handed way to unwrap an optional is to make use of the exclamation mark. This is called force unwrapping:

```Swift
var name: String? = "Alfie"
println(name) // Optional("Alfie")

name = name! + " Hanssen" 
println(name) // Optional("Alfie Hanssen")
```

The former is greatly preferred to the latter. By force unwrapping the optional we're asserting that we're positive the variable is not nil. But if we're wrong our program will crash. Force unwrapping puts the onus back on us humans to keep track of what is nil and what is not, and in doing so brings all of the risk and assumptions back into our code. 

###Optionals in Practice

So when do we use Optionals? We use Optionals when a `nil` value is meaningful. 

Consider the case of a model object in our iOS app that represents a video on our server. If the video's `image_url` property is non-`nil` we know it has a thumbnail image that we can use. And if it's `nil` we know it doesn't. 

Or consider the case of an `NSError` object passed as an argument to a network call's completion handler. Not every network call will return an error, so it makes sense for this error object to be Optional.

On the other hand, our program might make use of a `Person` object. And it might require that every `Person` have an `age`. In this scenario `age` would be non-optional, passed into the `Person` constructor and never allowed to be nil.  

###In Summary

In Swift, we have four different kinds of variables at our disposal:

- `vars` (mutable, never nil)
- Optional `vars` (mutable, nilable)
- `lets` (immutable, never nil)
- Optional `lets` (immutable, nilable)

This simple toolset empowers us to write clearer code and more stable programs. And it allows the compiler to be a better partner in helping us achieve these goals.
