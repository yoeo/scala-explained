# Functions

A function is a block of code that can be called.
Scala support a wide range of function related features including
**anonymous** functions, **higher order** functions, **currying**, etc...

That is why Scala is a great language for **functional programming**.

* TOC
{:toc}

## Defined functions

A function can be explicitly defined using the keyword `def`.
The defined function can take zero to many parameters
and it can return zero to one value.

* A function **without parameters**:

  ```scala
  // Retrieve the status of the current task.

  // define `status` function that takes no parameter and returns a `String`
  def status: String = "Task done"

  // call `status` function without parenthesis `()`
  val currentStatus = status

  println(currentStatus)
  // --> Task done
  ```

* A function **with multiple parameters**:

  ```scala
  // Retrieve the status of a given task.

  // `getStatus` function takes 2 parameters and returns a `String`
  def getStatus(taskID: Int, sucessRate: Double): String =
    s"Task $taskID done, with ${100 * sucessRate}% of success"

  val taskStatus = getStatus(666, 0.75)

  println(taskStatus)
  // --> Task 666 done, with 75.0% of success
  ```

### Functions parameters with default values

You can set default values to function parameters.
The function can then be called without giving a value to these parameters.

Example:

```scala
// Display a workflow.

// `workflow` function has 2 parameters `something` and `somewhere`
// that have a default value
def workflow(
    item: String,
    something: String = "clean",
    otherThing: String,
    somewhere: String = "memory"): Unit =
  println(
    s"take $item, do $something and $otherThing then store into $somewhere")

workflow("a value", otherThing = "reverse")
// --> take a value, do clean and reverse then store into memory
```

## Anonymous functions

A function can be create on the fly without the keyword `def`.
This kind of function is called an **anonymous function**
or a **lambda function**:

```scala
// Double a value.

// create an anonymous function that multiplies a value by two
val computeDouble = (x: Int) => x * 2

// call the function
val result = computeDouble(22)

println(result)
// --> 44
```

An anonymous function can take zero to several parameters.

* An anonymous function **without parameters**:

  ```scala
  // Retrieve the result.

  // an anonymous function with no parameters
  val getResult = () => "result"

  // the function is called using parenthesis `()`
  val result = getResult()

  println(result)
  // --> result
  ```

* An anonymous function with one parameter:

  ```scala
  // Print a text with HTML bold tag.

  // an anonymous function with one parameter
  val toHtmlBold = (value: String) => s"<b>$value</b>"

  val html = toHtmlBold("Welcome")

  println(html)
  // --> <b>Welcome</b>
  ```

* An anonymous function with **multiple parameters**:

  ```scala
  // Format a text template.

  // an anonymous function with multiple (two) parameters
  val format = (message: String, value: Int) => message.format(value)

  val text = format("Eat %d potatoes", 12)

  println(text)
  // --> Eat 12 potatoes
  ```

> Anonymous functions can take **up to 22 parameters**.

### Currying

Anonymous functions can be defined with a shortened syntax
using underscore `_` symbol.
The `_` is placed where a parameter of the function is used.
That is called **currying**.

Here is an example with the same function `divideByFour` function
written **with** and **without** currying:

```scala
// Divide a given number by four.

// no currying here
val divideByFourWithoutCurrying: Int => Int = (x) => x / 4

// currying used to replace the parameter `x`
val divideByFourWithCurrying: Int => Int = _ / 4
```

## Higher order functions

A higher order function is a function that takes an other function
as a parameter, or returns a function as a return value.

* A higher order function that takes **a function as parameters**:

  ```scala
  // Show a pretty text.

  // define a higher order function `beautify`,
  // its parameter `beautifyer` is a function
  def beautify(value: String, beautifyer: String => String): String =
    s"pretty $value => ${beautifyer(value)}"

  // define a classic function that will be sent to the higher order one
  def enclose(value: String): String = s"{ $value }"

  // ...

  // calling `beautify` with `enclose` function as argument
  val prettyCat = beautify("cat", enclose)

  println(prettyCat)
  // --> pretty cat => { cat }

  // ...

  // calling `beautify` with an anonymous function
  val prettyDog = beautify("dog", x => s"(~^_^)~ $x ~(^_^~)")

  println(prettyDog)
  // --> pretty dog => (~^_^)~ dog ~(^_^~)

  // ...

  // calling `beautify` with a curried function
  val prettyHorse = beautify("horse", _.capitalize)

  println(prettyHorse)
  // --> pretty horse => Horse
  ```

* A higher order function that **returns a function**:

  ```scala
  // Compute distances in different coordinate systems.

  // `distanceCalculator` returns a function that compute distances
  def distanceCalculator(system: String): (Double, Double) => Double =
    system match {
      case "cartesian" => (x: Double, y: Double) => math.sqrt(x*x + y*y)
      case "polar" => (a: Double, r: Double) => r
    }

  // produce a function that calculate distances in a Cartesian system
  val getCartesianDistance = distanceCalculator("cartesian")

  val cartesianDistance = getCartesianDistance(3, 4)

  println(s"distance from (0, 0) to (3, 4) is $cartesianDistance")
  // --> distance from (0, 0) to (3, 4) is 5.0

  // create an call a function that compute distances in a polar system
  val polarDistance = distanceCalculator("polar")(math.Pi, 5)

  println(s"distance from (0°, 0) to (π°, 5) is $polarDistance")
  // --> distance from (0°, 0) to (π°, 5) is 5.0
  ```

Scala API proposes a large set of higher order functions
for various uses.

