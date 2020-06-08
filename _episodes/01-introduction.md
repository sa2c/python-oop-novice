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

In Python, anything which can be stored in a variable or passed to a function is called an _object_. Objects are classified by their `type`, or their `class`.

> ## Class vs Type
>
> Note that in literature, you'll find a subtle
> distinction between `class` and `type`. But, since in Python
> we can't have one without the other, we will use the term class
> interchangably.
{: .callout}

> ## Let's find some types
>
> Can you find anything that you can store in a variable which
> does not have a class? What is the type of the number `1`,
> or the string `"hello"?
>
> Does the class change if they are passed directly to `type`,
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

> ## Does everything have a class?
>
> Try to find words that python recognises that do not have classes. What about `numpy.mean` or `numpy`?
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
>> The words `if` and `for` are part of the Python language itself, they can't be stored in variables. Only things which can be stored in variables can have a class.
> {: .solution}
{: .challenge}

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

You're probably not surprised by this, but there is something very fundamental going on here.

There is absolutely nothing we can do which will change the value of the underlying object that is referred to by the variable `a`. This kind of object is called _immutable_, it can't be changed (or _mutated_) once it has been created. It is created with a given value, and it keeps that same value forever.

> ## Assignment doesn't count
>
> At first glance, it might seem that we can change the value of the object which `a` points to with `a = 2`.
>
> In practice however, this is actually pointing `a` to a new object, and the original object remains unchanged.
{: .callout}

Not all objects in Python are immutable. Consider the following list of strings:
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
{: .output}

Note how we changed `a` through the variable `b`. We can do this because both `a` and `b` refer to the same underlying object.

Importantly, the underlying object can be changed. And the value referenced by both variables also changes. We call these objects _mutable_. The can be "mutated" after they're created.

All objects in Python are classified as mutable or immutable.

# Methods

We can check if an object is an _instance of_ a particular class with the `isinstance` function.

~~~
isinstance(numbers, numpy.ndarray)
~~~
{: .language-python}

~~~
True
~~~
{: .output}

Every object is created with a single class, which cannot be changed.

Objects can have functions associated with them. The functions associated with an object are determined by the class of the object.

For example, as above
~~~
numbers.mean()
~~~
{: .language-python}

The `numbers` object has a `mean` function provided by the `numpy.ndarray` class. This allows objects of a `numpy.ndarray` to provide functionality specific to that type of object.

Mutable objects can have methods too:
~~~
hello = "hello, world"
print(hello.capitalize())
~~~
{: .language-python}

When a function is associated to an object we call it a _method_ of the object, and we call it by using a dot, for example: `object_name.method_name()`

> ## Changing places
>
> It's worth noting that methods of immutable objects can't change
> the underlying object. They always return a brand new object with the new value. We can see this in action by looking at the variable `hello` after capitalisation above. The value of the object has not been changed. 
>
> Methods on mutable object, however, will typically change the object itself.
{. :callout}

We then say that particular objects are _instances_ of the class. To use a real world example, a chair is a particular type or class of object. The chair that you are sitting on right now is a specific _instance_ of the class of all chairs.

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
the same.

The second is _identity_, or whether the objects are in fact the
same instance, with names referring to the same underlying object.

Equality is tested with `==`, which you have probably used before. We can test for identity with the `is` keyword:

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

Object-oriented programming allows relationships to be defined between classes or types.
One class may be considered to be a specialisation or _subclass_ of another.
For a real world example, a car could be considered a specialisation or subclass of the class of all vehicles.

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

