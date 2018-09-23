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
| [underscore `_`](#underscore-_) | [colon `:`](#colon-) | [equals sign `=`](#equals-sign-) | [rightward double arrow `=>`, `⇒`](#rightward-double-arrow-) |
| [leftward arrow `<-`, `←`](#leftward-arrow-) | [less than percent `<%`](#less-than-percent-) | [less than colon `<:`](#less-than-colon-) | [more than colon `>:`](#more-than-colon-) |
| [hashtag `#`](#hashtag-) | [at `@`](#at-) | [star `*`](#star-) | [parenthesis `( )`](#parenthesis-) |
| [brackets `[ ]`](#brackets-) | [curly brackets `{ }`](#curly-brackets-) | [comment signs `//`, `/* */`](#comment-signs-) | [double quotes `"`, `"""`](#double-quotes-) |
| [single quotes `'`](#single-quotes-) | [back quotes `` ` ``](#back-quotes-) | [comma `,`](#comma-) | [semicolon `;`](#semicolon-) |

## 20 Scala symbols explained:

#### underscore `_`

* `x match { case CaseClassName(_, _) => 33 }`, here `_` is a placeholder
  for unused parameters. An other example: `x match { case _ => 0 }`.
* `import packagename._` imports every accessible identifier
  that is in the package.
* `import packagename.{name => _, _}` imports everything but `name`.
* `val functionName:Type1 => Type2 = (_.toType2)` curried form of a function
  definition. Same as `val functionName:Type1 => Type2 = (x) => x.toType2`.
  The curried form can also be used for function calls,
  example: `List(1, 2, 3).map(_ + 2) // --> List(3, 4, 5)`.
* `val x = tupleName._1` accesses the first element of a tuple.
* `val functionName = method(_, _, _)` creates a new function
  from an existing one using currying.
* `var x: Type = _` creates a variable that contains an empty value.
* `methodName(argList:_*)` expands the parameters when calling a method
  that has a variable number of parameters.
* `def unary_- = 0 - this.x` defines the unary operator `-x`
  for a class instances.
* `def methodName(param: GenericType[_])...` defines a method that takes
  a parameter that has a generic **existential type**.
  `_` here is a placeholder for any type.
* `def member_= ...` is part of the definition of a private class member
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

* `val name: Type = ...` defines the type of an identifier.
* `(x: @unchecked) match { case _ => ... }` annotates an expression.
* `methodName(argList:_*)` expands the parameters when calling a method
  that has a variable number of parameters.

#### equals sign `=`

* `val x = 5` creates an identifier with a given value.
* `varName = 5` assigns a new value to an existing variable.
* `var x: Type = _` creates a variable that contains an empty value.
  According to the variable's type, the empty value will be:
  `0`, `false`, `Unit` or `null`.
* `x(1, 2) = 3` runs an **assignment expansion**.
  This is equivalent to the following expression: `x.update(1, 2, 3)`
* `val x(param) = y` runs an **object extractor** statement.
  This is equivalent to the following expression: `val param = x.unapply(y)`
* `def methodName = ...` starts the body of a method.
* `def methodName(param: Type = 4) = ...`
  defines a function that has parameters with a default values.
  It is the same with classes: `class ClassName(param: Type = 4) = ...`.
* `type name = Type` defines an alias for a type.

#### rightward double arrow `=>`, `⇒`

* `val x:(Type1, Type2) => ReturnType = ...` defines a function type.
* `val x = (x: Type) => x + 3` defines a function.
* `def methodName(param => Type) ...` defines a **by-name** parameter.
  This parameter is evaluated at each access.
* `import mypackage.{Type => NewType}` renames an imported identifier.
* `{ this: InjectedClassName => ... }` injects a class parameters
  into an other class using. Its called **self typing**.
* `x match { case _: => 0 }` defines a pattern matching alternative.

#### leftward arrow `<-`, `←`

* `for (x <- itemList) { ... }`
  defines the for-comprehensions loop current item.

#### less than percent `<%`

* `def methodName[TypeAlias <% Converter[TypeAlias]](param: TypeAlias) ...`
  creates a generic method that takes arguments
  with a type that can be converted using the `Converter` generic class.
  This notation called
  [view bounds is deprecated!](https://github.com/scala/scala/pull/2909)

#### less than colon `<:`

* `type TypeAlias <: SuperType` defines an alias for types
  that are sub classes of the **upper bound** `SuperType`.

#### more than colon `>:`

* `type TypeAlias >: SubType` defines an alias for types
  that are super classes of the **lower bound** `SubType`.

#### hashtag `#`

* `val x OuterType#InnerType` creates a variable.
  The variable type `InnerType` is defined inside an other type `OuterType`.
  This notation is called **type projection**.

#### at `@`

* `@annotation` uses an annotation.
  Annotations are code structures that modifies the behavior of expressions.
* `x match { case a@(1 | 2) => a + 1 }` creates the identifier `a`
  with `x` value, when `x` matches the pattern `(1 | 2)`.
  This notation is called **pattern binder**.

#### star `*`

* `def myMethod(args: Type*)` defines a function with a variable
  number of arguments.
* `val x = myMethod(argsList:_*)` calls a function that takes a variable
  number of arguments with a list of arguments.
* `x match { case CaseClassName(a, _*) => a + 1 }"` extracts the specified
  parameters of a `case class` during pattern matching.

#### parenthesis `( )`

* `def methodName(param1: Type1, param2: Type2)...`
  defines a method with a list of parameters.
  Class parameters are also defined using parenthesis.
* `(x: Type1, y:Type2) => x + y` defines an anonymous function parameters.
* `val x = methodName(param1, param2)`
  calls a method or a function with a list of parameters.
  Class instance parameters are also listed using parenthesis.
* `val x = (1, "text")` defines a two elements tuple.
* `var x:(Type1, Type2) = _` defines a tuple variable by specifying its type.
* `if (x == 1) ...` defines the condition part of an expression.
  It is the same for `while (...)` & `do ... while (...)` expressions.
* `for (x <- items) ...` defines the loop enumerator.
* `(x + 1) + 2` groups a block of expressions.

#### brackets `[ ]`

* `def methodName[TypeAlias](param: TypeAlias)...` defines a generic method.
  Other generic structures can also be defined using brackets.
* `private[EnclosingClass] val classMember = 2`
  defines a private value of an enclosed class with special restrictions.
  `protected` values with special restrictions can also be defined this way.
* `private[this] val classMember = 3` defines an **object protected** member.
  `protected` values with special restrictions can also be defined this way.

#### curly brackets `{ }`

* `{ x + 1 }` defines a block of code.
* `import packagename.{Name1, Name2 => NewName2, ...}`
  imports multiple identifiers.
* `class ClassName { val x = 1 }`
  **refines** a class by adding some code inside.
* `x match { case ... }` matches patterns in the `match` block.
* `for { x <- items } ...` defines the loop enumerator.
* `try ... catch { case ... }` handles a run time error in the `catch` block.
* `package name { ... }` defines a package.
* `type T = A forSome { type A }` defines an alias for an existential type.
  [Existential types should be avoided when possible.](https://github.com/scala/scala/blob/25d596e52a6b4bfea5ab2e62e98854e273c5abe4/src/library/scala/language.scala#L136)

#### comment signs `//`, `/* */`

* `// inline comment` is an inline comment.
  ```scala
  /*
  multi-lines
  comment
  */
  ```
  is a multi-lines comment.

#### double quotes `"`, `"""`

* `val x = "text"` defines a string value.
  The following code defines a multilines string value:
  ```scala
  val x = """
    long
    text
  """
  ```

#### single quotes `'`

* `val c = 'X'` creates a character.
* `val x = 'symbolValue` creates a symbol.
  Strings that  are only used for the code logic can be replaced by symbols:
  `Map` keys for example.

#### back quotes `` ` ``

* ``val `reservedKeywords` = 2`` allows the usage of reserved keywords
  as an identifier. Example ``val `case` = 1``.

#### comma `,`

* `def methodName(x: Type1, y: Type2)` the comma separates multiple parameters
  in a definition (method, class, etc...).
* `methodName(x, y)` the comma separates multiple parameters in a call
  (method, class, etc...).
* `GenericType[Type1, Type2]`
  the comma separates multiple types in a type definition.
* `val x, y, z = 3` defines multiple identifiers with the same values.
* `import packagename.{Name1, Name2, ...}` imports multiple identifiers.

#### semicolon `;`

* `val x = 6; x + 1` defines a two lines expression in one line.