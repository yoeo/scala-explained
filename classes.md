# Classes

Classes are templates for new objects.

* TOC
{:toc}

## Available structures

In Scala language there are several structures that are related to classes.
The structures are **classes**, **traits**, **abstract classes**,
**case classes** and **objects**.

The following table show the main features of each class-like structures:

| | init params | instantiable | extensible |
--------------------------------- | - | - | -
[class](#class)                   | ✔ | ✔ | ✔
[trait](#trait)                   | ✘ | ✘ | ✔
[abstract class](#abstract-class) | ✔ | ✘ | ✔
[case class](#case-class)         | ✔ | ✔ | ✔
[object](#object)                 | ✘ | ✘ | ✘

> **Additional features**:
>
> * [Objects](#object) are not types, they are type instances.
> * You can create a class that extends as much [trait](#trait) as you want.
> * But your new class can extend only one [class](#class),
>   [case class](#case-class) or [abstract class](#abstract-class)

### Structure to use depending on the context

Which type of class to use for which purpose:


The goal | The structure to use
--------------------------------- | - | - | -
Create a **singleton** | use an [object](#object)
Create instances that **store information** | use a [case class](#case-class)
Create **instances** that share the same features | use a [class](#class)
Create a base class for **related classes** | use an [abstract class](#abstract-class)
Create a base class for **unrelated classes** | use a [trait](#trait)
Not sure... | use a [trait](#trait) first then change later if necessary

## Class

A class is a structure that can be instantiated and extended.
Class instances can be created with initialization arguments.

* Creating an empty class:

  ```scala
  // `Empty` is an empty class
  class Empty

  // create an instance of the empty class
  val item = new Empty
  ```

* Define a class can extend an other class:

  ```scala
  // Format URL values.

  // the base class `BaseUrl`
  class BaseUrl(val protocol: String, val location: String) {
    def fullUrl: String = s"$protocol://$location"
  }

  // the extended class `HttpsUrl`, the initialization parameter are passed
  // to the base class
  class HttpsUrl(override val location: String)
    extends BaseUrl("https", location)

  // create an instance
  val url = new HttpsUrl("www.example.com")

  println(url.fullUrl)
  // --> https://www.example.com
  ```

* Create a class that has attributes:

  ```scala
  // Display information about tomato.

  // the class that contains everything about tomato
  class Tomato(fresh: Boolean, var price: Double = 1) {
    // `fresh` is implicitly defined as a `private value` here
    // it cannot be accessed from outside of the class
    // nor modified inside the class

    def inflatePrice(multiplier: Int): Unit =
      price *= multiplier

    override def toString: String = fresh match {
      case true => s"fresh tomato at €$price per kilo"
      case false => s"rotten tomato at €$price per kilo"
    }
  }

  // create an instance
  val tomato = new Tomato(true, 1.5)

  println(s"tomato current price is €${ tomato.price }")
  // --> tomato current price is €1.5

  // run the instance's method `inflatePrice`
  tomato.inflatePrice(2)

  println(tomato)
  // --> fresh tomato at €3.0 per kilo
  ```

### Attribute modifiers: `private` and `protected`

Class attribute visibility can be tuned using **attributes modifiers**:

* `private`: the attribute can only be used in the
  **class where it is defined**.
* `protected`: the attribute can be used in the class where it is defined
  and **its derived classes**.
* **no modifier**: the attribute defined without modifiers is
  a public attribute and can be used **everywhere**.

Example:

```scala
// Print robot journalist articles.

class Bot {
  // a `private` member that can only be accessed in the current class
  private val commandAndControlServer: String = "bot.example.com"

  // a `protected` member that can be used in the current class
  // and its derived classes
  protected val author: String = "a Robot"

  // a member that is `public` by default
  val date: String = "today"
}

class NewsBot extends Bot {
  // the protected member `author` is used in the class that extends `Bot`
  def writeArticle: String = s"This is good news ── $author ($date)"
}

val botJournalist = new NewsBot

println(botJournalist.writeArticle)
// --> This is good news ── a Robot (today)
```

> The class initialization **attributes visibility** can also be set:
> * `class ClassName(val x: Int)`: the attribute is **public**.
> * `class ClassName(protected val x: Int)`: the attribute is **protected**.
> * `class ClassName(private val x: Int)`: the attribute is **private**.
> * `class ClassName(x: Int)`: the attribute is also **private**.

### Class member access control: getter and setter

To control the access to a private class member,
Scala supports the following syntax:

```scala
// Display the high scores of a game.

// the game class
class Game {
  // creates a private variable
  private var _highScore: Long = 0

  // create a getter for the private variable `_highScore`
  def highScore = _highScore

  // create a setter for the private variable `_highScore`
  def highScore_= (newScore: Long): Unit =
    if (newScore > _highScore) _highScore = newScore
}

val game = new Game

// run the setter
game.highScore = 13

// run the getter
val currentHighScore = game.highScore

println(s"New high score is $currentHighScore")
// --> New high score is 13

// run the setter again (the given value is discarded this time)
game.highScore = 1

// run the getter again
val actualHighScore = game.highScore

println(s"New high score is $actualHighScore")
// --> New high score is 13
```

### Define multiple class constructors

Classes has a member named `this` that is bound to the current object.
`this` member is also used to define constructors:

```scala
// Store some text.

// `TextHolder` default constructor takes a `String` parameter
class TextHolder(val text: String) {

  // an alternative constructor that calls the default constructor
  def this(base: String, repeat: Int) = this(base * repeat)

  // an other constructor that calls the first alternative one
  def this() = this("A", 3)
}

val a = new TextHolder
val b = new TextHolder("A", 3)
val c = new TextHolder("AAA")

val allSame = (a.text == b.text && b.text == c.text)

println(s"${ if (allSame) "same" else "not same"} value")
// --> same value
```

### Calling super class attributes `super`

`super` keyword gives access to the members of the super classes:

```scala
// Convert inches to metres.

// base class
class Converter {
  def inchToMeters(x: Double): Double = x * 2.54 / 100
}

// derived class
class SimpleConverter extends Converter {
  override def inchToMeters(x: Double): Double =
    // `inchToMeters` method from the base class is called through `super`
    super.inchToMeters(x).round
}

val converter = new SimpleConverter

println(s"a 40″ TV as a diagonal >= ${ converter.inchToMeters(40) } m")
// --> a 40″ TV as a diagonal >= 1.0 m
```

### Class modifiers: `final` and `sealed`

* A `final` class is a class that **cannot be extended**:

  ```scala
  // Filling bankruptcy.

  final class Bankruptcy {
    def action: String = "calling lawyer..."
  }

  val companyState = new Bankruptcy

  println(companyState.action)
  // --> calling lawyer...
  ```

* A `sealed class` is a class that can only be extended by classes that are
  **defined in the same file**:

  ```scala
  // Define an escape plan.

  // the sealed class that can only be extended in the current file
  sealed class EscapePlan {
    def exit: String = "to be defined..."
  }

  // all the existing escape plans are defined here, in this file
  class PlanA extends EscapePlan {
    override def exit: String = "roof"
  }

  class PlanB extends EscapePlan {
    override def exit: String = "window"
  }

  val plan = new PlanB

  println(s"escaped through the ${ plan.exit }")
  // --> escaped through the window
  ```

> `abstract` is also a class modifier, see [abstract classes](#abstract-class).

### Callable instance and assignment expansion: `apply` and `update`

A `class` will produce callable instance if it has a member named `apply`:
* `instance(...)` is the same as `instance.apply(...)`

A value can be assigned to a `class` instance call if the `class` has
a member named `update`. This is known as **assignment expansion**:
* `instance(...) = value` is the same as `instance.update(value, ...)`

Example:

```scala
// Work with a two-dimension matrix.

import scala.collection.mutable.{Map => MutMap}

class Matrix(x: Int, y: Int) {
  val map: MutMap[(Int, Int), Int] = MutMap()

  private def checkBounds(i: Int, j: Int): Boolean =
    i >= 0 && i < x && j >= 0 && j < y

  // makes the instances callable
  def apply(i: Int, j: Int): Int =
    if (checkBounds(i, j)) map.getOrElse((i, j), 0) else 0

  // allows assignment expansion
  def update(i: Int, j: Int, newValue: Int) =
    if (checkBounds(i, j)) map.update((i, j), newValue)
}

// create a matrix with bounds 0 <= x <= 4 and 0 <= y <= 4
val matrix = new Matrix(4, 4)

// implicitly call `apply` to retrieve the value
val valueAtPositionOne = matrix(1, 1)

println(s"matrix[1, 1] = $valueAtPositionOne")
// --> matrix[1, 1] = 0

// implicitly call `update` to update the value
matrix(2, 2) = 4

// retrieve the updated value
val valueAtPositionTwo = matrix(2, 2)

println(s"matrix[2, 2] = $valueAtPositionTwo")
// --> matrix[2, 2] = 4

// indices 10 are out of bounds, the change is dropped by `update` method
matrix(10, 10) = 100

// retrieve the default value, because indices 10 are out of bounds
val valueAtPositionTen = matrix(10, 10)

println(s"matrix[10, 10] = $valueAtPositionTen")
// --> matrix[10, 10] = 0
```

## Abstract class

An `abstract class` is a class that **cannot be instantiated**
but can take **initialization parameters**. A [class](#class) can only extend
one abstract class.

* An empty `abstract class`

  ```scala
  // an empty abstract class
  abstract class EmptyAbstract

  // a class that extends the empty abstract class
  class Empty extends EmptyAbstract
  ```

* An abstract class with initialization parameters:

  ```scala
  // Calculate the probability of winning.

  // an abstract class that takes two initialization parameters
  abstract class Fraction(val numerator: Long, val denominator: Long) {
    override def toString: String = s"$numerator/$denominator"
  }

  // extend the abstract class by setting the values
  // of the initialization parameters
  class Probability(val value: Double, val precision: Long = 100)
    extends Fraction((precision * value).round, precision)

  val win = new Probability(0.7)

  println(s"Winning probability is $win")
  // --> Winning probability is 70/100
  ```

## Trait

A `trait` is a structure close to a class that **cannot be instantiated**
and therefore, has no initialization parameters.

A [class](#class) can **extend** one or several `traits`.

* Creating an empty trait:

  ```scala
  // an empty trait
  trait EmptyTrait

  // a class that extends the empty trait
  class EmptyClass extends EmptyTrait
  ```

* Extending **multiple traits**:

  ```scala
  // Manage processes.

  trait Runnable {
    def run: Unit = println("Running fast...")
  }

  trait Stopable {
    def stop: Unit = println("Stop now!")
  }

  trait Restartable {
    def restart: Unit = println("Restarting*")
  }

  // create a class that extends all the traits defined above
  class Process extends Runnable with Stopable with Restartable

  val process = new Process

  process.run
  // --> Running fast...

  process.stop
  // --> Stop now!

  process.restart
  // --> Restarting*
  ```

* Using a trait as a **common type** for specialized classes:

  ```scala
  // Show different kind of boxes.

  trait Box {
    val size: Int
    def isBig: Boolean = size > 10
  }

  // `size` parameter must be a public value to match the definition
  // inside `Box` trait
  class ColoredBox(color: String, val size: Int) extends Box
  class BouncingBox(jumpHeight: Int, val size: Int) extends Box

  // `Box` trait represents all its specialized classes
  val boxes = Map[String, Box](
    "red box" -> new ColoredBox("red", 0),
    "top bouncing box" -> new BouncingBox(18, 12)
  )

  boxes.foreach {
    case (name, box) => println(
      s"a ${ if (box.isBig) "big" else "small" } $name"
    )
  }
  /* -->
  a small red box
  a big top bouncing box
  */
  ```

### Trait mixed-in pattern

**Trait mixed-in** is a design pattern where a feature is injected
into a [class](#class) through a [trait](#trait).
To be able to do that both `class` and `trait` should extend the same base
`trait`.

Example:

```scala
// Display screen specs.

// base trait with a method `specs` that is not fully defined
trait Screen {
  def specs: String
}

// a class that fully defines `specs` method
class LcdMonitor(val monitorType: String) extends Screen {
  def specs = s"$monitorType monitor"
}

// a trait with a method that uses `specs`. It will be mixed-in `LcdMonitor`
trait ScreenWall extends Screen {
  def fullSpecs(nbScreens: Int) = s"Wall made with $nbScreens $specs"
}

// generate a new class by mixing the two structures above
class HighResLcdWall extends LcdMonitor("4K") with ScreenWall

val wall = new HighResLcdWall

println(wall.fullSpecs(16))
// --> Wall made with 16 4K monitor
```

### Stackable trait pattern

Stackable trait pattern is a programming pattern where a functionality
is split into multiple components (traits) then assembled by extending a class.

With `abstract override` it is possible to partially define a method
in a trait, the rest of the implementation is executed through its super class
using `super.parentMethod(...)`.

This feature is used to implement the stackable trait pattern:

```scala
// Create a database engine.

trait Engine {
  def store(entry: Int, dataset: String): Unit
}

trait SQLEngine extends Engine {
  /** Modifies the `dataset` parameter then calls an unknown implementation
    * of the method.
    */
  abstract override def store(entry: Int, dataset: String): Unit =
    super.store(entry, s"sql://$dataset")
}

trait Database extends Engine {
  /** This is the implementation that is called by the `abstract override`
    * variant of this method.
    */
  override def store(entry: Int, dataset: String): Unit =
    println(s"stored $entry in $dataset")
}

// stacking the traits
class SQLDatabase extends Database with SQLEngine

val database = new SQLDatabase

database.store(5, "prices")
// --> stored 5 in sql://prices
```

### Instantiate combined traits

Instances can be created by **combining multiple traits** and classes
to create a type on the fly. This is done using `with` statement.

Example:

```scala
// Create a range object.

trait RangeStart {
  def start: Int = 0
}

trait RangeEnd {
  def end: Int = 10
}

// object created by combining two traits
val range = new RangeStart with RangeEnd

println(s"from ${ range.start } to ${ range.end }")
// --> from 0 to 10
```

### Instantiate a trait using an anonymous class

An instance can be created from a trait by **fully defining all its member**.
To define these members you can create an anonymous class
that extends the trait.

Example:

```scala
// Print greetings.

trait Greet {
  // trait member `greetings` is not fully defined
  def greetings: String
}

// instance of a class created on the fly that extends `Greet` trait
val instance = new Greet {
  // `greetings` is defined here
  def greetings: String = "Greetings to the world!"
}

println(instance.greetings)
// --> Greetings to the world!
```

## Case class

A `case class` is a [class](#class) created primary to store data.
`case class` instances have the following features:
1. They support pattern matching.
1. They can be compared with each other instances `a == b`,
1. They produce a readable message when printed, ex: `Point(1,2)`.
  
* Defining an empty `case class`:

  ```scala
  // an empty case class
  case class Empty() // parenthesis required

  // case class instantiation
  val item = Empty // no `new` keyword
  ```

* Creating a `case class` with multiple attributes and default values:

  ```scala
  // View buildings information.

  // a case class with 2 attributes
  case class Building(nbFloors: Int, builtIn: String = "1950")

  val smallOldBuilding = Building(3)

  println(s"This building was built in ${ smallOldBuilding.builtIn }")
  // --> This building was built in 1950
  ```

* Compare `case class` instances:

  While [class](#class) instances are compared **by reference**
  (the compared instances are at the same location in the memory),
  `case class` instances are compared **by structure**
  (all the attributes are the same).

  ```scala
  // Check if it's really hot.
 
  case class Temperature(celcius: Int)

  val hot = Temperature(45)
  val notCold = Temperature(45)

  // compare two instances: different references
  // but same structure `celcius=45`
  val same = hot == notCold

  println(s"${ if (same) "really" else "possible" } hot weather")
  // --> really hot weather
  ```

### Copy and modify a `case class` instance

Case `case class` instances have a built-in method `copy` that
creates a **copy of the instance**.

The new instance attributes values can be modified through the `copy` method.

```scala
// Sell fruits.

case class Fruit(name: String, price: Double)

// the original instance
val mango = Fruit("mango", 1)

// the copy, `price` attribute is modified
val expensiveMango = mango.copy(price = 10)

println(s"this ${ expensiveMango.name } costs ${ expensiveMango.price } €")
// --> this mango costs 10.0 €
```

### Extract `case class` instance parameters

`case class` allows the extraction of instance parameters:

```scala
case class VacationPlace(landscape: String, ratings: Int)

val location = VacationPlace("heavenly beach", 5)

// extracting `location` instance parameters into `place` and `stars` values
val VacationPlace(place, stars) = location

println(s"Going to a $stars stars $place.")
// --> Going to a 5 stars heavenly beach.
```

### Other `case class` features

`case class` instances expose the following methods:

* `instance.productPrefix`: get the `case class` name.
* `instance.productArity`: get the number of parameters of the `case class`.
* `instance.productIterator`: get an iterator through `case class` name.
* `instance.productElement(index)`: retrieve a parameter value
  from its position.

Example:

```scala
case class Movie(title: String, budget: Double, rating: Int)

val blockBuster = Movie("Triple-W 4 The Prequel", 500e6, 7)

// retrieve the number of fields
val nbFields = blockBuster.productArity

// get the `case class` name
val className = blockBuster.productPrefix

println(s"$className has $nbFields fields")
// --> Movie has 3 fields

// retrieve an instance parameter value
val rating = blockBuster.productElement(2)

println(s"$className rating is $rating/10")
// --> Movie rating is 7/10

// iterate through instance parameters
val fields = for (field <- blockBuster.productIterator) yield s"- $field"

println(s"""
  Full $className info:
  ${ fields.mkString("\n  ") }
""")
/* -->
  Full Movie info:
  - Triple-W 4 The Prequel
  - 5.0E8
  - 7
*/
```

## Object

An `object` is a singleton instance.
Unlike [classes](#class), objects are not types and cannot be instantiated.

* An empty object:

  ```scala
  // create an empty `object`
  object EmptyObject

  // copy the reference to the `object`
  val x: EmptyObject.type = EmptyObject
  ```

* An `object` with members:

  ```scala
  // Get a database server configuration.

  // create an object with two members
  object Database {
    def server: String = "localhost"
    def port: Int = 3306
  }

  println(s"Database server port is ${ Database.port }")
  // --> Database server port is 3306
  ```

### The `main` method

Scala program entry point is a method named `main`
that should be defined inside an `object`.

The `main` method must have the exact signature:
`main(args: Array[String]): Unit`

Example:

```scala
// Run a super fast software.

object SuperFastSoftware {
  // define the program entry point
  def main(args: Array[String]): Unit = {
    println("Already completed!")
  }
}
// --> Already completed!
```

### Importing `object` members

An `object` **member can be imported** in an other file
or an other execution context using the following syntax:
`import packagename.ObjectName.objectMember`

That is demonstrated in the example bellow.
The example requires two files and can be executed using `sbt` tools:

```scala
// Running a worker.

// ---

/** file 1: project/Worker.scala
  *
  */

package project

object Worker {
  // define a method inside the object that will be imported in an other file
  def doWork: Unit = println("Working!")
}

// ---

/** file 2: project/Main.scala
  *
  */

package project

// importing the member of the object `Worker`
import Worker.doWork

object Main {
  def main(args: Array[String]): Unit = doWork
}
// --> Working!
```

### Companion `object` and companion `class`

A **companion `object`** and a **companion `class`** are created when
a [class](#class) and an `object` share the same name.
The class can use the object members as if they where **static members**
of the class.

* Companion `object` & companion `class` example:

  ```scala
  // Convert speed.

  // the companion `class`
  class Speedometer(speedMetersPerSecond: Long) {
    // importing the object members
    import Speedometer._

    override def toString: String = {
      // using the `object` method as a static method
      val speedMetersPerHour = convert(speedMetersPerSecond)
      s"current speed is $speedMetersPerHour kilometers per hour"
    }
  }

  // the companion `object`
  object Speedometer {
    def convert(speedMetersPerSecond: Long): Long =
      (speedMetersPerSecond * 3600 / 1000).toLong
  }

  println(new Speedometer(100))
  // --> current speed is 360 kilometers per hour
  ```

* Companion `object` as a [class](#class) instance factory:

  ```scala
  // Generate customer entries.

  // companion class
  case class Customer(id: Long)

  // companion object
  object Customer {
    private var currentId: Long = 0

    // method that generates new `Customer` instances
    def next(): Customer = {
      currentId += 1
      new Customer(currentId)
    }
  }

  // generate an instance
  val consumerOne = Customer.next

  println(s"Please enter customer ${ consumerOne.id }")
  // --> Please enter customer 1

  // generate an other instance
  val consumerTwo = Customer.next

  println(s"Please enter customer ${ consumerTwo.id }")
  // --> Please enter customer 2
  ```

### Extractor `object`: `apply` and `unapply`

With the extractor `object` feature it is possible to **call an `object`**
and **extract values from an `object`**:
* `Object(value)` is the same as `Object.apply(value)`
* `val Object(value) = other` is the same as
  `val value = Object.unapply(other)`

This mechanism is used with pattern matching.

* Simple extractor `object`:

  ```scala
  // Display an unique ID.

  object UniqueId {
    val minIdValue: Long = 1000000

    // method that makes the object callable
    def apply(value: Int): Long = minIdValue + value

    // value extraction method
    def unapply(idValue: Long): Option[Int] =
      Some((idValue - minIdValue).toInt)
  }

  // calling the object (through `apply`)
  val fullId: Long = UniqueId(33)

  // extracting `shortID` from `fullID` (through `unapply`)
  val UniqueId(shortId) = fullId

  println(s"the short ID is $shortId")
  // the short ID is 33
  ```

* Using an extractor `object` with pattern matching:

  ```scala
  // Serialize and recover messages.

  // an extractor object 
  object Serialize {
    def apply(value: String) = s"<$value>"
    def unapply(serialized: String): Option[String] = {
      Some(serialized.slice(1, serialized.length - 1))
    }
  }

  // serialize message
  val frozen = Serialize("orange")

  // deserialize message
  frozen match {
    case Serialize(value) => println(s"recovered message: $value")
    case _ => println("message damaged")
  }
  // --> recovered message: orange
  ```

### Object protected members

The `private[this]` modifier is used to create an object members
that is only visible inside the object and cannot be used by a companion class:

```scala
// Store a secret key, protected by a password.

// this class cannot directly access the object private `key` member
// of its companion object
class KeyStore(passPhrase: String) {
  def getKey: Option[Long] = KeyStore.getKey(passPhrase)
}

object KeyStore {
  // only visible inside the object
  private[this] val key = 123456789L

  def getKey(keyPass: String): Option[Long] = keyPass match {
    case "azerty" => Some(key)
    case _ => None
  }
}

val keyStore = new KeyStore("azerty")
val secretKey = keyStore.getKey

println(s"the secret key is $secretKey")
// --> the secret key is Some(123456789)
```

> `protected[this] ...` defines a member that is only visible
> in the current class and its derived classes.
> The companion object cannot access it.

## Outer and inner classes

An **inner class** is a [class](#class) defined inside other classes.
The enclosing class is called **outer class**.

* **Each instance** of the outer class has its own version of the inner class:

  ```scala
  // Manage dashboard metrics.

  // the outer class
  class Dashboard(dashboardName: String) {

    // the inner class
    class Metric(private val name: String) {
      def metricName: String = s"$dashboardName.$name"
    }
  }

  val finance: Dashboard = new Dashboard("finance")

  // the inner class of this instance is `finance.Metric`
  val income: finance.Metric = new finance.Metric("income")

  println(income.metricName)
  // --> finance.income

  val risks: Dashboard = new Dashboard("risks")

  // the inner class of this instance is `risks.Metric`
  // Note that `finance.Metric` and `risks.Metric` are two different types
  val criticalErrors: risks.Metric = new risks.Metric("critical_errors")

  println(criticalErrors.metricName)
  // --> risks.critical_errors
  ```

* Outer class instances that **share the same inner class**:

  ```scala
  // Stream data.

  // the outer class
  class Stream {

    // the inner class
    class Chunk(val content: String)

    // `Stream#Chunk` is the "absolute" type name of the inner class
    // that is not related to an instance
    def receive: Stream#Chunk = new Chunk("keep-alive")

    def send(chunk: Stream#Chunk): Unit = println(s"sent: ${ chunk.content }")
  }

  val inputStream: Stream = new Stream
  val outputStream: Stream = new Stream

  val chunk: Stream#Chunk = inputStream.receive

  outputStream.send(chunk)
  // --> sent: keep-alive
  ```

## Compound types

A **compound type** is a type created by the composition of multiple types:
`TypeA with TypeB with TypeC with...`

The **compound type** reproduces the subtyping relations of
the component classes. In other words:

if `TypeY < TypeX` then `TypeY with TypeZ < TypeX with TypeZ`

* **compound type** example:

  ```scala
  // Run a factory.

  trait Manufacturer {
    def manufacture(rawMaterial: String): String =
      rawMaterial.replaceAll(raw"[^\w\s]", "")
  }

  trait Packer {
    def pack(manufacturedProduct: String): String =
      s"${ manufacturedProduct.capitalize }."
  }

  trait CleanPacker extends Packer {
    override def pack(manufacturedProduct: String): String =
      s"${ manufacturedProduct.toLowerCase.capitalize }."
  }

  class Factory extends Manufacturer with CleanPacker

  // create a compound type associating `Manufacturer` with `Packer`
  // Moreover:
  //  `CleanPacker < Packer` => `Factory < Manufacturer with Packer`
  def run(factory: Manufacturer with Packer, rawMaterial: String): String =
    factory.pack(factory.manufacture(rawMaterial))

  val rawMaterial = "~t*$^His# i~S @ve§R^µy% ;cL€{`}e&Aŀn"
  val factory = new Factory

  val product = run(factory, rawMaterial)

  println(product)
  // --> This is very clean.
  ```

## Self-type mix-in

**self-type mix-in** is a syntax used to inject the members of a class
into an other class.

The instantiation of the resulting type must conform to the `self-type`:

If `ClassA` is mixed into `ClassB` then `ClassB` cannot be instantiated.
`ClassB with ClassA` should be instantiated instead.

```scala
// Get information about a place.

trait Place {
  def name: String
  def location: String
}

trait Reputation {
  // Mix `Place` trait in `Reputation` trait
  this: Place =>

  def stars: Int
  def describe: String = s"$name at $location has $stars stars"
}

// Extend both classes here to satisfy the mix-in requirement:
// `Reputation with Place`
class Recommendation(
  val name: String,
  val location: String,
  val stars: Int) extends Reputation with Place

val reco = new Recommendation("The Music Club", "Lagos", 5)

println(reco.describe)
// --> The Music Club at Lagos has 5 stars
```

## Generic class

A **generic class** is a [class](#class) that takes type parameters.
Generic classes produces an actual class when the type parameter
is replaced by an actual type.

* A generic class example:

  ```scala
  // Decorate a value.

  // create a generic class with the type parameter `T`
  case class Decorator[T](value: T) {
    def decorate: String = s"~{ $value }~"
  }

  // instantiate the class by replacing explicitly `T` by `Long`
  val decoratedNumber = Decorator[Int](1000)

  println(decoratedNumber.decorate)
  // --> ~{ 1000 }~

  // instantiate the class by replacing implicitly `T` by `Char`
  // `Char` being the type of the argument `'X'`
  val decoratedChar = Decorator('X')

  println(decoratedChar.decorate)
  // --> ~{ X }~
  ```

* A [trait](#trait) using a type variable:

  ```scala
  // Display a message multiple times.

  // define a generic trait
  trait Repeat[T] {
    val value: T

    def printTwice: Unit = {
      println(value)
      println(value)
    }
  }

  // extending a variant of the generic trait
  class Message(val value: String) extends Repeat[String]

  val message = new Message("Yes!")

  message.printTwice
  // --> Yes!
  // --> Yes!
  ```

### Type variance: invariant, covariant and contravariant

Let's take two types `Parent` and `Child` that have a subtyping relation
`Child < Parent` and a generic class `Generic[T]`.
The variance of the type `T` defines the relation between
`Generic[Parent]` and `Generic[Child]`:

* If `T` is **invariant** then `Generic[Parent]` has no special relation
  with `Generic[Child]`. 
* If `T` is **covarient** then the subtyping relation is kept,
  `Child < Parent` => `Generic[Child] < Generic[Parent]`.
* If `T` is **contravariant** then the subtyping relation is reversed
  `Child < Parent` => `Generic[Parent] < Generic[Child]`.

By default Scala type variables are **invariant**.
A **covariant** type variable is defined by adding `+` symbol
to the class variable name, ex: `+T`.
On the other hand, a **contravariant** type variable is defined
by adding `-` symbol to its name, ex: `-T`.

* A generic class using a **covariant** type:

  ```scala
  // Embed a value.

  // generic class using a covariant type variable `+T`
  case class Embed[+T](value: T)

  // `Any` is the subtype of any Scala type
  // `Embed[Any]` is then the base type of any `Embed[T]`
  def show(embedded: Embed[Any]): Unit =
    println(s"embedded value: ${ embedded.value }")

  val embeddedMessage = new Embed[String]("secret")

  show(embeddedMessage)
  // --> embedded value: secret

  val embeddedNumber = new Embed(147)

  show(embeddedNumber)
  // --> embedded value: 147
  ```

* A generic class using a **contravariant** type:

  ```scala
  // Build a tree.

  trait TextNode
  trait ImageNode
  trait KnownNode extends TextNode with ImageNode

  // generic abstract class using a contravariant variable `-T`
  abstract class Tree[-T] {
    def info: String
  }

  // subtyping a variant of the generic class `Tree`
  class TextTree extends Tree[TextNode] {
    def info: String = "text-tree"
  }

  // `-T` being contravariant:
  // `KnownNode < TextNode` => `Tree[TextNode] < Tree[KnownNode]`
  val value: Tree[KnownNode] = new TextTree

  println(s"tree type is: ${ value.info }")
  // --> tree type is: text-tree
  ```

### Type upper and lower bounds

Optional **upper** and **lower bounds** define which types can be applied
to the generic class.

* **Upper bound**:

  If a generic class type variable `T` has an **upper bound** `U`,
  then `T` can only be replaced by `U` or the types that extends `U`.

  Example:

  ```scala
  // Calculate the brightness of a room.

  abstract class Room {
    def nbWindows: Int = 2
  }

  class BedRoom extends Room
  class LivingRoom extends Room {
    override def nbWindows: Int = 6
  }

  // `T` can be `Room` or any class that extends `Room`
  class RoomInfo[T <: Room] {
    def getBrightness(room: T): String = room match {
      case r if (r.nbWindows <= 0) => "dark"
      case r if (r.nbWindows <= 5) => "bright"
      case _ => "ultra bright"
    }
  }

  val roomInfo = new RoomInfo[LivingRoom]
  val room = new LivingRoom
  val brightness = roomInfo.getBrightness(room)

  println(s"the room is $brightness")
  // --> the room is ultra bright
  ```

* **Lower bound**:

  If a generic class type variable `T` has an **lower bound** `L`,
  then `T` can only be replaced by `L` or the base types of `L`.

  Example:

  ```scala
  // Handle an error.

  // base class for all errors
  abstract class BaseError {
    override def toString: String = "something happened"
  }

  class RuntimeError extends BaseError {
    override def toString: String = "program crashed"
  }

  class FatalError extends RuntimeError {
    override def toString: String = "system shutdown"
  }

  // `T` can be `FatalError` or any error class that `FatalError` extends:
  // `RuntimeError` or `BaseError`
  class ErrorHandler[T >: FatalError] {
    def alert(error: T): Unit = println(error)
  }

  val error = new RuntimeError
  val handler = new ErrorHandler[RuntimeError]

  handler.alert(error)
  // --> program crashed
  ```

### Using type variance and type bounds together

Here is an example that uses type variance and type bounds at the same time:

```scala
// Compute input data to produce output data.

abstract class Output(val data: String)

class RichOutput(override val data: String) extends Output(data)

abstract class BaseInput(data: String) {
  def compute: RichOutput =
    new RichOutput(data.replace("-", "..."))
}

class Input(data: String) extends BaseInput(data) {
  override def compute: RichOutput =
    new RichOutput(data.replace("-", ""))
}

/** ComputingUnit type variables bounds and variance.
  *
  * Type P:
  *   Bounds: Input < P < BaseInput
  *   Variance: -P is contravariant
  *   P can then be replaced as follows:
  *
  *   `val x: ComputingUnit[Input, R] = new ComputingUnit[BaseInput, R]`
  *
  * Type R:
  *   Bounds: R < Output
  *   Variance: +R is covariant
  *   R can then be replaced as follows:
  *
  *   `val x: ComputingUnit[P, Output] = new ComputingUnit[P, RichOutput]`
  */
abstract class ComputingUnit[
    -P >: Input <: BaseInput,
    +R <: Output] {
  def run(input: P): R
}

class ComplexComputingUnit extends ComputingUnit[BaseInput, RichOutput] {
  def run(input: BaseInput): RichOutput = input.compute
}

val unit: ComputingUnit[Input, Output] = new ComplexComputingUnit
val input: Input = new Input("e-p-s-i-l-o-n")
val result: Output = unit.run(input)

println(s"result: ${ result.data }")
// --> result: epsilon
```

### View bounds (deprecated)

View bounds use [implicit conversion](#implicit-conversion) to convert types.
[This feature is deprecated](https://github.com/scala/scala/pull/2909).

```scala
// Compare values.

// using implicit convertion to convert `T` => `Ordered[T]`
// to be able to use `>` operator on `T` values
def moreThan[T <% Ordered[T]](x: T, y: T) = x > y

val compareIntValues = moreThan(4, 9)

println(s"result is $compareIntValues")
// --> result is false

val compareCharValues = moreThan('z', 'a')

println(s"result is $compareCharValues")
// --> result is true
```

## Abstract types

An **abstract type** is a type alias defined inside a [class](#class)
an [abstract class](#abstract-class) or a [trait](trait).
The type that matches this alias is defined in the **sub classes**.

Constraints like upper are lower bounds can be applied to abstract types.
Therefore, abstract types can be used to reduce the number type parameters
defined in a [generic class](#generic-class).

```scala
// Create a network datagram.

trait Header {
  // define an abstract type `T`
  type T
  val header: T
}

trait Payload {
  // define an abstract type `T` (same name as above)
  type T
  val payload: T
}

trait Extract {
  // define an abstract type `R` that is a subtype of `String`
  type R <: String
  def extract: R
}

class Datagram(
    val header: Int = 0xA,
    val payload: Int) extends Header with Payload with Extract {

  // set the type that matches `T` in `Header` and `Payload` traits
  type T = Int

  // set the type that matches `R` in `Extract` traits
  type R = String

  def extract: String = f"|header=$header%02x|payload=$payload%04x|"
}

val datagram = new Datagram(payload = 1470)

val content = datagram.extract

println(s"datagram content: $content")
// --> datagram content: |header=0a|payload=05be|
```

### Existential types (to avoid)

Existential types are some kind of [generic types](#generic-types)
that matches any type.
They are used mostly for compatibility with Java wildcard or raw types.

Existential types might generate **obscure type errors** if not used properly
[therefore they should be avoided](https://github.com/scala/scala/blob/7ff0aed5fdb9a355019de53f2645ab521d0d17e8/src/library/scala/language.scala#L164).

```scala
// Count items that doesn't have the same types.

// enable the controlled feature => feature to avoid when possible!
import scala.language.existentials

// define an existantial type `A forSome { type A }`
def countAll(parameters: A forSome { type A }*) = parameters.length

val count = countAll(1, "lol", 'c')

println(s"counted $count unrelated values")
// --> counted 3 unrelated values
```

## Implicit conversion

**Implicit conversion** is a system designed to define various operations
that will be **executed implicitly**.

The implicit operations are defined using the keyword `implicit`.
In addition to that, the implicit operations **must be accessible**
from the place where they are implicitly used.

* An example of **implicit conversion**:

  ```scala
  // Compute modulo operation.

  case class ModuloNumber(value: Int, modulus: Int)

  // define an operation that will implicitly convert
  // `ModuloNumber` instances to `Int` instances
  implicit def toInt(modulo: ModuloNumber): Int = modulo.value % modulo.modulus

  val modulo: ModuloNumber = ModuloNumber(16, 3)

  // `ModuloNumber` instance is implicitly converted to a `Int`
  val intValue: Int = modulo

  println(s"Modulo operation result is $intValue")
  // --> Modulo operation result is 1
  ```

* Implicitly **inject arguments** into a function:

```scala
// Plan a trip.

object Travel {
  // a method that takes an implicit value `to`
  def details[A](from: A)(implicit to: A): String = s"travel from $from to $to"
}

// implicit values
implicit val toName: String = "Helsinki"
implicit val toCode: Int = 666

// the value `toName` is implicitly added to `details` parameters list
// because `A` type variable is inferred to `String` and
// `toName` matches the implicit parameter definition `to: A` & `A = String`
val planning = Travel.details("Toronto")

println(planning)
// --> travel from Toronto to Helsinki

// the value `toCode` is implicitly added to `details` parameters list
// because `A` type variable is inferred to `Int` and now
// `toCode` matches the implicit parameter definition `to: A` & `A = Int`
val planningWithCityCodes = Travel.details(6)

println(planningWithCityCodes)
// --> travel from 6 to 666
```

* Implicitly **inject a function** into an other function arguments list:

```scala
// Compute the median value.

// a function that implicitly take a parameter `strategy` and
// `strategy` is also a function
def median[A](items: List[A])(implicit strategy: List[A] => A): A =
  strategy(items)

// an implicit candidate for `A = Int`
implicit val numberMean = (items: List[Int]) => {
  val index = (items.length / 2).toInt
  items.sorted.apply(index)
}

// an implicit candidate for `A = String`
implicit val textMean = (items: List[String]) => {
  val index = (items.length / 2).toInt
  items.sortBy(_.length).apply(index)
}

val cities = List("Gao", "Tamanrasset", "Antananarivo", "Koupela", "Lome")

// implicitly adds the argument `textMean` because here `A = String`
val medianCity = median(cities)

println(s"please visit $medianCity")
// --> please visit Koupela

val temperatures = List(30, 27, 45, 26, 18)

// implicitly adds the argument `numberMean` because here `A = Int`
val medianTemperature = median(temperatures)

println(s"it's $medianTemperature degrees there")
// --> it's 27 degrees there
```

## Operators overload

* `operators overload`: operators are class methods that can be overloaded.

**Operators** are **functions** that represented by **symbols**
and use the **infix notation**.

For example `x + y` is the infix notation of `x.+(y)`.

Operators features:
* Operators can be **overloaded** like any function.
* Commutative operators can be implemented using an **an implicit class**.
* **Unary** operator are defined using the prefix `unary_`

Example:

```scala
// Points that supports addition and subtraction operations.

case class Point(val x: Int = 0, val y: Int = 0) {

  // operator: `Point + Point`
  def +(that: Point): Point = new Point(this.x + that.x, this.y + that.y)

  // operator: `Point - Point`
  def -(that: Point): Point = new Point(this.x - that.x, this.y - that.y)

  // operator: `Point + Int`
  def +(that: Int): Point = new Point(this.x + that, this.y + that)

  // operator: `Point - Int`
  def -(that: Int): Point = new Point(this.x - that, this.y - that)

  // unary operator: `-Point`
  def unary_- : Point = new Point(-this.x, -this.y)

  // unary operator: `+Point`
  def unary_+ : Point = this
}

// implement commutative operations for + and -:
//    by using an implicit conversion to extend `Int` values
//    to `IntExtendedForPoint`, a class that support operations with `Point`
implicit class IntExtendedForPoint(val x: Int) {

  // operator: `Int + Point`
  def +(point: Point): Point = point + x

  // operator: `Int - Point`
  def -(point: Point): Point = point - x
}

val a = Point(1, 2)

// `Point - Int`
val b = a - 4

// `Int + Point` (commutative operation)
val c = 2 + b

// `-Point` (unary operation)
val d = -c

// `Point + Point`
val e = d + Point(y = 2)

val isSame = a == e

println(if (isSame) "same point" else "different points")
// --> same point
```
