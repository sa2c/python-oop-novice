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

We can do this because `numbers` is an _object_, and in this particular case it
provides the `mean` method. Let's investigate this further by using `type`
to identify what the data type of `numbers` is:

~~~
type(numbers)
~~~
{: .language-python}

~~~
<class 'numpy.ndarray'>
~~~
{: .output}

We can see here that `numbers` is an object of the _class_ `numpy.ndarray`.

The distinction between a class and an object is subtle but important. A class
is a description or specification of what an object will look like, how to
create it, and what can be done with it. An object is a specific _instance_ of
what the class describes. To use a real-world example, a design specifying
how a chair can be constructed is equivalent to a class, while the chairs
made to that blueprint are objects. The chairs may be different colours, or
different widths, or have different feet to match the flooring they are used
on, as allowed by the design of the chair. Similarly, more often that not
objects that are instances of the same class will be different, but all will
share the common elements that the class defines.

Classes can (and, in general, should) provide functions that operate on
instances of the class. When a function is part of a class, we call it a
_method_ of the class. The distinction is that a method is aware of the object
to which it relates, whereas a function is not. For example, when we used
the `numbers.mean()` method above, we did not need to pass `numbers` as an
argument, since `mean()` already knew that it was attached to the `numbers`
object.

We can check whether an object is of a particular class with the `isinstance`
function:

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

