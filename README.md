# Sage

A general-purpose, **multi-paradigm** programming language with plain-English, Python-style
syntax. The interpreter is written in C (no dependencies beyond the C library).

> **Disclaimer.** Sage was designed and written with AI assistance (Claude, by Anthropic),
> including AI-assisted research into language design and into which Python features to
> adopt. It is a young, independent project that has been tested with its own test suite but
> not battle-tested in production. Review and test it before relying on it for anything
> important. The same notice is printed by `sage --about`.

Sage supports three styles in one language:

| Style | Features |
|---|---|
| Procedural | variables, `if` / `while` / `for` / `repeat`, functions, `try` / `catch` / `finally` |
| Object-oriented | classes, `init`, inheritance (`extends`), `super`, `to_text`, operator methods (`__add__` ...) |
| Functional | first-class functions, closures, `lambda`, `map` / `filter` / `reduce`, comprehensions, decorators, the `\|>` pipe |

**Version 1.0.0.0.** (The VS Code extension is numbered 1.0.0 because VS Code only accepts
three-part version numbers.)

## What Sage can do, cannot do, and is best for

**What it can do today**

- Run programs in three styles in one language: procedural, object-oriented (classes,
  inheritance, operator methods) and functional (closures, lambdas, `map`/`filter`/`reduce`,
  comprehensions, decorators, the `|>` pipe).
- Work with text (Unicode-aware length, indexing and slicing; case conversion for Latin, Greek
  and Cyrillic), numbers, lists, dictionaries, tuples and sets.
- Read and write files and folders; parse and produce JSON and CSV; match and replace text
  with regular expressions; read the environment and run other programs (`os`).
- Handle errors with `try` / `catch Kind as e` / `finally`, your own error classes, and `with`.
- Split programs into modules and packages, keep per-project packages in a `.sage_env`, and
  install packages from folders or git (`sage pkg`).
- Test code with `sage test`; log with the `log` module; use `Stack`, `Queue` and `Counter`.
- Manage memory automatically with a garbage collector (long-running programs do not grow).
- Run interactively in a REPL, with `sage -e`, in VS Code (extension), and in other editors.

**What it cannot do (yet)**

- No generators (`yield`), `async`/`await` or threads, so no concurrency.
- No database drivers, HTTP or networking, web servers, JWT/OAuth, or graphics/GUI.
- No machine-learning or data-science libraries, and no numerical arrays: numbers are
  64-bit floats, so integers above 2^53 lose precision.
- No central package registry: packages come from folders or git URLs.
- Regular expressions have no lookahead and no non-greedy matching.
- No debugger, no profiler, no language server (no go-to-definition or auto-complete).
- It is a tree-walking interpreter: it is much slower than Python, and far slower than
  compiled languages, on heavy computation.
- Young project: the VS Code extension has not been installed in a real VS Code, and the
  Emacs and Sublime files have not been tried in those editors.

**What it is best for at the moment**

- **Learning to program and teaching**: plain-English keywords, clear error messages with line
  numbers, and a verified tutorial.
- **Small scripts and automation**: file processing, text and log wrangling, JSON/CSV
  conversion, small command-line tools (including with the framework patterns in tutorial
  chapter 18).
- **Prototyping and experimenting with language ideas**: the interpreter is a small, readable
  C code base (about 5,000 lines) that is easy to extend with new built-ins and modules.
- **Not recommended yet** for performance-sensitive work, network services, data analysis, or
  anything that needs libraries beyond the standard library listed below.

## Build and run

```
make                 # builds ./sage
make test            # runs tests/*.sage against their .out files (11 programs)
sudo make install    # optional: sage -> /usr/local/bin, libraries -> /usr/local/share/sage/lib
sage                 # interactive prompt
sage file.sage       # run a program
sage -e 'say "hi"'   # run one line
sage --about         # version and the AI-assistance disclaimer
```

Needs a C compiler and `make`. Developed and tested on Linux (WSL on Windows works).
The VS Code extension (`vscode/sage-lang-1.0.0.vsix`) and the setup guide for VS Code and other editors
(Vim, Emacs, Sublime Text, JetBrains, nano, ...) are in `docs/EDITOR_SETUP.md`; ready-made files
for Vim, Emacs and Sublime are in `editors/`.

## Reading and writing files

Yes, Sage can read and write files. Quick forms, then the `open` form:

```
write_file("notes.txt", "first\n")          # create or replace
append_file("notes.txt", "second\n")        # add to the end
let text = read_file("notes.txt")            # whole file as text
say file_exists("notes.txt")

with open("notes.txt", "a") as f:           # modes: "r" (default), "w", "a"
    f.write("third\n")
with open("notes.txt") as f:
    for line in f.lines(): say line         # f.read() gives all the text
```

