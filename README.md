# Scala explained

Scala is a programming language that let you write **cool stuff** like:

```scala
def sing(i: Int) = s"Happy Birthday ${ if (i == 3) "dear Tom" else "to You" }"
(1 to 4).map(sing).foreach(println)

/* -->
Happy Birthday to You
Happy Birthday to You
Happy Birthday dear Tom
Happy Birthday to You
*/
```

This website **explains Scala features** that will help you master
Scala programming.
Each explanation is illustrated by a **code snippet**
that can be **copy-pasted** into a Scala interpreter.

Talking about Scala interpreter, here's how you can set it up:

## Setup Scala environment

1. **Install Scala** from the official website:
  [https://www.scala-lang.org/download/](https://www.scala-lang.org/download/)

1. Create a **Scala source code file** named `MyScalaProgram.scala`
  with the following content:

  ```scala
  object MyScalaProgram {
    // The `main` method is the program entry point
    def main(args: Array[String]): Unit =
      println("I was here.")
  }
  ```

1. Compile and run your **Scala program** using your favorite Scala environment
  (IntelliJ, sbt) or through a terminal:

  ```bash
  scala MyScalaProgram.scala
  # --> I was here.
  ```

## Let's start now

First of all, we will talk about [Scala basic syntax](syntax.md).

If you are already familiar with Scala, you can learn more about
[Scala cool functional programming (FP) features here](functions.md).

If you want to build beautiful software architectures,
you can take a look at [Scala mind blowing object concepts](classes.md).

For the most curious among you, there is also an exhaustive list of
Scala [keywords](keywords.md) and [symbols](symbols.md)
with plenty of examples.

Enjoy.
