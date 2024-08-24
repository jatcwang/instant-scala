# instant-scala
This is a wrapper script around `scala-cli` which aims to let you write scripts with instant start-up time.

How it works:

- Compiles your script to a native binary using GraalVM
- Calculate content hash of your script
- When executed it will check if the content hash. If it hasn't changed, the cached binary will be executed - instant start-up!
  If script content has changed, the binary will be regenerated.

## Installation

Put the script `instant-scala` somewhere in your `$PATH` and make it executable.

## Usage

With an example script like this:
(Note the use of `instant-scala` as the shebang on the first line.)
```
#! /usr/bin/env -S instant-scala
//> using scala "3.4.2"

@main def main = println("Hello, world!")
```

Make it executable and run it

```
$ chmod +x my_script.scala
$ time ./my_script.scala
Detected Scala script not yet built (no hash file). Building...
Compiling project (Scala 3.5.0, JVM (21))
Compiled project (Scala 3.5.0, JVM (21))
... Many lines of GraalVM compilation outputs ...

Hello, world!

./my_script.scala  30.05s user 3.49s system 400% cpu 34.848 total

$ time ./my_script.scala
Hello, world!

./my_script.scala  0.01s user 0.00s system 53% cpu 0.018 total
```

As you can see, the first run takes much longer because it needs to generate the native binary.
However, subsequent runs starts instantly.