`os` lists, creates, renames and removes files and folders; `json` and `csv` turn text into
data and back. A missing file raises `IOError`. Sage can also write `.sage` files, which is
how programs generate modules (see chapter 18 of the tutorial).

## Learning Sage

`docs/TUTORIAL.md` teaches the language in 20 short chapters with real, verified output,
including how to build your own modules, packages and frameworks.

## The language

```
# comments start with #
let name = "Sage"            # 'let' makes a new variable in this scope
count = 3                    # '=' updates the nearest existing variable, or creates one
count += 1                   # also -=  *=  /=  %=

say "Hello", name, count     # 'say' prints its arguments separated by spaces
say f"{name} has {len(name)} letters"     # f-strings; {{ and }} are literal braces
```

**Values:** numbers (all 64-bit floats), text, `true` / `false`, `nothing`, lists `[1, 2]`,
dicts `{"a": 1}`, functions, classes, objects. Falsy: `false`, `nothing`, `0`, `""`, `[]`, `{}`.

**Operators:** `+ - * / // % **`, comparisons `== != < > <= >=`, and the English forms
`is`, `is not`, `is greater than`, `is less than`, `is at least`, `is at most`.
Also `and or not`, `in`, `not in`, `x if cond else y`, and the pipe `x |> f(a)` (means `f(x, a)`).
`+` joins text with anything; `*` repeats text and lists.

**Control flow** (blocks are indented, ended by a colon):

```
if x > 10:
    say "big"
else if x > 5:          # 'elif' also works
    say "medium"
else:
    say "small"

while n > 0:  n -= 1    # one-line bodies are fine
for item in [1, 2, 3]: say item
for key, value in items(some_dict): say key, value    # unpacking
for i in range(10): ...       # range(stop) / range(start, stop[, step]); lazy inside 'for'
repeat 3 times: say "hi"
break / continue / pass
```

**Functions, closures and scope**

```
define greet(name, punctuation = "!"):
    return "Hello, " + name + punctuation       # 'give back x' is the same as 'return x'

greet("Ada", punctuation = "?")                 # keyword arguments

define total(*numbers, scale = 1, **extras):    # *numbers -> list, **extras -> dict
    return sum(numbers) * scale
total(1, 2, *[3, 4], scale = 2)                 # *list and **dict spread out in a call

let double = lambda x: x * 2

define make_counter():
    let n = 0
    define next():
        nonlocal n                               # change the enclosing n
        n += 1
        return n
    return next
```

Assigning to a name inside a function makes a new local variable. To change an outer
variable, declare it first with `global name` (top level) or `nonlocal name` (enclosing
function). Forgetting is reported as an error rather than silently doing the wrong thing.

**Unpacking:** `let a, b = 1, 2`, `a, b = b, a`, `for key, value in d.items():`.

**Classes** — methods get an implicit `self`; `init` is the constructor.

```
class Animal:
    define init(name):
        self.name = name
    define speak():
        say self.name, "makes a sound"

class Dog extends Animal:                        # or: class Dog(Animal):
    define speak():
        super.speak()                            # call the parent's method
        say self.name, "barks"
    define to_text():                            # used by say / str()
        return "Dog " + self.name

Dog("Rex").speak()
```

**Errors**

```
try:
    risky()
catch problem:           # the name is optional
    say "failed:", problem
raise "something broke"  # any value can be raised, including an object
assert x > 0, "x must be positive"
```

Uncaught errors print `Error on line N: message` and exit with status 1.

**Modules:** `use "other.sage"` runs another file once in the global scope.

## Modules, packages and environments

```
import math                     # standard library or your own math.sage
import util.text as t           # util/text.sage (a folder with init.sage is a package)
from json import parse, stringify as dump
if __name__ == "__main__": ...  # only when run directly
```

`import` searches: the script's folder, the current folder, the nearest `.sage_env/packages`,
`SAGE_PATH` (colon separated), then Sage's own `lib/` (next to the executable, or
`PREFIX/share/sage/lib` after `make install`). Each module has its own scope and is run once.

```
sage env create                 make a .sage_env folder here (like python -m venv)
sage pkg install DIR|FILE|GIT   install into the nearest environment
sage pkg install -r requirements.txt
sage pkg list | sage pkg remove NAME
sage test [paths]               run test_* functions in test_*.sage / *_test.sage files
```

There is no central package registry: packages are folders, single files or git URLs.

Standard library: `math random os time json csv re` (written in C) and `log collections
testing` (written in Sage, in `lib/`).

## Types and errors

Tuples `(1, 2)` and sets `{1, 2}` (`set()` is the empty set; `| & -` for union, intersection,
difference), plus set and dict comprehensions. `enumerate`, `zip` and `.items()` yield tuples.

