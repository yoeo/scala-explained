# Scala library explained

* TOC
{:toc}

### Classes hierarchy

Scala is primarily a object language, all the values are objects.
Different classes of objects are defined in
[Scala Standard Library](https://www.scala-lang.org/api/current/),
based on the following class hierarchy:

![classes](https://docs.scala-lang.org/resources/images/tour/unified-types-diagram.svg)

* `Any` is Scala root class. Any value is somehow an instance of `Any`
  and can be cast into a `Any`.
* `AnyRef` is the root class of all reference classes,
  including all classes defined in Scala programs.
* `AnyVal` is the root class of all values that cannot be set to `null`.
* `Null` is the subtype of all reference types. It has exactly one instance,
  `null`, that can be assigned to any `AnyRef` variable.
* `Nothing` is the subtype of every types, it has no instance.
  `Nothing` is used to define functions that
  never returns and empty objects (`Nil`, `None`).

### Cast

Values of a given type can be transformed into an equivalent values
of an other type, both implicitly or explicitly.

### Implicit cast

Scala can cast a variable from a type to an other type using several hints,
like the destination variable type.

```scala
// Converts implicitly a char to a Long
val codePoint: Long = '☻'
println(s"Smiley Unicode value is $codePoint")
// --> Smiley Unicode value is 9787
```

The following graph shows which number types can be implicitly cast to
an other number type:

![cast](https://docs.scala-lang.org/resources/images/tour/type-casting-diagram.svg)

### Explicit cast

An explicit cast is done by calling a cast method to produce
an equivalent value with the desired type.
By convention the cast methods starts with `to...`
followed by the name of the new type. Example: `toLong`, `toChar` etc...

```scala
val longValue = 12L
val intValue = longValue.toInt // convert a `Long` value to a `Int` value
```

> All Scala values can be cast to a `String` using `toString` method.

## Primary value types

In most programming languages the primary values are
**numbers** (boolean, integer, rational), **characters** and **strings**.

These value types are defined in Scala standard library
and here is how you can use them:

#### True / False values: `Boolean`

Boolean values are either `true` or `false`.
They are the result of comparison operations, for example `a > b`.

Boolean values supports the following operations:
* **logical** operations:
  * `&&` and
  * `||` or
  * `!` not
  * `^` xor
  * `&` strict-and: evaluates all the parameters
  * `|` strict-or: evaluates all the parameters
* **comparison** operations:
  * `==` equal
  * `!=` not equal
  * `>` greater than
  * `<` lower than
  * `<=` lower than or equal
  * `>=` greater than or equal

```scala
// Example operations on Boolean values
val isFrontEngineRunning = true
val isRearEngineRunning = true
val isThereAPiloteInThePlane = false

val thePlaneWillLand =
  isThereAPiloteInThePlane || (isFrontEngineRunning && isRearEngineRunning)

println(s"the plane will ${ if (thePlaneWillLand) "land" else "crash" }!")
// --> the plane will land!
```

#### Numbers: `Byte`, `Char`, `Short`, `Int`, `Long`, `Float`, `Double`

Different number types are defined for different usages:
* `Byte`, 8-bit signed integer that represents a **data byte**.
  Example: `0xFF.toByte`
* `Char`, 16-bit unsigned integer that represents a **character**.
  Example: `'Z'`
* `Short`, 16-bit signed integer, for **short integer** values.
  Example: `123.toShort`
* `Int`, 32-bit signed integer, for **medium integer** values.
  Example: `-123456`
* `Long`, 64-bit signed integer, for **big integer** values.
  Example: `9876543210L`
* `Float`, 32-bit floating point number, to approximate **real values**.
  Example: `-36.15e-4F`
* `Double`, 64-bit floating point number, to approximate real values
  with **better precision**. Example: `9.0123`

Number representations are explained in depth in the
[literals section](literals).

Number values supports the following operations:
* **arithmetical** operations:
  * `+` add
  * `-` subtract
  * `*` multiply
  * `/` divide
  * `%` modulo
* **comparison** operations:
  * `==` equal
  * `!=` not equal
  * `>` greater than
  * `<` lower than
  * `<=` lower than or equal
  * `>=` greater than or equal

```scala
// Operation on numbers
val sucessRate = 100 * (4294967296L - 1048576) / 4294967296D
println(f"sucess rate = $sucessRate%.2f percent")
// --> sucess rate = 99.98 percent
```

Integer types `Byte`, `Char`, `Short`, `Int` & `Long` also support:
* **bitwise** operations:
  * `&` bit by bit and
  * `|` bit by bit or
  * `^` bit by bit xor
  * `~` reverse all bits (bitwise not)
  * `<<` left shift
  * `>>` right shift that preserves the value sign
  * `>>>` right shift that doesn't preserves the value sign

```scala
// Bitwise operations
// here is a filtering statement
val number = 19
val lowBits = number & ((1<<4) - 1)
println(s"number filtered to only keep the 4 lowest bits = $lowBits")
// --> number filtered to only keep the 4 lowest bits = 3
```

#### Text values: `String`

Scala `String` are based on Java `String`.

String values supports the following operations:
* **arithmetical** operations:
  * `+` concatenates two string values
  * `*` repeats a string N-times
* **comparison** operations:
  * `==` equal
  * `!=` not equal
  * `>` greater than
  * `<` lower than
  * `<=` lower than or equal
  * `>=` greater than or equal

##### Useful `String` methods

* Extracting data from a `String`:

  ```scala
  // Select a character from a string
  val text = "Here be dragons."
  val firstChar = text(0)
  println(s"first char is $firstChar")
  // --> first char is H

  // Split a string
  val lastWord = text.split(raw"[ \.]").last
  println(s"last word is $lastWord")
  // --> last word is dragons.

  // Extract a sub-strings
  val verb = text.slice(5, 7)
  println(s"extracted sub-string: $verb")
  // --> extracted sub-string: be

  // Remove white space characters before and after a text
  val vegetable = "   carrot   "
  val shortVegetable = vegetable.trim
  println(s"vegetable: $shortVegetable")
  // --> vegetable: carrot

  // Get the position of a sub string
  val vacationSchedule = "beach and sun"
  val position = vacationSchedule.indexOf("and")
  val firstActivity = vacationSchedule.slice(0, position)
  println(s"vacation activity: $firstActivity")
  // --> vacation activity: beach
  ```

* Transforming a `String`:

  ```scala
  // Replace patterns in a string
  val magicWord = "Abracadabra"
  val nearCityName = magicWord.replaceAll(raw".a.a", "u ")
  println(s"word almost changed into a city name: $nearCityName")
  // --> word almost changed into a city name: Abu dabra

  // Change text case to lower case
  val loud = "I DEMAND SILENCE"
  val quiet = loud.toLowerCase
  println(quiet)
  // --> i demand silence

  // Change text case to upper case
  val missionCode = "Alpha delta Tango"
  val formalCode = missionCode.toUpperCase
  println(formalCode)
  // --> ALPHA DELTA TANGO

  // Capitalize: first letter in upper case
  val words = "tomato is RED."
  val cleanPhrase = words.capitalize
  println(cleanPhrase)
  // --> Tomato is RED.
  ```

* Checking the content of a `String`:

  ```scala
  // Check if a text matches a regular expression
  val phoneNumber = "+226 43 00 00 00"
  val isAPhoneNumber = phoneNumber.matches(raw"[\s\d\-\+]+")
  println(s"the text ${ if (isAPhoneNumber) "is" else "is not" } a phone number.")
  // --> the text is a phone number.

  // Check if a text contains a given string
  val cookingRecipe = "Mélangez bien le bouillon."
  val isInFrench = cookingRecipe.contains("é")
  println(s"the text ${ if (isInFrench) "is" else "is not" } in French.")
  // --> the text is in French.

  // Check if a text starts with a given string
  val story = "Once upon a time, a rabbit and a turtle where arguing."
  val isATale = story.startsWith("Once upon a time")
  println(s"the text ${ if (isATale) "is" else "is not" } a tale.")
  // --> the text is a tale.

  // Check if a text ends with a given string
  val shortText = "WAR FINALLY OVER STOP"
  val isATelegram = shortText.endsWith("STOP")
  println(s"the text ${ if (isATelegram) "is" else "is not" } a telegram.")
  // --> the text is a telegram.
  ```

##### `String` Interpolation and formatting

Values can be added to a `String` template using formatting and interpolation:

* **formatting**: inject values into a `String` using C-printf-like syntax.

  ```scala
  val message = "%s invested %.2f€".format("Bob", 405.123)
  println(message)
  // --> Bob invested 405.12€
  ```

* **s-interpolation**: directly inject values and expressions into a `String`.

  ```scala
  val user = "Bob"
  val amount = 405.123
  val message = s"$user invested ${ amount.round }€"
  println(message)
  // --> Bob invested 405€
  ```

* **f-interpolation**: inject formatted expressions into the `String`,
  a mix of s-interpolation and formatting.

  ```scala
  val user = "Bob"
  val amount = 405.123
  val message = f"$user invested $amount%.2f€"
  println(message)
  // --> Bob invested 405.12€
  ```

* **raw-interpolation**: represent all the characters as they are written.
  No escape character required.

  ```scala
  val message = raw"the new line character is '\n'"
  println(message)
  // --> the new line character is '\n'
  ```

## Convenient collection types

Scala collections types are containers that hold values:
`List`, `Map`, `Set`, etc...
There are two families of collections:
* **mutable** collections: items **can** be added or removed from the collection.
* **immutable** collections: items **cannot** be added or removed
  from the collection.

By default all available **collections are immutable**,
to use a mutable version of a collection, it is required to `import` it first:

```scala
// Seq type is immutable by default
val immutableCollection = Seq(1, 2, 3)

// the mutable Seq type is imported and used here
import collection.mutable.Seq

val mutableCollection = Seq(1, 2, 3)
mutableCollection(0) = 4 // updates the element at the position 0
```

There are many different collections types.
The concepts behind each collection type are described in
[Scala documentation](https://docs.scala-lang.org/overviews/collections/introduction.html).

Here are some convenient collection types that can be used in Scala programs:
> `String` type is equivalent to a sequential collection.

#### Sequence of elements: `List`

Scala `List` is an immutable sequence of elements.

##### Combine `List` elements

* Merge or exclude `List` elements:

  ```scala
  // Merge two lists
  val merged = List(1, 2, 3) ++ List(4, 5)
  println(s"${ merged.mkString(", ") }")
  // --> 1, 2, 3, 4, 5

  val alsoMerged = List(1, 2, 3) ::: List(4, 5)
  println(s"${ alsoMerged.mkString(", ") }")
  // --> 1, 2, 3, 4, 5

  val united = List(1, 2, 3).union(List(4, 5))
  println(s"${ united.mkString(", ") }")
  // --> 1, 2, 3, 4, 5

  // Compute differences, Diff = List1 - List2
  val diff = List(1, 2, 3, 4, 5).diff(List(0, 1, 3, 4, 6))
  println(s"differences = ${ diff.mkString(", ") }")
  // --> differences = 2, 5
  ```

* Add elements to a `List`:

  ```scala
  // Append an element to a list
  val appended = List(1, 2, 3) :+ 4
  println(s"${ appended.mkString(", ") }")
  // --> 1, 2, 3, 4

  // Prepend an element to a list
  val prepended = 0 +: List(1, 2, 3)
  println(s"${ prepended.mkString(", ") }")
  // --> 0, 1, 2, 3

  val alsoPrepended = 0 :: List(1, 2, 3)
  println(s"${ alsoPrepended.mkString(", ") }")
  // --> 0, 1, 2, 3
  ```

* Zip two `List` instances:

  ```scala
  // Zip, merge two lists item by item
  val definitions = List("eucalyptus", "orange").zip(List("plant", "fruit"))
  definitions.foreach { case (item, group) => println(s"$item is a $group") }
  // --> eucalyptus is a plant
  // --> orange is a fruit
  ```

* Flatten nested `List` instances:

  ```scala
  // Flatten nested lists
  val nested = List(List(1, 2), List(3, 4, 5, 6))
  val flattened = nested.flatten
  println(s"flat list =  ${ flattened.mkString(", ") }")
  // --> flat list =  1, 2, 3, 4, 5, 6
  ```

* Rearrange `List` elements:

  ```scala
  // Generate an `Iterator` with grouped chunks of items
  val dataStream = List('a', 'b', 'c', 'd', 'e', 'f', 'g')
  val chunks = dataStream.grouped(3)
  val firstChunk = chunks.next
  println(s"the first chunk of data is ${ firstChunk.mkString("") }")
  // --> the first chunk of data is abc

  // Reverse a list
  val reversedCount = List(1, 2, 3, 4).reverse
  println(s"reversed count: ${ reversedCount.mkString(", ") }")
  // --> reversed count: 4, 3, 2, 1

  // Sort a list
  val sortedAges = List(35, 64, 53, 24, 89).sorted
  println(s"sorted ages: ${ sortedAges.mkString(", ") }")
  // --> sorted ages: 24, 35, 53, 64, 89
  ```

* Group `List` elements:

  ```scala
  // Group elements that have a given property in common
  val roots = List(3, -2, 6, 2, -3)
  val squaresInfo = roots.groupBy((x) => x * x)
  squaresInfo.foreach {
    case(square, roots) =>
      println(s"$square root squares: ${ roots.mkString(", ") }")
  }
  // --> 4 root squares: -2, 2
  // --> 36 root squares: 6
  // --> 9 root squares: 3, -3
  ```

##### Access `List` elements

* Get an element:

  ```scala
  // Get element & Get element with a default value
  val items = List(3, 4, 5, 6)
  val third = items(2)
  val missing = items.applyOrElse(10, (x: Int) => 0)
  println(s"third element = $third, default value = $missing")
  // --> third element = 5, default value = 0
  ```

* Search an element:

  ```scala
  // Get the position of the first element that satisfy a predicate
  val orderedSquares = List(1, 4, 9, 16, 25, 36)
  val threshold = orderedSquares.indexWhere(_ >= 10)
  val squaresLowerThanTen = orderedSquares.slice(0, threshold)
  println(s"squares lower than 10 are: ${ squaresLowerThanTen.mkString(", ") }")
  // --> squares lower than 10 are: 1, 4, 9
  ```

* Get first or last elements:

  ```scala
  // Get or drop first and last elements
  val positions = List("one", "two", "three")
  val first = positions.head
  val last = positions.last
  val allButFirst = positions.tail
  val allButLast = positions.init
  println(s"""
    positions:
    - all           : ${ positions.mkString(", ") }
    - first         : $first
    - last          : $last
    - all but first : ${ allButFirst.mkString(", ") }
    - all but last  : ${ allButLast.mkString(", ") }
    """
  )
  /* -->
    positions:
    - all           : one, two, three
    - first         : one
    - last          : three
    - all but first : two, three
    - all but last  : one, two
  */
  ```

* Get minimum and maximum elements:

  ```scala
  // Min and Max
  val rainyYears = List(1120, 2010, 310, 1010, 1950)
  val lastRainyYear = rainyYears.max
  val firstRainyYear = rainyYears.min
  val lastRainyYear2ndMillenium = rainyYears.maxBy(
    (x) => if (x >= 1000 && x < 2000) x else -9999
  )
  val firstRainyYear2ndMillenium = rainyYears.minBy(
    (x) => if (x >= 1000 && x < 2000) x else 9999
  )
  println(s"""
    rainy years:
    - all rainy years                 : ${ rainyYears.mkString(", ") }
    - last rainy year                 : $lastRainyYear
    - first rainy year                : $firstRainyYear
    - 2nd millenium last rainy year   : $lastRainyYear2ndMillenium
    - 2nd millenium first rainy year  : $firstRainyYear2ndMillenium
    """
  )
  /* -->
    rainy years:
    - all rainy years                 : 1120, 2010, 310, 1010, 1950
    - last rainy year                 : 2010
    - first rainy year                : 310
    - 2nd millenium last rainy year   : 1950
    - 2nd millenium first rainy year  : 1010
  */
  ```

* Check a `List` size:

  ```scala
  // Check if a list is empty
  val emptyList = List()
  val isEmptyList = emptyList.isEmpty // `List.nonEmpty` also exists
  println(s"there are ${ if (isEmptyList) "no " }elements in the list")
  // --> there are no elements in the list

  // Get list size
  val manyItems = List(2, 4, 1, 2, 5, 3, 2, 0)
  val numberOfItems = manyItems.length
  println(s"here we have $numberOfItems items")
  // --> here we have 8 items
  ```

##### Filter/search `List` items

* Look for a values:

  ```scala
  // Check if an element is in the list
  val values = List(7, 8)
  val found = values.contains(8)
  val notFound = values.contains(0)
  println(s"found(8) = $found, found(0) = $notFound")
  // --> found(8) = true, found(0) = false
  ```

* Count matching elements:

  ```scala
  // Count elements which pass a test function
  val elements = List(9, 8, 7, 6, 5, 4)
  val nbElements = elements.count(_ % 2 == 0)
  println(s"nb even = $nbElements")
  // --> nb even = 3
  ```

* Drop unwanted elements:

  ```scala
  // Create a list of unique elements
  val duplicates = List(1, 2, 3, 3, 3, 4, 4, 5)
  val unique = duplicates.distinct
  println(s"unique elements = ${ unique.mkString(", ") }")
  // --> unique elements = 1, 2, 3, 4, 5

  // Drop elements which pass a test function
  val allNumbers = List(1, 2, 3, 4, 5, 6)
  val oddNumbers = allNumbers.filter(_ % 2 != 0)
  println(s"odd numbers =  ${ oddNumbers.mkString(", ") }")
  // --> odd numbers =  1, 3, 5
  ```

* Split a `List` according to a condition:

  ```scala
  // Split/partition a list into two, according to a predicate
  val winterTemperatures = List(2, -3, -1, -16, 3)
  val (negativeTemperatures, positiveTemperatures) =
    winterTemperatures.partition(_ > 0)
  println(s"""
    Recorded winter temperatures:
    - all temperatures      : ${ winterTemperatures.mkString(", ") }
    - positive temperatures : ${ negativeTemperatures.mkString(", ") }
    - negative temperatures : ${ positiveTemperatures.mkString(", ") }
    """
  )
  /* -->
    Recorded winter temperatures:
    - all temperatures      : 2, -3, -1, -16, 3
    - positive temperatures : 2, 3
    - negative temperatures : -3, -1, -16
  */

  ```

* Check all elements:

  ```scala
  // Check that all elements pass a test function
  val lessThanTen = List(2, 3, 4, 5, 6, 7)
  val allLessThanTen = lessThanTen.forall(_ < 10)
  println(s"all items are < 10 ? $allLessThanTen")
  // --> all items are < 10 ? true
  ```

##### Map-reduce `List` items

* Iterate over a `List`:

  ```scala
  // Apply an function to each item, the result is discarded.
  val values = List(6, 7)
  values.foreach((x) => println(s"current value is $x"))
  // --> current value is 6
  // --> current value is 7
  ```

* Transform each `List` item:

  ```scala
  // Apply a function to all elements, (`reverseMap` = `map` in reverse order)
  val familyMembersAge = List(40, 15, 44)
  val ageCaregories = familyMembersAge.map(
    (x) => if (x >= 18) "adult" else "minor"
  )
  println(s"family members age categories: ${ ageCaregories.mkString(", ") }")
  // --> family members age categories: adult, minor, adult
  ```

* Compute a value using `List` items:

  > * `reduce` method is in most cases the same as `reduceLeft`.
  > * `fold` method is in most cases the same as `foldLeft`.

  ```scala
  // Fold or reduce left
  // --> 123
  val leftFolded = (0 /: List(1, 2, 3))(_*10 + _)
  println(s"$leftFolded")
  // --> 123
  val alsoLeftFolded = List(1, 2, 3).foldLeft(0)(_*10 + _)
  println(s"$alsoLeftFolded")
  // --> 123
  val leftReduced = List(1, 2, 3).reduceLeft(_*10 + _)
  println(s"$leftReduced")
  // --> 123

  // Fold or reduce right
  val rightFolded = (List(1, 2, 3) :\ 0)(_ + _*10)
  println(s"$rightFolded")
  // --> 321
  val alsoRightFolded = List(1, 2, 3).foldRight(0)(_ + _*10)
  println(s"$alsoRightFolded")
  // --> 321
  val rightReduced = List(1, 2, 3).reduceRight(_ + _*10)
  println(s"$rightReduced")
  // --> 321
  ```

* Compute sum and product:

  ```scala
  // Numeric list items sum and product
  val teamHealthPoints = List(99, 10, 63)
  val totalHealthPoints = teamHealthPoints.sum
  val combinedHealthPoints = teamHealthPoints.product
  println(s"""
    Multiplayer game health:
    - team health     : ${ teamHealthPoints.mkString("", "/100, ", "/100") }
    - total health    : $totalHealthPoints/300
    - combined health : $combinedHealthPoints/1000000
    """
  )
  /* -->
    Multiplayer game health:
    - team health     : 99/100, 10/100, 63/100
    - total health    : 172/300
    - combined health : 62370/1000000
  */
  ```

#### Key-value association: `Map`

Scala `Map` are collections that hold pairs of key-value.

##### Combine `Map` elements

* Combine a `Map` with a pair or with an other `Map`:

  > The right arrow `->` operator creates a tuple: `(a -> b) == (a, b)`

  ```scala
  // Build a new map from a map and a pair
  val merged = Map('a' -> 1, 'b' -> 2) + ('c' -> 3)
  println(s"${ merged.mkString(", ") }")
  // --> a -> 1, b -> 2, c -> 3

  // Combine two maps into one
  val mergedAgain = Map('a' -> 1) ++ Map('b' -> 2, 'c' -> 3)
  println(s"${ mergedAgain .mkString(", ") }")
  // --> a -> 1, b -> 2, c -> 3
  ```

##### Access `Map` elements

* Retrieve a value from a `Map`:

  ```scala
  // Get value
  val scores = Map("Fatmir" -> 16, "Bora" -> 15)
  val fatmirScore = scores("Fatmir")
  val boraScore = scores.get("Bora")
  val unknownScore = scores.getOrElse("unknown", 0)
  println(s"""
    Players scores:
    - Fatmir  : $fatmirScore
    - Bora    : $boraScore
    - unknown : $unknownScore
    """
  )
  /*
    Players scores:
    - Fatmir  : 16
    - Bora    : Some(15)
    - unknown : 0
  */
  ```

* Count `Map` items with a predicate or check its size:

  ```scala
  // Count and check size
  val romanNumbers = Map("I" -> 1, "II" -> 2, "III" -> 3, "IV" -> 4, "V" -> 5)
  val size = romanNumbers.size
  val nbEvenThatContainV = romanNumbers.count {
    case (key, value) => key.contains("V") && value % 2 == 0
  }
  println(s"""
    Roman numbers: ${ romanNumbers.mkString(", ") }
    - nb items                        : $size
    - nb even numbers that contain V : $nbEvenThatContainV
    """
  )
  /*
    Roman numbers: II -> 2, IV -> 4, I -> 1, V -> 5, III -> 3
    - nb items                        : 5
    - nb even numbers that contain V : 1
  */
  ```

* Access `Map` keys and values:

  ```scala
  // Get map keys and values
  val softwareVersion = Map("document-writer" -> 3.4, "video-player" -> 10)
  val softwares = softwareVersion.keys
  val versions = softwareVersion.values
  println(s"""
    Softwares:
    - names: ${ softwares.mkString(", ") }
    - versions: ${ versions.mkString(", ") }
    """
  )
  /*
    Softwares:
    - names: document-writer, video-player
    - versions: 3.4, 10
  */
  ```

##### Remove `Map` elements

* Remove elements from a `Map` using their keys:

  ```scala
  // Remove keys
  val postalCodes = Map(
    "Moon Town" -> 1243,
    "Wet Lake" -> 9809,
    "Zero City" -> 0
  )
  val everythingButZero = postalCodes - "Zero City"
  val onlyMoon = postalCodes -- List("Wet Lake", "Zero City")
  println(s"""
    Postal codes:
    - all                 : ${ postalCodes.mkString(", ") }
    - all except Zero City: ${ everythingButZero.mkString(", ") }
    - only Moon Town      : ${ onlyMoon.mkString(", ") }
    """
  )
  /*
    Postal codes:
    - all                 : Moon Town -> 1243, Wet Lake -> 9809, Zero City -> 0
    - all except Zero City: Moon Town -> 1243, Wet Lake -> 9809
    - only Moon Town      : Moon Town -> 1243
  */
  ```

##### Filter/search `Map` items

* Check if a key is in the `Map`:

  ```scala
  // Check the presence of a key
  val sales = Map("shoes" -> 3214, "shirts" -> 756)
  val isSellingSalad = sales.contains("salad")
  println(s"the trader is selling salads: $isSellingSalad")
  // --> the trader is selling salads: false
  ```

* Filter a `Map`:

  ```scala
  // Filter map items
  val artistInfo = Map(
    "genre" -> "pop",
    "artist-name" -> "The PopStar",
    "last-album" -> "Best Hit"
  )
  val artist = artistInfo.filter { case (key, _) => key.endsWith("name") }
  val genre = artistInfo.filterKeys(_ == "genre")
  val albumName = artistInfo.find {
    case (key, _) => key.endsWith("album")
  } match {
    case Some((_, value)) => value
    case None => "unknown"
  }
  println(s"""
    Artist info:
    - ${ artist.mkString }
    - ${ genre.mkString }
    - album => $albumName
    """
  )
  /*
    Artist info:
    - artist-name -> The PopStar
    - genre -> pop
    - album => Best Hit
  */
  ```

##### Map-reduce `Map` items

* Transform each pair contained in the `Map`:

  ```scala
  // Process pairs
  val inhabitantsInMillion = Map(
    "Old City" -> 1.5,
    "New City" -> 0.9
  )
  val inhabitants = inhabitantsInMillion.map {
    case (key, value) => (key -> (value * 1e6).toInt) }
  println(s"number of citizens: ${ inhabitants.mkString(", ") }")
  // --> number of citizens: Old City -> 1500000, New City -> 900000
  ```

  Flat map `Map` items:

  ```scala
  // Flat map items
  val sportFans = Map(
    "football" -> Map("Botswana" -> 23, "Brunei" -> 16),
    "basket-ball" -> Map("Chile" -> 41, "Latvia" -> 99)
  )
  val allCountriesHaveFans = sportFans.forall {
    case (sport, countryFans) => countryFans.nonEmpty
  }
  val allFansExceptLatvians = sportFans.flatMap {
    case (_, value) => value - "Latvia"
  }
  println(s"""
    Sport fans:
    - fans except Latvia      : ${ allFansExceptLatvians.mkString(", ") }
    - all countries have fans : $allCountriesHaveFans
    """
  )
  /*
    Sport fans:
    - fans except Latvia      : Botswana -> 23, Brunei -> 16, Chile -> 41
    - all countries have fans : true
  */
  ```

* Group `Map` items:

  ```scala
  // Group items using a chunk size
  val robotMoves = Map(
    "move-up" -> 0,
    "move-down" -> 1,
    "move-left" -> 2,
    "move-right" -> 3,
    "move-forward" -> 4,
    "move-backward" -> 5
  )
  val groupedByTwo = robotMoves.grouped(2)
  groupedByTwo.foreach {
    moves => println(s"two moves: ${ moves.keys.mkString(" & ") }")
  }
  // --> two moves: move-forward & move-backward
  // --> two moves: move-up & move-right
  // --> two moves: move-down & move-left

  // Group items by a criteria
  val eachDimension = robotMoves.groupBy { case (_, v) => v % 2}
  println(s"""
    One steps in each dimension:
    - ${ eachDimension(0).keys.mkString(" then ") }
    - ${ eachDimension(1).keys.mkString(" then ") }
    """)
  /*
    One steps in each dimension:
    - move-forward then move-up then move-left
    - move-backward then move-right then move-down
  */
  ```

* Fold a `Map` to reduce all items into one item:

  ```scala
  // Fold left, same as `foldLeft` method and `fold` (in most cases)
  val cssMargins = Map("top" -> 10, "bottom" -> 5, "left" -> 25, "right" -> 15)
  val topBottom = (0 /: cssMargins) {
    case (previousResult, (key, value)) if (key == "top" || key == "bottom") =>
      previousResult + value
    case (previousResult, _) => previousResult
  }

  // Fold right, same as `foldRight` method
  val rightLeft = (cssMargins :\ 0) {
    case ((key, value), previousResult) if (key == "right" || key == "left") =>
      previousResult + value
    case (_, previousResult) => previousResult
  }
  println(s"""
    CSS margins: ${ cssMargins.mkString(", ") }
    - Sum top & bottom  : $topBottom
    - Sum left & right  : $rightLeft
    """
  )
  /*
    CSS margins: top -> 10, bottom -> 5, left -> 25, right -> 15
    - Sum top & bottom  : 15
    - Sum left & right  : 40
  */
  ```

* Reduce a `Map` to a resulting value:

  > * `reduce` method is in most cases the same as `reduceLeft`.
  > * `reduceRight` is like `reduceLeft` but in opposite order.

  ```scala
  // Reduce a map
  val elements = Map(
    "milk" -> "liquid",
    "rock" -> "solid",
    "water" -> "liquid",
    "tree" -> "solid"
  )
  val solidElements = elements.reduce {
    (a, b) => {
      val (key_a, value_a) = a
      val (key_b, value_b) = b

      val elementNames = List[String]() ++ {
        if (value_a == "solid") List[String](key_a) else Nil
      } ++ {
        if (value_b == "solid") List[String](key_b) else Nil
      }
      (elementNames.mkString(" & "), "solid")
    }
  }
  println(s"Solid elements: ${ solidElements._1 }")
  // --> Solid elements: rock & tree
  ```
