# Scala symbols explained

Here we will explain **all Scala language reserved symbols**.

Reserved symbols are special character, punctuation
that have a special meaning in Scala.
They cannot be used as an identifier (variable name or class name).
For each reserved symbol there is a list of use cases
and for each use case there is a concise explanation.
The uses cases and explanations are mostly taken from the
[Scala language specification](https://www.scala-lang.org/files/archive/spec/2.12/)

Note that operators `+`, `-`, etc.. are not always reserved symbols.
They are identifiers that can be redefined.

There are currently **20 reserved symbols in Scala** language:

| | | | |
|-|-|-|-|
| [`_`](#underscore-_) | [`:`](#colon-) | [`=`](#equals-sign-) | [`=>`, `⇒`](#rightward-double-arrow--) |
| [`<-`, `←`](#leftward-arrow---) | [`<%`](#less-than-percent-) | [`<:`](#less-than-colon-) | [`>:`](#more-than-colon-) |
| [`#`](#hashtag-) | [`@`](#at-) | [`*`](#star-) | [`( )`](#parenthesis--) |
| [`[ ]`](#brackets--) | [`{ }`](#curly-brackets--) | [`//`, `/* */`](#comment-signs---) | [`"`, `"""`](#double-quotes--) |
| [`'`](#single-quotes-) | [`` ` ``](#back-quotes-) | [`,`](#comma-) | [`;`](#semicolon-) |

## 20 Scala symbols explained:

#### underscore `_`

```scala
x match { case CaseClassName(_, _) => 33 }
```

* here `_` is a placeholder
  for unused parameters. An other example: `x match { case _ => 0 }`.

```scala
import packagename._
```

* imports every accessible identifier
  that is in the package.

```scala
import packagename.{name => _, _}
```

* imports everything but `name`.


```scala
val functionName:Type1 => Type2 = (_.toType2)
```

* curried form of a function
  definition. Same as `val functionName:Type1 => Type2 = (x) => x.toType2`.
  The curried form can also be used for function calls,
  example: `List(1, 2, 3).map(_ + 2) // --> List(3, 4, 5)`.

```scala
val x = tupleName._1
```

* accesses the first element of a tuple.

```scala
val functionName = method(_, _, _)
```

* creates a new function
  from an existing one using currying.

```scala
var x: Type = _
```

* creates a variable that contains an empty value.

```scala
methodName(argList:_*)
```

* expands the parameters when calling a method
  that has a variable number of parameters.

```scala
def unary_- = 0 - this.x
```

* defines the unary operator `-x`
  for a class instances.

```scala
def methodName(param: GenericType[_])...
```

* defines a method that takes
  a parameter that has a generic **existential type**.
  `_` here is a placeholder for any type.

```scala
def member_= ...
```

* is part of the definition of a private class member
  getter and setter. Full example:

  ```scala
  class ClassName {
    private var _member: Long = _
    // getter
    def member = _member
    // setter
    def member_= (x: Long): Unit =
      _member = x
  }
  ```

#### colon `:`


```scala
val name: Type = ...
```

* defines the type of an identifier.

```scala
(x: @unchecked) match { case _ => ... }
```

* annotates an expression.

```scala
methodName(argList:_*)
```

* expands the parameters when calling a method
  that has a variable number of parameters.

#### equals sign `=`


```scala
val x = 5
```

* creates an identifier with a given value.

```scala
varName = 5
```

* assigns a new value to an existing variable.

```scala
var x: Type = _
```

* creates a variable that contains an empty value.
  According to the variable's type, the empty value will be:
  `0`, `false`, `Unit` or `null`.

```scala
x(1, 2) = 3
```

* runs an **assignment expansion**.
  This is equivalent to the following expression: `x.update(1, 2, 3)`

```scala
val x(param) = y
```

* runs an **object extractor** statement.
  This is equivalent to the following expression: `val param = x.unapply(y)`

```scala
def methodName = ...
```

* starts the body of a method.

```scala
def methodName(param: Type = 4) = ...
```

* defines a function that has parameters with a default values.
  It is the same with classes: `class ClassName(param: Type = 4) = ...`.

```scala
type name = Type
```

* defines an alias for a type.

#### rightward double arrow `=>`, `⇒`


```scala
val x:(Type1, Type2) => ReturnType = ...
```

* defines a function type.

```scala
val x = (x: Type) => x + 3
```

* defines a function.

```scala
def methodName(param => Type) ...
```

* defines a **by-name** parameter.
  This parameter is evaluated at each access.

```scala
import mypackage.{Type => NewType}
```

* renames an imported identifier.

```scala
{ this: InjectedClassName => ... }
```

* injects a class parameters
  into an other class using. Its called **self typing**.

```scala
x match { case _: => 0 }
```

* defines a pattern matching alternative.

#### leftward arrow `<-`, `←`


```scala
for (x <- itemList) { ... }
```

* defines the for-comprehensions loop current item.

#### less than percent `<%`


```scala
def methodName[TypeAlias <% Converter[TypeAlias]](param: TypeAlias) ...
```

* creates a generic method that takes arguments
  with a type that can be converted using the `Converter` generic class.
  This notation called
  [view bounds is deprecated!](https://github.com/scala/scala/pull/2909)

#### less than colon `<:`


```scala
type TypeAlias <: SuperType
```

* defines an alias for types
  that are sub classes of the **upper bound** `SuperType`.

#### more than colon `>:`


```scala
type TypeAlias >: SubType
```

* defines an alias for types
  that are super classes of the **lower bound** `SubType`.

#### hashtag `#`


```scala
val x OuterType#InnerType
```

* creates a variable.
  The variable type `InnerType` is defined inside an other type `OuterType`.
  This notation is called **type projection**.

#### at `@`


```scala
@annotation
```

* uses an annotation.
  Annotations are code structures that modifies the behavior of expressions.

```scala
x match { case a@(1 | 2) => a + 1 }
```

* creates the identifier `a`
  with `x` value, when `x` matches the pattern `(1 | 2)`.
  This notation is called **pattern binder**.

#### star `*`


```scala
def myMethod(args: Type*)
```

* defines a function with a variable
  number of arguments.

```scala
val x = myMethod(argsList:_*)
```

* calls a function that takes a variable
  number of arguments with a list of arguments.

```scala
x match { case CaseClassName(a, _*) => a + 1 }
```

* extracts the specified
  parameters of a `case class` during pattern matching.

#### parenthesis `( )`


```scala
def methodName(param1: Type1, param2: Type2)...
```

* defines a method with a list of parameters.
  Class parameters are also defined using parenthesis.

```scala
(x: Type1, y:Type2) => x + y
```

* defines an anonymous function parameters.

```scala
val x = methodName(param1, param2)
```

* calls a method or a function with a list of parameters.
  Class instance parameters are also listed using parenthesis.

```scala
val x = (1, "text")
```

* defines a two elements tuple.

```scala
var x:(Type1, Type2) = _
```

* defines a tuple variable by specifying its type.

```scala
if (x == 1) ...
```

* defines the condition part of an expression.
  It is the same for `while (...)` & `do ... while (...)` expressions.

```scala
for (x <- items) ...
```

* defines the loop enumerator.

```scala
(x + 1) + 2
```

* groups a block of expressions.

#### brackets `[ ]`


```scala
def methodName[TypeAlias](param: TypeAlias)...
```

* defines a generic method.
  Other generic structures can also be defined using brackets.

```scala
private[EnclosingClass] val classMember = 2
```

* defines a private value of an enclosed class with special restrictions.
  `protected` values with special restrictions can also be defined this way.

```scala
private[this] val classMember = 3
```

* defines an **object protected** member.
  `protected` values with special restrictions can also be defined this way.

#### curly brackets `{ }`


```scala
{ x + 1 }
```

* defines a block of code.

```scala
import packagename.{Name1, Name2 => NewName2, ...}
```

* imports multiple identifiers.

```scala
class ClassName { val x = 1 }
```

* **refines** a class by adding some code inside.

```scala
x match { case ... }
```

* matches patterns in the `match` block.

```scala
for { x <- items } ...
```

* defines the loop enumerator.

```scala
try ... catch { case ... }
```

* handles a run time error in the `catch` block.

```scala
package name { ... }
```

* defines a package.

```scala
val x = new TraitName { ... }
```

* extends a trait (or a class)
  then instantiates the resulting anonymous class.

```scala
type T = A forSome { type A }
```

* defines an alias for an existential type.
  [Existential types should be avoided when possible.](https://github.com/scala/scala/blob/25d596e52a6b4bfea5ab2e62e98854e273c5abe4/src/library/scala/language.scala#L136)

#### comment signs `//`, `/* */`


```scala
// inline comment
```

* is an inline comment.

  ```scala
  /*
  multi-lines
  comment
  */
  ```

  is a multi-lines comment.

#### double quotes `"`, `"""`


```scala
val x = "text"
```

* defines a string value.
  The following code defines a multilines string value:

  ```scala
  val x = """
    long
    text
  """
  ```

#### single quotes `'`


```scala
val c = 'X'
```

* creates a character.

```scala
val x = 'symbolValue
```

* creates a symbol.
  Strings that  are only used for the code logic can be replaced by symbols:
  `Map` keys for example.

#### back quotes `` ` ``


```scala
val `reservedKeywords` = 2
```

* allows the usage of reserved keywords
  as an identifier. Example ``val `case` = 1``.

#### comma `,`


```scala
def methodName(x: Type1, y: Type2)
```

* the comma separates multiple parameters
  in a definition (method, class, etc...).

```scala
methodName(x, y)
```

* the comma separates multiple parameters in a call
  (method, class, etc...).

```scala
GenericType[Type1, Type2]
```

* the comma separates multiple types in a type definition.

```scala
val x, y, z = 3
```

* defines multiple identifiers with the same values.

```scala
import packagename.{Name1, Name2, ...}
```

* imports multiple identifiers.

#### semicolon `;`


```scala
val x = 6; x + 1
```

* defines a two lines expression in one line.
