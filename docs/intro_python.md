---
layout: page
title: Introduction to Python
permalink: /intro_python
---

# Introduction to Python
Python is a high-level, interpreted programming language known for its readability and versatility. It is widely used in various fields such as web development, data analysis, artificial intelligence, scientific computing, and more. Python's simple syntax allows beginners to learn programming concepts quickly while also providing powerful libraries and frameworks for advanced users.

This workshop assumes no prior programming experience. We'll start by getting Python running on your system (whether that's HMS's O2 cluster or your own computer), then work through the core building blocks of the language: variables, data structures, control flow, functions, and files. By the end, you'll be able to read and write simple Python scripts on your own.

## Why Learn Python?
- **Easy to Learn**: Python's syntax is straightforward and resembles natural language, making it accessible for beginners.
- **Versatile**: Python can be used for web development, data science, automation, machine learning, and more.
- **Large Community**: Python has a vast and active community, providing extensive resources, libraries, and frameworks.
- **Cross-Platform**: Python runs on various operating systems, including Windows, macOS, and Linux. This workshop's examples are run from a Linux terminal (as that's what you'll get on O2), but everything here works the same way on Windows or Mac.
- **Interpreted Language**: Rather than being compiled into a standalone program ahead of time, Python code is read and executed line by line by an interpreter. That makes it quick to experiment with and easy to debug, though it comes at some cost to raw performance compared to compiled languages.

## Getting Started with Python
You can follow along with this workshop in one of two ways: on HMS's O2 research computing cluster (if you have O2 access, or are attending this as a live training session), or on your own computer with Python installed locally. Everything from the interpreter onward works identically either way — only the initial setup below differs, so pick whichever section applies to you and skip the other.

### Option A: Installing Python on Your Own Computer
If you don't have (or don't need) O2 access, this is the simpler path:

- **Mac or Linux**: Python 3 is usually already installed. If not, install it with your distribution's package manager (e.g. `sudo apt install python3` on Ubuntu/Debian), or download an installer from [python.org](https://www.python.org/downloads/) for Mac.
- **Windows**: rather than installing Python natively, we'd recommend using [WSL (Windows Subsystem for Linux)](https://learn.microsoft.com/en-us/windows/wsl/install), which gives you a real Linux environment to work in. Desktop Windows installs of Python vary enough (PATH configuration, which terminal you're using, etc.) that we can't reliably cover them here — WSL sidesteps that by making your setup match the Linux instructions on this page exactly. Once WSL is installed and you've opened its terminal, follow the Linux instructions above to install Python.

Once installed, open a terminal and confirm Python is available:

```bash
python3 --version
```

If that prints a version number, you're all set — skip ahead to [Using the Python Interpreter](#using-the-python-interpreter) below.

### Option B: Using O2
On O2, we want to do a bit of extra legwork first, and get an interactive session going via the slurm scheduler.

#### Logging In and Starting an Interactive Session
First, we'll log in. Use your HMS ID, or training account credentials. You can do this by running in a terminal or other similar SSH client (MobaXterm, etc.):

```bash
ssh HMSID@o2.hms.harvard.edu
```

It will prompt for your password - note that you will receive no visual feedback as you type your password, so just type it out and hit enter. Depending on the network you are on, you may also receive a 2-factor authentication prompt. Please select the method you prefer to authenticate.

Once you are logged in, you should avoid running heavy work directly on the login node. Instead, start an interactive session on a compute node with the following command:

```bash
srun --pty --mem=4G --time=02:00:00 -p interactive bash
```

You may see a message that your job is waiting for resources. Once resources are allocated, you'll be placed on a compute node and your prompt will change to reflect that (e.g. from `login01` to something like `compute-a`). From here, you can enter any commands you want, including the ones below.

#### Loading Python
You can check if Python is installed by running:

```bash
python3 --version
```

Most Linux systems, including O2's login and compute nodes, come with some version of Python 3 pre-installed. However, we recommend loading the `python` module instead of relying on the system default, so that you're not tied to a version that could change out from under you with future OS updates.

You can see which Python versions are available to load with:

```bash
module spider python
```

Some newer Python modules also depend on a specific compiler module being loaded first. If you try to load Python on its own and it doesn't seem to be found, check the specific version's requirements:

```bash
module spider python/3.13.1
```

