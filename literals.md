# Scala literals explained

Literals are raw values that are written in a source code.

* TOC
{:toc}

## Most used literals

### 1. Integer literals

There are two representations of integer literals in Scala,
* the decimal representation: `0`, `3615`, `-99`, etc...
* and the hexadecimal representation: `0x0`, `0xe1f`, `-0x63`, etc...

For `Long` integer values, a `l` or `L` indicator should be appended to
the literal: `9016651116L`, `-0x2196F2D6Cl`

Example:

```scala
val x = 1234
val y = -0x4d2 // 0x4d2 is the hexadecimal representation of 1234
println(y)
// --> 1234

val sum = x + y
println(s"the sum is: ${ sum }")
// --> the sum is: 0
```

### 2. Floating point literals (for real numbers)

Real number are [approximately represented by floating point numbers](https://en.wikipedia.org/wiki/Floating-point_arithmetic).

There are two notations of floating point numbers,
* the "classic" notation: `0.0`, `36.15`, `-.99` (same as -0.99)
* and the exponential notation: `0e0`, `.3615e2`, `-9900e-4`, etc...

Dy default the floating point literals have the type `Double`.
But it is possible to choose the literal type by appending:
* `f` or `F` to create a `Float`: `0.0f`, `.3615e2F`, `-.99f`...
* `d` or `D` to create a `Double`: `0.0d`, `.3615e2D`, `-.99d`...

```scala
val pi = 31415e-4d
println(pi)
// --> 3.1415

val onePercent = .01
println(s"the percentage is ${ 100 * onePercent }%")
// --> the percentage is 1.0%
```

### 3. Boolean

There are two Boolean literals: `true` and `false`

```scala
val isOK = true
val isKO = false
println(s"that's OK? ${ isOK != isKO }")
// --> that's OK? true
```

### 4. Character

In Scala character literals are written in simple quotes `'`,
and there are two way to represent a character literal:
* the "classic" representation: `'a'`, `'b'`, `'@'`, `'5'`, `'☺'`...
* escaped representation: `'\n'`, `'\t'`, `'\u0020'`

```scala
val asciiTCharacter = 't'
val unicodeTopArrow = '\u2191'
println(s"go to the ${ asciiTCharacter }op $unicodeTopArrow")
// --> go to the top ↑
```

### 5. Strings

#### Single & multi line strings

String literals are written using single double quotes or triple double quotes.
* Single line string are written with single double quotes: `"hello"`.
  Inside a string, the double quote character should always be escaped `\"`,
  ex: `"{\"json_value\": 4}"`.

  There are over characters that can be escaped too:
  * `\\`, `\'`,
  * `\t`, `\n`, `\r`,
  * `\f` [form feed](htts://en.wikipedia.org/wiki/Page_break),
  * `\b` [backspace](https://en.wikipedia.org/wiki/Backspace),
  * `\u...` any Unicode character.

* Multi-lines string are written with triple double quotes: `"""C:\path\to"""`.
  All the characters are represented as they are.

```scala
// newline character escaped
val textPart1 = "Car\nMotorbike"

// no need to escape newline characters
val textPart2 = """
Bike
Plane
"""

println(textPart1 + textPart2)
// --> Car
// --> Motorbike
// --> Bike
// --> Plane
```

#### String interpolation

String literals can be represented with an interpolation indicator:
* `s"${variableName}"`: replaces the variable name by its value.
* `f"${variableName}%d"`: replaces the variable name by its value
  and applies the specified formatting.
* `raw"unescaped\text"`: the escape characters are not interpreted.

```scala
val pi = 3.1415

println(s"value: $pi")
// --> value: 3.1415

println(f"value: $pi%.1f")
// --> value: 3.1

println(raw"\new look")
// --> \new look
```

## Other literals

### 6. Symbols

A symbols is a kind-of string only used by the application logic,
in opposition to actual `String` values that are used as application data.
A symbol literal is represented by a word starting with a simple quote:
`'connectionStarted`

```scala
val statusCodes = Map('exitSuccess -> 0, 'exitFailure -> 1)
val code = statusCodes('exitSuccess)

println(s"the status code is $status")
// --> the status code is 0
```

### 7. Bonus: XML literals

An XML literal represents an XML content that is written inline,
in the source code:

```scala
val htmlDiv =
    <div>
      <a href="https://example.com">
        Click here
      </a>
    </div>

val link = (htmlDiv \\ "@href").text
println(s"the extracted link is $link")
// --> the extracted link is https://example.com
```
