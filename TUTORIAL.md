# Learn Sage

A hands-on tutorial. By the end you will have written programs with loops, functions,
classes, inheritance, closures, and error handling, and you will know which of Sage's three
styles (procedural, object-oriented, functional) to reach for.

Every example below was run, and the output shown is the real output. Type them in yourself:
that is the fastest way to learn. To run a file, use `sage myfile.sage`, or the play button
in VS Code (see `EDITOR_SETUP.md`). To experiment line by line, run `sage` with no arguments to get
the interactive prompt.

**Contents**

1. [Hello, Sage](#1-hello-sage)
2. [Values and variables](#2-values-and-variables)
3. [Making decisions](#3-making-decisions)
4. [Loops](#4-loops)
5. [Lists](#5-lists)
6. [Dictionaries](#6-dictionaries)
7. [Working with text](#7-working-with-text)
8. [Functions](#8-functions)
9. [Functional programming](#9-functional-programming)
10. [Classes and objects](#10-classes-and-objects)
11. [Inheritance](#11-inheritance)
12. [When things go wrong](#12-when-things-go-wrong)
13. [Files and modules](#13-files-and-modules)
14. [Modules, packages and the standard library](#14-modules-packages-and-the-standard-library)
15. [Tuples and sets](#15-tuples-and-sets)
16. [Operators, decorators and context managers](#16-operators-decorators-and-context-managers)
17. [Testing your code](#17-testing-your-code)
18. [Building your own modules, packages and frameworks](#18-building-your-own-modules-packages-and-frameworks)
19. [A small project](#19-a-small-project)
20. [Cheat sheet](#20-cheat-sheet)

---

## 1. Hello, Sage

```sage
say "Hello, Sage!"
```

<sub>Output</sub>
```text
Hello, Sage!
```

`say` prints. Give it several things separated by commas and it puts a space between them:

```sage
say "The answer is", 6 * 7
say
say "(that was an empty line)"
```

<sub>Output</sub>
```text
The answer is 42

(that was an empty line)
```

A `#` starts a comment that runs to the end of the line:

```sage
# this line is ignored
say "visible"   # and so is this part
```

<sub>Output</sub>
```text
visible
```

**Try it:** print your name and your favourite number on one line.

---

## 2. Values and variables

Sage has a handful of kinds of values:

| Kind | Examples |
|---|---|
| number | `42`, `3.14`, `-7`, `1e3` |
| text | `"hello"`, `'hello'` |
| boolean | `true`, `false` |
| nothing | `nothing` (the "no value" value) |
| list | `[1, 2, 3]` |
| dict | `{"name": "Ada"}` |

Use `let` to create a variable, and `=` to change it later:

```sage
let age = 30
say age
age = 31
say age
age += 1        # same as: age = age + 1
say age
```

<sub>Output</sub>
```text
30
31
32
```

### Several at once (unpacking)

Give several names on the left and several values on the right. The right side is worked out
first, so swapping needs no temporary variable:

```sage
let x, y = 1, 2
x, y = y, x
say x, y

let first, second, third = "abc"     # any list or text with the right length
say first, third

let name, age = ["Ada", 36]
say f"{name} is {age}"
```

<sub>Output</sub>
```text
2 1
a c
Ada is 36
```

The counts must match, or you get an error such as `cannot unpack 3 values into 2 names`.

`type(x)` tells you what kind of value something is:

```sage
say type(42), type("hi"), type(true), type(nothing), type([1]), type({"a": 1})
```

<sub>Output</sub>
```text
number text boolean nothing list dict
```

### Numbers

```sage
say 7 + 2, 7 - 2, 7 * 2
say 7 / 2        # normal division
say 7 // 2       # whole-number division
say 7 % 2        # remainder
say 2 ** 10      # power
say -7 // 2, -7 % 2
say 0.1 + 0.2
```

<sub>Output</sub>
```text
9 5 14
3.5
3
1
1024
-4 1
0.3
```

### Text and f-strings

Put `f` in front of a string and anything inside `{ }` is calculated and inserted:

```sage
let name = "Ada"
let year = 1815
say f"{name} was born in {year}"
say f"In ten years: {year + 10}"
say f"Shout: {name.upper()}!"
say f"Double the braces to show them: {{ and }}"
```

<sub>Output</sub>
```text
Ada was born in 1815
In ten years: 1825
Shout: ADA!
Double the braces to show them: { and }
```

Joining text with `+` converts the other side to text automatically, and `*` repeats:

```sage
say "Score: " + 10
say "ha" * 3
say "-" * 20
```

<sub>Output</sub>
```text
Score: 10
hahaha
--------------------
```

Triple quotes allow several lines:

```sage
say """Roses are red,
Violets are blue."""
```

<sub>Output</sub>
```text
Roses are red,
Violets are blue.
```

### Truthiness

In an `if` or `while`, these count as false: `false`, `nothing`, `0`, `""` (empty text), `[]`
and `{}`. Everything else is true. `bool(x)` shows you which:

```sage
say bool(0), bool(1), bool(""), bool("a"), bool([]), bool([0]), bool(nothing)
```

<sub>Output</sub>
```text
false true false true false true false
```

**Try it:** make variables for a price and a quantity, then print the total using an f-string.

---

## 3. Making decisions

```sage
let temperature = 22

if temperature > 30:
    say "hot"
else if temperature > 15:
    say "pleasant"
else:
    say "cold"
```

<sub>Output</sub>
```text
pleasant
```

The body of an `if` is **indented** (4 spaces is the habit) and the line before it ends with a
colon. `elif` also works as a shorter `else if`.

### Comparing

```sage
say 3 == 3, 3 != 4, 3 < 4, 3 >= 4
say "apple" < "banana"
say [1, 2] == [1, 2]
```

<sub>Output</sub>
```text
true true true false
true
true
```

Sage also lets you write comparisons in English:

```sage
let n = 7
say n is 7
say n is not 8
say n is greater than 5
say n is less than 5
say n is at least 7
say n is at most 6
```

<sub>Output</sub>
```text
true
true
true
false
true
false
```

### Combining conditions

```sage
let age = 20
let has_ticket = true

if age >= 18 and has_ticket:
    say "come in"

if not has_ticket or age < 18:
    say "stay out"
else:
    say "welcome"
```

<sub>Output</sub>
```text
come in
welcome
```

`in` checks membership:

```sage
say 3 in [1, 2, 3], "z" in "abc", "name" in {"name": "Ada"}
say 5 not in [1, 2, 3]
```

<sub>Output</sub>
```text
true false true
true
```

### A decision inside an expression

```sage
let hours = 9
let label = "morning" if hours < 12 else "afternoon"
say label
```

<sub>Output</sub>
```text
morning
```

A one-line body is allowed when it fits:

```sage
if 1 < 2: say "short form"
```

<sub>Output</sub>
```text
short form
```

**Try it:** write a grade calculator: given a score from 0 to 100, print A (90+), B (80+),
C (70+) or "needs work".

---

## 4. Loops

### `while`

```sage
let n = 3
while n > 0:
    say n
    n -= 1
say "liftoff"
```

<sub>Output</sub>
```text
3
2
1
liftoff
```

### `for` and `range`

`range(5)` is 0, 1, 2, 3, 4. Give it two numbers for a start and stop, or three for a step:

```sage
for i in range(3):
    say i
for i in range(1, 4):
    say i
for i in range(10, 0, -5):
    say i
```

<sub>Output</sub>
```text
0
1
2
1
2
3
10
5
```

You can loop over a list, over text, or over the keys of a dict:

```sage
for fruit in ["apple", "pear"]:
    say fruit
for letter in "abc":
    say letter
```

<sub>Output</sub>
```text
apple
pear
a
b
c
```

### `repeat`

```sage
repeat 3 times:
    say "hip"
say "hooray"
```

<sub>Output</sub>
```text
hip
hip
hip
hooray
```

### `break` and `continue`

```sage
for i in range(10):
    if i % 2 == 0:
        continue          # skip even numbers
    if i > 7:
        break             # stop completely
    say i
```

<sub>Output</sub>
```text
1
3
5
7
```

### Looping with two names

When each item is a pair, name both parts. `enumerate` pairs each item with its position:

```sage
for i, color in enumerate(["red", "green", "blue"]):
    say i, color
```

<sub>Output</sub>
```text
0 red
1 green
2 blue
```

**Try it:** print the multiplication table for 7, from `7 x 1 = 7` to `7 x 10 = 70`.

---

## 5. Lists

A list holds values in order. Positions start at 0, and negative positions count from the end:

```sage
let colors = ["red", "green", "blue"]
say colors[0], colors[-1], len(colors)
colors[1] = "yellow"
say colors
```

<sub>Output</sub>
```text
red blue 3
["red", "yellow", "blue"]
```

### Adding and removing

```sage
let xs = [10, 20]
xs.add(30)
xs.insert(0, 5)
say xs
say xs.pop()          # removes and returns the last item
say xs.pop(0)         # removes and returns item 0
xs.remove(20)         # removes the first 20
say xs
```

<sub>Output</sub>
```text
[5, 10, 20, 30]
30
5
[10]
```

### Slicing and combining

```sage
let nums = [1, 2, 3, 4, 5]
say nums[1:3], nums[:2], nums[3:], nums[:-1]
say nums + [6, 7]
say [0] * 4
say [1, 2] == [1, 2]
```

<sub>Output</sub>
```text
[2, 3] [1, 2] [4, 5] [1, 2, 3, 4]
[1, 2, 3, 4, 5, 6, 7]
[0, 0, 0, 0]
true
```

### Searching, sorting

```sage
let xs = [5, 3, 9, 1]
xs.sort()
say xs
xs.reverse()
say xs
say xs.contains(9), xs.index(9), xs.index(100), xs.count(3)
say sorted(["pear", "fig", "banana"])
say sum(xs), min(xs), max(xs)
```

<sub>Output</sub>
```text
[1, 3, 5, 9]
[9, 5, 3, 1]
true 0 -1 1
["banana", "fig", "pear"]
18 1 9
```

`sorted` and `.sort()` accept a function that says what to sort *by*:

```sage
let words = ["banana", "fig", "cherry"]
say sorted(words, lambda w: len(w))
```

<sub>Output</sub>
```text
["fig", "banana", "cherry"]
```

**Try it:** make a list of five numbers and print the largest and the average.

---

## 6. Dictionaries

A dict maps keys to values:

```sage
let person = {"name": "Ada", "age": 36}
say person["name"]
person["email"] = "ada@example.com"
say person
say len(person)
```

<sub>Output</sub>
```text
Ada
{"name": "Ada", "age": 36, "email": "ada@example.com"}
3
```

### Looking things up safely

`d["x"]` is an error if the key is missing. `d.get` is not:

```sage
let stock = {"apples": 3}
say stock.get("apples"), stock.get("pears"), stock.get("pears", 0)
say stock.has("apples"), "pears" in stock
```

<sub>Output</sub>
```text
3 nothing 0
true false
```

### Looping over a dict

```sage
let prices = {"tea": 2, "coffee": 3}
for name in prices:
    say name, prices[name]
for name, price in prices.items():
    say f"{name} costs {price}"
say keys(prices), values(prices)
prices.remove("tea")
say prices
```

<sub>Output</sub>
```text
tea 2
coffee 3
tea costs 2
coffee costs 3
["tea", "coffee"] [2, 3]
{"coffee": 3}
```

Keys can be text, numbers, or booleans. Dicts remember the order you added things.

**Try it:** count how many times each letter appears in `"mississippi"` using a dict.

---

## 7. Working with text

Text works like a list of characters for indexing and slicing:

```sage
let s = "Hello, World"
say s[0], s[-1], s[0:5], len(s)
```

<sub>Output</sub>
```text
H d Hello 12
```

Text is Unicode. `len`, positions and slices count characters, not bytes, so accents,
other alphabets and symbols work the way you expect:

```sage
let t = "héllo wörld ✓"
say len(t), t[1], t[-1], t[6:]
say "日本語"[1], len("日本語")
for ch in "añb":
    say ch
```

<sub>Output</sub>
```text
13 é ✓ wörld ✓
本 3
a
ñ
b
```

(`upper` and `lower` only change the plain English letters A to Z.)

Methods return new text; they never change the original:

```sage
let s = "  Sage is fun  "
say s.strip()
say s.upper(), s.lower()
say "hello world".title()
say "banana".replace("an", "AN")
say "banana".count("an")
```

<sub>Output</sub>
```text
Sage is fun
  SAGE IS FUN     sage is fun  
Hello World
bANANa
2
```

### Finding and testing

```sage
say "hello".starts_with("he"), "hello".ends_with("lo")
say "hello".contains("ell"), "hello".find("l"), "hello".find("z")
```

<sub>Output</sub>
```text
true true
true 2 -1
```

### Splitting and joining

```sage
let words = "one two  three".split()
say words
say "a,b,c".split(",")
say "-".join(words)
say ", ".join(["x", 1, 2.5])
```

<sub>Output</sub>
```text
["one", "two", "three"]
["a", "b", "c"]
one-two-three
x, 1, 2.5
```

### Converting

```sage
say num("42") + 1
say num("3.5") * 2
say int(7.9)
say str(99) + " bottles"
say fixed(3.14159, 2)
say chr(65), ord("A")
```

<sub>Output</sub>
```text
43
7
7
99 bottles
3.14
A 65
```

**Try it:** write code that checks whether a word reads the same backwards.
(Hint: `reversed(word)` gives a list; `"".join(...)` turns it back into text.)

---

## 8. Functions

A function packages up steps you can reuse. `define` makes one, `return` (or `give back`)
hands a result back:

```sage
define greet(name):
    say "Hello,", name

greet("Ada")
greet("Grace")

define square(x):
    return x * x

say square(9)
```

<sub>Output</sub>
```text
Hello, Ada
Hello, Grace
81
```

### Default values

```sage
define power(base, exponent = 2):
    return base ** exponent

say power(5), power(2, 8)
```

<sub>Output</sub>
```text
25 256
```

Parameters with defaults must come after the ones without.

### Keyword arguments

Pass arguments by name with `name = value`. Names can come in any order, and they make calls
easier to read:

```sage
define connect(host, port = 80, secure = false):
    let scheme = "https" if secure else "http"
    return f"{scheme}://{host}:{port}"

say connect("example.com")
say connect("example.com", secure = true)
say connect(port = 8080, host = "localhost")
```

<sub>Output</sub>
```text
http://example.com:80
https://example.com:80
http://localhost:8080
```

Positional arguments come first, then named ones. Using a wrong name, or giving the same
parameter twice, is an error:

```sage
define area(width, height):
    return width * height

try:
    area(2, width = 5)
catch e:
    say e
try:
    area(2, depth = 5)
catch e:
    say e
```

<sub>Output</sub>
```text
area() got more than one value for 'width'
area() has no parameter called 'depth'
```

### Any number of arguments

A parameter written `*name` collects the extra positional arguments into a list.
`**name` collects extra named arguments into a dict. Going the other way, `*list` and
`**dict` in a call spread their contents out as arguments:

```sage
define total(*numbers):
    return sum(numbers)

say total(), total(5), total(1, 2, 3, 4)

let scores = [90, 85, 70]
say total(*scores)

define show(title, *items, sep = " / ", **details):
    say title + ": " + sep.join(items)
    say details

show("Menu", "soup", "salad", "pie", sep = " | ", vegan = true, price = 12)

let settings = {"sep": "-", "vegan": false}
show("Sides", "rice", "beans", **settings)
```

<sub>Output</sub>
```text
0 5 10
245
Menu: soup | salad | pie
{"vegan": true, "price": 12}
Sides: rice-beans
{"vegan": false}
```

Parameters written after `*items` can only be given by name.

### Returning early, returning nothing

A function with no `return` gives back `nothing`:

```sage
define sign(n):
    if n < 0:
        return "negative"
    if n == 0:
        return "zero"
    give back "positive"

say sign(-4), sign(0), sign(12)

define shout(text):
    say text.upper()
say shout("hi")
```

<sub>Output</sub>
```text
negative zero positive
HI
nothing
```

### Recursion

A function can call itself:

```sage
define factorial(n):
    if n <= 1:
        return 1
    return n * factorial(n - 1)

say factorial(10)

define fib(n):
    if n < 2:
        return n
    return fib(n - 1) + fib(n - 2)

say [fib(i) for i in range(12)]
```

<sub>Output</sub>
```text
3628800
[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89]
```

Recursion stops with an error if it goes about 2,500 calls deep.

### Functions are values

You can store a function in a variable, put it in a list, or pass it to another function:

```sage
define twice(f, x):
    return f(f(x))

define add_three(n):
    return n + 3

say twice(add_three, 10)

let operations = [add_three, lambda n: n * 100]
say operations[0](1), operations[1](1)
```

<sub>Output</sub>
```text
16
4 100
```

### Where variables live (scope)

A variable you create inside a function belongs to that function. Assigning to a name there
makes a new local variable, even if an outer variable has the same name:

```sage
let score = 10

define try_to_change():
    score = 99            # a new local 'score'
    return score

say try_to_change(), score
```

<sub>Output</sub>
```text
99 10
```

Functions can *read* outer variables freely. To *change* one, say so first with `global`
(a top-level variable) or `nonlocal` (a variable in an enclosing function):

```sage
let visits = 0

define record_visit():
    global visits
    visits += 1

record_visit()
record_visit()
say visits

define make_counter():
    let count = 0
    define tick():
        nonlocal count
        count += 1
        return count
    return tick

let t = make_counter()
say t(), t(), t()
```

<sub>Output</sub>
```text
2
1 2 3
```

If you forget, Sage tells you instead of silently doing the wrong thing:

```sage
# expect-error
let total = 0

define add(n):
    total += n

add(5)
```

<sub>Output</sub>
```text
Error on line 5: 'total' belongs to an outer scope; write 'global total' or 'nonlocal total' inside this function before changing it
```

`let` always creates a new variable in the current scope, which is handy when you want to
be explicit that a name is local.

**Try it:** write `is_prime(n)` and use it to print all primes below 50.

---

## 9. Functional programming

### Lambdas

A `lambda` is a small unnamed function written in one expression:

```sage
let double = lambda x: x * 2
let add = lambda a, b: a + b
say double(21), add(3, 4)
```

<sub>Output</sub>
```text
42 7
```

### map, filter, reduce

```sage
let nums = [1, 2, 3, 4, 5, 6]

say map(nums, lambda n: n * n)
say filter(nums, lambda n: n % 2 == 0)
say reduce(nums, lambda total, n: total + n)
say reduce(nums, lambda total, n: total + n, 100)   # with a starting value
```

<sub>Output</sub>
```text
[1, 4, 9, 16, 25, 36]
[2, 4, 6]
21
121
```

Lists also have these as methods, which chain nicely:

```sage
say [1, 2, 3, 4].map(lambda n: n * 10).filter(lambda n: n > 15)
```

<sub>Output</sub>
```text
[20, 30, 40]
```

### List comprehensions

`[expression for name in items if condition]` builds a list in one line:

```sage
say [n * n for n in range(1, 6)]
say [n for n in range(20) if n % 7 == 0]
say [c.upper() for c in "sage"]
say [a + b for a, b in [[1, 2], [3, 4]]]
```

<sub>Output</sub>
```text
[1, 4, 9, 16, 25]
[0, 7, 14]
["S", "A", "G", "E"]
[3, 7]
```

### The pipe operator

`x |> f` means `f(x)`, and `x |> f(a)` means `f(x, a)`. Read it left to right like a recipe:

```sage
let result = range(1, 11)
    |> filter(lambda n: n % 2 == 1)
    |> map(lambda n: n * n)
    |> sum()
say result
```

<sub>Output</sub>
```text
165
```

Without a pipe, the same thing reads inside-out: `sum(map(filter(range(1, 11), ...), ...))`.

### Closures

A function remembers the variables around where it was created:

```sage
define make_multiplier(factor):
    return lambda x: x * factor

let triple = make_multiplier(3)
let tenfold = make_multiplier(10)
say triple(5), tenfold(5)

define make_counter():
    let count = 0
    define next():
        nonlocal count
        count += 1
        return count
    return next

let a = make_counter()
let b = make_counter()
say a(), a(), a(), b()
```

<sub>Output</sub>
```text
15 50
1 2 3 1
```

Each counter has its own private `count`. `nonlocal count` lets the inner function change
the variable it closes over (see *Where variables live* in chapter 8).

### More helpers

```sage
say any([0, 0, 1]), all([1, 1, 0])
say zip([1, 2, 3], ["a", "b", "c"])
say reversed([1, 2, 3])
say sorted([3, 1, 2], lambda n: -n)
each([1, 2], print)
```

<sub>Output</sub>
```text
true false
[(1, "a"), (2, "b"), (3, "c")]
[3, 2, 1]
[3, 2, 1]
1
2
```

**Try it:** with one pipeline, take the words of a sentence, keep those longer than 3
letters, and capitalise them.

---

## 10. Classes and objects

A class is a blueprint. Each object made from it carries its own data and its own methods.

```sage
class Dog:
    define init(name, age):
        self.name = name
        self.age = age

    define bark():
        say self.name, "says woof"

    define birthday():
        self.age += 1

let rex = Dog("Rex", 3)
let fido = Dog("Fido", 5)
rex.bark()
rex.birthday()
say rex.age, fido.age
```

<sub>Output</sub>
```text
Rex says woof
4 5
```

- `init` is the constructor. It runs when you write `Dog(...)`.
- Inside a method, `self` is the object. You do not list it as a parameter.
- Data lives in attributes such as `self.name`; you can add them any time.

### Methods that return `self` chain

```sage
class Counter:
    define init():
        self.n = 0
    define bump():
        self.n += 1
        return self

let c = Counter()
c.bump().bump().bump()
say c.n
```

<sub>Output</sub>
```text
3
```

### Controlling how an object prints

Define `to_text` and `say` will use it:

```sage
class Point:
    define init(x, y):
        self.x = x
        self.y = y
    define to_text():
        return f"({self.x}, {self.y})"
    define distance_to(other):
        return sqrt((self.x - other.x) ** 2 + (self.y - other.y) ** 2)

let p = Point(0, 0)
let q = Point(3, 4)
say p, q
say p.distance_to(q)
say [p, q]
```

<sub>Output</sub>
```text
(0, 0) (3, 4)
5
[(0, 0), (3, 4)]
```

### Objects that hold other things

```sage
class Library:
    define init():
        self.books = []
    define add(title):
        self.books.add(title)
        return self
    define count():
        return len(self.books)

let lib = Library()
lib.add("Dune").add("Emma")
say lib.count(), lib.books
```

<sub>Output</sub>
```text
2 ["Dune", "Emma"]
```

**Try it:** write a `Rectangle` class with `area()` and `perimeter()` methods.

---

## 11. Inheritance

A class can extend another to reuse and specialise it. `super.method()` calls the parent's version.

```sage
class Animal:
    define init(name):
        self.name = name
    define speak():
        say self.name, "makes a sound"
    define describe():
        return f"{self.name} the {type(self)}"

class Dog extends Animal:
    define speak():
        say self.name, "barks"

class Cat extends Animal:
    define init(name, indoor):
        super.init(name)
        self.indoor = indoor
    define speak():
        super.speak()
        say self.name, "also purrs"

let pets = [Dog("Rex"), Cat("Tom", true)]
for pet in pets:
    pet.speak()
    say pet.describe()
```

<sub>Output</sub>
```text
Rex barks
Rex the Dog
Tom makes a sound
Tom also purrs
Tom the Cat
```

Both pets respond to `speak()` in their own way. That is *polymorphism*: code that loops
over `pets` does not care which kind each one is.

### Checking the family tree

```sage
class Shape:
    define area():
        return 0

class Square extends Shape:
    define init(side):
        self.side = side
    define area():
        return self.side ** 2

let s = Square(3)
say isinstance(s, Square), isinstance(s, Shape), isinstance(Shape(), Square)
say hasattr(s, "side"), hasattr(s, "color")
```

<sub>Output</sub>
```text
true true false
true false
```

`class Dog(Animal):` is an equivalent way to write `class Dog extends Animal:`.

### Mixing styles

Because everything is a value, objects and functional tools work together:

```sage
class Shape:
    define init(name, area):
        self.name = name
        self.area = area

let shapes = [Shape("small", 2), Shape("large", 50), Shape("medium", 12)]
let big_first = sorted(shapes, lambda s: -s.area)
say [s.name for s in big_first]
say shapes |> filter(lambda s: s.area > 5) |> map(lambda s: s.name)
```

<sub>Output</sub>
```text
["large", "medium", "small"]
["large", "medium"]
```

**Try it:** build `Employee`, then `Manager extends Employee` with a list of reports and a
`describe()` that mentions how many people they manage.

---

## 12. When things go wrong

An uncaught error stops the program and says where:

```sage
# expect-error
say "before"
say 10 / 0
say "never reached"
```

<sub>Output</sub>
```text
before
Error on line 3: division by zero
```

### try and catch

```sage
try:
    say 10 / 0
catch problem:
    say "caught:", problem
say "the program keeps going"
```

<sub>Output</sub>
```text
caught: division by zero
the program keeps going
```

### Raising your own errors

`raise` accepts any value: text for simple cases, or an object when you want details:

```sage
define withdraw(balance, amount):
    if amount > balance:
        raise f"cannot withdraw {amount}, only {balance} left"
    return balance - amount

try:
    say withdraw(100, 30)
    say withdraw(100, 500)
catch e:
    say "error:", e
```

<sub>Output</sub>
```text
70
error: cannot withdraw 500, only 100 left
```

```sage
class ValidationError:
    define init(field, message):
        self.field = field
        self.message = message
    define to_text():
        return f"{self.field}: {self.message}"

define check_age(age):
    if age < 0:
        raise ValidationError("age", "cannot be negative")
    return age

try:
    check_age(-5)
catch e:
    say "invalid ->", e
    say e.field
```

<sub>Output</sub>
```text
invalid -> age: cannot be negative
age
```

### assert

```sage
try:
    assert 2 + 2 == 5, "arithmetic is broken"
catch e:
    say e
```

<sub>Output</sub>
```text
assertion failed: arithmetic is broken
```

### Errors pass up through functions

```sage
define inner():
    raise "deep problem"

define outer():
    inner()

try:
    outer()
catch e:
    say "handled at the top:", e
```

<sub>Output</sub>
```text
handled at the top: deep problem
```

### Catching particular kinds of error

Sage has a family of built-in error classes: `Error` (the parent of all of them),
`ValueError`, `TypeError`, `KeyError`, `IndexError`, `IOError`, `ImportError`,
`ZeroDivisionError`, `RuntimeError`, `AssertionError` and `NotImplementedError`.
Mistakes Sage finds itself (like dividing by zero) are matched to the right kind, so you can
react differently to each. Use `catch Kind as name:`; list several kinds with commas.

```sage
define attempt(f):
    try:
        return f()
    catch ZeroDivisionError as e:
        return "math problem: " + str(e)
    catch KeyError, IndexError as e:
        return "lookup problem: " + str(e)
    catch Error as e:
        return "something else: " + str(e)

say attempt(lambda: 1 / 0)
say attempt(lambda: {"a": 1}["b"])
say attempt(lambda: [1, 2][5])
say attempt(lambda: num("x") + nothing)
```

<sub>Output</sub>
```text
math problem: division by zero
lookup problem: key "b" not found
lookup problem: index 5 is out of range (length 2)
something else: cannot turn "x" into a number
```

A bare `catch e:` (or `catch:`) with no kind catches everything, so put it last.

### Your own error classes

Extend `Error` to make errors of your own. `raise` accepts any value, but an `Error` has a
`message`, prints nicely, and can be caught by kind.

```sage
class OutOfStock(Error):
    define init(item, wanted):
        super.init(f"no {item} left, wanted {wanted}")
        self.item = item

define buy(item, n):
    if n > 0:
        raise OutOfStock(item, n)

try:
    buy("tea", 3)
catch OutOfStock as e:
    say "sorry:", e
    say "item was", e.item
    say e.describe()
```

<sub>Output</sub>
```text
sorry: no tea left, wanted 3
item was tea
OutOfStock: no tea left, wanted 3
```

### finally

A `finally` block runs no matter what: after success, after a caught error, after an uncaught
error, and even when the `try` block returns.

```sage
define read_config(ok):
    try:
        if not ok:
            raise ValueError("config is broken")
        return "config loaded"
    finally:
        say "(cleanup runs either way)"

say read_config(true)
try:
    read_config(false)
catch ValueError as e:
    say "caught:", e
```

<sub>Output</sub>
```text
(cleanup runs either way)
config loaded
(cleanup runs either way)
caught: config is broken
```

Common error messages and what they mean:

| Message | Usual cause |
|---|---|
| `undefined variable 'x'` | typo, or used before it was created |
| `division by zero` | dividing by 0 |
| `index 5 is out of range (length 3)` | list position too big |
| `key "x" not found` | missing dict key (use `.get`) |
| `syntax error: expected an indented block` | forgot to indent after a `:` |
| `f() takes 2 arguments but 1 was given` | wrong number of arguments |

---

## 13. Files and modules

### Reading and writing text files

```sage
write_file("notes.txt", "first line\nsecond line\n")
append_file("notes.txt", "third line\n")
let text = read_file("notes.txt")
say text.strip().split("\n")
say file_exists("notes.txt"), file_exists("nope.txt")
```

<sub>Output</sub>
```text
["first line", "second line", "third line"]
true false
```

### open() and with

`open(path, mode)` returns a file object. The modes are `"r"` (read, the default), `"w"`
(write, replacing the file) and `"a"` (append). Use it with `with`, which closes the file for
you even if something goes wrong inside the block.

```sage
with open("log.txt", "w") as f:
    f.write("alpha\n")
    f.write("beta\n")

with open("log.txt") as f:
    for line in f.lines():
        say "read:", line

try:
    open("missing.txt")
catch IOError as e:
    say "could not open:", e
```

<sub>Output</sub>
```text
read: alpha
read: beta
could not open: cannot open file 'missing.txt'
```

### Splitting a program across files

`use "file.sage"` runs another file once, making its functions and classes available.

```sage
write_file("helpers.sage", "define shout(t):\n    return t.upper() + \"!\"\n")
use "helpers.sage"
say shout("modules work")
```

<sub>Output</sub>
```text
MODULES WORK!
```

### Command-line arguments and input

`args` is a list of the words typed after the file name. `ask("prompt")` reads a line of
input. For example, a program saved as `hi.sage`:

```text
let who = ask("Your name? ")
say f"Nice to meet you, {who}"
```

---

## 14. Modules, packages and the standard library

A **module** is just a `.sage` file. `import name` finds `name.sage`, runs it once, and gives
you a value whose attributes are the names the file defined.

```sage
write_file("greetings.sage", "let language = \"English\"\ndefine hello(who):\n    return \"Hello, \" + who\n")
import greetings
say greetings.hello("modules"), greetings.language
```

<sub>Output</sub>
```text
Hello, modules English
```

`from module import a, b as c` pulls names straight in. Every file also has a `__name__`:
it is `"__main__"` when the file is run directly and the module's name when it is imported,
so a file can be both a library and a program.

```sage
write_file("shapes.sage", "define area(w, h):\n    return w * h\n\nif __name__ == \"__main__\":\n    say \"shapes run directly\"\nelse:\n    say \"shapes imported as\", __name__\n")
from shapes import area as rect_area
say rect_area(3, 4)
```

<sub>Output</sub>
```text
shapes imported as shapes
12
```

Each module has its own scope, so two modules can both define `helper` without clashing.
Importing the same module twice runs it only once.

### Packages

A folder containing an `init.sage` file is a package. `import mathx.stats` loads
`mathx/stats.sage`; `import mathx` loads `mathx/init.sage`.

```sage
import os
os.mkdir("mathx")
write_file("mathx/init.sage", "say \"mathx loaded\"\n")
write_file("mathx/stats.sage", "define mean(xs):\n    return sum(xs) / len(xs)\n")
import mathx.stats
say mathx.stats.mean([2, 4, 9])
```

<sub>Output</sub>
```text
mathx loaded
5
```

Where does `import` look? In this order: the folder of the script you ran, the folder you ran
it from, the `packages` folder of the nearest `.sage_env`, any folders listed in the
`SAGE_PATH` environment variable (separated by `:`), and Sage's own `lib` folder.

### Environments and installing packages

Like Python's `venv`, a `.sage_env` folder keeps one project's packages separate from every
other project's. Sage finds it by looking in the current folder and then upwards.

```text
sage env create                      make .sage_env in this folder
sage env                             show which environment is active
sage pkg install ../mylib            install a package from a folder
sage pkg install tool.sage           install a single-file module
sage pkg install https://github.com/someone/cool-lib.git    install from git
sage pkg install -r requirements.txt    install everything listed, one per line
sage pkg list                        show what is installed
sage pkg remove mylib                uninstall
```

A `requirements.txt` here is a plain list of folders, files or git URLs, one per line (`#`
starts a comment). There is no central package website; packages come from folders and git
repositories you point at. Git must be installed to use git URLs.

### The standard library

These modules come with Sage. The `from`/`import` forms work for all of them.

| Module | What it gives you |
|---|---|
| `math` | `sqrt floor ceil trunc sin cos tan asin acos atan atan2 log log10 log2 exp pow abs hypot degrees radians gcd lcm`, and the constants `pi e tau inf nan` |
| `random` | `seed random uniform randint choice shuffle` |
| `os` | `getcwd listdir exists is_dir is_file mkdir remove rename getenv join basename dirname system capture` |
| `time` | `now monotonic sleep format parts` |
| `json` | `parse(text)` and `stringify(value, indent)` |
| `csv` | `parse(text)`, `stringify(rows)`, `dicts(rows)` |
| `re` | `test search find_all replace split` (regular expressions) |
| `log` | leveled messages: `debug info warn error set_level show_time` |
| `collections` | `Stack`, `Queue`, `Counter` |
| `testing` | `assert_equal assert_true assert_false assert_raises` (chapter 17) |

```sage
import json
import re
import math
from collections import Counter

let record = json.parse("{\"name\": \"Ada\", \"langs\": [\"sage\", \"c\"]}")
say record["name"], len(record["langs"])
say json.stringify({"ok": true, "items": [1, 2]})
say re.find_all("[0-9]+", "room 12, floor 3, door 407")
say re.replace("(\\w+)@(\\w+)", "ann@home", "\\2 has \\1")
say math.sqrt(2) * math.sqrt(2) == 2, math.floor(math.pi)
say Counter("mississippi").most_common(2)
```

<sub>Output</sub>
```text
Ada 2
{"ok": true, "items": [1, 2]}
["12", "3", "407"]
home has ann
false 3
[("i", 4), ("s", 4)]
```

Patterns use POSIX extended syntax, with `\d \w \s` (and their capitals) added for
convenience. In a Sage string, write the backslash twice: `"\\d+"`. Replacement text can use
`\1`, `\2` for captured groups.

---

## 15. Tuples and sets

### Tuples

A tuple is a fixed group of values: like a list, but it cannot be changed, and it can be a
dictionary key. Write one with parentheses; a one-item tuple needs a trailing comma.

```sage
let point = (3, 4)
say point, point[0], len(point), point + (5,), point * 2
say (1,), ()
let x, y = point
say x + y
say type(point), (1, 2) == (1, 2)

let where = {(0, 0): "origin", (1, 2): "spot"}
say where[(1, 2)]
```

<sub>Output</sub>
```text
(3, 4) 3 2 (3, 4, 5) (3, 4, 3, 4)
(1,) ()
7
tuple true
spot
```

Returning several values from a function gives you a tuple:

```sage
define min_max(xs):
    return min(xs), max(xs)

let low, high = min_max([4, 9, 2, 7])
say low, high

for i, letter in enumerate("abc"):
    say i, letter
for pair in zip([1, 2], ["x", "y"]):
    say pair
```

<sub>Output</sub>
```text
2 9
0 a
1 b
2 c
(1, "x")
(2, "y")
```

`enumerate`, `zip` and `.items()` all produce tuples. Tuples can be sliced, searched with
`in`, and looped over; they have `count` and `index` methods. Assigning to a position is an
error.

### Sets

A set holds each value once, in no meaningful order (Sage keeps insertion order). Write one
with braces. `{}` on its own is an empty dictionary, so make an empty set with `set()`.

```sage
let tags = {"red", "green", "red", "blue"}
say tags, len(tags), "red" in tags
tags.add("pink")
tags.discard("green")
say sorted(tags)
say set([3, 1, 3, 2]), set()
```

<sub>Output</sub>
```text
{"red", "green", "blue"} 3 true
["blue", "pink", "red"]
{3, 1, 2} set()
```

Sets support the usual maths: `|` union, `&` intersection, `-` difference.

```sage
let a = {1, 2, 3, 4}
let b = {3, 4, 5}
say a | b, a & b, a - b, b - a
say a.is_subset({1, 2, 3, 4, 5}), {1, 2} == {2, 1}
say {n * n for n in range(5)}
say {w: len(w) for w in ["sage", "is", "fun"]}
```

<sub>Output</sub>
```text
{1, 2, 3, 4, 5} {3, 4} {1, 2} {5}
true true
{0, 1, 4, 9, 16}
{"sage": 4, "is": 2, "fun": 3}
```

Use a set to remove duplicates: `list(set(items))`. Items must be hashable: numbers, text,
booleans, `nothing`, tuples of those, and objects. Lists, dicts and sets cannot go inside a set.

---

## 16. Operators, decorators and context managers

### Making your classes work with operators

Define a method with the special name and the operator calls it:

| Method | Used by |
|---|---|
| `__add__ __sub__ __mul__ __truediv__ __floordiv__ __mod__ __pow__` | `+ - * / // % **` |
| `__radd__ __rmul__ ...` | the same, when your object is on the right |
| `__eq__ __lt__ __gt__ __le__ __ge__` | `== < > <= >=` |
| `__neg__` | `-x` |
| `__len__ __getitem__ __setitem__ __contains__` | `len(x)  x[i]  x[i] = v  v in x` |
| `__iter__` | `for item in x` (return a list or other iterable) |
| `__call__` | `x(...)` |
| `__str__` | printing (`to_text` also works) |

```sage
class Money:
    define init(cents):
        self.cents = cents
    define __add__(other):
        return Money(self.cents + other.cents)
    define __mul__(k):
        return Money(self.cents * k)
    define __rmul__(k):
        return Money(self.cents * k)
    define __eq__(other):
        return self.cents == other.cents
    define __lt__(other):
        return self.cents < other.cents
    define __str__():
        return "$" + fixed(self.cents / 100, 2)

let price = Money(250)
say price + Money(99), price * 3, 2 * price
say price == Money(250), price < Money(300), price > Money(300)
```

<sub>Output</sub>
```text
$3.49 $7.50 $5.00
true true false
```

Objects are hashable by identity, so they can be dictionary keys and set members.

### Decorators

A decorator is a function that takes a function and returns a replacement. Put `@name` on
the line above a `define` (or a method, or a class).

```sage
define loud(f):
    define wrapper(*args):
        say "calling", f
        return f(*args)
    return wrapper

define announce(f):
    define wrapper(*args):
        let result = f(*args)
        say "got", result
        return result
    return wrapper

@announce
@loud
define add(a, b):
    return a + b

add(2, 3)
```

<sub>Output</sub>
```text
calling <function add>
got 5
```

Decorators stack: the one nearest the function is applied first.

### Context managers and with

`with thing as name:` calls `thing.__enter__()` first (its result becomes `name`) and
`thing.__exit__(error)` last, whether the block succeeded or failed. `error` is `nothing` on
success. If `__exit__` returns `true`, an error inside the block is considered handled.

```sage
class Timer:
    define init(label):
        self.label = label
    define __enter__():
        say "start", self.label
        return self
    define __exit__(err):
        if err != nothing:
            say "failed with:", err
        say "stop", self.label
        return true

with Timer("job") as t:
    say "working in", t.label
    raise "oops"
say "carried on"
```

<sub>Output</sub>
```text
start job
working in job
failed with: oops
stop job
carried on
```

---

## 17. Testing your code

Name a file `test_something.sage` and inside it write functions whose names start with
`test_`. The `testing` module provides the assertions, and `sage test` finds and runs them.

```sage
write_file("test_math.sage", """import testing

define double(x):
    return x * 2

define test_double():
    testing.assert_equal(double(4), 8)

define test_text():
    testing.assert_true("abc".upper() == "ABC")

define test_error():
    testing.assert_raises(lambda: 1 / 0, ZeroDivisionError)
""")
import os
say os.exists("test_math.sage")
```

<sub>Output</sub>
```text
true
```

```text
$ sage test                 run every test_*.sage and *_test.sage under this folder
$ sage test test_math.sage  run just one file

test_math.sage
  ok    test_double
  ok    test_error
  ok    test_text
3 passed, 0 failed in 1 file(s)
```

A failing assertion reports what it expected and what it got, and `sage test` exits with a
non-zero status so a build script can stop. For quick checks you can always use `assert`.

Use `log` instead of `say` when you want messages that can be filtered:

```sage
import log
log.show_time(false)
log.info("starting")
log.debug("this is hidden by default")
log.set_level("debug")
log.debug("now it shows")
log.warn("careful", 42)
```

<sub>Output</sub>
```text
[INFO] starting
[DEBUG] now it shows
[WARN] careful 42
```

---

## 18. Building your own modules, packages and frameworks

Everything in the standard library is just Sage code or native modules behind `import`, and
you can build the same kinds of things yourself. This chapter goes from a single reusable
file up to a small framework, using only what you have already learned. Sage reads and writes
files freely (`write_file`, `open`, `os`), so programs can also create modules and projects.

### Step 1: a module is a file

Anything you would use in more than one program belongs in its own file. A module can hold
functions, classes and settings. By convention, names that start with `_` are private helpers:
they still work, but `dir(module)` hides them.

```sage
write_file("textkit.sage", """
define _clean(t):
    return t.strip().lower()

define slug(title):
    return _clean(title).replace(" ", "-")

define word_count(t):
    return len(_clean(t).split(" "))

let VERSION = "1.0"
""")

import textkit
say textkit.slug("  Hello Big World "), textkit.word_count("a b c"), textkit.VERSION
say dir(textkit)
```

<sub>Output</sub>
```text
hello-big-world 3 1.0
["slug", "word_count", "VERSION"]
```

### Step 2: modules that keep data in files

A module can load and save its own state. Here `json` turns a dict into text and back, and a
missing file is handled with `IOError`.

```sage
write_file("settings.sage", """
import json

let path = "settings.json"
let values = {}

define load():
    try:
        values.update(json.parse(read_file(path)))
    catch IOError as e:
        say "(no saved settings yet)"

define set(key, value):
    values[key] = value
    write_file(path, json.stringify(values, 2))

define get(key, fallback = nothing):
    return values.get(key, fallback)
""")

import settings
settings.load()
settings.set("theme", "dark")
settings.set("size", 14)
say settings.get("theme"), settings.get("missing", "none")
say read_file("settings.json")
```

<sub>Output</sub>
```text
(no saved settings yet)
dark none
{
  "theme": "dark",
  "size": 14
}
```

Because a module runs only once and is then shared, `settings.values` is the same dictionary
everywhere it is imported. That is how a module can act as a simple shared store.

### Step 3: a package

When one file gets long, turn it into a folder. A folder with an `init.sage` file is a
package, and the other files in it are its submodules. `init.sage` can pull the useful names
up to the top so users write `toolbox.slug(...)` instead of `toolbox.text.slug(...)`.

```sage
import os
os.mkdir("toolbox")
write_file("toolbox/text.sage", """
define slug(t):
    return t.strip().lower().replace(" ", "-")
""")
write_file("toolbox/maths.sage", """
define clamp(x, low, high):
    return max(low, min(high, x))
""")
write_file("toolbox/init.sage", """
from toolbox.text import slug
from toolbox.maths import clamp
let VERSION = "0.1"
""")

import toolbox
say toolbox.slug("A Fine Day"), toolbox.clamp(15, 0, 10), toolbox.VERSION
import toolbox.maths
say toolbox.maths.clamp(-5, 0, 10)
```

<sub>Output</sub>
```text
a-fine-day 10 0.1
0
```

### Step 4: a framework

A library is code *you call*. A framework is code that *calls yours*: it provides the
structure, and you plug in pieces. Decorators and classes are the two tools for this.

The first framework below is a tiny command-line toolkit. You register functions with a
decorator, and the framework decides which one to run. Note that `command("greet")` is
called first and returns the real decorator.

```sage
import os
os.mkdir("minicli")
write_file("minicli/init.sage", """
let registry = {}

define command(name, help = ""):
    define decorator(fn):
        registry[name] = {"run": fn, "help": help}
        return fn
    return decorator

define run(argv):
    if len(argv) == 0 or argv[0] not in registry:
        say "commands:"
        for name in sorted(registry):
            say "  " + name + "  " + registry[name]["help"]
        return nothing
    return registry[argv[0]]["run"](*argv[1:])
""")

import minicli

@minicli.command("greet", help = "say hello")
define greet(who = "world"):
    say "Hello,", who

@minicli.command("add", help = "add two numbers")
define add(a, b):
    say num(a) + num(b)

minicli.run(["greet", "Sage"])
minicli.run(["add", "2", "40"])
minicli.run([])
```

<sub>Output</sub>
```text
Hello, Sage
42
commands:
  add  add two numbers
  greet  say hello
```

The second kind of framework gives users a **base class** to extend. The framework runs the
steps in a fixed order, and the user fills in the blanks (this is sometimes called the
template-method pattern).

```sage
write_file("pipeline.sage", """
class Step:
    define init(name):
        self.name = name
    define run(data):
        raise NotImplementedError(self.name + " must define run()")

class Pipeline:
    define init():
        self.steps = []
    define add(step):
        self.steps.add(step)
        return self
    define execute(data):
        for step in self.steps:
            data = step.run(data)
            say "after", step.name, "->", data
        return data
""")

from pipeline import Step, Pipeline

class Double(Step):
    define init():
        super.init("double")
    define run(data):
        return [x * 2 for x in data]

class KeepBig(Step):
    define init(limit):
        super.init("keep-big")
        self.limit = limit
    define run(data):
        return [x for x in data if x > self.limit]

let result = Pipeline().add(Double()).add(KeepBig(5)).execute([1, 2, 3, 4])
say result

try:
    Pipeline().add(Step("broken")).execute([1])
catch NotImplementedError as e:
    say "framework says:", e
```

<sub>Output</sub>
```text
after double -> [2, 4, 6, 8]
after keep-big -> [6, 8]
[6, 8]
framework says: broken must define run()
```

### Step 5: programs that write programs

Since files are just text, a program can create whole projects. This scaffold writes a new
package with a test file next to it:

```sage
import os

define scaffold(name):
    os.mkdir(name)
    write_file(name + "/init.sage", "let VERSION = \"0.1\"\n\ndefine hello():\n    return \"hello from " + name + "\"\n")
    write_file("test_" + name + ".sage", "import testing\nimport " + name + "\n\ndefine test_hello():\n    testing.assert_equal(" + name + ".hello(), \"hello from " + name + "\")\n")
    say "created", name, "and test_" + name + ".sage"

scaffold("newlib")
import newlib
say newlib.hello()
say sorted(os.listdir("newlib")), os.exists("test_newlib.sage")
```

<sub>Output</sub>
```text
created newlib and test_newlib.sage
hello from newlib
["init.sage"] true
```

### Step 6: sharing and installing

A package you want to share is simply its folder. Keep this layout:

```text
mylib/
  init.sage          the package entry point
  core.sage          submodules
  test_core.sage     tests (run them with: sage test)
  README.md          what it is and how to use it
```

Other people (or other projects of yours) install it into their own environment:

```text
cd my-app
sage env create
sage pkg install ../mylib                                 # from a folder
sage pkg install https://github.com/you/mylib.git         # from git (needs git installed)
sage pkg list
```

Then `import mylib` works in that project, and only that project. To list what a project
needs, put one source per line in `requirements.txt` and run `sage pkg install -r requirements.txt`.

### Guidelines for good modules

- One clear job per module; name the file after it.
- Put your tests in `test_<name>.sage` and run `sage test` before sharing.
- Use `if __name__ == "__main__":` for code that should run only when the file is executed
  directly (a demo or a command-line entry point).
- Raise `Error` subclasses of your own (chapter 12) so callers can catch your errors by kind.
- Avoid names that match the standard library (`math`, `json`, `os`, `collections`, `testing`,
  `log`): your folder is searched first, so yours would hide the built-in one.
- Do not rely on import order or on global variables in other modules; pass values in.

---

## 19. A small project

An inventory that mixes all three styles: a class for the data, plain functions for the
flow, and functional tools for the reports.

```sage
class Item:
    define init(name, price, qty):
        self.name = name
        self.price = price
        self.qty = qty
    define total():
        return self.price * self.qty
    define to_text():
        return f"{self.name} x{self.qty} @ {fixed(self.price, 2)}"

class Inventory:
    define init():
        self.items = {}

    define add(name, price, qty):
        if name in self.items:
            self.items[name].qty += qty
        else:
            self.items[name] = Item(name, price, qty)
        return self

    define take(name, qty):
        if name not in self.items:
            raise f"no such item: {name}"
        let item = self.items[name]
        if item.qty < qty:
            raise f"only {item.qty} {name} in stock"
        item.qty -= qty

    define value():
        return values(self.items) |> map(lambda i: i.total()) |> sum()

    define report():
        let rows = sorted(values(self.items), lambda i: -i.total())
        for item in rows:
            say f"{item.name}: {fixed(item.total(), 2)}"

let inv = Inventory()
inv.add("pen", 1.5, 10).add("book", 12, 3).add("pen", 1.5, 5)
inv.take("book", 1)
inv.report()
say "stock value:", fixed(inv.value(), 2)

for order in [["pen", 20], ["lamp", 1], ["book", 1]]:
    try:
        inv.take(order[0], order[1])
        say "shipped", order[1], order[0]
    catch e:
        say "failed:", e
```

<sub>Output</sub>
```text
book: 24.00
pen: 22.50
stock value: 46.50
failed: only 15 pen in stock
failed: no such item: lamp
shipped 1 book
```

**Ideas to extend it:** save the inventory with `write_file`, add a `low_stock(limit)`
method that uses `filter`, or make `Perishable extends Item` with an expiry date.

### A note on memory

You never free memory yourself. Sage has a garbage collector that reclaims anything your
program can no longer reach. `gc()` forces a collection and returns how many objects are
still alive, which is useful for checking that a long-running program is not holding on to
things it should have dropped:

```sage
let before = gc()
for i in range(100000):
    let scratch = [i, "item " + str(i)]
let after = gc()
say "objects kept alive by the loop:", after - before
```

<sub>Output</sub>
```text
objects kept alive by the loop: 2
```

---

## 20. Cheat sheet

```text
say a, b                    print
let x = 1                   new variable
x += 1                      update (also -= *= /= %=)
f"{x} items"                text with values inside

if c: ... else if c: ... else: ...
while c:      for x in items:      for k, v in d.items():
repeat 3 times:             break    continue    pass

define f(a, b = 2): ...     return v     give back v
define f(*rest, key = 1, **more): ...     f(*xs, key = 2, **opts)
global x     nonlocal x                   change an outer variable from a function
a, b = b, a                 unpacking (also: let a, b = 1, 2)
lambda x: x * 2             x |> f(a)    [x * 2 for x in xs if x > 0]
x if cond else y            a and b      a or b      not a
== != < > <= >=             is  is not  is greater than  is at least ...
in    not in                + - * / // % **

class A: / class B extends A:
    define init(...): ...   self.x = ...   super.method(...)
    define to_text(): ...

try: ... catch Kind as e: ... catch e: ... finally: ...      raise value      assert cond, "msg"
class MyError(Error): pass  Error ValueError TypeError KeyError IndexError IOError ...
with open("f.txt") as f:    @decorator (above define / class)
import m   from m import a as b   import pkg.sub         use "file.sage" (older, shares scope)
(1, 2)  (1,)  ()            tuples     {1, 2}  set()     sets:  | & -  add discard
{x: v for x in xs}          {x for x in xs}              __add__ __eq__ __len__ __getitem__ ...
sage env create    sage pkg install X    sage test
read_file  write_file  append_file  file_exists  open
```

| Lists | Dicts | Text |
|---|---|---|
| `add pop insert remove` | `get has remove` | `upper lower strip title` |
| `sort reverse copy clear` | `keys values items` | `split join replace` |
| `index contains count` | `update copy clear` | `find contains count` |
| `first last extend join` | `d[k] = v` | `starts_with ends_with` |
| `map filter reduce each` | `len(d)` | `s[i]  s[a:b]` |

Global functions: `len str num int bool list type isinstance hasattr range print ask abs min
max sum round floor ceil sqrt pow sin cos tan log exp random randint seed time clock exit
sorted reversed map filter reduce each any all zip enumerate keys values items fixed chr ord
tuple set getattr dir`.
