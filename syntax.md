# General syntax

This section explains Scala language basic structures like:
variables, **blocks**, loops, **pattern matching**, exceptions handling,
**lazy values**, etc...

Scala [functions](functions) and [classes](classes)
are explained in their own pages.

* TOC
{:toc}


## Comments

A comment is a block of text that won't be executed:

```scala
// inline comment

/* multi-line
comment */

/** Scaladoc
 *
 *  comment
 */
```

## Values & variables

* A **value** is a placeholder where an object is stored.
  Values are constants, their content cannot change:

  ```scala
  // create a value (constant)
  val x = 49 + 0.3

  println(x) // prints the value `x`
  // --> 49.3
  ```

* A **variable** are values whose content can change:

  ```scala
  // create and increment a variable
  var z = 13
  z += 7

  println(z)
  // --> 20
  ```

> * Variable and values must always be **initialized**.
> * Moreover **values** should be preferred over variables.

### Explicit and implicit typing

Value and variable types can be omitted most of the time because
Scala can deduce (infer) them. This mechanism is called **type inference**:

```scala
// the value's type `String` is explicit set
val name: String = "Hanna"

// the value's type `String` is inferred from the content type
val nickname = "Nana"
```

> * It is recommended to explicitly write the type
    **when the lack of type hurts the usability** of a the code.
    Example: a library public API.

## Statements and blocks

* A **statement** is an unitary piece of code that produces a result:

  ```scala
  // An statement that prints something.

  println("Something")
  // --> Something
  ```

* A **block** is composed by multiple statements surrounded by `{ ... }`:

  ```scala
  // `k` value is computed in a block
  val k = {
    val x = 3
    val y = 13
    y - x
  }

  println(k)
  // --> 10
  ```

## `if`-`else` conditional block

A conditional execution statement is written using a `if`-`else` block:

```scala
val x =
  if (2 > 1)
    "math works"
  else
    "math is broken"

println(x)
// --> math works
```

## `while` and `do`-`while` loops

Both loops run a statement while a condition is satisfied.

* `while` loop:

  ```scala
  // Print multiples of 3.

  var i = 1
  while (i < 4) {
    println(i * 3)
    i += 1
  }
  // --> 3
  // --> 6
  // --> 9
  ```

* `do`-`while` loop:

  ```scala
  // Find the first power of 2 that is bigger than 100.

  var powerOfTwo = 1
  do {
    powerOfTwo *= 2
  } while (powerOfTwo < 100)

  println(powerOfTwo)
  // --> 128
  ```

> * Most of the time,
>   **collection methods**: `map`, `flatMap`, `foreach`, etc...
>   can be used instead of loops.

## `for` comprehensions

You can use `for` comprehensions to iterate other one or multiple collections.

* Iterating other a list of values, using one enumerator `score <- scores`:

  ```scala
  // Convert the scores to percentages.

  val scores = List(89, 12, 58, 5)
  val maxScore = 128

  val scorePercentages =
    for (score <- scores) yield (100 * score / maxScore).toInt

  println(s"scores (%): ${ scorePercentages.mkString(", ") }")
  // --> scores (%): 69, 9, 45, 3
  ```

* Iterating other multiple ranges.
  A `if`-guard is used here to filter-out some values:

  ```scala
  // Compute a set of 3D vectors: (x, y, z), that have a given norm (length)
  // using Euclidean norm formula: x^2 + y^2 + z^2 = norm^2

  def spacialVectors(norm: Int, maxValue: Int = 10) =
    for {
      x <- 0 until maxValue
      y <- 0 until maxValue
      z <- 0 until maxValue if (x*x + y*y + z*z == norm*norm)
    } yield (x, y, z)

  val vectors = spacialVectors(3)

  println(s"generated ${ vectors.length } spacial vectors with norm = 3")
  // --> generated 6 spacial vectors with norm = 3
  ```

* Using `for` statement without yielding a value:

  ```scala
  // List music genres.

  case class Music(genre: String)

  val musicStyles = List(Music("pop"), Music("rap"))

  for (music <- musicStyles) println(s"genre: ${ music.genre.toUpperCase }")
  // --> genre: POP
  // --> genre: RAP
  ```

