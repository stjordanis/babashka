# Developing Babashka

To work on Babashka itself make sure Git submodules are checked out.

``` shellsession
$ git clone https://github.com/borkdude/babashka --recursive
```

To update later on:

``` shellsession
$ git submodule update --recursive
```

You need [Leiningen](https://leiningen.org/), and for building binaries you need GraalVM.

## REPL

`lein repl` will get you a standard REPL/nREPL connection. To work on tests use `lein with-profiles +test repl`.

### Adding classes

Add necessary classes to `babashka/impl/classes.clj`.  For every addition, write
a unit test, so it's clear why it is added and removing it will break the
tests. Try to reduce the size of the binary by only adding the necessary parts
of a class in `:instance-check`, `:constructors`, `:methods`, `:fields` or
`:custom`.

The `reflection.json` file that is needed for GraalVM compilation is generated
with:

    lein with-profiles +reflection run

## Test

Test on the JVM (for development):

    script/test

Test the native version:

    BABASHKA_TEST_ENV=native script/test

## Build

To build this project, set `$GRAALVM_HOME` to the GraalVM distribution directory.

Then run:

    script/compile

## Binary size

Keep notes here about how adding libraries and classes to Babashka affects the binary size.

<!-- On macOS: -->

<!-- 2020/01/08, ..., 38.7mb / 11.3mb zipped -->
<!-- Added: `clojure.data.xml`. Growth: 1.8mb / ... -->

2020/01/24, 43eef7075f9dac038d8d28a5ee4e49b6affd9864: 38.3mb, 11.1mb zipped
Added hierarchies (derive, isa?, etc).

2020/01/23, 485fef7df54d6701936704573468a1ec4c66d221: 37.4mb / 10.9mb zipped
Added: StringBuilder, java.io.{Reader,Writer,PrinterWriter,PushbackReader}

2020/01/08, 303ca9e825d76a4a45bc4240a59139d342c13964: 36.9mb / 10.8mb zipped

Removing cheshire from bb: 36.2mb / 10.5mb zipped.
