# Python

## Python 3 vs Python 2

- `print`
    - Python 3
        - `print("Hello")`
        - `print("Hello", end=' ')` with newline added
    - Python 2
        - `print "Hello"`
        - `print "Hello",` with newline added
- Division with Integers
    - When the two numbers on either side of the division / symbol are integers, Python 2 does floor division
    - a = 5 / 2 => 2
    - In Python 3, integer division became more intuitive; a = 5 / 2 => 2.5
    - To get floor division in Python 3, use `//`; a = 5 // 2 => 2
- Python 3 uses Unicode by default
    - it saves extra development time, and allows many more characters
- In Python 3, raising exceptions with arguments requires parentheses, and strings cannot be used as exceptions
    - `raise Exception(args)` in Python 3 vs `raise Exception, args` in Python 2
    - `raise Exception("Error")` in Python 3 vs `raise "Error"` in Python 2
- `except`
    - `except (Exception1, Exception2), target:` in Python 2
    - `except (Exception1, Exception2) as target:` in Python 3
- `exec`
    - The exec statement of Python 2 has changed to a function in Python 3
    - `exec code` in Python 2 vs `exec(code)` in Python 3
- String Formatting
    - Python 2: `"%d %s" % (i, s)` vs Python 3: `"{} {}".format(i, s)`
    - Python 2: `"%d/%d=%f" % (355, 113, 355/113)` vs Python 3: `"{:d}/{:d}={:f}".format(355, 113, 355/113)`
- `class`
    - There is no need to state `object` in Python 3
    - `class MyClass(object):` in Python 2 vs `class MyClass:` in Python 3
- Dictionary methods
    - In Python 2 dictionaries have the methods `iterkeys()`, `itervalues()` and `iteritems()` that return iterators instead of lists. In Python 3 the standard `keys()`, `values()` and `items()` return dictionary views, which are iterators, so the iterator variants become pointless and are removed.
- `filter()`
    - In Python 2 `filter()` returns a list while in Python 3 it returns an iterator.
- `map()`
    - In Python 2 `map()` returns a list while in Python 3 it returns an iterator.
- Parameter unpacking
    - In Python 2 you have parameter unpacking, while Python 3 does not support this, so you need to do your own unpacking

    ```python
    # Python 2
    def unpacks(a, (b, c)):
        return a,b,c

    unpacks(1, (2,3))  # (1, 2, 3)

    # Python 3
    def unpacks(a, (b, c)):
        return a,b[0],b[1]

    unpacks(1, (2,3))  # (1, 2, 3)
    ```
- `range()` and `xrange()`
    - In Python 2 `range()` returns a list, and `xrange()` returns an object that will only generate the items in the range when needed, saving memory.
    - In Python 3, the `range()` function is gone, and `xrange()` has been renamed `range()`.
- `repr()` as backticks
    - In Python 2 you can generate a string representation of an expression by enclosing it with backticks. It has been removed in Python 3, since the `repr()` builtin does exactly the same.

### Update Code to Python 3

