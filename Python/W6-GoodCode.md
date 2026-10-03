# Python III - Writing Good Code

## Coding for Science

Now that we have been coding for a few weeks, you may have recognized
that it is easy to make write code that runs with no errors and performs
*a* task. However we usually want our code to perform a specific task,
not something along the lines of it. In some cases (e.g. a simple game
or an entreteinment app), some errors are fine, and our code can be
\"close enough\". In science, however, we want our code to perform
exactly *the* task that we want it to do. Nothing more and nothing less.
Errors in our code can lead to incorrect results, which could upend the
conclusions of your analysis.

With this in mind, today we will focus on ways to write code so that the
probability of error is minimized. Tidy code that is easy to read (by
ourselves and others) doesn't only result in less errors in the first
place, but allows us to test and *debug* it more easily. We will
therefore begin by covering guidelines for organized code writing,
including the use of publicly available packages and modules, which also
make our code more *reproducible*. After this, we will cover error
messages and how to find their causes.

## Writing Modular Code

When we design a program, we come up with an algorithm that is followed
by our program. This algorithm is usually made up of multiple
\"sub-algorithms\" that come together as building blocks of our main
program. Breaking our code into more basic components is therefore a
great way to 1. keep track of the general flow of the program as you
write it, and 2. make it easier to read, understand, and modify.

### Defining your own functions

Over the past two weeks we have used a variety of built-in functions in
Python, such as `len()`, `max()`, `pow()`, texttttype(), etc\... These
functions are themselves programs, which our computers run \"under the
hood\" when we call each specific function, and that developers
considered would be used often enough to be called with a single
command. We can apply the same logic to our own programs. If there is no
function available for a specific tast that is important to the flow of
our program we can write it and *define* it so we can invoke it in our
programs.

For example, consider the code we used last week to calculate the GC
content of a DNA sequence

``` python
    #Define sequence
    seq="GATCGTGCTAGCTGATAGCTAAATATGCACCATGAT"

    # Count Gs and Cs
    g_cont = seq.count("G")
    c_cont = seq.count("C")

    # Calculate GC
    gc = (g_cont + c_cont) / len(seq)

    print(gc)
```

If we needed to do this calculation multiple times through the course of
a program, we could define it as a function

``` python
    def GC_content(seq):
        
        # Count Gs and Cs
        g_cont = seq.count("G")
        c_cont = seq.count("C")
        
        # Calculate GC
        gc = (g_cont + c_cont) / len(seq)
        
        # Output result
        return(gc)
```

Take note of how we changed the code to define our function. First, we
used the `def` function to let
Python know that we intend to define a function. We then followed with
the function name (sometimes known as *keyword*), in this case
`GC_content`, and between
parentheses we defined the arguments that our function should take. Our
function takes a single argument, a sequence which will be assigned to
the variable name `seq`. In the
following lines we write the actual code of our function, and return its
output to the program using
`return`.

We can make sure our function was created using `whos`. Similar to
`who`, this function displays all variables currently saved in this
Python environment, along with some additional information.

``` python
    whos
    Variable    Type        Data/Info
    ---------------------------------
    GC_content  function    <function GC_content at ...
    gc                  float       0.4166666666666667
    c_cont          int         7
    g_cont          int         8
    seq                 str         GATCGTGCTAGCTGATAGCTAAATATGCACCATGAT
```

We can now run our function on any sequence

``` python
    >>> GC_content("GATCGTGCTAGCTGATAGCTAAATATGCACCATGAT")
        0.4166666666666667
    >>> GC_content("GCGCGCGCGCGCGCGCGCGCGCGCGCGCGCGCGCGCGCGCGCGCGCG")
        1.0
    >>> GC_content("AATAAATTTAATTTAATTAATTATAAATTAATACTAATTAATTAATTA")
        0.020833333333333332
```

Like with any other function, we can save its result as a variable

``` python
    Cytb="TAGCTATACACTATACTGCAGATACCACCATAGCCTTCTCCTCTGTC"
    cytb_gc=GC_content(Cytb)
    print(cytb_gc)
```

