# What is ast-grep?

## Introduction

ast-grep is a new AST based tool for managing your code, at massive scale.

Using ast-grep can be as simple as running a single command in your terminal:

```bash
sg --pattern 'var code = $PATTERN' --rewrite 'let code = $PATTERN' -l js
```

The command above will replace `var` statement with `let` for all JavaScript files.

---

ast-grep is a versatile tool for searching, linting and rewriting code in various languages.

* **Search**: As a command line tool in your terminal, ast-grep, `sg`, can precisely search code based on AST, running through ten thousand files in sub seconds.
* **Lint**: You can also use ast-grep as a linter. Thanks to the flexible rule configuration, adding a new customized rule is more intuitive and straightforward. It also has a pretty error reporting out of box
* **Rewrite**: ast-grep provide jQuery like utility methods to traverse and manipulate syntax tree. Besides, you can also use operators to compose complex matching from simple patterns.

> Think ast-grep as an hybrid of [grep](https://www.gnu.org/software/grep/manual/grep.html), [eslint](https://eslint.org/) and [codemod](https://github.com/facebookincubator/fastmod).

## Supported Languages

ast-grep supports a wide range of programming languages. Here is a list of notable programming languages it supports.

|Language Domain|Supported Languages|
|:--------------|------------------:|
|System Programming| `C`, `Rust`|
|Server Side Programming| `Go`, `Java`, `Python`, `C-sharp`|
|Web Development| `JS(X)`, `TS(X)`, `HTML`, `CSS`|
|Mobile App Development| `Dart`, `Kotlin`, `Swift`|
|Scripting, Protocols, etc.| `Lua`, `Thrift`|

Thanks to [tree-sitter](https://tree-sitter.github.io/tree-sitter/), a popular parser generator library, ast-grep manages to support many community-maintained languages!

## Motivation

Using text-based tool for searching code is fast but imprecise. We usually prefer to parse the code into [abstract syntax tree](https://www.wikiwand.com/en/Abstract_syntax_tree) for precise matches.

However, developing with AST is tedious and frustrating. Consider this "hello-world" level task: matching `console.log` in JavaScript using Babel. We will need to write code like below.

```javascript
path.parentPath.isMemberExpression() &&
path.parentPath.get('object').isIdentifier({ name: 'console' }) &&
path.parentPath.get('property').isIdentifier({ name: 'log' })
```

This snippet deserves a detailed explanation for beginners. Even for experienced developers, authoring this snippet also requires a lot of looking up references.

ast-grep solves the problem by providing a simple core mechanism: using code to search code with the same pattern.
Consider it as same as `grep` but based on AST instead of text.

In comparison to Babel, we can complete this hello-world task in ast-grep trivially

```javascript
sg -p "console.log"
```

See [playground](/playground.html) in action!

Upon the simple pattern code, we can build a series of operators to compose complex matching rules for various scenarios.

Though we use JavaScript in our introduction, ast-grep is not language specific. It is a _polyglot_ tool backed by the renowned library [tree-sitter](https://tree-sitter.github.io/).
The idea of ast-grep can be applied to many other languages!

## Features

There are a lot of existing tools that looks like ast-grep, notable predecessor including [Semgrep](https://semgrep.dev/), comby, shisho, gogocode.

What makes astgrep stands out is:

### Performance

It is written in Rust, a native language and utilize multiple cores. (It can even beat ag when searching simple pattern). Astgrep can handle tens of thousands files in seconds.

### Progressiveness
You can start from writing a oneliner to rewrite code at command line with minimal investment. Later if you see some code smell recurrently appear in your projects, you can write a linter rule in YAML with a few patterns combined. Finally if you are a library author or framework designer, astgrep provide programmatic interface to rewrite or transpile code efficiently.

### Pragmatism
ast-grep comes with batteries included. Interactive code modification is available. Linter and language server work out of box when you install the command line tool. Astgrep is also shipped with test à for rule authors.

