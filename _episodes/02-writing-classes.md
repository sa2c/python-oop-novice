---
title: "Writing classes"
teaching: 20
exercises: 10
questions:
- "How are classes written in Python?"
- "What do methods look like?"
- "How can a class customise how its instances are constructed?"
objectives:
- "Write classes from scratch"
- "Write methods for classes"
- "Write custom `__init__` methods"
keypoints:
- "Classes in Python are blocks started with the `class` keyword"
- "Method definitions look like functions, but must take a `self` argument"
- "The `__init__` method is called when instances are constructed"
---

One important aim when programming is to reduce the amount of repetition as
much as practical. In many cases, using objects will help with this, since
they can carry state that would otherwise have to be passed between functions
explicitly.

For example, if we wanted to plot a variety of quadratic functions, with a
consistent set of styles, we could define a class that does this:

~~~
import matplotlib.pyplot
import numpy

class QuadraticPlotter:
    color = 'red'
    linewidth = 1
    
    def plot(self, a, b, c):
        '''Plot the line a * x ** 2 + b * x + c and output to the screen.
        x runs between -10 and 10, with 1000 intermediary points.
        The line is plotted in the colour specified by color, and with width
        linewidth.'''
        
        fig, ax = matplotlib.pyplot.subplots()
        x = numpy.linspace(-10, 10, 1000)
        ax.plot(x, a * x ** 2 + b * x + c,
                color=self.color, linewidth=self.linewidth)
        fig.show()
~~~
{: .language-python}

Similarly to how `def` is used to define a function, the `class` keyword is
used to define a new class. Both functions and variables can be created inside
the class block, and these will be accessible on objects of the class that are
created.

When functions are defined within a class, they will become methods of instances
of the class. In order for the function to be aware of the object that they need
to refer to, methods are always given the instance as their first argument. By
convention, the first argument of methods is always called `self`, so that the
object can be referred to consistently whenever it is needed.

Note that variables within methods are local to that method. For example, `fig`
and `ax` will be deleted once the method finishes running. To access variables
attached to the object, their names must be prefixed by `self.`.

> ## Other names than `self`
>
> While it is possible to use any variable name for the first argument of a
> method, and Python will not complain, other programmers will. Since one aim
> when programming is to be as clear as possible to others who may read the
> program later, we strongly recommend following the convention of calling
> the first argument to methods `self`.
{: .callout}

> ## Naming classes
>
> Another convention in Python is that class names start with a capital letter,
> and instead of underscores, initial letters of subsequent words are also
> capitalised. This makes it easier to distinguish classes from objects and
> other variables at a glance.
{: .callout}

So far this code hasn't visibly done anything; while we have defined a class,
we have yet to use it. Let's do that now.

~~~
plotter = QuadraticPlotter()
plotter.plot(1, 2, 3)
plotter.plot(1, 0, -1)
~~~
{: .language-python}

Notice that we only supply the arguments `a`, `b`, and `c` to `plotter.plot()`—
Python automatically adds the object to become the `self` parameter.

So far, this hasn't done anything that we couldn't have done with a function
to perform the setup and then do the plot. However, what if we wanted to plot
some of the curves in a thick blue line?

~~~
blue_plotter = QuadraticPlotter()
blue_plotter.color = 'blue'
blue_plotter.linewidth = 5

plotter.plot(3, -5, 5)
blue_plotter.plot(-3, 1, 0)
plotter.plot(2, 10, 2)
blue_plotter.plot(-2, 13, 4)
~~~
{: .language-python}

The two objects `plotter` and `blue_plotter` can store the different states
needed to set up the two styles of plot, whilst keeping the plotting
functionlity common, so it doesn't need to be written separately for red and
blue versions.

If we need to, we can check the values of the variables we defined:

~~~
print("Line width of red plotter is", plotter.linewidth)
print("Line width of blue plotter is", blue_plotter.linewidth)
~~~
{: .language-python}

~~~
Line width of red plotter is 1
Line width of blue plotter is 5
~~~
{: .output}