For example the `map` function of collection object applies a given function
to all the element inside the collection and returns a new collection
that contains the result of the operation:

```scala
// Capitalize first names.

val names = List("kader", "sakura")

// call the higher order function `map` with a curried function as an argument
val capitalized = names.map(_.capitalize)

println(capitalized.mkString(" & "))
// --> Kader & Sakura
```

## Functions with multiple parameter lists

In Scala functions can take multiple parameter lists.
This allows syntactic shortcuts like partial parameter application.

* Defining a function with **two parameter lists**:

  ```scala
  // Compute the volume of a parallelepiped.

  // `getVolume` takes two parameter lists
  def getVolume(height: Int, width: Int)(length: Int): Int =
    height * width * length

  // parenthesis around each parameter list
  val volume = getVolume(3, 4)(5)

  println(volume)
  // --> 60
  ```

* One of Scala API functions that has multiple parameter lists `foldLeft`:

  ```scala
  // Compute root mean of a list of values.

  val scores = List(3, 2, 4)

  // calling `foldLeft` a method of `List` class that has two parameter lists
  val squaresSum = scores.foldLeft(0)((sum, value) => sum + value*value)

  val rootMean = math.sqrt(squaresSum / scores.length)

  println(s"root mean of the scores is ${rootMean}")
  // --> root mean of the scores is 3.0
  ```

* The same function as above using currying:

  ```scala
  // Compute root mean of a list of values (curried version).

  val scores = List(3, 2, 4)

  // calling `foldLeft` with a curried function
  val squaresSum = scores.foldLeft(0)(_ + math.pow(_, 2).toInt)

  val rootMean = math.sqrt(squaresSum / scores.length)

  println(s"root mean of the scores is ${rootMean}")
  // --> root mean of the scores is 3.0
  ```

### Partial functions

It is easy to create partial functions from a multiple parameter list function
using currying or type conversion.

* a partial function created with **currying**:

  ```scala
  // List cool jobs.

  val jobs = List("Astronaut", "Physicist")

  // partial function created from a multiple parameter list function `foldRight`
  // using currying `_`
  val foldWithThumbsUp = jobs.foldRight("👍")_

  println(foldWithThumbsUp(_ + " " + _))
  // --> Astronaut Physicist 👍
  ```

* a partial function created with **type conversion**:

  ```scala
  // Choose a value based on a condition.

  // a multiple parameter lists function
  def choice(condition: Boolean)(first: Int, last: Int): Int =
    if (condition) first else last

  // create a partial function from `choice` function
  val chooseFirst: (Int, Int) => Int = choice(true)

  val result = chooseFirst(2, 3)

  println(s"the chosen value is $result")
  // --> the chosen value is 2

  // `select` takes a parameter `choiceFunction` that is compatible with
  // a partial function created from `choice`
  def select(choiceFunction: (Int, Int) => Int, first: Int, last: Int): Unit = {
    val chosen = choiceFunction(first, last)
    println(s"the chosen one is $chosen")
  }

  select(choice(false), 9, 10)
  // --> the chosen one is 10
  ```

## Functions with a variable number of parameters

Functions with a variable number of parameters can be defined by adding `*`
after type of the parameter that will be repeated.
These function are sometimes called **variadic functions**.

It is also possible to send a **list of values as arguments** to
a variadic function by expanding the argument list with `: _*` symbol.

```scala
// Print status messages.

// define a variadic function, the parameter `items` will receive the
// arguments list
def prettyPrint(items: String*): Unit = items.foreach(println)

// call `prettyPrint` with some arguments
prettyPrint("download started", "downloading", "failed")
// --> download started
// --> downloading
// --> failed

// call `prettyPrint` by expanding an arguments list
prettyPrint(List("restarted", "finished"): _*)
// --> restarted
// --> finished
```

## Polymorphic functions

Polymorphic functions are **generic functions** whose arguments type
or return value type is a **type variable**.

The type variables are replaced by a concrete types
when the function is called. The concrete types are **inferred** from
the parameters or the return type.

```scala
// Format a list of values using a dot as separator.

// `dot` is a generic function that uses a type variable `A`,
// `dot` also has a variable number of parameters
def dot[A](items: A*): String = items.mkString(".")

// `A` becomes `String` because the arguments are `String` values
val hostname = dot("www", "example", "org")

println(hostname)
// --> www.example.org

// `A` becomes `Int` because the arguments are `Int` values
val ipAddress = dot(127, 0, 0, 1)

println(ipAddress)
// --> 127.0.0.1
```

## By-name parameters

By name parameters are function parameters that are **evaluated every time
they are used**. If they are never used they will never be evaluated:

```scala
// Repeat a statement a defined number of times.

// runs `block` by name parameter a multiple number of times
def repeat(times: Int)(block: => Unit): Unit =
  (0 until times).foreach(_ => block)

repeat(3) {
  val status = "task ongoing..."
  println(s"status: $status")
}
// --> status: task ongoing...
// --> status: task ongoing...
// --> status: task ongoing...
```

## Definition annotations

Scala annotations are statements starting with `@` symbol that associates
information to an element definition.

Scala API defines several annotations `@deprecated`, `@tailrec`, `@throws`,
etc...
To create new annotations it is recommended to implement them
in **Java language**.

```scala
// Give π value.

def π: Double = 3.1415

// added `deprecated` decoration to the function,
// to specify that the function is deprecated
@deprecated("use π method", "version 2.0")
def pi: Double = 3.14

var radius = 5
var circumference = 2 * pi * radius

// run with -deprecation
// --> warning: method pi is deprecated (since version 2.0): use π method
```
