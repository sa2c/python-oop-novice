---
title: "Objects in Python"
teaching: 20
exercises: 15
questions:
- "What are objects in Python?"
- "What is a class or type?"
- "Can objects belong to more than one class?"
- "How can objects be created from a class?"
objectives:
- "Be able to distinguish between class and object"
- "Be able to construct objects via a class's constructor"
- "Be able to distinguish between equality and identity of objects"
keypoints:
- "Anything that we can store in a variable in Python is an object"
- "Every object in Python has a class (or type)"
- "`list` and `numpy.ndarray` are commonly-used classes; lists and arrays are corresponding objects"
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

Let's see if we can do this with a normal list:
~~~
more_numbers = [1, 2, 3, 4]
print(more_numbers.mean())
~~~
{: .language-python}

In this case Python will complain with an error. How does Python know it can do this for `numbers` but not `more_numbers`?

## What type is it?
Let's investigate this further by using `type` to identify what the data type of `numbers` is:

~~~
type(numbers)
~~~
{: .language-python}

~~~
<class 'numpy.ndarray'>
~~~
{: .output}

What about the type of the variable `more_numbers`?

~~~
type(more_numbers)
~~~
{: .language-python}

~~~
<class 'list'>
~~~
{: .output}
We can see here that `numbers` is an object of the type `numpy.ndarray`. In Python, anything which can be stored in a variable or passed to a function is called an _object_. Objects are classified by their `type`, or their `class`.

> ## Class or Type?
>
> Note that in literature, you'll find a subtle
> distinction between _class_ and _type_. However, since in Python 3
> we can't have one without the other, we will use both terms interchangably.
{: .callout}

> ## Let's find some types
>
> Can you find anything that you can store in a variable which
> does not have a class? What is the type of the number `1`,
> or the string `"hello"`?
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
>> <class 'int'>
>> ~~~
>> {: .output}
>>
>> ~~~
>> type("hello")
>> ~~~
>> {: .language-python}
>>
>> ~~~
>> <class 'string'>
>> ~~~
>> {: .output}
> {: .solution}
{: .challenge}

> ## Does everything have a class?
>
> Try to find words that Python recognises that do not have classes. What about `numpy.mean` or `numpy`?
> What about `if` or `for`? Can you think of others?
>
>> ## Solution
>>
>> ~~~
>> type(numpy.mean)
>> ~~~
>> {: .language-python}
>>
>> ~~~
>> <class 'function'>
>> ~~~
>> {: .output}
>>
>> ~~~
>> type(numpy)
>> ~~~
>> {: .language-python}
>>
>> ~~~
>> <class 'module'>
>> ~~~
>> {: .output}
>> The objects `numpy.mean` and `numpy` are things that we typically wouldn't store in variables or passed around. However, they could in principle be stored in variables, and therefore are objects with a class.
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

In Python, there are two ways in which objects can behave. The most intuitive case is when object are created with a value, and they keep the value forever. Many objects we're familiar with, such as integers or strings, are value objects.

Let's store a string in a variable:
~~~
string_one = "hello"
~~~
{: .language-python}

The variable `string_one` now refers to an object, which has the value `"hello"`. We can point another variable at the same object with:
~~~
string_two = string_one
~~~
{: .language-python}

But, we can never change the value of the string object itself. The string "hello" will always be the string "hello". We can set the variable `string_two` to a new object, with
~~~
string_two = string_one + ", world"
~~~
{: .language-python}

But the original object is still there, unchanged. We can still get to it by typing
~~~
print(string_one)
~~~
{: .language-python}

This may not seem surprising, but not all objects in Python behave this way. Consider the following list of strings:
~~~
maybe = ["We", "probably", "can't", "change", "this"]
~~~
{: .language-python}

Let's assign `maybe` to a new variable, `same_maybe`:
~~~
same_maybe = maybe
~~~
{: .language-python}

