# Changelog for Elixir v1.16

## Code snippets in diagnostics

Elixir v1.15 introduced a new compiler diagnostic format and the ability to print multiple error diagnostics per compilation (in addition to multiple warnings).

With Elixir v1.16, we also include code snippets in exceptions and diagnostics raised by the compiler. For example, a syntax error now includes a pointer to where the error happened:

```
** (SyntaxError) invalid syntax found on lib/my_app.ex:1:17:
    error: syntax error before: '*'
    │
  1 │ [1, 2, 3, 4, 5, *]
    │                 ^
    │
    └─ lib/my_app.ex:1:17
```

For mismatched delimiters, it now shows both delimiters:

```
** (MismatchedDelimiterError) mismatched delimiter found on lib/my_app.ex:1:18:
    error: unexpected token: )
    │
  1 │ [1, 2, 3, 4, 5, 6)
    │ │                └ mismatched closing delimiter (expected "]")
    │ └ unclosed delimiter
    │
    └─ lib/my_app.ex:1:18
```

Errors and warnings diagnostics also include code snippets. When possible, we will show precise spans, such as on undefined variables:

```
  error: undefined variable "unknown_var"
  │
5 │     a - unknown_var
  │         ^^^^^^^^^^^
  │
  └─ lib/sample.ex:5:9: Sample.foo/1
```

Otherwise the whole line is underlined:

```
error: function names should start with lowercase characters or underscore, invalid name CamelCase
  │
3 │   def CamelCase do
  │   ^^^^^^^^^^^^^^^^
  │
  └─ lib/sample.ex:3
```

A huge thank you to Vinícius Muller for working on the new diagnostics.

## Revamped documentation

Elixir's Getting Started guided has been made part of the Elixir repository and incorporated into ExDoc. This was an opportunity to revisit and unify all official guides and references.

We have also incorporated and extended the work on [Understanding Code Smells in Elixir Functional Language](https://github.com/lucasvegi/Elixir-Code-Smells/blob/main/etc/2023-emse-code-smells-elixir.pdf), by Lucas Vegi and Marco Tulio Valente, from [ASERG/DCC/UFMG](http://aserg.labsoft.dcc.ufmg.br/), into the official document in the form of anti-patterns. The anti-patterns are divided into four categories: code-related, design-related, process-related, and meta-programming. Our goal is to give all developers with both positive and negative examples of Elixir code, with context and examples on how to improve their codebases.

Another [ExDoc](https://github.com/elixir-lang/ex_doc) feature we have incorporated in this release is the addition of cheatsheets, starting with [a cheatsheet for the Enum module](https://hexdocs.pm/elixir/main/enum-cheat.html). If you would like to contribute future cheatsheets to Elixir itself, feel free to start a discussion with an issue.

Finally, we have started enriching our documentation with [Mermaid.js](https://mermaid.js.org/) diagrams. You can find examples in the [GenServer](https://hexdocs.pm/elixir/main/GenServer.html) and [Supervisor](https://hexdocs.pm/elixir/main/Supervisor.html) docs.

## v1.16.0-dev

### 1. Enhancements

#### EEx

  * [EEx] Include relative file information in diagnostics

#### Elixir

  * [Code] Automatically include columns in parsing options
  * [Code] Introduce `MismatchedDelimiterError` for handling mismatched delimiter exceptions
  * [Code.Fragment] Handle anonymous calls in fragments
  * [Kernel] Suggest module names based on suffix and casing errors when the module does not exist in `UndefinedFunctionError`
  * [Kernel.ParallelCompiler] Introduce `Kernel.ParallelCompiler.pmap/2` to compile multiple additional entries in parallel
  * [Macro] Add `Macro.compile_apply/4`
  * [String] Update to Unicode 15.1.0

#### Mix

  * [mix test] Allow testing multiple file:line at once, such as `mix test test/foo_test.exs:13 test/bar_test.exs:27`

### 2. Bug fixes

#### Elixir

  * [IO] Raise when using `IO.binwrite/2` on terminated device (mirroring `IO.write/2`)
  * [Kernel] Do not expand aliases recursively (the alias stored in Macro.Env is already expanded)
  * [Kernel] Ensure `dbg` module is a compile-time dependency
  * [Kernel] Warn when a private function or macro uses `unquote/1` and the function/macro itself is unused
  * [Kernel.ParallelCompiler] Consider a module has been defined in `@after_compile` callbacks to avoid deadlocks
  * [Path] Ensure `Path.relative_to/2` returns a relative path when the given argument does not share a common prefix with `cwd`

#### ExUnit

  * [ExUnit] Raise on incorrectly dedented doctests

### 3. Soft deprecations (no warnings emitted)

#### Elixir

  * [File] Deprecate `File.stream!(file, options, line_or_bytes)` in favor of keeping the options as last argument, as in `File.stream!(file, line_or_bytes, options)`
  * [Kernel.ParallelCompiler] Deprecate `Kernel.ParallelCompiler.async/1` in favor of `Kernel.ParallelCompiler.pmap/2`
  * [Path] Deprecate `Path.safe_relative_to/2` in favor of `Path.safe_relative/2`

### 4. Hard deprecations

#### Elixir

  * [Date] Deprecate inferring a range with negative step, call `Date.range/3` with a negative step instead
  * [Enum] Deprecate passing a range with negative step on `Enum.slice/2`, give `first..last//1` instead
  * [Kernel] `~R/.../` is deprecated in favor of `~r/.../`. This is because `~R/.../` still allowed escape codes, which did not fit the definition of uppercase sigils
  * [String] Deprecate passing a range with negative step on `String.slice/2`, give `first..last//1` instead

#### ExUnit

  * [ExUnit.Formatter] Deprecate `format_time/2`, use `format_times/1` instead

#### Mix

  * [mix compile.leex] Require `:leex` to be added as a compiler to run the `leex` compiler
  * [mix compile.yecc] Require `:yecc` to be added as a compiler to run the `yecc` compiler

## v1.15

The CHANGELOG for v1.15 releases can be found [in the v1.15 branch](https://github.com/elixir-lang/elixir/blob/v1.15/CHANGELOG.md).