There are two main tools you can use to automatically update your code to Python 3 while keeping it compatible with Python 2: [future](http://python-future.org/automatic_conversion.html) and [modernize](https://python-modernize.readthedocs.io/en/latest/).

## Useful Packages / Libraries

- **Coverage.py**
    - https://github.com/nedbat/coveragepy
    - It measures code coverage, typically during test execution. It uses the code analysis tools and tracing hooks provided in the Python standard library to determine which lines are executable, and which have been executed.
- **Six**
    - https://github.com/benjaminp/six
    - It is a Python 2 and 3 compatibility library. It provides utility functions for smoothing over the differences between the Python versions with the goal of writing Python code that is compatible on both Python versions.
- **Pylint**
    - https://github.com/PyCQA/pylint/
    - It is a Python source code analyzer which looks for programming errors, helps enforcing a coding standard and sniffs for some code smells.
- **aiohttp**
    - https://github.com/aio-libs/aiohttp
    - Async http client/server framework

## Best practices

- `from modu import *`: This is generally considered bad practice.
    - Using `from modu import func` is a way to pinpoint the function you want to import and put it in the global namespace.
    - Very bad: `from modu import *`
        - `x = sqrt(4)` #Is sqrt part of modu? A builtin? Defined above?
    - Better: `from modu import sqrt`
        - `x = sqrt(4)` #sqrt may be part of modu, if not redefined in between
    - Best: `import modu`
        - `x = modu.sqrt(4)` #sqrt is visibly part of modu's namespace
- Any directory with an \_\_init\_\_.py file is considered a Python package.
    - Different modules in the package are imported in a similar manner as plain modules, but with a special behavior for the \_\_init\_\_.py file, which is used to gather all package-wide definitions.
    - A file modu.py in the directory pack/ is imported with the statement `import pack.modu`. This statement will look for an \_\_init\_\_.py file in pack, execute all of its top-level statements. Then it will look for a file named pack/modu.py and execute all of its top-level statements. After these operations, any variable, function, or class defined in modu.py is available in the pack.modu namespace.
    - Leaving an \_\_init\_\_.py file empty is considered normal and even a good practice, if the package’s modules and sub-packages do not need to share any code.
    - Importing deeply nested packages: import very.deep.module as mod. This allows you to use mod in place of the verbose repetition of very.deep.module.
- One peculiarity of Python that can surprise beginners is that strings are immutable. This means that when constructing a string from its parts, it is much more efficient to accumulate the parts in a list, which is mutable, and then glue ('join') the parts together when the full string is needed. One thing to notice, however, is that list comprehensions are better and faster than constructing a list in a loop with calls to append().

```python
#Bad
# create a concatenated string from 0 to 19 (e.g. "012..1819")
nums = ""
for n in range(20):
    nums += str(n)   # slow and inefficient
print nums

#Good
# create a concatenated string from 0 to 19 (e.g. "012..1819")
nums = []
for n in range(20):
    nums.append(str(n))
print "".join(nums)  # much more efficient

#Best
# create a concatenated string from 0 to 19 (e.g. "012..1819")
nums = [str(n) for n in range(20)]
print "".join(nums)

#You can also use the % formatting operator to concatenate a pre-determined number of strings besides str.join() and +.
foo = 'foo'
bar = 'bar'
foobar = '%s%s' % (foo, bar) # It is OK
foobar = '{0}{1}'.format(foo, bar) # It is better
foobar = '{foo}{bar}'.format(foo=foo, bar=bar) # It is best

#Using join() is not always best. In the instances where you are creating a new string from a pre-determined number of strings, using the addition operator is actually faster, but in cases like above or in cases where you are adding to an existing string, using join() should be your preferred method.
foo = 'foo'
bar = 'bar'
foobar = foo + bar  # This is good
foo += 'ooo'  # This is bad, instead you should do:
foo = ''.join([foo, 'ooo'])
```
- Explicit code

```python
#Bad
def make_complex(*args):
    x, y = args
    return dict(**locals())

#Good
def make_complex(x, y):
    return {'x': x, 'y': y}
```
- PEP 8
    - PEP 8 is the de-facto code style guide for Python.
    - `pip install pep8`
    - `pep8 fileName.py`
- autopep8
    - The program autopep8 can be used to automatically reformat code (in-place) in the PEP 8 style. Install the program with
        - `pip install autopep8`
        - `autopep8 --in-place fileName.py`
    - Excluding the `--in-place` flag will cause the program to output the modified code directly to the console for review
    - The `--aggressive` flag will perform more substantial changes and can be applied multiple times for greater effect
- Check if variable equals a constant
    - You don’t need to explicitly compare a value to True, or None, or 0 - you can just add it to the if statement. See Truth Value Testing for a list of what is considered false.
    - Truth Value Testing
        - The following values are considered false: None, False, 0, 0L, 0.0, 0j, '', (), [], {}, instances of user-defined classes if the class defines a \_\_nonzero\_\_() or \_\_len\_\_() method, when that method returns the integer zero or bool value False
        - All other values are considered true — so objects of many types are always true

    ```python
    #Bad
    if attr == True:
        print 'True!'

    if attr == None:
        print 'attr is None!'

    #Good
    # Just check the value
    if attr:
        print 'attr is truthy!'

    # or check for the opposite
    if not attr:
        print 'attr is falsey!'

    # or, since None is considered false, explicitly check for it
    if attr is None:
        print 'attr is None!'
    ```
- Access a Dictionary Element
    - Don’t use the dict.has_key() method. Instead, use x in d syntax, or pass a default argument to dict.get().

    ```python
    #Bad
    d = {'hello': 'world'}
    if d.has_key('hello'):
        print d['hello']    # prints 'world'
    else:
        print 'default_value'

    #Good
    d = {'hello': 'world'}

    print d.get('hello', 'default_value') # prints 'world'
    print d.get('thingy', 'default_value') # prints 'default_value'

    # Or:
    if 'hello' in d:
        print d['hello']
    ```
- Short Ways to Manipulate Lists

```python
#Bad
# Filter elements greater than 4
a = [3, 4, 5]
b = []
for i in a:
    if i > 4:
        b.append(i)

#Good
a = [3, 4, 5]
b = [i for i in a if i > 4]
# Or:
b = filter(lambda x: x > 4, a)

#Bad
# Add three to all list members.
a = [3, 4, 5]
for i in range(len(a)):
    a[i] += 3

#Good
a = [3, 4, 5]
a = [i + 3 for i in a]
# Or
a = map(lambda i: i + 3, a)

#Use enumerate() keep a count of your place in the list
a = [3, 4, 5]
for i, item in enumerate(a):
    print i, item
```
- Read from a file
    - Use the with open syntax to read from files. This will automatically close files for you.

    ```python
    #Bad
    f = open('file.txt')
    a = f.read()
    print a
    f.close()

    #Good
    with open('file.txt') as f:
        for line in f:
            print line
    ```
    - The with statement is better because it will ensure you always close the file, even if an exception is raised inside the with block.
- Line Continuations

```python
#Bad
my_very_big_string = """For a long time I used to go to bed early. Sometimes, \
    when I had put out my candle, my eyes would close so quickly that I had not even \
    time to say “I’m going to sleep.”"""

from some.deep.module.inside.a.module import a_nice_function, another_nice_function, yet_another_nice_function

#Good
my_very_big_string = (
    "For a long time I used to go to bed early. Sometimes, "
    "when I had put out my candle, my eyes would close so quickly "
    "that I had not even time to say “I’m going to sleep.”"
)

from some.deep.module.inside.a.module import (a_nice_function, another_nice_function, yet_another_nice_function)
```
- Installation instructions are often reduced to one command: `pip install <module>` or `python setup.py install`
- Documenting Code (Comments)
    - Do not use triple-quote strings to comment code
        - This is not a good practice, because line-oriented command-line tools such as grep will not be aware that the commented code is inactive.
        - It is better to add hashes at the proper indentation level for every commented line.
    - Block Comments
        - Block comments generally apply to some (or all) code that follows them, and are indented to the same level as that code.
        - Each line of a block comment starts with a # and a single space (unless it is indented text inside the comment).
        - Paragraphs inside a block comment are separated by a line containing a single # .
    - Inline Comments
        - An inline comment is a comment on the same line as a statement.
        - Inline comments should be separated by at least two spaces from the statement. They should start with a # and a single space.



<br>

## Useful Stuff

- The `import modu` statement will look for the file modu.py in the same directory as the caller if it exists.
    - If it is not found, the Python interpreter will search for modu.py in the "path" recursively and raise an ImportError exception if it is not found.
    - Once modu.py is found, the Python interpreter will execute the module in an isolated scope.
    - Any top-level statement in modu.py will be executed, including other imports if any. Function and class definitions are stored in the module's dictionary.
- Mutable and immutable types
    - Python has two kinds of built-in or user-defined types: *Mutable* and *Immutable*
        - Mutable types are those that allow in-place modification of the content. Typical mutables are lists and dictionaries: All lists have mutating methods, like list.append() or list.pop(), and can be modified in place. The same goes for dictionaries.
        - Immutable types provide no method for changing their content. For instance, the variable x set to the integer 6 has no "increment" method. If you want to compute x + 1, you have to create another integer and give it a name.

        ```python
        my_list = [1, 2, 3]
        my_list[0] = 4
        print my_list  # [4, 2, 3] <- The same list as changed

        x = 6
        x = x + 1  # The new x is another object
        ```
- Create an ignored variable
    - If you need to assign something (for instance, in Unpacking) but will not need that variable, use \_\_

    ```python
    filename = 'foobar.txt'
    basename, __, ext = filename.rpartition('.')
    ```
- Create a length-N list of the same thing: `four_nones = [None] * 4`
- Create a length-N list of lists: `four_lists = [[] for __ in xrange(4)]`
- Create a string from a list

```python
letters = ['s', 'p', 'a', 'm']
word = ''.join(letters)
```
- Why Python is Slow: Looking Under the Hood: https://jakevdp.github.io/blog/2014/05/09/why-python-is-slow/
- What is the difference between `@staticmethod` and `@classmethod` in Python?: http://stackoverflow.com/questions/136097/what-is-the-difference-between-staticmethod-and-classmethod-in-python

<br>


## Working on a new project

```sh
# Change directory to where new project is located
$ cd <PathToYourProject>

# Install pip
$ sudo easy_install pip

# First install virtualenv
$ sudo pip install virtualenv

# Create a new virtual environment
$ virtualenv env
# NOTE: you shouldn't commit the env directory. Add it to your .gitignore file if it is not there

# Activate the environment
$ source env/bin/activate
# NOTE: You should always activate the virtual environment before you start working on your project

# If you want to output installed packages in requirements format (this will be saved to requirements.txt)
# This should only be ran if you install new libraries in the virtual environment that your project depends on
$ pip freeze > requirements.txt

# Install the dependencies into this virtual environment
$ pip install -r requirements.txt

# Finally, install the project so it is ready to be used. This will create a command line tool depending on your setup.py file
$ python setup.py install
$ python setup.py install --record files.txt  # Run this if you want to save the installed stuff for easy deletion later on

# Deactivate the virtual environment when you are done working on your project
$ deactivate


# ######### Additional commands #########
# To delete the installed  binaries and other data
$ cat files.txt | xargs rm -rf
```

<br>

### Source

- http://docs.python-guide.org/en/latest/
- http://learnpythonthehardway.org/book/index.html
- Introduction to Pip and Virtualenv: https://www.dabapps.com/blog/introduction-to-pip-and-virtualenv-python/
- Python @classmethod and @staticmethod
    - http://stackoverflow.com/questions/12179271/python-classmethod-and-staticmethod-for-beginner
    - http://stackoverflow.com/questions/136097/what-is-the-difference-between-staticmethod-and-classmethod-in-python
- What does the yield keyword do in Python?
    - http://stackoverflow.com/questions/231767/what-does-the-yield-keyword-do-in-python
    - https://jeffknupp.com/blog/2013/04/07/improve-your-python-yield-and-generators-explained/
- Iterators, generators and decorators:
    - http://pymbook.readthedocs.org/en/latest/igd.html
    - http://nvie.com/posts/iterators-vs-generators/
- [Creating a Package](https://the-hitchhikers-guide-to-packaging.readthedocs.io/en/latest/creation.html)
