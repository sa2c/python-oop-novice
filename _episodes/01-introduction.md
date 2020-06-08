---
title: "Classes and Objects"
teaching: 20
exercises: 15
questions:
- "Why is object oriented programming useful?"
- "Where are classes and objects encountered in Python?"
- "How can objects be created from a class?"
- "How can objects correspond to more than one class?"
objectives:
- "Be able to distinguish between classes and objects"
- "Be able to construct objects via a class's constructor"
- "Be able to distinguish between equality and identity of objects"
keypoints:
- "Object oriented programming allows related sets of data and functionality to be grouped together"
- "`list` and `numpy.ndarray` are very commonly-used classes; lists and arrays are corresponding objects"
- "Calling the class as a function constructs new objects of that class"
- "Classes can inherit from other classes; objects of the subclass are automatically also of the parent class"
---

You may recall that we calculated the mean of a Numpy array by using
`numpy.mean`, as

~~~
import numpy
numbers = numpy.arange(10)
print(numpy.mean(numbers))
~~~
{: .language-python}

~~~
4.5
~~~
{: .output}

However, we can also calculate the mean of a Numpy array as:

~~~
print(numbers.mean())
~~~
{: .language-python}

~~~
4.5
~~~
{: .output}

How does Python know it can do this?

Let's investigate this further by using `type`
to identify what the data type of `numbers` is:

~~~
type(numbers)
~~~
{: .language-python}

~~~
<class 'numpy.ndarray'>
~~~
{: .output}

We can see here that `numbers` is an object of the type `numpy.ndarray`.

~~~
type(numbers)
~~~
{: .language-python}

~~~
<class 'int'>
~~~
{: .output}

In Python, anything which can be stored in a variable or passed to a function is called an _object_. There are many types of objects. Objects are classified by their `type`, or their `class`.

> Note that in Object Oriented literature, you'll find a subtle
> distinction between `class` and `type`. But, since the Python
> implementation does not distinguish, we use these terms
> interchangably.
{: .callout}

# Ch-Ch-Changes

In python, there are two ways in which objects can behave.

Some objects, like integers, string or tuples, are created with one value, and they then keep the same value forever.

Let's create a variable:
~~~
a = "hello"
~~~
{: .language-python}

And add one to that variable, and store the result in b:
~~~
b = a + " world"
~~~
{: .language-python}

The value of `a` remains unchanged.
~~~
print(a)
~~~
{: .language-python}

~~~
"hello"
~~~
{: .output}

You're probably not surprised by this, but there is something more fundamental going on here.

There is absolutely nothing we can do which will change the value of the underlying _object_ that is stored in `a`. This kind of object is called _immutable_, it can't be _mutated_. It is created with a value, and it keeps that value forever.

> There is one way that we can change the value of the variable `a`, and that is by setting it a new value, for example `a = 2`.
>
> Note however that this is creating a new object, with a new value (2 in this case) and pointing `a` at that object.
{: .callout}

The other kind of object we come across in python is are _mutable_ objects. These can be changed, or mutated, after they've been created.

Keep the previous example in mind as you do with the following:
~~~
a = ["We", "probably", "can't", "change", "this"]
~~~
{: .language-python}

Let's assign `a` to a new variable, this essentially points `b` at the object created by `a`:
~~~
b = a
~~~
{: .language-python}

Let's now change `b`:
~~~
b[2] = "can"
~~~
{: .language-python}

What is the value of `a` now?
~~~
print(a)
~~~
{: .language-python}
~~~
['We', 'probably', 'can', 'change', 'this']
~~~

Note how we changed `a` by only touching `b`, because they refer to the same underlying object.