```
try: ... catch ValueError, KeyError as e: ... catch e: ... finally: ...
class MyError(Error): pass          # Error, ValueError, TypeError, KeyError, IndexError,
                                    # IOError, ImportError, ZeroDivisionError, RuntimeError, ...
with open("f.txt") as f: ...        # any object with __enter__ / __exit__
@decorator                          # above define, methods and classes
```

Operator methods: `__add__ __sub__ __mul__ __truediv__ __floordiv__ __mod__ __pow__` (and the
`__r...__` forms), `__eq__ __lt__ __gt__ __le__ __ge__ __neg__ __len__ __getitem__ __setitem__
__contains__ __iter__ __call__ __str__`. Instances can be dictionary keys and set members.

## Built-ins

Global: `len str num int bool list tuple set type isinstance hasattr getattr dir range print ask
abs min max sum round floor ceil sqrt sin cos tan log exp pow random randint seed time clock exit
sorted reversed map filter reduce each any all zip enumerate keys values items fixed chr ord
read_file write_file append_file file_exists open import_module`. `args` holds command-line
arguments.

Text methods: `upper lower strip split replace starts_with ends_with find contains count join title`
(`upper`/`lower`/`title` handle Latin, Greek and Cyrillic).
List methods: `add/push pop insert remove index contains count sort reverse map filter reduce each
join copy clear first last extend`.
Dict methods: `get has remove keys values items update copy clear`.
Set methods: `add remove discard has copy clear union intersection difference is_subset
is_superset to_list`. Tuple methods: `count index`.

`xs.sort(key_function)` and `sorted(xs, key_function)` are stable and take an optional key.
`fixed(3.14159, 2)` gives `"3.14"`.

## Memory

Sage has a mark-and-sweep garbage collector, so programs can run for a long time without
growing. `gc()` forces a collection and returns the number of live objects. The collector scans
the C stack conservatively, so a rare unreachable object may survive a cycle; that costs memory,
never correctness. Set `SAGE_GC_STRESS=1` to collect at every statement (used to test the
collector) or `SAGE_GC_LOG=1` to print collection statistics.

## Known limits (v1.0.0.0)

- Numbers are floats, so whole numbers above 2^53 lose precision.
- Case conversion covers Latin (with accents), Greek and Cyrillic only; other scripts pass through.
- No generators, `async`, threads, or `first, *rest = xs` unpacking. No database, HTTP or
  third-party packages (such as ML libraries) are included. See *Next update*.
- Built-in functions take no keyword arguments (except `key =` for `sorted` and `.sort`).
- Regular expressions use POSIX extended syntax (with `\d \w \s` added): no lookahead, no
  non-greedy matching.
- Runtime errors are matched to error classes by their message, so a custom message from a
  library may be seen as `RuntimeError`. Errors you `raise` as `Error` objects match exactly.
- Recursion depth is capped at 2,500 calls and reports an error instead of crashing.
- Collection runs at statement and call boundaries, so one huge expression (for example a
  comprehension over millions of items that never calls a function) is not collected midway.

## Next update (planned)

These are known gaps, listed here so they are not mistaken for finished features:

- **Generators, `async` and threads.** These need a different evaluator design. Sage is a
  tree-walking interpreter that uses the C stack, so pausing a function midway (for `yield`
  or `await`) means redesigning how calls are run.
- **Databases, HTTP, JWT and ML libraries.** These would need large external dependencies
  (for example SQLite, a TLS library, a crypto library); Sage deliberately has none today.
- **Richer regular expressions.** The `re` module is built on POSIX extended regex, so it has
  no lookahead and no non-greedy matching. A purpose-built regex engine would fix this.
- **Precise error classes.** Runtime errors are matched to error classes (`KeyError`,
  `ZeroDivisionError`, ...) by reading their message text, so a library's custom message may
  be seen as `RuntimeError`. Raising real `Error` objects inside the interpreter would make
  the matching exact.

## Layout

```
src/sage.h      shared types
src/value.c     strings, lists, dicts, printing, scopes
src/gc.c        garbage collector
src/lexer.c     tokens with INDENT / DEDENT
src/parser.c    recursive-descent parser -> AST
src/interp.c    evaluator, calls, classes, try/finally, with, modules
src/builtins.c  built-in functions and methods
src/prelude.c   Error classes and open()/File, written in Sage
src/stdlib.c    math, random, os, time modules
src/lib_json.c  json and csv modules
src/lib_re.c    re module
src/unicode.c   upper/lower/title for Latin, Greek, Cyrillic
src/main.c      REPL, command line, env / pkg / test commands
lib/            log, collections, testing (written in Sage)
docs/           TUTORIAL.md (learn the language), EDITOR_SETUP.md (VS Code and other editors)
vscode/         VS Code extension source and the installable .vsix
editors/        Vim, Emacs and Sublime Text support files
examples/       sample programs
tests/          *.sage programs with expected *.out files
```