This will tell you which other module(s) need to be loaded first — for Python 3.13.1 on O2, that's a matching `gcc` module. So to load everything correctly, run:

```bash
module load gcc/14.2.0 python/3.13.1
```

Once loaded, you can confirm which `python3` you're now using:

```bash
which python3
```

### Using the Python Interpreter
Whichever setup path you followed above, you should now be able to start Python's interactive interpreter by running `python3` in your terminal:

```bash
python3
```

This will open the Python interpreter, where you can type and execute Python code line by line. You'll know you're in the shell when you see the `>>>` prompt. To exit the Python shell, you can type `exit()`, `quit()`, or press `Ctrl+D`.

This workshop will take a more holistic approach to Python, and we will be writing and modifying scripts in text files and executing them, rather than using the interactive shell for everything. This is a more realistic workflow for data analysis and scientific computing. That said, the interpreter is a great sandbox for testing small snippets of code quickly, and we'll use it throughout the next few sections to explore Python's building blocks before we return to writing full scripts.

### Setting Up a Personal Package Environment
Python code often relies on additional packages (also called modules) beyond what's built into the language — things like NumPy or Matplotlib, which we'll touch on later. Rather than installing packages into your main Python installation (which, on a shared system like O2, you typically don't have permission to modify anyway), it's best practice to use a **virtual environment**: your own private, writable copy of a Python installation, where you're free to install whatever packages a given project needs without affecting anything else.

Python includes a built-in tool for this, so the same commands work whether you're on O2 or your own computer. To create and activate one:

```bash
python3 -m venv nameofenv
source nameofenv/bin/activate
```

Your prompt will now be prefixed with `(nameofenv)`, indicating the environment is active. From here, you can install packages with `pip3`:

```bash
pip3 install packagename
```

When you're done, you can leave the environment with:

```bash
deactivate
```

**If you're on O2**, after loading the `gcc` and `python` modules as shown above, you can alternatively use the `virtualenv` command provided by the `python` module — functionally equivalent to `venv` above, but requires the modules to be loaded first:

```bash
virtualenv nameofenv --system-site-packages
source nameofenv/bin/activate
```

If you'd like to follow along interactively with the rest of this workshop on O2 using a pre-built environment with common scientific packages already installed, you can activate the training environment instead (after loading the modules mentioned above):

```bash
source /n/groups/rc-training/python/env/trainingvenv/bin/activate
```

A few examples later in this workshop (see [A Few Useful Modules for Scientific Computing](#a-few-useful-modules-for-scientific-computing)) use NumPy, SciPy, and Matplotlib. If you're using the pre-built `trainingvenv` above, these are already installed for you. If you've created your own environment instead — on O2 or on your own computer — you can install the exact versions this workshop was tested with from the pinned [`requirements.txt`](https://github.com/alextruong/user-training/blob/gh-pages/Intro_Python/requirements.txt) in this workshop's repository. After downloading it into your working directory:

```bash
pip3 install -r requirements.txt
```

## Writing Your First Python Script
You can create a simple Python script using any text editor. For this example, we'll use `nano`, a command-line text editor available on O2, Mac/Linux terminals, and WSL alike. Create a new file called `hello.py` by running:

```bash
nano hello.py
```

In the `nano` editor, type the following code:

```python
print("Hello, World!")
```

Save the file by pressing `Ctrl+O`, then press `Enter` to confirm. Exit the editor by pressing `Ctrl+X`.

To run your Python script, use the following command:

```bash
python3 hello.py
```

You should see the output:

```
Hello, World!
```

Congratulations! You've just written and executed your first Python script. Now let's break down what's actually happening in that line of code, and build up the rest of the language piece by piece — starting back in the interpreter, where we can try things out one line at a time.

## Basic Syntax: Printing, Quotes, and Comments
Open the interpreter again (`python3`) and try typing a couple of lines. In Python, each line is its own statement, ended simply by pressing Enter — there's no semicolon required like in some other languages:

```python
>>> a = 1
>>> print("hello world")
hello world
```

The `print()` function is how you display output — you'll use it constantly, both to communicate results and to help debug your code as you write it.

### Quotes
Text values in Python are called **strings**, and they're wrapped in quotes. Single and double quotes are interchangeable, both for printing and for passing strings as arguments:

```python
>>> print("hello world")
hello world
>>> print('hello world')
hello world
>>> word = 'abcdefg'
>>> word2 = "abcdefg"
>>> word == word2
True
```

Occasionally you'll want a quote character to appear *inside* a string that's already wrapped in that same kind of quote. In that case, you need to **escape** it with a backslash so Python knows you mean a literal quote character, not the end of the string:

```python
>>> print(""hello world"")
  File "<stdin>", line 1
    print(""hello world"")
          ^^^^^^^
SyntaxError: invalid syntax. Perhaps you forgot a comma?
>>> print("\"hello world\"")
"hello world"
```

A full list of escape characters (like `\n` for a new line, or `\t` for a tab) can be found in the [Python documentation](https://docs.python.org/3/reference/lexical_analysis.html#literals).

### Building Strings from Variables
Often you'll want to combine text with the value of a variable, rather than typing out a fixed string. One way is to concatenate strings together with `+` (as long as everything you're joining is already a string):

```python
>>> name = "world"
>>> print("hello, " + name)
hello, world
```

A more flexible option is an **f-string**: put an `f` right before the opening quote, and any variable (or expression) inside `{}` gets substituted in automatically, including converting non-string values like numbers into text for you:

```python
>>> age = 5
>>> print(f"{name} is {age} years old")
world is 5 years old
```

f-strings are extremely common in real Python code, and we'll use them for the rest of this workshop whenever we need to build a string out of other values.

### Comments
Comments are notes in your code meant for humans to read — Python ignores them entirely. They're denoted with `#`:

```python
# this line does nothing when the script runs
number = 2
print(number)  # this prints 2 — anything after the # on this line is also ignored
```

For longer, multi-line comments, you can use a triple-quoted string instead of stacking up `#` on every line:

```python
"""
this is a
multi-line comment
"""
```

## Variables and Data Types
Python is **dynamically typed**, which means you don't need to declare what kind of value a variable will hold before you use it (unlike some languages, where you must write something like `int number = 1`). You simply assign a value, and Python figures out the type on its own. You can also reassign a variable to a completely different value — even a different type — at any time:

```python
>>> number = 1
>>> number
1
>>> number = 2
>>> number
2
>>> word = 'abc'
>>> word
'abc'
>>> word = 'def'
>>> word
'def'
>>> word = 1
>>> word
1
```

Let's look at a few of the fundamental types you'll use constantly — and the operations that go with them — in a bit more depth.

### Numbers and Arithmetic
Python has two main numeric types you'll run into early on: whole numbers (**int**) and decimal numbers (**float**). The usual arithmetic operators work as you'd expect, with a couple of Python-specific additions:

```python
>>> 7 + 3
10
>>> 7 - 3
4
>>> 7 * 3
21
>>> 7 / 3       # division always gives back a float
2.3333333333333335
>>> 7 // 3      # floor division: divide, then round down to a whole number
2
>>> 7 % 3       # modulo: the remainder left over after floor division
1
>>> 7 ** 3      # exponentiation
343
```

The `%` (modulo) operator is worth remembering in particular — it's a common way to test whether a number is even (`number % 2 == 0`) or to check for multiples of something, since it gives you the remainder rather than the result of the division itself.

### Booleans and Comparisons
Python's boolean type, `bool`, has exactly two possible values: `True` and `False`. You'll most often produce one by comparing two things:

```python
>>> 3 < 5
True
>>> 3 > 5
False
>>> 3 == 3
True
>>> 3 != 5
True
```

The comparison operators are `==` (equal), `!=` (not equal), `<`, `>`, `<=`, and `>=`. You can also combine multiple conditions with the logical operators `and`, `or`, and `not`:

```python
>>> age = 25
>>> age >= 18 and age < 65
True
>>> not (age < 18)
True
```

We'll put these to work in the next section, when we start making decisions in our code with `if` statements and loops.

### `None`
Python also has a special value, `None`, which represents the absence of a value — Python's version of "nothing here yet." It's often used as a placeholder before real data is available:

```python
>>> lst = [None, None, None]
>>> lst[0] is None
True
```

Note the use of `is` rather than `==` above — checking specifically for `None` is conventionally done with `is`, though the distinction between the two is a bit beyond what we need today.

### Checking and Converting Types
If you're ever unsure what type a variable currently holds, `type()` will tell you:

```python
>>> type(7)
<class 'int'>
>>> type("abc")
<class 'str'>
>>> type(True)
<class 'bool'>
```

You can also convert a value from one type to another using `int()`, `float()`, or `str()`:

```python
>>> int("7")
7
>>> float("7.5")
7.5
>>> str(7)
'7'
>>> int("7") + 3
10
>>> "7" + 3
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: can only concatenate str (not "int") to str
```

That last example is a common early stumbling block: Python won't automatically convert a string into a number for you (or vice versa) — you have to do it explicitly. This will matter shortly, since anything read in from a file or typed in by a user always arrives as a string, numbers included.

### A Note on Mutability
One important nuance: strings are **immutable**, meaning you cannot modify their contents in place once created. When you "change" a string, you're actually creating a brand-new string and pointing the variable at it — the original is left untouched (and, since nothing refers to it anymore, is eventually cleaned up automatically). You can read individual characters out of a string, but you can't assign into one:

```python
>>> word = "abc"
>>> word[0]
'a'
>>> word[0] = "o"
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'str' object does not support item assignment
```

## Data Structures: Lists, Tuples, Sets, and Dictionaries
Beyond single values like numbers and strings, Python has several built-in structures for holding collections of values. Each one fills a different niche.

### Lists
A **list** is an ordered, changeable collection of values. You can add to it, remove from it, mix different types of values together, and have duplicates. Lists are probably the data structure you'll reach for most often:

```python
>>> lst = []              # start with an empty list
>>> lst.append('a')
>>> lst
['a']
>>> lst.extend(['b', 'c'])  # add multiple items at once
>>> lst
['a', 'b', 'c']
>>> lst.insert(2, 'new_element')   # insert at a specific position
>>> lst
['a', 'b', 'new_element', 'c']
>>> lst.pop(2)             # remove and return the item at a position
'new_element'
>>> lst
['a', 'b', 'c']
>>> lst.pop()              # with no argument, removes and returns the last item
'c'
>>> lst
['a', 'b']
```

You can also pre-size a list, if you know how many slots you'll need up front:

```python
>>> lst2 = [None] * 5
>>> lst2
[None, None, None, None, None]
>>> lst2[1] = 'foo'
>>> lst2
[None, 'foo', None, None, None]
```

Lists are **zero-indexed**, meaning the first element is at position `0`, not `1`. You can also pull out sub-sections (called *slices*) using a `start:stop` notation, and reach backward from the end using negative numbers:

```python
>>> lst = [1, 2, 3, 4, 5]
>>> lst[1]       # the second element
2
>>> lst[1:]      # from the second element to the end
[2, 3, 4, 5]
>>> lst[1:3]     # from the second up to (but not including) the fourth
[2, 3]
>>> lst[:3]      # from the beginning up to the fourth
[1, 2, 3]
>>> lst[1::2]    # from the second to the end, taking every other element
[2, 4]
>>> lst[-1]      # the last element
5
```

Lists can also be nested inside one another:

```python
>>> lst2 = [1, 2, 3, [4, 5]]
>>> lst2[3][1]   # index into the outer list, then the inner one
5
```

### Tuples
A **tuple** looks like a list, but is immutable — once created, it can't be changed. If you know a collection's contents won't need to change, a tuple is a natural fit (and slightly more efficient, since Python knows up front exactly how much space it needs):

```python
>>> empty_tuple = ()
>>> one_element = 1,       # a trailing comma is what makes this a tuple, not just the number 1
>>> one_element
(1,)
>>> mixed_tuple = (1, 'a', [1, 'a'])
>>> lst = [1, 2, 3]
>>> tuple(lst)             # convert a list into a tuple
(1, 2, 3)
```

Tuples show up naturally whenever you want to assign several values at once, since a comma-separated list of values (with or without parentheses) *is* a tuple. Python lets you unpack one directly into multiple variables in a single line:

```python
>>> a, b = 1, 2
>>> a
1
>>> b
2
>>> a, b = b, a    # a common idiom for swapping two variables
>>> a
2
>>> b
1
```

This is called **tuple unpacking**, and it comes up often enough — including later today, when we loop over `(index, value)` pairs — that it's worth recognizing on sight.

### Sets
A **set** is an unordered collection with no duplicates. Sets are handy whenever you care about uniqueness, or need to compare groups of values against each other:

```python
>>> lst3 = [1, 1, 2, 3, 4, 4]
>>> set(lst3)
{1, 2, 3, 4}
>>> set1 = {1, 2, 3}
>>> set2 = {3, 4, 5}
>>> set1 | set2     # union: everything in either set
{1, 2, 3, 4, 5}
>>> set1 & set2     # intersection: only what's in both
{3}
>>> set1 - set2     # difference: in set1 but not set2
{1, 2}
>>> set1 ^ set2     # symmetric difference: in one set or the other, but not both
{1, 2, 4, 5}
```

More set operations are documented [here](https://docs.python.org/3/library/stdtypes.html#set-types-set-frozenset).

### Dictionaries
A **dictionary** stores `key: value` pairs, letting you look up a value by a meaningful key instead of a numeric position:

```python
>>> dict1 = {1: 'a', 2: 'b', 3: 'c'}
>>> dict1[1]
'a'
>>> dict1[4] = 'd'          # add a new key/value pair
>>> dict1[4] = 'e'          # assigning to an existing key overwrites it
>>> dict1.update({5: 'f', 6: 'g'})   # add several pairs at once
>>> dict1
{1: 'a', 2: 'b', 3: 'c', 4: 'e', 5: 'f', 6: 'g'}
>>> dict1.pop(1)            # remove a key (and get its value back)
'a'
>>> del dict1[2]            # or remove a key with `del`
>>> dict1.keys()
dict_keys([3, 4, 5, 6])
>>> dict1.values()
dict_values(['c', 'e', 'f', 'g'])
>>> dict1.get(99, "no such key")   # a safe lookup with a fallback default
'no such key'
```

### Checking Membership with `in`
Across all of these structures, the `in` operator lets you check whether something is present, without writing your own loop to search for it:

```python
>>> lst = ['a', 'b', 'c']
>>> 'b' in lst
True
>>> 'z' in lst
False
>>> 3 in dict1
True
>>> 'hello' in "hello world"
True
```

This works the same way on lists, sets, and strings (checking for a substring), and on dictionaries (checking their keys).

There's a lot more you can do with all of these data structures. If you find yourself wondering whether a particular structure can do a particular thing, it's very likely someone has already asked (and answered) that exact question online.

## Controlling the Flow of Your Program
So far, every piece of code we've written has run once, top to bottom. Real programs need to repeat steps and make decisions — that's what **loops** and **conditionals** are for.

### `for` Loops
A `for` loop walks through each item in a collection, one at a time. Note that in Python, the body of the loop is marked by **indentation** rather than braces or an explicit "end" keyword — how far a line is indented determines which block of code it belongs to:

```python
>>> things = ['apple', 'banana', 'cherry']
>>> for thing in things:
...     print(thing)
...
apple
banana
cherry
```

This matters beyond just loops: any indented block (loops, conditionals, functions, and so on) is its own **scope**. Code inside that block can see variables from outside it, but anything created fresh inside the block doesn't necessarily exist once you're back outside of it. We'll see this again shortly with functions.

If you need the position of each item as well as its value, use `enumerate()`:

```python
>>> for idx, thing in enumerate(things):
...     print(idx, thing)
...
0 apple
1 banana
2 cherry
```

Notice that `idx` and `thing` are assigned together on one line — that's the tuple unpacking from the [Tuples](#tuples) section earlier, applied here to the `(index, value)` pairs that `enumerate()` produces one at a time.

And if you just need to repeat something a fixed number of times, `range()` gives you a simple sequence of numbers to loop over:

```python
>>> for i in range(5):
...     print(i)
...
0
1
2
3
4
```

### Building Lists with Comprehensions
A very common pattern is looping over something just to build a new list out of it — for example, doubling every number in a list:

```python
>>> numbers = [1, 2, 3, 4]
>>> doubled = []
>>> for n in numbers:
...     doubled.append(n * 2)
...
>>> doubled
[2, 4, 6, 8]
```

Python offers a shorthand for exactly this pattern, called a **list comprehension**, which packs the loop and the `append` into a single expression:

```python
>>> doubled = [n * 2 for n in numbers]
>>> doubled
[2, 4, 6, 8]
```

It reads left to right as "give me `n * 2`, for each `n` in `numbers`." List comprehensions can feel a little dense at first, but they're extremely common in real Python code, so it's worth recognizing the pattern even if you stick with a regular loop while you're still getting comfortable.

### `while` Loops
A `while` loop repeats as long as some condition stays true. It's useful when you don't know in advance how many times you'll need to iterate — for example, while waiting for a calculation to converge:

```python
>>> toggle = True
>>> while toggle == True:
...     toggle = False
...     print(toggle)
...
False
```

This particular loop only runs once, since `toggle` becomes `False` on the first pass and the condition is no longer met on the next check.

### `if` / `elif` / `else`
Conditionals let your program take different actions depending on the situation:

```python
# take some input (let's say, a number)
if number == 0:            # if the number is zero
    print('zero')
elif number % 2 == 0:       # otherwise, if it's even
    print('even')
else:                        # otherwise, it must be odd
    print('odd')
```

You can chain as many `elif` branches as you need, and the `else` branch (if present) always catches anything not matched above it.

### A Note on Errors: `try`/`except`
Sometimes code fails in ways you want to anticipate and handle gracefully, rather than letting the whole program crash. Python lets you wrap risky code in a `try` block, and catch the resulting error in an `except` block if it happens. Unlike some languages, Python's error handling is relatively lightweight — it only costs you performance if an error actually occurs. This is a large enough topic that we won't cover it in depth today, but it's worth knowing it exists; the [Python documentation](https://docs.python.org/3/tutorial/errors.html) is a good starting point.

## Reading and Writing Files
Working with files in Python conventionally uses a **context manager**, created with the `with` keyword. You can think of it as a block of code that runs "inside" an open file — the file is automatically closed for you once the block finishes, even if something goes wrong partway through:

```python
with open('file.txt', 'r') as f:
    for line in f:
        print(line)
```

This is the same scoping idea from the loops section: `f` (the open file) only really needs to exist for as long as that indented block is running.

The first argument to `open()` is the filename; the second is the **mode**, which controls how the file is opened:
- `"r"` — read the file (the default)
- `"w"` — write to the file, overwriting anything already there
- `"a"` — append to the end of the file, keeping existing content
- `"r+"` / `"w+"` — read and write together

A couple of common patterns:

```python
# reading a few lines at a time
with open('file.txt', 'r', encoding="utf-8") as f:
    first_line = f.readline()
    for line in f:            # continue through the rest of the file
        print(line)
```

```python
# writing several lines to a new file
with open('numbers.txt', 'w', encoding="utf-8") as f:
    for x in range(100):
        f.write(f"{x}\n")
```

If you run into garbled or unexpected characters while reading a file, try explicitly passing `encoding="utf-8"` to `open()`, as shown above.

## Functions
Functions let you package up a piece of code so you can reuse it, instead of copying and pasting the same lines over and over. You define one with `def`, and run ("call") it by name wherever you need it:

```python
>>> def greet():
...     print("hello world")
...
>>> greet()
hello world
```

Functions can also accept **arguments** — values passed in when the function is called — and can hand a value back to whoever called them using `return`:

```python
>>> def greet(name):
...     print("hello,", name)
...     farewell = f"goodbye, {name}"
...     return farewell
...
>>> message = greet("world")
hello, world
>>> print(message)
goodbye, world
```

If you don't include a `return` statement, Python assumes you meant to return nothing — but it's good practice to write `return` explicitly (even bare, with nothing after it) when a function isn't meant to hand back a value, just for clarity.

## Structuring a Complete Script
Now that we've covered the core building blocks, let's put them to use on something a bit more substantial than `hello.py`.

Strictly speaking, all a Python program needs is a text file with a **shebang** line at the top:

```python
#!/usr/bin/env python3
```

This line tells the operating system what program should be used to run this file — in this case, whatever `python3` is currently active in your environment. Similar shebangs exist for other languages, like `#!/bin/bash` or `#!/usr/bin/perl`. We specifically use `#!/usr/bin/env python3` (rather than, say, `#!/usr/bin/python3`) for portability: `env` looks up whatever Python is currently active in your environment (for example, inside your loaded module or virtual environment), rather than hard-coding one specific installation path. This matters especially on a shared system like O2, where multiple versions of Python may be installed side by side.

As scripts grow beyond a single `print()` statement, it's conventional to organize them with imports at the top, any functions or classes you need, and a `main()` function that contains the code that actually runs, guarded by an `if __name__ == "__main__":` check. In its most generic form, that shape looks like this:

```python
#!/usr/bin/env python3

# imports go here


def main():
    pass  # the code that actually runs goes here


if __name__ == "__main__":
    main()
```

You'll see this same skeleton — imports, then functions, then a `main()` guarded by that `if __name__ == "__main__":` check — at the top of the vast majority of standalone Python scripts, regardless of what the script actually does. Let's fill it in with something more substantial: a small script that reads a FASTA file — a common plain-text format for biological sequences — and reports each sequence's **GC content**, a basic measure of how much of a DNA sequence is made up of G and C bases rather than A and T.

Here's a small FASTA file, `sequences.fasta`, with three short sequences (each record is a header line starting with `>`, followed by one or more lines of sequence):

```
>seq_001
ATGCATGCATGC
>seq_002
GGGGCCCCAAAA
>seq_003
AAAATTTTGGGG
```

And here's `sequences.py`, which parses that file and reports on it:

```python
#!/usr/bin/env python3


def parse_fasta(filename):
    """Read a FASTA file into a list of (header, sequence) tuples."""
    records = []
    header = None
    sequence = ""
    with open(filename, "r", encoding="utf-8") as f:
        for line in f:
            line = line.strip()
            if line.startswith(">"):
                if header is not None:
                    records.append((header, sequence))
                header = line[1:]
                sequence = ""
            else:
                sequence += line
    if header is not None:
        records.append((header, sequence))
    return records


def gc_content(sequence):
    """Return the percentage of a sequence's bases that are G or C."""
    gc_count = 0
    for base in sequence:
        if base in "GC":
            gc_count += 1
    return gc_count / len(sequence) * 100


def main():
    records = parse_fasta("sequences.fasta")

    gc_values = []
    for header, sequence in records:
        percent_gc = gc_content(sequence)
        gc_values.append(percent_gc)
        print(f"{header}: {percent_gc:.2f}% GC")

    average = sum(gc_values) / len(gc_values)
    print(f"Average GC content: {average:.2f}%")


if __name__ == "__main__":
    main()
```

There's a fair amount going on here, but every piece builds on something from earlier in this workshop:

- `parse_fasta` opens the file with a context manager and loops over it line by line, same as in [Reading and Writing Files](#reading-and-writing-files)
- `line.startswith(">")` is a string method, and `line[1:]` reuses the slice syntax from the [Lists](#lists) section — strings can be sliced too, so this just means "everything after the first character," dropping the leading `>`
- `sequence += line` is shorthand for `sequence = sequence + line`, building up the full sequence one line at a time
- `if header is not None:` uses the `is`-based check from the [`None`](#none) section
- `gc_content` loops over a string one character at a time — the same `for x in y` pattern works on strings, not just lists — and `if base in "GC":` is the same `in` operator from [Checking Membership with `in`](#checking-membership-with-in), just checking a single character against a short string
- `for header, sequence in records:` is tuple unpacking again, same as `for idx, thing in enumerate(things)` earlier
- `len(sequence)` and `sum(gc_values)` are two more built-in functions worth knowing: `len()` gives you the number of items in a list or characters in a string, and `sum()` adds up a list of numbers
- the f-strings use one new trick: `{percent_gc:.2f}` means "format this as a decimal number rounded to 2 places" — handy any time you're printing something you don't want ten digits of

The `if __name__ == "__main__":` line looks a little cryptic at first, but the idea is simple: it makes sure `main()` only runs when you execute this file directly (e.g. `python3 sequences.py`), and not if this file is instead imported as a module from somewhere else. You'll see this pattern in the vast majority of standalone Python scripts, so it's worth recognizing even before it fully makes sense.

You may have noticed the two blank lines separating each top-level function above, rather than just one — that's not arbitrary spacing. It follows [PEP 8](https://peps.python.org/pep-0008/), the Python community's official style guide, which recommends surrounding top-level function and class definitions with two blank lines (and a single blank line between methods inside a class, which we won't get to today). A few other PEP 8 conventions are also already reflected in the example above, worth pointing out explicitly since you'll see them everywhere in real Python code:

- **Indentation** is 4 spaces per level (not a tab, and not some other number of spaces).
- **Function and variable names** are lowercase, with underscores separating words (`parse_fasta`, `gc_content`, `percent_gc`) — this style is often called `snake_case`.
- **Whitespace around operators and after commas** — a single space around `+=`, `/`, and `*`, and after commas in argument lists, but none directly inside parentheses.
- **Quote consistency** — pick single quotes or double quotes for your strings, then stick with that choice throughout a given piece of code, rather than mixing the two arbitrarily.

PEP 8 covers a lot more than this — line length limits, import ordering, and so on — and it's worth a skim once you're comfortable with the basics, since most Python code you'll read (and most collaborators you'll work with) follow it by default.

To run it, save both files in the same directory, then run:

```bash
python3 sequences.py
```

You should see:

```
seq_001: 50.00% GC
seq_002: 66.67% GC
seq_003: 33.33% GC
Average GC content: 50.00%
```

## A Few Useful Modules for Scientific Computing
Python's standard library covers a lot of ground, but scientific and data-focused work usually leans on a few extra packages (install these with `pip3` inside a virtual environment, as described earlier).

A quick note on imports before we dive in: `import numpy` makes everything in a package available as `numpy.something`; adding `as np` gives it a shorter alias, so you can write `np.something` instead; and `from scipy.stats import ttest_1samp` pulls one specific name directly into your script, so you can refer to it with no prefix at all. You'll see both forms below.

Let's pick up right where `sequences.py` left off: it built `gc_values`, a plain list of GC percentages, entirely by hand with a loop. Here's what a few libraries buy you once you have data like that.

- **NumPy** is the go-to package for numerical arrays and matrix operations. Instead of writing your own `sum(...) / len(...)`, it gives you ready-made statistics:

  ```python
  >>> import numpy as np
  >>> gc_values = [50.0, 66.66666666666667, 33.333333333333336]
  >>> print(f"{np.mean(gc_values):.2f}")
  50.00
  ```

  `np.std(gc_values)` gets you the standard deviation just as easily, without writing the variance calculation out by hand.

- **SciPy** builds on NumPy with ready-made functions for statistics and other scientific computing needs. For example: is our average GC content actually different from 50%, or is that close enough to be down to chance?

  ```python
  >>> from scipy.stats import ttest_1samp
  >>> result = ttest_1samp(gc_values, 50)
  >>> print(f"t = {result.statistic:.2f}, p = {result.pvalue:.2f}")
  t = 0.00, p = 1.00
  ```

  With only three sequences this isn't a meaningful test in practice — and a p-value of 1.00 here just reflects that our tiny sample's average happens to land exactly on the 50% we're testing against — but it shows how little code it takes to ask a real statistical question once a library is handling the math for you.

- **Matplotlib** is a plotting library, for visualizing data either interactively or by saving it to an image file. Let's plot the same three values:

  ```python
  >>> import matplotlib.pyplot as plt
  >>> names = ["seq_001", "seq_002", "seq_003"]
  >>> plt.bar(names, gc_values)
  >>> plt.ylabel("GC content (%)")
  >>> plt.savefig("gc_content.png")   # save to a file, useful when working without a display
  ```

Each of these has extensive documentation: [NumPy/SciPy](https://docs.scipy.org/doc/) and [Matplotlib](https://matplotlib.org/stable/users/index.html).

Python also supports **object-oriented programming** through classes, which let you bundle related data and functions together into your own custom types. That's a bigger topic than we can cover today — once your programs grow complex enough that you find yourself managing a lot of related functions and data together, it's worth reading up on classes in the [Python documentation](https://docs.python.org/3/tutorial/classes.html).

## A Note on Python Versions
This workshop uses Python 3.13.1. You may occasionally encounter older code online written for Python 2, which has a handful of syntax differences (the most visible being that `print` was a statement rather than a function). Python 2 reached its official end of life some time ago, so we'd recommend writing any new code in Python 3. If you do need to port old Python 2 code, a search for "Python 2 to 3 cheat sheet" will turn up several good references.

## Getting Help
If you have questions after this workshop, feel free to reach out to us at [rchelp@hms.harvard.edu](mailto:rchelp@hms.harvard.edu), or visit our website to submit a ticket. We also offer consulting for research computing needs beyond what we can cover in a single workshop.

We'd also appreciate your feedback on this workshop — you can share it via our [course survey](https://hms.az1.qualtrics.com/jfe/form/SV_02LPnmjTaLKkFdY).
