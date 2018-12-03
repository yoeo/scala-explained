# Scala keywords explained

We will take a tour of **all Scala language reserved keywords**.

Reserved keywords are words that have a special meaning in Scala.
They cannot be used as an identifier (variable name or class name).
For each reserved keyword there is a list of use cases
and for each use case there is a concise explanation.
The uses cases and explanations are mostly taken from the
[Scala language specification](https://www.scala-lang.org/files/archive/spec/2.12/)

There are currently **40 reserved keywords in Scala** language:

| | | | | |
|-|-|-|-|-|
| [abstract](#abstract) | [case](#case) | [catch](#catch) | [class](#class) | [def](#def)
| [do](#do) | [else](#else) | [extends](#extends) | [false](#false) | [final](#final)
| [finally](#finally) | [for](#for) | [forSome](#forSome) | [if](#if) | [implicit](#implicit)
| [import](#import) | [lazy](#lazy) | [macro](#macro) | [match](#match) | [new](#new)
| [null](#null) | [object](#object) | [override](#override) | [package](#package) | [private](#private)
| [protected](#protected) | [return](#return) | [sealed](#sealed) | [super](#super) | [this](#this)
| [throw](#throw) | [trait](#trait) | [try](#try) | [true](#true) | [type](#type)
| [val](#val) | [var](#var) | [while](#while) | [with](#with) | [yield](#yield)

Let's explain them all!

## abstract

```scala
abstract class ClassName { ... }
```

* Defines an [abstract class](classes.md#abstract-class),
  a class that allows incomplete members.

```scala
abstract override def traitMethod = ...
```

* Defines a [trait](classes.md#trait) method that calls an unrelated
  [super](#super) method. In short, it allows the usage of the
  [stackable trait pattern](classes.md#stackable-trait-pattern-abstract-override).

## case

```scala
x match {
  case 1 => ...
  case 2 => ...
  ...
}
```

* When `x` value matches one of the cases, the **associated statement**
  is executed.
  This feature is called [pattern matching](syntax.md#match-values).

```scala
x match {
  case a if (a > 2) => ...
}
```

* Defines a pattern matching with a **guard**.
  The [if](#if) condition must be satisfied to match the pattern.

```scala
try {
  ...
} catch {
  case error: ErrorClassName => ...
}
```

* Handles an error when it matches the
  [pattern matching](syntax.md#match-values).

```scala
case class ClassName(param: Type, ...)
```

* Defines a [case class](classes.md#case-class).
  It is a class made to hold data.
  Case class instances have the following features:
  1. supports [pattern matching](syntax.md#match-values),
  1. can be compared with each other `a == b`,
  1. produces a readable message when printed.

```scala
case object ObjectName { ... }
```

* Defines an object that can be serialized.

```scala
def x: Type1 => Type2 = {
  case x => ...
}
```

* Defines a function that processes its parameter
  through  [pattern matching](syntax.md#match-values).

## catch

```scala
try { ... } catch { ... }
```

* Handles a run time error.
  The expected errors are specified with [case](#case) keyword.

## class

```scala
class ClassName { ... }
```

* Defines a [class](#classes.md#class).
  A class is a blueprint for values called instances.
  A class can be defined with the following modifiers:

  ```scala
  abstract class ClassName { ... }
  ```

  * Defines an [abstract class](classes.md#abstract-class)
    that allows incomplete members.

  ```scala
  final class ClassName { ... }
  ```

  * Defines a [final class](classes.md#class-modifiers-final-and-sealed)
    that cannot be extended.

  ```scala
  sealed class ClassName { ... }
  ```

  * Defines a [sealed class](classes.md#class-modifiers-final-and-sealed)
    that can only be extended
    by classes defined in the same source file.

```scala
case class ClassName(...)
```

* Defines a [case class](classes.md#case-class),
  a class made to hold data.

## def

```scala
def myFunction(...) = { ... }
```

* Defines a [function](functions.md#defined-functions).
  A function is an identifier that can be called with or without attributes.

## do

```scala
do {
  ...
} while (...)
```

* Executes the statement in the `do` block as long as
  the [while](#while) condition is [true](#true).
  That is a [do-while loop](syntax.md#while-and-do-while-loops)

## else

```scala
if (a > 2) ... else ...
```

* Defines the statement that is executed
  when the [if](#if) condition is [false](#false).

## extends

```scala
class ClassName extends BaseClassName
```

* Creates a [class](classes.md#class) that inherits from an other class.

## false

```scala
val x: Boolean = false
```

* The Boolean literal `false`.

## final

```scala
final class ClassName { ... }
```

* Defines a class that cannot be extended.
  This class modifier cannot be combined with [sealed](#sealed).

```scala
final def classMember = ...
```

* Defines a class member that cannot be overridden.

```scala
final val x = 10
```

* **Without a type**, it defines a constant.

## finally

```scala
try {
  ...
} catch {
  ...
} finally {
  ...
}
```

* Runs the associated statement after a [try-catch](#try) block.
  Whether an error is caught or not.

## for

```scala
for (x <- items) ...
```

* Runs a **for-loop**. It retrieves each element of the container `items`.

```scala
for (x <- items) yield ...
```

* Runs a [for comprehension](syntax.md#for-comprehensions).
  This sentence produces a value that can be iterated through.

```scala
for (x <- items if (x > 2)) ...
```

* Defines a [for comprehension](syntax.md#for-comprehensions) with a guard.
  When the [if](#if) guard is [false](#false) the current element is skipped.

## forSome

```scala
type T = A forSome { type A }
```

* Defines an alias for an existential type.
  Existential type are generic types that are used to work with
  Java wildcard types and Java raw types.
  [Existential types should be avoided unless you don't have any other choice](https://github.com/scala/scala/blob/25d596e52a6b4bfea5ab2e62e98854e273c5abe4/src/library/scala/language.scala#L136).

## if

```scala
if (a > 2) ... else ...
```

* Starts a conditional
  [execution statement](syntax.md#if-else-conditional-block).

```scala
for (x <- items if (x > 2)) ...
```

* Defines a [for](#for) loop or a for comprehension with a **guard**.
  When the `if` condition is [false](#false) the current element is skipped.

```scala
x match {
  case a if (a > 2) => ...
}
```

* Defines a [pattern matching](#match) alternative
  with a **guard**. The associated statement is executed when the `x` value
  matches the pattern and the `if` condition is [true](#true).

## implicit

```scala
def methodName(implicit param: String) = ...
```

* Defines a parameter that is
  [implicitly replaced](classes.md#implicit-conversion)
  by a statement when the method is called. This mechanism is called
  [implicit conversion](classes.md#implicit-conversion).

```scala
implicit val x = 2
```

* Defines an identifier that will be used to replace
  an [implicit parameter](classes.md#implicit-conversion).

## import

```scala
import packagename.ClassName
```

* [imports an identifier](syntax.md#import-names).
  It makes the identifier visible and usable in the current scope.

```scala
import packagename._
```

* [Imports](syntax.md#import-names) every accessible identifier
  that is in the package.

```scala
import packagename.{Name1, Name2, ...}
```

* [Imports](syntax.md#import-names) the listed identifiers.

```scala
import packagename.{Name => NewName}
```

* [Imports](syntax.md#import-names) and renames an identifier.

## lazy

```scala
lazy val x = ...
```

* Defines a [lazy value](syntax.md#lazy-values),
  a value that is evaluated only when it is used for the first time.

## macro

```scala
def methodName: Unit = macro methodImplementation
```

* Defines a Scala macro implementation method.

## match

```scala
x match {
  case 1 => ...
  case 2 => ...
}
```

* Defines a [pattern matching](syntax.md#pattern-matching-with-match-and-case).
  It executes the statement associated with the [case](#case) pattern
  that matches `x` value.

## new

```scala
val x = new ClassName
```

* Creates a **new instance** of a [class](classes.md#class).

## null

```scala
val x: String = null
```

* Assigns a null value to `x`.
  Null value exists for compatibility reasons with Java.
  Because [Arnold hates NullPointerException](https://memegenerator.net/img/instances/500x/31084346/null-pointer-exception.jpg)
  it is safer to not explicitly use `null` and handle
  `null` values through [optional values](https://www.scala-lang.org/api/current/scala/Option.html).

## object

```scala
object ObjectName
```

* Defines an [object](classes.md#object).
  An object is a singleton class's instance.

```scala
case object ObjectName
```

* Defines an [object](classes.md#object) that can be serialized.

```scala
package object ObjectName
```

* Defines an object that is used as a [package](#package).
  The object members can be imported through `import ObjectName.memberName`.

## override

```scala
override val classMember = 2
```

* Redefines a [class](classes.md#class) member
  that have been previously defined in a parent class.

```scala
abstract override traitMethod = {...}
```

* Defines an [abstract method](#abstract) in a [traits](#trait).

## package

```scala
package packagename
```

* Placed on top of a file:
  means that **every item** defined in the file is part of the package.

```scala
package name { ... }
```

* Means that all the identifiers defined in the block
  are parts of the package.

```scala
package object ObjectName
```

* Defines an [object](classes.md#object) that is used as a package.
  The object members can be imported through `import ObjectName.memberName`

## private

```scala
private val classMember = 1
```

* Defines a [class](classes.md#class) member that can **only be accessed**
  from inside the **current class** &
  the **companion class** or [object](object).
  Private members cannot be used by sub classes.

```scala
private[EnclosingClass] val classMember = 2
```

* Defines a private value of an
  [enclosed class](classes.md#outer-and-inner-classes-instanceinner-and-outerinner)
  that can be accessed by the **enclosing class**.

```scala
private[this] val classMember = 3
```

* Defines an
  [object protected](/classes.md#object-protected-members-privatethis-protectedthis)
  member.
  This member is only visible in the current class.
  **The companion class** or companion object **cannot access it**.

## protected

```scala
protected val classMember = 1
```

* Defines a [class](classes.md#class) member that can **only be accessed**
  from **inside the class**, from its **derived classes** and from its
  [companion class or object](classes.md#companion-object-and-companion-class).

```scala
protected[EnclosingClass] val classMember = 2
```

* Defines a protected value of an
  [enclosed class](classes.md#outer-and-inner-classes-instanceinner-and-outerinner)
  that can be accessed by the **enclosing class** & **derived classes**.

```scala
protected[this] val classMember = 2
```

* Defines a member that is only visible in the
  [current class and its derived classes](/classes.md#object-protected-members-privatethis-protectedthis).
  **The companion class** or companion object **cannot access it**.

## return

```scala
return x
```

* Returns a value from a [function](functions.md).
  Scala implicitly returns the last statement of a function
  that have been executed.
  Thanks to that, **explicit returns can be avoided** in most cases.

## sealed

```scala
sealed class ClassName
```

* Defines a [class](#class) that can only be inherited
  by classes defined in the same source file.
  Therefore `sealed` and [final](#final) modifiers
  cannot be used on the same class.

## super

```scala
super.superClassMember
```

* Gives access to the super [classes](#class) members.

## this

```scala
this.classMember
```

* Here `this` represents the current instance
  of the [class](#class).

```scala
class ClassName {this: InjectedClassName => ... }
```

* Injects a class members into an other class.
  This mechanism is called **self typing**.

```scala
def this(param: Int) = this(param, 2L, ...)
```

* Defines a **constructor** with other parameter types.

```scala
private[this] val classMember = 2
```

* Defines an **object protected** member.
  This member is only visible in the current class.
  The companion class or companion object cannot access it.

```scala
protected[this] val classMember = 2
```

* Defines a member that is only visible in the current class and sub classes.
  The companion class or companion object cannot access it.

## throw

```scala
throw new ErrorClass(message)
```

* Throws an error.
  The error should be handled by a [try-catch](#try) block.

## trait

```scala
trait TraitName { ... }
```

* Defines an incomplete [class](#class) called `trait`.
  Traits features:
  1. cannot take initialization parameters,
  1. cannot be instantiated.

## try

```scala
try {
  ...
} catch {
  ...
}
```

* Handles run time errors that occurs
  in the `try` block. The error is [caught](#catch)
  and processed through [pattern matching](#case).

## true

```scala
val x: Boolean = true
```

* The Boolean literal `true`.

## type

```scala
type T = Int
```

* Defines an alias for a type.

## val

```scala
val x = 1
```

* Defines an identifier whose content **cannot be modified**.

## var

```scala
var x = 2
```

* Defines an identifier whose content **can be modified**.

## while

```scala
do {
  ...
} while (...)
```

* Starts a [do-while](#do) loop.
  Loops until the `while` condition is [false](#false).

```scala
while (...) {
  ...
}
```

* Starts a `while` loop.
  Loops until the `while` condition is [false](#false).

## with

```scala
class ClassName extends Base1 with Trait1 with Trait2
```

* Creates a [class](#class) that inherits from multiple classes
  and [traits](#trait).

```scala
val x = new ClassName with Trait1 with Trait2
```

* Creates an instance
  by combining multiple types ([class](#class) & [traits](#trait))
  to generate a new type.

## yield

```scala
for (x <- items) yield ...
```

* Defines a [for generator](#for).
  The generator produces a composite value (List, Array, etc...).