> ## Zoom in
>
> Currently `QuadraticPlotter` is hardcoded to plot between -10 and 10.
> Try adjusting it so that it can be adjusted in the same way as the `color`
> and `linewidth` can, while keeping the current defaults.
>
> Use the new class to plot the curve with `a = 3, b = 2, c = 1` both between
> -10 and 10, and between -5 and 50.
>
>> ## Solution
>>
>> ~~~
>> class QuadraticPlotter:
>>    color = 'red'
>>    linewidth = 1
>>    x_min = -10
>>    x_max = 10
>>
>>    def plot(self, a, b, c):
>>        '''Plot the line a * x ** 2 + b * x + c and output to the screen.
>>        x runs between -10 and 10, with 1000 intermediary points.
>>        The line is plotted in the colour specified by color, and with width
>>        linewidth.'''
>>
>>        fig, ax = matplotlib.pyplot.subplots()
>>        x = numpy.linspace(self.x_min, self.x_max, 1000)
>>        ax.plot(x, a * x ** 2 + b * x + c,
>>                color=self.color, linewidth=self.linewidth)
>>        fig.show()
>>
>> narrow_plot = QuadraticPlotter()
>> wide_plot = QuadraticPlotter()
>> wide_plot.x_min = -5
>> wide_plot.x_max = 50
>>
>> narrow_plot.plot(3, 2, 1)
>> wide_plot.plot(3, 2, 1)
>> ~~~
>> {: .language-python}
> {: .solution}
{: .challenge}


## Initialising instances

So far we can create an object with the defaults that we set in the class
definition, and then customise it afterwards. But wouldn't it be nice to be
able to create an object with the attributes that we want straight out of the
box?

To do this, we can define an _initialiser_ for the class. When Python creates
an instance of a class, it looks for a method called `__init__`. If it finds
one, then it calls it, giving it all the arguments passed to the class.

For example, for the `QuadraticPlotter`, the variables `color` and `linewidth`
could be passed as arguments to `__init__` and set on initialisation, rather
than being defined as part of the class definition:

~~~
class QuadraticPlotter:
    def __init__(self, color='red', linewidth=1):
        '''Set the initial attributes of this plotter.'''
        self.color = color
        self.linewidth = linewidth

    def plot(self, a, b, c):
        '''Plot the line a * x ** 2 + b * x + c and output to the screen.
        x runs between x_min and x_max, with 1000 intermediary points.
        The line is plotted in the colour specified by color, and with width
        linewidth.'''
        
        fig, ax = matplotlib.pyplot.subplots()
        x = numpy.linspace(-10, 10, 1000)
        ax.plot(x, a * x ** 2 + b * x + c,
                color=self.color, linewidth=self.linewidth)
        fig.show()

pink_plotter = QuadraticPlotter(color='magenta', linewidth=3)
pink_plotter.plot(0, 1, 0)
~~~
{: .language-python}

> ## Zoom in again
>
> Try rewriting the "Zoom in" example above to set the bounds of the plot
> using arguments to the constructor.
>
>> ## Solution
>>
>> ~~~
>> class QuadraticPlotter:
>>     def __init__(self, color='red', linewidth=1, x_min=-10, x_max=10):
>>         '''Set the initial attributes of this plotter.'''
>>         self.color = color
>>         self.linewidth = linewidth
>>         self.x_min = x_min
>>         self.x_max = x_max
>>
>>    def plot(self, a, b, c):
>>        '''Plot the line a * x ** 2 + b * x + c and output to the screen.
>>        x runs between x_min and x_max, with 1000 intermediary points.
>>        The line is plotted in the colour specified by color, and with width
>>        linewidth.'''
>>
>>        fig, ax = matplotlib.pyplot.subplots()
>>        x = numpy.linspace(self.x_min, self.x_max, 1000)
>>        ax.plot(x, a * x ** 2 + b * x + c,
>>                color=self.color, linewidth=self.linewidth)
>>        fig.show()
>>
>> narrow_plot = QuadraticPlotter()
>> wide_plot = QuadraticPlotter(x_min=-5, x_max=50)
>>
>> narrow_plot.plot(3, 2, 1)
>> wide_plot.plot(3, 2, 1)
>> ~~~
>> {: .language-python}
> {: .solution}
{: .challenge}

{% include links.md %}