> A `for` comprehension can also simplify the use of a **monad**:
> *an advanced concept of functional programming*.
> [Learn more here (external link)](https://docs.scala-lang.org/overviews/core/futures.html#functional-composition-and-for-comprehensions)

## Pattern matching with `match` and `case`

Pattern matching is one of Scala's **most interesting features**.
It allows you to check if a value matches one of the defined patterns
and runs the associated block of code.

Pattern matching looks like an advanced version of C language's `switch-case`,
or an alternative to long lists of `if`-`else` statements.

### Match values

* A simple pattern matching example:

  ```scala
  // Select an exit status message from an exit code.

  val code = 0
  val exitStatus = code match {
    // a pattern that matches `code == 0`
    case 0 => "SUCCESS"

    // an other pattern for `code == -1`
    case -1 => "FAILURE"

    // default pattern for all other values of `code`
    case _ => "UNDEFINED"
  }

  println(s"The program exit status is $exitStatus")
  // --> The program exit status is SUCCESS
  ```

* A function that processes a pattern matching on its parameter:

  ```scala
  // Find a word that begins with a given character.

  def wordThatStartsWith(c: Char): String = c match {
    case 'a' => "Abracadabra"
    case 'b' => "Bonjour"
    case _ => "etc..."
  }

  println(wordThatStartsWith('b'))
  // --> Bonjour
  ```

### Match types

It is possible to match a value according to its type:

```scala
// Print a description of different kinds of books.

sealed abstract class Book
case class Poem() extends Book
case class Dictionary() extends Book {
  def nbWords = 300000
}

def getComments(book: Book): String = book match {
  case dict: Dictionary => s"a ${ dict.nbWords } words dictionary"
  case poem: Poem => s"just a book"
  // `case _` default pattern can be ignored thanks to the `sealed` modifier
}

println(getComments(Poem()))
// --> just a book

println(getComments(Dictionary()))
// --> a 300000 words dictionary
```

### Match and unpack a `case class`

Case classes are types (classes) that contains some attributes (values).
It is possible to **access these attributes** through pattern matching:

```scala
// Display information about music artists.

abstract class Artist
case class Producer(realName: String, style: String) extends Artist
case class Singer(artistName: String, playInstrument: Boolean) extends Artist
case class GuitarPlayer(artistName: String, guitarType: String) extends Artist

def introduce(artist: Artist): String = artist match {
  // match `Producer` type and unpack all its attributes
  case Producer(realName, style) => s"The famous $style producer $realName"

  // match `Singer` type and unpack `artistName` attribute only
  case Singer(artistName, _) => s"The super star $artistName"

  // match `GuitarPlayer` only, no attribute is unpacked
  case GuitarPlayer(_, _) => "A great guitar player"
}

val producer = Producer("Jack Back", style = "Jazz")
val singer = Singer("Tororo", false)
val player = GuitarPlayer("Golden Finger", "expensive")

println(introduce(producer))
// --> The famous Jazz producer Jack Back

println(introduce(singer))
// --> The super star Tororo

println(introduce(player))
// --> A great guitar player
```

### Combining multiple patterns

Multiple patterns can be combined with `|` operator:

```scala
// Tell if a number is in a defined interval.

def tinyEvenNumber(x: Int): String = x match {
  case 0 | 2 | 4 | 6 | 8 => "even number inside interval [0, 10["
  case _ => "other"
}

println(s"result is ${ tinyEvenNumber(4) }")
// --> result is even number inside interval [0, 10[

println(s"result is ${ tinyEvenNumber(33) }")
// --> result is other
```

### Pattern guards

Pattern guards are extra conditions that are added to patterns.
They are represented by a `if` condition with no `else`:

```scala
// Delete files and folders.

abstract class FileSystemItem
case class File(name: String) extends FileSystemItem
case class Dir(
  name: String,
  content: List[FileSystemItem]) extends FileSystemItem

def remove(item: FileSystemItem): Unit = {
  println(
    item match {
      // deleting an empty directory,
      // using a `if-guard` to ensure that the directory is empty
      case Dir(_, content) if content.isEmpty => "directory deleted"

      // deleting a file
      case File(_) => "file deleted"

      // non empty directory and other cases...
      case _ => "it is not safe to delete this item"
    }
  )
}

remove(File("file.txt"))
// --> file deleted

remove(Dir("empty-dir", List()))
// --> directory deleted

remove(Dir("full-dir", List(File("file.txt")))
// --> it is not safe to delete this item
```

### Match regular expressions

In Scala, `String` values can be converted to regular expressions
using `String.r` method.

* Regular expressions can be used as patterns to **extracts** the matching
  **captured group**:

  ```scala
  // Extract an email or a phone number from a given text.

  def looksLike(value: String): String = {
    val emailRE = raw".*?(\S+@\S+).*?".r
    val phonenumberRE = raw".*?(\+?\d[\d\s]+).*?".r

    value match {
      // if `value` contains a valid email,
      // this email is extracted into `email` value
      case emailRE(email) => s"email found: «$email»"

      // if `value` contains a valid phone number,
      // the phone number is extracted into `number` value
      case phonenumberRE(number) => s"phone number found: «$number»"

      // `value` doesn't contain an email nor a phone number
      case _ => "nothing found"
    }
  }

  println(looksLike("please send me an e-mail to john@doe.com thanks"))
  // --> email found: «john@doe.com»

  println(looksLike("my new phone number is: +226 50 32 12 33"))
  // --> phone number found: «+226 50 32 12 33»
  ```

* It is also possible to simply check if a value matches
  a given regular expressions, using pattern matching:

  ```scala
  // Check if a given text contains special characters (punctuation).

  def containsSpecialChars(text: String): Boolean = {
    val specialChars = raw"[^\w\s]+".r

    // Check if `text` matches the regular expression
    specialChars.findFirstMatchIn(text) match {
      case Some(_) => true
      case None => false
    }
  }

  val text = "This is Sparta"
  val found = containsSpecialChars(text)

  println(s"${ if (found) "some" else "no" } special chars found in '$text'")
  // --> no special chars found in 'This is Sparta'
  ```

### Anonymous functions with pattern matching

Anonymous functions can use pattern matching on their parameters
while omitting the `match` keyword:

```scala
// Read the temperature and tell if its hot or cold.

// the following function is a shorter version of:
// `def calorimeter(x: Int) => String = x match { ... }`
def calorimeter: (Int) => String = {
  case temperature if temperature > 30 => "hot"
  case _ => "cold"
}

println(s"it is ${ calorimeter(34) }")
// --> it is hot
```

## Manage exceptions with `throw` and `try`-`catch`-`finally`

Here is how an **exception** can be thrown and handled in Scala:

```scala
// Running computations on a remote "cloud" system.

// create a custom exception class
final case class CloudError(message: String = "") extends Exception(message)

try {
  // run code that may throw an exception
  println("running computations on the cloud")
  // throw an exception
  throw CloudError("cannot connect to the cloud")
} catch {
  // catch the thrown exceptions
  case CloudError(message) => println(s"error caught: $message")
} finally {
  // run this code whether an exception has been caught or not
  println("cloud computations done")
}

/* -->
running computations on the cloud
error caught: cannot connect to the cloud
cloud computations done
*/
```

## `import` names

`import` statement is used to import names from packages.
The imported names are then available in the current name scope:

```scala
// import all names defined in `scala.sys` package
import scala.sys._

// import `Pi` value
import scala.math.Pi

// import several functions: `sin` and `cos`, `cos` function is renamed
import scala.math.{sin, cos => cosinus}

// import all but `tan` function (`tan` name is "erased")
import scala.math.{tan => _, _}
```

## `lazy` values

`lazy` values are values that are evaluated when they are used for
the **first time**. If a `lazy` is never used it will not be evaluated:

```scala
// A system that waters your plants for you.

// this block will be evaluated when `humidity` will be used
lazy val humidity = {
  println("retrieving humidity from sensors")
  30
}

println("prepare the watering of the plants")

// evaluate the block above to retrieve `humidity` value
if (humidity < 50)
  println("watering the plants")
else
  println("not wasting water")

/* -->
prepare the watering of the plants
retrieving humidity from sensors
watering the plants
*/
```
