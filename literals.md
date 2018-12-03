# Scala literals explained

A literal is a raw value that are directly written in a source code.
Ex: `3.14`, `"Hi!"`, `false`, etc...

* TOC
{:toc}

## Integer literals

There are two representations of integer literals in Scala:
* the decimal representation: `0`, `3615`, `-99`, etc...
* and the hexadecimal representation: `0x0`, `0xe1f`, `-0x63`, etc...

The character `l` or `L` can be appended to an integer literal to specify
that its type is `Long`. Ex: `9016651116L`, `-0x2196F2D6Cl`

Example:

```scala
// Sum integer values.

val x = 1234
val y = -0x4d2 // 0x4d2 is the hexadecimal representation of 1234

println(y)
// --> 1234

val sum = x + y

println(s"the sum is: ${ sum }")
// --> the sum is: 0
```

## Floating point literals (for real numbers)

Real number are [approximately represented by floating point numbers](https://en.wikipedia.org/wiki/Floating-point_arithmetic).

There are two notations for floating point numbers:
* the decimal notation: `0.0`, `36.15`, `-.99` (same as -0.99)
* and the exponential (or scientific) notation:
  `0e0`, `.3615e2`, `-9.9e-1`, etc...

Dy default the floating point literals are instances of `Double` type.
But it is possible to choose the literal type by appending:
* `f` or `F` to create a `Float` value: `0.0f`, `.3615e2F`, `-.99f`...
* `d` or `D` to create a `Double` value: `0.0d`, `.3615e2D`, `-.99d`...

```scala
// Printing floating point numbers.

val pi = 31415e-4d
println(pi)
// --> 3.1415

val onePercent = .01
println(s"the percentage is ${ 100 * onePercent }%")
// --> the percentage is 1.0%
```

## Boolean

There are two Boolean literals: `true` and `false`

```scala
// Comparing boolean numbers.

val isOK = true
val isKO = false
val isStillOK = isOK != isKO

println(s"that's still OK? $isStillOK")
// --> that's still OK? true
```

## Character

In Scala, character literals are written in simple quotes `'`
and there are two way to represent a character literal:
* the unescaped representation: `'a'`, `'b'`, `'@'`, `'5'`, `'☺'`...
* the escaped representation: `'\n'`, `'\t'`, `'\u0020'`

```scala
// Print special characters.

val asciiT = 'T'
val unicodeTopArrow = '\u2191'

println(s"go to the ${ asciiT }op $unicodeTopArrow")
// --> go to the Top ↑
```

## Strings

### Single & multi line strings

String literals are written in double quotes `"` or triple double quotes `"""`.
* **Single line string** are written in double quotes: `"hello"`.
  A double quote char that is inside a single line string must be escaped `\"`.
  Ex: `"{\"json_value\": 4}"`.

  There are over characters that can be escaped too:
  * `\\` anti-slash,
  * `\'` single quote,
  * `\t` tabulation,
  * `\n` new line,
  * `\r` carriage return,
  * `\f` [form feed](htts://en.wikipedia.org/wiki/Page_break),
  * `\b` [backspace](https://en.wikipedia.org/wiki/Backspace),
  * `\u...` any Unicode character.

* **Multi-lines string** are written in triple double quotes:
  `"""C:\path\to"""`.
  **Unicode characters can still be escaped** in multi-line strings,
  but the other escape sequences above are not interpreted.
  The sequences are represented as they are written.

```scala
// Conveyance.

// newline character is escaped in the following single line string
val textPart1 = "Car\nMotorbike"

// no need to escape the newline characters in this multi-lines string
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

### String interpolation

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

## Symbols

A symbols is a *kind-of string* that can be used by the application logic,
while actual `String` values are used as application data.
A symbol literal is represented by a word starting with a simple quote `'`:
`'connectionStarted`

```scala
// Display exit cause.

val statusCodes = Map('exitSuccess -> 0, 'exitFailure -> 1)
val code = statusCodes('exitSuccess)

println(s"the status code is $code")
// --> the status code is 0
```

## Bonus: XML literals

An XML literal represents an XML content that is written inline
in the source code:

```scala
// Find a link in HTML content.

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