Lets create some more functions. For instance, the function below
returns the square of a given range of integers.

``` python
    def squared(start = 1, end = 10):
        results = []
        for i in range(start, end):
            r = i ** 2
            results.append(r)
        print(results)
```

Note two things about this function. First, note that it accepts
multiple arguments, and that we have specified the *default* values for
these arguments. What this means, is that if the user doesn't supply a
value for an argument, the default value is used.

``` python
    # Run giving values for all arguments
    >>> squared(start=3, end=6)
        [9, 16, 25]

    #Just for the end of the range
    >>> squared(end=12)
        [1, 4, 9, 16, 25, 36, 49, 64, 81, 100, 121]

    #With no arguments given. Defaults to (1,10)
    >>> squared()
        [1, 4, 9, 16, 25, 36, 49, 64, 81]
```

We don't always have to specify argument names. If we don't the passed
arguments will be taken in the same order as they are declared in the
function.

``` python
    # Between 2 and 8
    >>> squared(2, 8)
        [4, 9, 16, 25, 36, 49]
```

This being said, code where arguments are called is much more readable
than the opposite case. The second thing to note about this function is
that it doesn't have a `return`
statement. This means that the function will simply perform its
operation, but its results will not be assignable as variables, or
useable as input for other functions.

``` python
    # Between 2 and 8
    >>> a = squared(2, 8)
        [4, 9, 16, 25, 36, 49]
    >>> print(a)
        None
    >>> print("The squares are", squared(2, 8))
        [4, 9, 16, 25, 36, 49]
        The squares are None
```

With this in mind, it is usually advisable to use `return` statements in
functions

``` python
    def squared(start = 1, end = 10):
        results = []
        for i in range(start, end):
            r = i ** 2
            results.append(r)
        return results

     a = squared(2, 8
     
     print(a)
     [4, 9, 16, 25, 36, 49]
```

**Exercises:**

1.  The code below prints multiple things about the course instructor to
    the screen.

    ``` python
        # Define variables
        name = "Roberto"
        activity = "Coding"

        # Print to screen
        print(
        "My name:", name,
        "\nMy favorite activity:", activity)
    ```

    Modify this code so it becomes a function that can print the same
    information about any user who supplies their name and favorite
    activity. The expected output is

    ``` python
        My name: Roberto 
        My favorite activity: Coding
    ```

2.  Determine what each of the following functions does:

    1.  ``` python
            def foo1(x = 7):
                    return x ** 0.5
        ```

    2.  ``` python
            def foo2(x = 3, y = 5):
                    if x > y:
                        return x
                    else:
                        return y
        ```

    3.  ``` python
            def foo3(x = 1729):
                    d=2
                    myfactors = []
                    while x > 1:
                        if x % d == 0:
                            myfactors.append(d)
                            x=x/d
                        else:
                            d=d+1
                    return myfactors
        ```

#### A note on variables

When you create a variable within a script or notebook, that variable
usually goes to the *global* environment. That is, it can be called from
anywhere in our code. However, when you create a variable as part of a
function, this variable only exists *within* the function. For example

``` python
    # set x to 10
    x = 10

    # Function that sets x to a different value
    def foo3(z):
        x=z
        print("x =", x)

    # Print x
    print("x =" x)
    x = 10

    # run the function
    foo3(z=2)
    x = 2

    # Print x again (remains unchanged)

    print("x =" x)
    x = 10
```

### Modules

Chances are a lot of the time we are not the first person to have needed
a particular function. Given the collaborative nature of scientific
computation, and the Free Software movement in general, there are
probably millions of freely available functions that we can download and
use. In Python, collections of such functions are called *modules*.
Large modules that contain contain many functions, and sometimes
*submodules* are called packages. The standard installation of Python
already ships with many modules, but many many others can be installed
using a *package manager* such as Anaconda (or Miniconda), which we used
to download `Jupyter` (and Python itself in some cases).

Packages are imported using the function
`import`.

