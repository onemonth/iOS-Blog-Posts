
##Swift Optionals

###Intro

Optionals are a powerful feature of the Swift programming language. But they can be difficult to understand and use effectively. Let's review the essentials.

Optionals work in tandem with variables. A variable is a construct that we use to hold onto a piece of data. At a minimum we usually give a variable a name, type and value:

```Swift
// Here we explicitly specify the type
var myName: String = "Fred"

// But the type can also be inferred from the fact that we're assigning it a String value
var myName = "Fred"
```

###vars and lets

Specifically, Optionals work in tandem with two distinctly different kinds of variables: `vars` and `lets`. In order to understand Optionals we must first understand `vars` and `lets`.

The primary distinction between `vars` and `lets` is that `vars` are mutable and `lets` are immutable. In other words, we can change the value of a `var` as many times as we want, but once we set the value of a `let`, we can never change it. Let's look at some examples.

```Swift
// vars

var name = "Fred" // "Fred"

name = name + " Velvet" // "Fred Velvet"

name = "Freddy" // "Freddy"

name = nil // Compiler error (this is where Optionals come into play)

// lets

let name = "Fred"

name = name + " Velvet" // Compiler error

name = "Freddy" // Compiler error

name = nil // Compiler error (this is where Optionals come into play)
```

As you can see the compiler enforces these mutability rules for us. And in doing so it allows us to be explicit about our intentions. If we used `vars` alone we'd be wandering into a lawless land. Mad Max territory. Variables that we intend to be immutable might unintentionally be mutated. But in this day and age we can elect to use `vars` and `lets` where appropriate, and proceed with confidence.

###Optionals

Optionals add a layer of complexity to `vars` and `lets`. They modify `vars` and `lets` to make a distinction between variables whose value can be either something or nothing, and variables whose value can be something but never nothing. In the context of Swift, `nil` is equivalent to nothing, the absence of a value. Let's look at some examples:

```Swift
// vars

var name: String? = "Fred" // ???

name = nil // ???

name = "Freddy" // ???

name = name + " Velvet" // ???

name = nil // ???

// lets

let name: String? = "Fred"

name = name + " Velvet" // Compiler error

name = "Freddy" // Compiler error

name = nil // Ok
```

Note the addition of a question mark to our variable declaration. The question mark signifies that the variable is an Optional, a variable that can be something (a value) or nothing (`nil`). Without the question mark these variables can never be `nil`. Note that declaring a variable as Optional does not effect its mutability. `vars` remain mutable and `lets` remain immutable.

So this means that we have four different kinds of variables at our disposal.

- `vars` (mutable, never nil)
- Optional `vars` (mutable, nilable)
- `lets` (immutable, never nil)
- Optional `lets` (immutable, nilable)

###In practice

What does this look like in practice?

How to unwrap?

They empower us to write clearer and more stable code and empower the compiler to better help us achieve this goal.