Think of this as pointing `same_maybe` at the _same object_ contained in `maybe`: Now let's change a part of `same_maybe`:
~~~
same_maybe[2] = "can"
~~~
{: .language-python}

What is the value of `maybe` now?
~~~
print(maybe)
~~~
{: .language-python}

~~~
['We', 'probably', 'can', 'change', 'this']
~~~
{: .output}

Note how we changed `maybe` through the variable `same_maybe`. We can do this because both `maybe` and `same_maybe` refer to the same underlying object, and that underlying object can be changed.

We say that objects which can't be changed, like numbers and string, are _immutable_. Numbers are an intuitive example of immutable objects, the number 1000 will always be the number 1000. We say that these objects that can be changed are _mutable_, they can be "mutated" after they've created.

> ## Immutable lists
>
> Python has a class similar to a list called a `tuple`. Is a `tuple` mutable or immutable?
>
> Check if you can change a tuple by setting:
>
> ~~~
> maybe = ("Maybe", "we", "can't", "change", "this?")
> ~~~
> {: .language-python}
>
> and trying to modify the 3rd element with:
> ~~~
> maybe[2] = "can"
> ~~~
> {: .language-python}
>
>> ## solution
>>
>> ~~~
>> maybe = ("Maybe", "we", "can't", "change", "this?")
>> maybe[2] = "can"
>> ~~~
>> {: .language-python}
>>
>> You should see an error containing the text:
>> ~~~
>> TypeError: 'tuple' object does not support item assignment
>> ~~~
>> {: .output}
>>
>> This is telling you that you can't modify the tuple object, this is true because the tuple object is immutable.
>> {: .output}
> {: .solution}
{: .challenge}

> ## What kind of objects?
>
> List some objects that you think are mutable and immutable. Verify this by trying to find ways to change the objects.
>
> Note: Be careful that you're not "cheating" by using `=` to point to a new object.
{: .challenge}

# Instances and Methods

We can check if an object is an _instance of_ a particular class with the `isinstance` function.

~~~
isinstance(numbers, numpy.ndarray)
~~~
{: .language-python}

~~~
True
~~~
{: .output}

Every object is created with a single class, and this can't be changed.

Any object can have functions associated with it, which can be called by using a dot after the variable name, for example:
~~~
numbers.mean()
~~~
{: .language-python}

The functions which are associated with an object are provided by the class of the object. When a class provides a function to an object we call that function a _method_ of that class.

We say that the `numpy.ndarray` class provides the `mean` _method_. Since `numbers` belongs to the class `numpy.ndarray`, we can use the `mean` method on the object referred to by `numbers`, by calling `numbers.mean()`. This allows objects of a `numpy.ndarray` to provide functionality specific to objects of class `numpy.ndarray`.

Immutable objects can have methods too:
~~~
hello = "hello, world"
print(hello.capitalize())
~~~
{: .language-python}

> ## Changing places
>
> It's worth noting that both mutable and immutable objects can have methods. Methods of immutable objects however can't change
> the underlying object.  They always return a brand new object, and set the expected value in the new object.
> Methods of mutable object can (and often do) change the class.
>
> Methods on mutable object, however, will typically change the object itself.
{: .callout}

We then say that particular objects are _instances_ of the class. To use a real world example, a chair is a particular type or class of object. The chair that you are sitting on right now is a specific _instance_ of the class of all chairs.

> ## finding out what things are
>
> use `type()` to find the type of `students`, defined as
>
> ~~~
> students = ['petra', 'aalia', 'faizan', 'shona']
> ~~~
> {: .language-python}
>
> check this with `isinstance`
>
>> ## solution
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
> You can check the type of the object you've created with `isinstance`
>
> Hint: `zip()` can be used to turn two lists into tuples of corresponding
> pairs of elements.
>
>> ## Solution
>>
>> ~~~
>> student_grades = dict(zip(students, grades))
>> isinstance(student_grades)
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