``` python
    # import the module csv to deal with character-delimited tables
    import csv

    # get help for the function csv.dictreader
    help(csv.DictReader)
```

Note that when we load a module, we call its functions prefacing their
names with the package name. For example, the `DictReader` function in
`csv` is called as
`csv.DictReader`.

If we only need a specific function from a module, or a submodule from a
package, we can load it as

``` python
    # import the sample function from the random module
    from random import sample

    # Randomly sample elements of a list 
    ls = ["a", "b", "c", "d", "e"]
    sample(ls, 3)

    ['e', 'b', 'c']
```

Note that in this case the function was imported without its module name
appended (i.e. `sample` instead of
`random.sample`). This can lead to
two functions (or other objects loaded with the module) having the same
name and overwriting each other. With that in mind, it is usually
recommended to load whole modules.

``` python
    # import the sample function from the random module
    import random

    # Randomly sample elements of a list 
    ls = ["a", "b", "c", "d", "e"]
    random.sample(ls, 3)

    ['e', 'b', 'c']
```

Modules can be installed in a number of ways. Modules sometimes use
functions from other modules, so it is advisable to install them using
package managers, which automatically make sure all dependencies are
met. Common package managers are `pip` and `[Ana/Mini]conda`. To install
a package with `conda`, we can type
`conda install repository::<package name>`.

``` python
    # Install scipy, hosted in conda-forge
    $ conda install conda-forge::scipy
```

In addition to the many freely available Python modules, you can also
create your own. In its most basic form, a module is a text file with
Python code defining functions and other objects. To be importable into
Python environments, these files should have a `.py` extension. These
files can be imported just like installed modules.

The following module file is located at
`IntroBiolComp-2026/Python/example_module.py`

``` python
    def add(a, b):
        return a + b

    def odd_even(num):
        print("Even" if num % 2 == 0 else "Odd")
```

We can load and use it

``` python
    import example_module

    example_module.odd_even(8)
        Even
```

