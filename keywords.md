# Scala keywords explained

We will take a tour of **all Scala language reserved keywords**.

Reserved keywords are words that have a special meaning in Scala.
They cannot be used as an identifier (variable name or class name).
For each reserved keyword there is a list of use cases
and for each use case there is a concise explanation.
The uses cases and explanations are mostly taken from the
[Scala language specification](https://www.scala-lang.org/files/archive/spec/2.12/)

There are currently **40 reserved keywords in Scala** language:

| | | | | | |
|-|-|-|-|-|-|
| [abstract](#abstract) | [case](#case) | [catch](#catch) | [class](#class) | [def](#def) | [do](#do) |
| [else](#else) | [extends](#extends) | [false](#false) | [final](#final) | [finally](#finally) | [for](#for) |
| [forSome](#forSome) | [if](#if) | [implicit](#implicit) | [import](#import) | [lazy](#lazy) | [macro](#macro) |
| [match](#match) | [new](#new) | [null](#null) | [object](#object) | [override](#override) | [package](#package) |
| [private](#private) | [protected](#protected) | [return](#return) | [sealed](#sealed) | [super](#super) | [this](#this) |
| [throw](#throw) | [trait](#trait) | [try](#try) | [true](#true) | [type](#type) | [val](#val) |
| [var](#var) | [while](#while) | [with](#with) | [yield](#yield) | | | |

Let's explain them all!

## 40 Scala keywords explained:

#### abstract

* `abstract class ClassName`
  defines a class with incomplete members. Abstract classes features:
  1. they can take initialization parameters,
  1. they cannot be instantiated.
* `abstract override def traitMethod = { ... }`
  defines [traits](#trait) methods that calls unrelated
  [super](#super) methods. In short, it allows the usage of the
  [stackable trait pattern](https://www.artima.com/scalazine/articles/stackable_trait_pattern.html).

#### case

* `x match { case 2 => ... }`
  executes the statement when the `x` value matches the pattern.
  This feature is called **pattern matching**.
* `x match { case a if (a > 2) => ... }` like above but the [if](#if) condition
  must be satisfied too.
* `try { ... } catch { case error: ErrorClassName => ... }`
  handles an error when it matches the specified pattern.
  The "if condition" can be use here too.
* `case class ClassName(param: Type, ...)` defines a [case class](#class).
  It is a class made to hold data.
  Case class instances have the following features:
  1. supports pattern matching,
  1. can be compared with each other instances `a == b`,
  1. produces a readable message when printed.
* `case object ObjectName { ... }` defines an object that can be serialized.
* `def x: Type1 => Type2 = { case x => ... }`
  defines a method that is bound an anonymous function.
  This anonymous function processes its parameter using pattern matching.

#### catch

* `try { ... } catch { ... }` handles a run time error.
  The error is processed through [pattern matching](#match).

#### class

* `class ClassName` defines a class, a blueprint for values called instances.
  A class can be associated with the following modifiers:
  * `abstract class ClassName` defines a class with incomplete members.
  * `final class ClassName` defines a class that cannot be inherited.
  * `sealed class ClassName` defines a class that can only be inherited
    by classes defined in the same source file.
* `case class ClassName` defines a class made to hold data:
  a [case class](#case).

#### def

* `def myMethod(...)` defines a method.
  A method is an identifier that can be called with or without parameters.

#### do

* `do { ... } while (...)` starts a **do-while** loop.

#### else

* `if (a > 2) ... else ...` defines the statement that is executed
  when the [if](#if) condition is [false](#false).

#### extends

* `class ClassName extends BaseClassName` creates a [class](#class)
  that inherits from an other class or a [trait](#trait).

#### false

* `val x: Boolean = false`, the Boolean literal `false`.

#### final

* `final class ClassName` defines a class that cannot be inherited.
  This class modifier cannot be combined with [sealed](#sealed).
* `final def classMember` defines a class member that cannot be overridden.
* `final val x = 10`, without a type, it defines a constant.

#### finally

* `try { ... } catch { ... } finally { ... }`
  runs the associated statement after the [try-catch](#try) block.
  Whether an error is caught or not.

#### for

* `for (x <- items) ...` defines a **for loop**.
* `for (x <- items) yield ...` defines a **for generator**.
  The generator produces a value that can be iterated through
  (List, Array, etc...).
* `for (x <- items if (x > 2)) ...` defines a loop or a generator with a guard.
  When the [if](#if) guard is [false](#false) the current element is skipped.

#### forSome

* `type T = A forSome { type A }` defines an alias for an existential type.
  Existential type are generic types that are used to work with
  Java wildcard types and Java raw types.
  [Existential types should be avoided unless you don't have any other choice](https://github.com/scala/scala/blob/25d596e52a6b4bfea5ab2e62e98854e273c5abe4/src/library/scala/language.scala#L136)

#### if

* `if (a > 2) ... else ...` starts a conditional execution statement.
* `for (x <- items if (x > 2)) ...`
  defines a [for](#for) loop or a for generator with a guard.
  When the `if` condition is [false](#false) the current element is skipped.
* `x match { case a if (a > 2) => ... }`
  defines a [pattern matching](#match) alternative
  with a guard. The associated statement is executed when the `x` value
  matches the pattern and the `if` condition is [true](#true).

#### implicit

* `def methodName(implicit param: String)` defines a parameter that is
  implicitly replaced by a statement when the method is called.
  This mechanism is called **implicit conversion**.
* `implicit val x = 2` defines an identifier that will be used to replace
  an implicit parameter.

#### import

* `import packagename.ClassName` **imports an identifier**.
  It makes the identifier visible and usable in the current scope.
* `import packagename._` imports every accessible identifier
  that is in the package.
* `import packagename.{Name1, Name2, ...}` imports the listed identifiers.
* `import packagename.{Name => NewName}` imports and renames an identifier.

#### lazy

* `lazy val x = { ... }` defines a **lazy value**,
  a value that is evaluated only when it is used for the first time.

#### macro

* `def methodName: Unit = macro methodImplementation`
  defines a Scala macro implementation method.

#### match

* `x match { case 1 => ...; case 2 => ... }` defines a **pattern matching**.
  It executes the statement associated with the [case](#case) pattern
  that matches `x` value.

#### new

* `val x = new ClassName` creates a **new instance** of a class.

#### null

* `val x: String = null` assigns a null value to `x`.
  Null value exists for compatibility reasons with Java.
  Because [Arnold hates NullPointerException](https://memegenerator.net/img/instances/500x/31084346/null-pointer-exception.jpg)
  it is safer to not explicitly use `null` and handle
  `null` values through [optional values](https://www.scala-lang.org/api/current/scala/Option.html).

#### object

* `object ObjectName` defines a singleton [class](#class).
* `case object ObjectName` defines an object that can be serialized.
* `package object ObjectName`
  defines an object that is used as a [package](#package).
  The object members can be imported through `import ObjectName.memberName`.

#### override

* `override val classMember = 2`
  redefines a [class](#class) member that have been previously defined
  in a parent class.
* `abstract override traitMethod = {...}`
  defines an [abstract method](#abstract) in a [traits](#trait), to use of the
  [stackable trait pattern](https://www.artima.com/scalazine/articles/stackable_trait_pattern.html).

#### package

* `package packagename` placed on top of a file:
  means that **every item** defined in the file is part of the package.
* `package name { ... }` means that all the identifiers defined in the block
  are parts of the package.
* `package object ObjectName`
  defines an [object](#object) that is used as a package.
  The object members can be imported through `import ObjectName.memberName`

#### private

* `private val classMember = 1`
  defines a [class](#class) member that can only be *accessed
  from inside the current class* & the companion class or [object](object).
  Private members cannot be used by sub classes.
* `private[EnclosingClass] val classMember = 2`
  defines a private value of an enclosed class
  that can be *accessed by the enclosing class*.
* `private[this] val classMember = 3` defines an **object protected** member.
  This member is only visible in the current class.
  *The companion class or companion object cannot access it*.

#### protected

* `protected val classMember = 1`
  defines a [class](#class) member that can only be *accessed
  from inside the class, from its sub classes* and
  from their companion classes/[objects](#object).
* `protected[EnclosingClass] val classMember = 2`
  defines a protected value of an enclosed class
  that can be accessed by the *enclosing class & sub classes*.
* `protected[this] val classMember = 2`
  defines a member that is only visible in the current class and sub classes.
  *The companion class or companion object cannot access it*.

#### return

* `return x` returns a value from a function or a [method](#def).
  Scala implicitly returns the last statement of a function
  that have been executed.
  Thanks to that, explicit returns can be avoided in most cases.

#### sealed

* `sealed class ClassName` defines a [class](#class) that can only be inherited
  by classes defined in the same source file.
  Therefore `sealed` and [final](#final) modifiers
  cannot be used on the same class.

#### super

* `super.superClassMember` gives access to the super [classes](#class) members.

#### this

* `this.classMember`, here `this` represents the current instance
  of the [class](#class).
* `class ClassName {this: InjectedClassName => ... }`
  injects a class members into an other class.
  This mechanism is called **self typing**.
* `def this(param: Int) = this(param, 2L, ...)`
  defines a **constructor** with other parameter types.
* `private[this] val classMember = 2` defines an **object protected** member.
  This member is only visible in the current class.
  The companion class or companion object cannot access it.
* `protected[this] val classMember = 2`
  defines a member that is only visible in the current class and sub classes.
  The companion class or companion object cannot access it.

#### throw

* `throw new ErrorClass(message)` throws an error.
  The error should be handled by a [try-catch](#try) block.

#### trait

* `trait TraitName` defines an incomplete [class](#class) called `trait`.
  Traits features:
  1. cannot take initialization parameters,
  1. cannot be instantiated.

#### try

* `try { ... } catch { ... }` handles run time errors that occurs
  in the `try` block. The error is [caught](#catch)
  and processed through [pattern matching](#case).

#### true

* `val x: Boolean = true`, the Boolean literal `true`.

#### type

* `type T = Int` defines an alias for a type.

#### val

* `val x = 1` defines an identifier whose content **cannot be modified**.

#### var

* `var x = 2` defines an identifier whose content **can be modified**.

#### while

* `do { ... } while (...)` starts a [do-while](#do) loop.
  Loops until the `while` condition is [false](#false).
* `while (...) { ... }` starts a `while` loop.
  Loops until the `while` condition is [false](#false).

#### with

* `class ClassName extends Base1 with Trait1 with Trait2`
  creates a [class](#class) that inherits from multiple classes
  and [traits](#trait).
* `val x = new ClassName with Trait1 with Trait2` creates an instance
  by combining multiple types ([class](#class) & [traits](#trait))
  to generate a new type.

#### yield

* `for (x <- items) yield ...` defines a [for generator](#for).
  The generator produces a composite value (List, Array, etc...).
