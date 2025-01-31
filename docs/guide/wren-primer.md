#![](../images/luxe-dark.svg){width="96em"}

## Introduction to Wren scripting

Scripting in luxe is done with the [Wren](https://wren.io/) programming language.   
Wren is a class based language, which should be familiar if you're used to other languages.   

This page isn't intended as a programming tutorial,   
but rather a quick start guide for getting started with Wren in luxe.

## String interpolation

A useful thing to start with, is that within a string,    
you can put Wren expressions inside of a `%( )` section to combine the values.    

```js
var value = "luxe"
Log.print("hello %(value)")     //prints "hello luxe"
Log.print("hello %(5 + 5 - 2)") //prints "hello 8"
```

## Annotations

Classes and variables use `#annotations` to signal information to the engine or language.

The most common useful one would be `:::js #doc = "A documentation string"`. 

```js
#doc = "Whether or not to display the thing"
var visible : Bool = true
```

You should add documentation to methods, variables and classes whenever you can. These show up in
code completion as well as other places. You can also use the raw string (longer string) syntax 
for documentation. Here's a full example:

```js
#doc="""
  Attach a `Sprite` modifier to `entity`, drawn using `image`,
  with a given size of `width`x`height`.

    ```js
    var entity = Entity.create(world)
    var image = Assets.image("luxe: image/logo")
    Sprite.create(entity, material, 128, 128)
    ```
"""
#args(
  entity = "The entity to attach the `Sprite` to",
  image = "The image to display on the sprite",
  width = "The width in world units for the sprite",
  height = "The height in world units for the sprite"
)
foreign static create(entity: Entity, image: Image, width: Num, height: Num) : None
```

## A note on typing

The version of Wren in luxe allows optional typing, and the completion handles inferring types.

`:::js var num: Num = 5` can often be written as `:::js var num = 5` and still have a known type.

Types allow error checking when calling methods, and accidental typos on variables. While the
typing isn't fully whole just yet, it continues to improve. Aim to always type your code, 
especially at the API edges.  

A variable is typed like this:   
`:::js var name: Type`   

And a method is typed like this:   
`:::js method(param: Type) : Return { }`

## A basic class

```js
class Hello {
  construct new() {
    Log.print("hello")
  }
}

//prints:
// 'hello'
var hello = Hello.new()
```

Some useful notes: 

- `construct` is the keyword for a constructor
- The `new` is on the right hand side, instead of `new Hello()`, it's `Hello.new()`
- That's because `new` is just a function name! `construct create` is fine, or as you'll see in the luxe game class, `construct ready`
- You call a constructor on the class itself, via the class name

## Basic functions

```js
class Hello {
  construct new() { Log.print("hello") }
  //simple method
  luxe() { Log.print("luxe") }
}

//prints:
// 'hello'
// 'luxe'
var hello = Hello.new()
//call simple method
hello.luxe()
```

- That `simple method` is called `luxe`, it requires no keywords
- It is a class method, and is called on an _instance_ of the class, not the class itself

## Variables

Variables in luxe will be familiar as well.
Scope works as you'd expect, with local variables and class variables.

```js
class Hello {

  //explicit class fields must come first in the class
  //and must be initialized. can use any expression!
  var value = 3

  construct new() {

    //a class field that is private
    _private = 0

  }

  print() {

    var local = "cannot be seen outside this scope"
    Log.print(local)

    //we can access our value variable from here,
    //because it belongs to this class
    value = value + 2
    //prints 5
    Log.print(value)
    //also prints 5, as _value is a private field declared by `var value`
    Log.print(_value)
    //prints 0
    Log.print(_private)
    //prints null, prefer declaring explicit fields instead for errors
    Log.print(_not_defined)

  }
}
```

- local variables work as expected!
- class variables _**are typically explicitly defined**_
- class fields can be local only using the regular wren _field syntax
- explicit class fields also declare an underscore getter

## Getters and setters

Private fields in a class **cannot be seen from outside**.    
In other languages terms: all private variables (non explicit var) are private to this class only.

In order to make our values accessible from outside, we make them available first.
We do that with getters and setters, which gives us read/write access control as well.

```js
class Hello {

  //automatic form
  //generates `auto { _auto }`
  //and `auto=(v) { _auto=v }`
  var auto = true

  //manual short form
  value { _value }
  value=(new_value) { _value = new_value }

  //long form, read only
  other_value {
    return _other_value
  }
  
  construct new() { 
    _value = 4
    _other_value = 5
  }

}

var hello = Hello.new()
    hello.value = 6             //update value
Log.print(hello.value)       //prints 6
Log.print(hello.other_value) //prints 5
```

- Explicit class var fields declare a getter/setter for you, making them public
- A getter is a method without arguments, i.e no `()` after the name
- A setter is a method with `=(one_arg)`, where the incoming value is passed in
- Short form is a convenient way to say "return whatever I put in a single line", like these:
- `number { 3 }`, `string { "string" }`, `list { [] }` 
- (This comes in really handy later as you'll see)
- If you need to use more than one line/one expression, you must use `return` 
- note that `other_value` doesn't have a setter, it can't be set from outside

## Static methods and variables

Static methods work largely the same as the class instance ones,   
except for the static keyword up front. Like in other languages,   
you can't access class variables or methods from static methods.

```js
class Hello {
  //static getter
  static get { 5 }
  //static setter
  static set=(value) { Log.print(value) }
  //static method
  static method() {
    Log.print("static method!")
  }
}

Log.print(Hello.get) //prints 5
Hello.set = "hello"     //prints hello
Hello.method()          //prints "static method!"
```

- You call the methods directly on the class itself

Static variables work the same as class variables, except for an extra underscore.

`__static_value` instead of `_value`


```js
class Hello {
  //static getter
  static get { __value }
  //static setter
  static set=(value) { __value = value }
  //static method
  static init() {
    __value = 5
  }
}

Hello.init()
Log.print(Hello.get) //prints 5
Hello.set = 6           //prints hello
Log.print(Hello.get) //prints 6
```

## Functions 

Wren can store functions in variables and pass them to other functions and methods.
The distinction between functions and methods is important.

```js
var notify = Fn.new() {
  Log.print("you have been notified!")
}

notify.call()
```

- note that we use `notify.call()` and not `notify()`

**Arguments**   

Arguments in a function using the `|arg1, arg2, arg3|` syntax.

```js
var notify = Fn.new() {|message|
  Log.print("you have been notified, message: %(message)")
}

notify.call("the message!")
```

## Passing functions to methods

You can pass functions to other functions or methods.
Note that **you cannot pass a method this way**. 

```js
class Hello {
  construct new() {
    _function = null
    _name = null
  }
  set_function(name, fn) {
    _name = name
    _function = fn
  }

  call_function(value) {
    if(_function != null) _function.call(value) 
  }
}

var hello = Hello.new()

//we can store a function in a variable, and pass it into the method
var variable = Fn.new() {|value| Log.print(value) } 
hello.set_function("name", variable)
hello.call_function(5) //print 5

//We can also pass a function directly
hello.set_function("name", Fn.new() {|value| Log.print(value + 8) })
hello.call_function(8) //print 16
``` 

## Function short form syntax

Wren has some special syntax for functions to make them more succinct.   
This relates to the "short form: where a single expression is a return value" mentioned before as well. 
Using the same example as before, but with the short form for a function:

```js
hello.set_function("name") {|value|
  Log.print("hello %(value)")
}
```

How does this work?

- `Fn.new {|value| }` becomes `{|value| }`   

When does this work?

- It works **only** when the last argument accepts a function
- It works when calling `Fn.new` which we saw before!
- The below example doesn't work, we have to use a variable or a longer form

```js
  set_function(fn, name) {
    _name = name
    _function = fn
  }
  
  ...
  
  //requires the long form!
  hello.set_function(Fn.new(){|value| }, "name")
```

## Why short form is nice

In Wren, a lot of methods accept a function. 
A good example is the [Sequence](https://wren.io/modules/core/sequence.html) class. 

Notice how compact the usage of the `where` function is. 
It uses both short forms: A single expression returns `true` or `false`, inside a short form function. 
The `where` function is defined as `where(predicate)`, since it's only one argument, the last argument, it's valid to use the short form.

```js
var list = [1,2,3,4]
var even = list.where {|value| value % 2 == 0 }
Log.print(even.toList) //prints [2, 4]
```

Many many functions are much nicer this way 

```js
["one", "two", "three"].each {|word| Log.print(word) }
```

---

## Try Wren online

You can try Wren in your browser or on your phone, just in case you didn't know.

https://wren.io/try/