Note that to load a module it must be either in the same folder as your
Python script, or in a pre-defined folder where Python knows to look for
modules. Modules installed using package managers are in one of these
folders. Defining the folders where Python can find packages is beyond
the scope of our class, but a good summary of how this can be done can
be found
[here](https://www.geeksforgeeks.org/python/python-import-module-from-different-directory/).

### Passing arguments to `Python` scripts

Just like our `Python` scripts can be made up of multiple parts, they
can also be part of larger pipelines where we combine programs in
multiple languages to run complex analyses, for instance from a UNIX
terminal. In this case (and many others), it is key to be able to pass
information to our scripts from the outside environment. The easiest way
to achieve this is by passing arguments to our script when we run it (we
learned a way to do this for UNIX shell scripts on Week 2).

The module `sys` allows us to access arguments passed to a script when
it is run on the terminal.

``` python
    #!/usr/bin/env python3
    import sys

    print("The name of the script is saved as", sys.argv[0])
    print("The first argument is", sys.argv[1])
    print("The second argument is", sys.argv[2])
```

Now from the terminal

``` python
    $ python myscript.py one two
      The name of the script is saved as myscript.py
      The first argument is one
      The second argument is two
```

## Writing style

As Allessina & Wilmes put it \"code is read more often than it is
written\". Writing code that is easy to read by others, and ourselves
for that matter is key. For other scientsits to be able to evaluate our
code, and perhaps use it in their own research, they must be able to
understand what it does and how it is doing it. Similarly, you will
usually use your own code multiple times. Perhaps new data come in and
you need to redo a a particular analysis, or you receive feedback from
colleagues or reviewers and need to modify your code. Perhaps you are
doing a similar analysis to what you've done before. In these and many
other cases having well documented, easily readable code will make your
life a lot easier.

Several guidelines have been published to make code more readable. Key
among them are:

- Using new lines for each operation

- Separating related code blocks with blank lines, and using comments to
  document what they are doing

- Splitting long lines to make them more readable

- Using tidy and consistent indentation to visually differentiate code
  blocks.

- Importing all needed modules and functions at the start of the text.

- Importing whole modules to keep track of where each function comes
  from.

- Keeping frequently-used functions in modules that can be loaded into a
  main script to reduce clutter.

A more detailed style guide with examples can be found in section 4.3 of
Allessina & Wilmes. A printout of this guide is also available on
Canvas.

## Errors and Exceptions

By now, you have surely gotten many error messages when trying to run
code. Even if it doesn't seem like it some times, these are put in place
by developers to stop code from running in unpredictable ways, and let
the user know that the code may not be proceeding as expected. In simple
terms, when a program encounters something problematic, it stops and
outputs a message about why it stopped. Learning how to read, *and
write* your own error messages and exceptions is key to writing good
code.

There are many types of errors and exceptions in Python. Errors are
problems in the code that prevent it from running, such as syntax errors
(e.g. you're missing a comma or forgot to add a colon after a `for`
statement), or indentation errors (a line of code is not indented as
Python expects). Exceptions are problems that occur as a
gramatically-correct program runs, which preclude it from running
properly. For example, dividing by zero, or failing to provide a
required input.

When you get an error or exception in Python, you will be told what type
of error it is (e.g.
`SyntaxError, IndentationError, TypeError, IOError`), which can give you
an idea of what is going on. There are many types of errors. Some are
self-explanatory, but others can be harder to understand. A good
resource for this is the official Python
[documentation](https://docs.python.org/3/builtins/exceptions.html) on
exceptions, and
[here](https://pythonforbiologists.com/articles/29-errors.html) is a
good cheat-sheet to deal with common errors.

### Raising your own Exceptions

Because they must apply to any code ever written in Python (or any
language for that matter), built-in exceptions can be confusing and/or
uninformative. It is therefore good practice to anticipate errors that
may occur in our own programs, and adding exceptions to stop execution
and deliver a more informative message. For example, assume we want to
divide one number by another.

``` python
    x = 8
    y = 2

    print(x / y)
```

If y is zero, Python will raise an exception

``` python
    x = 8
    y = 0

    print(x / y)
    ------------------------------------------------------
    ZeroDivisionError    Traceback (most recent call last)
    ----> 4 print(x / y)
    ZeroDivisionError: division by zero
```

If we want to raise our own exception, we can use the
`try` and
`except` functions.

``` python
    x = 8
    y = 0

    # Add code to catch exception
    try:
        print(x / y)
    except:
        print("Cannot divide by zero")

    # Keep going
    print("Done")
```

What the code above does is try to run
`print(x / y)`, and if an exception
is raised print a message (\"Cannot divide \...\"). Then it keeps going.
This is a good start, but not ideal. For example, what if the error is
not division by zero but the user not providing one of the values? We
can pass arguments to `except` so it only runs in particular cases.

``` python
    x = 8
    y = "two"

    # Add code to catch exception
    try:
        print(x / y)
    except ZeroDivisionError:
        print("Cannot divide by zero")
    except TypeError:
        print("Incorrect variable type")

    # Keep going
    print("Done")
```

Here, we have told `except` what to do if `ZeroDivisionError` or
`TypeError` exceptions occur. You may have also noticed that the last
line of code runs regardless of whether an exception occurs. Sometimes
we want code to run only if there are no exceptions. For example, if we
were going to use the result of the division above in another operation,
it would only make sense to do this if the division was successfully
calculated. We can add an `else` statement to our code to run code only
if there were no exceptions.

``` python
    x = 8
    y = 0

    # Add code to catch exception
    try:
        print(x / y)
    except ZeroDivisionError:
        print("Cannot divide by zero")
    except TypeError:
        print("Incorrect variable type")
    else:
        # Keep going
        print("It worked!")
```

The above is a useful way to stop the execution of a program if there
are exceptions. However, in a larger program where we may want to catch
multiple exceptions it may be more practical to force the code execution
to stop. A way to do this is using the `exit` function of the `sys`
module. Note that because Jupyter notebooks keep a python session open
as you run your code, `sys.exit` doesn't work in this context. To
demonstrate this, we need to write a python script and execute it from
the terminal.

``` python
    #!/usr/bin/env python3
    import sys

    x = 8
    y = 0

    # Add code to catch exception
    try:
        print(x / y)
    except ZeroDivisionError:
        print("Cannot divide by zero")
        sys.exit()
    except TypeError:
        print("Incorrect variable type")
        sys.exit()

    # Keep going
    print("It worked!")
```

### Logical Errors

A special kind of error are *logical errors*, where the code is able to
run, but produces an incorrect result. These can be detected by running
\"proof of concept\" tests on our code to make sure it behaves as
expected under controlled conditions. We will learn how this is done
next week.

### Debugging

Bugs will appear in your code. Fixing bugs often involves taking our
program apart, to see whether each step is running as it should, and
identify what specific bits of code are causing the problem. Although we
could do this by, for example adding a bunch of `print` statements this
may end up making a mess out of our code that may be difficult to undo.
For most programming languages thereis software that allows us to go
into `debugging` mode, where, instead of terminating, whenever an
exception occurs the program stops but keeps the environment open at the
line where the exception occurred. In Python we can use the function
`pdb` to start a debugger.

To demonstrate this lets use our GC content function from the start, but
introduce an error

``` python
    def GC_content(seq):
        
        # Count Gs and Cs
        g_cont = str(seq.count("G"))
        c_cont = seq.count("C")
        
        # Calculate GC
        gc = (g_cont + c_cont) / len(seq)
        
        # Output result
        return(gc)
```

If we run this function, we will get an error.

``` python
    GC_content("ATTTAGGACT")
    ----------------------------------------------
    TypeError    Traceback (most recent call last)
    Cell In[41], line 1
    ----> 1 GC_content("ATTTAGGACT")

    Cell In[40], line 8, in GC_content(seq)
          4     g_cont = str(seq.count("G"))
          5     c_cont = seq.count("C")
          6 
          7     # Calculate GC
    ----> 8     gc = (g_cont + c_cont) / len(seq)
          9 
         10     # Output result
         11     return(gc)

    TypeError: can only concatenate str (not "int") to str
```

This only tells us that the problem occurs when running line 8. Lets
find out more

``` python
    #open debugger
    pdb

    ## rerun function 
    GC_content("ATTTAGGACT")
```

You will get the same error, but now there is is a command line prompt,
called the ipdb shell, that you can use to investigate further. From
here we can examine the values of all variables

``` python
    ipdb>  seq
    'ATTTAGGACT'
    ipdb>  c_cont
    1
    ipdb>  g_cont
    '2'
    ipdb>  gc
    *** NameError: name 'gc' is not defined. Did you forget to import 'gc'
    GC_content("ATTTAGGACT")
```

We can tell two things from this 1. the variable `gc` is not being
called. Second, note how the value of `g_cont` is surrounded by quotes.
This tells uts it may not be a numerical variable. Lets investigate
further.

``` python
    ipdb>  type(g_cont)
    <class 'str'>
```

Indeed, `g_cont` is a string. Strings can't be used to perform
calculations, so this is probably our problem. Type
`q` to exit the debugger.

#### Debugging Logical Errors

We've now covered how to debug errors that stop execution of a program.
If we start `pdb`, it will trigger debugging mode as soon as an
exception or error occurs. This, of course, only works with errors that
stop the execution of the code. Logical errors don't cause execution to
stop, so they must be debugged in a slightly different way.: We can use
the `pdb.set_trace()` function to
create a *breakpoint* in the code, where a debugger will start. Lets try
this with our `GC_content` function, now with a logical error instead of
a `TypeError`.

``` python
    def GC_content(seq):
        
        # Count Gs and Cs
        g_cont = seq.count("G")
        c_cont = seq.count("C")
        
        # Calculate GC
        gc = (g_cont + c_cont) * len(seq)
        
        # Output result
        return(gc)
```

Sequence `AAGTCGTGAGCA` is 12 nucleotides long, and has 4 `G` and 2 `C`
bases, so we expect the GC content to be 0.5. If we run our function we
get

``` python
    >>> GC_content("AAGTCGTGAGCA")
        72
```

This is clearly wrong, not only is it not 0.5, but it is greater than 1,
and considering the GC content is the proportion of GC nucleotides in a
sequence, it should range between 0 and 1. Lets set a break point to see
if the G and C counts are being done correctly.

``` python
    import pdb

    def GC_content(seq):
        
        # Count Gs and Cs
        g_cont = seq.count("G")
        c_cont = seq.count("C")
        
        pdb.set_trace()
        
        # Calculate GC
        gc = (g_cont + c_cont) * len(seq)
        
        # Output result
        return(gc)
```

When we run our function, the debugger will launch, and we can begin
debugging.

``` python
    GC_content("AAGTCGTGAGCA")

    ipdb> c_cont
    2
    ipdb> g_cont
    4
```

These values are as expected. Lets move the debugger a bit further down
the code.

``` python
    def GC_content(seq):
        
        # Count Gs and Cs
        g_cont = seq.count("G")
        c_cont = seq.count("C")
        
        # Calculate GC
        gc = (g_cont + c_cont) * len(seq)
        
        pdb.set_trace()
        
        # Output result
        return(gc)
```

Now lets see what `gc` is being stored as.

``` python
    GC_content("AAGTCGTGAGCA")

    ipdb>  gc
    72
```

Clearly incorrect. So we know the error is not due to the G and C
counts, but rather due to how the GC content is being calculated. When
writing code that you think is prone to errors (e.g. complex functions),
it is always a good idea to periodically set breakpoints to preemptively
make sure things look right. Logical errors are the hardest to spot so
preventing them should be a priority in scientific computing.

# Final Problems

Answer the following questions in a **single markdown-formatted
document** within a folder in your GitHub repository (e.g.
`Practicals/W6/`. Add any scripts and other files you create to the
folder as well.

### Debug this code!

Below is a program that translates an mRNA sequence into amino acids.
Briefly, it starts at the first base, and iterates through all
nucleotide triplets, translating to amino acids, and stopping when it
encounters a stop codon (UAA, UAG, or UGA). A dictionary containing the
genetic code has previously been created, and is stored in a file format
called *pickle*, which saves ready-to-use Python objects (more on that
[here](https://docs.python.org/3/library/pickle.html). However, the code
has a bug and is not running properly.

For your convenience, the script is in the
`IntroBiolComp-2026/Python/genetic_code.py` directory.

``` python
    import pickle
    # load dictionary with genetic code from pickle file
    genetic_code = pickle.load(open("../data/genetic_code.pickle", "rb"))

    # test case: desired amino acid sequence
    # MEFSL[stop]
    test_mRNA = "AUGGAAUUCUCGCUCUGAAGGUAA"

    def get_amino_acids(mRNA):
        i = 0
        aa_sequence = []
        while (i + 3) < len(mRNA):
            codon = mRNA[i:(i + 3)]
            aa = genetic_code[codon]
            if aa == "Stop":
                break
            else:
                aa_sequence.append(aa)
            # advance to the next codon
            i = i + 4
        return "".join(aa_sequence)

    print(get_amino_acids(test_mRNA))
    # problem: the program returns MNLLEV instead of the expected MEFSL!
```

1.  In your own words, explain what each line is doing.

2.  Copy the code into a Jupyter cell, and use `pdb` to find the bug.

### Gobbler proteins

Fix the function above, and use it to translate the fifteen mRNA
sequences present in
`IntroBiolComp-2026/Python/DataFiles/Turkey_transcripts_15_coding.fasta`.
Output a fasta file with the corresponding protein sequences. Give your
new sequences the same sequence headers as in the transcript file, but
instead of ending in \"gbskey=CDS\", end in \"gbskey=CDS\". **Note:**
The transcripts in our file are expressed as DNA sequences, so they have
a Ts instead of Us for bases that would be an uracyl in teh actual RNA
molecule.