> ## Let's find some types
>
> Can you find anything that you can store in a variable which
> does not have a `type`? What is the type of the number `1`,
> or the string `"hello"?
>
> Does the type change if they are passed directly to `type`,
> or if they are stored in a variable?
>
>> ## Solution
>>
>> ~~~
>> type(1)
>> ~~~
>> {: .language-python}
>>
>> ~~~
>> <type 'int'>
>> ~~~
>> {: .output}
>>
>> ~~~
>> type([1,2,3])
>> ~~~
>> {: .language-python}
>>
>> ~~~
>> <type 'list'>
>> ~~~
>> {: .output}
> {: .solution}
{: .challenge}

> ## Does everything have a type?
>
> Try to find words that python recognises that are not types. What about `numpy.mean` or `numpy`?
>
> What about `if` or `for`?
>
>> ## Solution
>>
>> ~~~
>> type(numpy.mean)
>> ~~~
>> {: .language-python}
>>
>> ~~~
>> <type 'function'>
>> ~~~
>> {: .output}
>>
>> ~~~
>> type(numpy)
>> ~~~
>> {: .language-python}
>>
>> ~~~
>> <type 'module'>
>> ~~~
>> {: .output}
>> ~~~
>> type(if)
>> ~~~
>> {: .language-python}
>>
>> ~~~
>>  File "<stdin>", line 1
>>    type(if)
>>          ^
>>SyntaxError: invalid syntax
>> ~~~
>> {: .output}
>> ~~~
>> type(for)
>> ~~~
>> {: .language-python}
>>
>> ~~~
>>  File "<stdin>", line 1
>>    type(for)
>>          ^
>>SyntaxError: invalid syntax
>> ~~~
>> {: .output}
>> The words `if` and `for` are part of the language, they can't be stored in variables. And only things which can be stored in variables have a type.
> {: .solution}
{: .challenge}

# Classes

You may have noted that the type of some objects had the word _class_ in front of it, for example:
~~~
type(numbers)
~~~
{: .language-python}

~~~
True
~~~
{: .output}


We can check whether an object belong to a particular type with the `isinstance` function. This may seem like a confusing name for the function, but the name will become :

~~~
isinstance(students, numpy.ndarray)
~~~
{: .language-python}

~~~
True
~~~
{: .output}

> ## Finding out what things are
>
> Use `type()` to find the type of `students`, defined as
>
> ~~~
> students = ['Petra', 'Aalia', 'Faizan', 'Shona']
> ~~~
> {: .language-python}
>
> Check this with `isinstance`
>
>> ## Solution
>>
>> ~~~
>> type(students)
>> ~~~
>> {: .language-python}
>>
>> ~~~
>> <class 'list'>
>> ~~~
>> {: .output}
>>
>> ~~~
>> isinstance(students, list)
>> ~~~
>> {: .language-python}
>>
>> ~~~
>> True
>> ~~~
>> {: .output}
> {: .solution}
{: .challenge}

> ## Other common classes
>
> What other classes have you encountered previously when using Python?
> What methods did they provide?
{: .challenge}

## Making an object

A class can be called as a function, in which case it constructs new instances
of itself. While this is not the only way to make objects, it is one that all
classes offer. For example, a new list can be created as:

~~~
students = list()
print(students)
print(type(students))
~~~
{: .language-python}

~~~
[]
<class 'list'>
~~~
{: .output}

> ## Making a Numpy array
>
> While all classes can be constructed by calling their name, some classes
> don't recommend this route. For example, `numpy.ndarray` is used internally
> by Numpy to initialise its arrays, but Numpy recommends using one of the
> higher-level functions like `numpy.zeros`, `numpy.ones`, `numpy.empty`, or
> `numpy.asarray` to construct an array (of zeroes, of ones, without
> initialising the data, and initialising from an existing data structure like
> a list, respectively).
{: .callout}

> ## Make a dict
>
> Given the following list of students and their grades, how would you
> construct a `dict` with students as keys, and grades as values?
>
> ~~~
> students = ['Petra', 'Aalia', 'Faizan', 'Shona']
> grades = [84, 78, 91, 66]
> ~~~
> {: .language-python}
>
> Hint: `zip()` can be used to turn two lists into tuples of corresponding
> pairs of elements.
>
>> ## Solution
>>
>> ~~~
>> student_grades = dict(zip(students, grades))
>> ~~~
>> {: .language-python}
> {: .solution}
{: .challenge}

## Equality and identity

Python has two ways of testing whether two objects are the "same". The first
is _equality_, or whether the associated values or contents of the object are
the same. The second is _identity_, or whether the objects are in fact the
same instance, with names referring to the same space in Python's memory.

Equality is tested with `==`, which you have probably used before. We can test
for identity with the `is` keyword:

~~~
old_students = students
new_students = ['Petra', 'Aalia', 'Faizan', 'Shona']

if old_students == students:
    print("old_students is equal to the students list")
if new_students == students:
    print("new_students is equal to the students list")
if old_students is students:
    print("old_students is identical to the students list")
if new_students is students:
    print("new_students is identical to the students list")
~~~
{: .language-python}

Constructing a new list that has the same elements as an existing list
gives a list that is equal, but not identical, to the existing one. This is
true for any class: constructing a new object that is the same as an existing
one will give a result that is equal, but not identical, to the existing one.


## Inheritance

Object-oriented programming allows relationships to be defined between classes.
One class may be considered to be a specialisation or _subclass_ of another.
This is very frequently seen in the way Python handles exceptions. For example,
if we check what type a `ValueError` is, we see that it is of
`class 'ValueError'`:

~~~
an_error = ValueError("A value must be provided")
print(type(an_error))
if isinstance(an_error, ValueError):
    print("an_error is a ValueError")
~~~
{: .language-python}

~~~
<class 'ValueError'>
an_error is a ValueError
~~~
{: .output}

However, we can also check if it is an `Exception`:

~~~
if isinstance(an_error, Exception):
    print("an_error is an Exception")
~~~
{: .language-python}

~~~
an_error is an Exception
~~~
{: .output}

This is because `ValueError` is a subclass of `Exception`: value errors are a
specific type of exception that can occur, and so should have all the same
logic that is common to all exceptions.

One place this can be used is to structure exception handling; for example:

~~~
numerator = 5
denominator = 0

try:
    print(numerator, "divided by", denominator, "is", numerator / denominator)
except ZeroDivisionError:
    print("You can't divide by zero!")
except Exception:
    print("Something else went wrong.")
~~~
{: .language-python}

`ZeroDivisionError` is another subclass of `Exception`. On encountering an
exception, Python checks each `except` in turn to see whether the exception
matches the class being tested for. The more specific `ZeroDivisionError`
catches the specific case of dividing by zero, but the block is skipped for
all other issues, which are then handled by the more general `Exception`.

{% include links.md %}

