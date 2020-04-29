---
title: 'Class 9: Chapter 15/16/17: Classes and Objects/Functions/Methods'
separator: '\-\-\-\-\-'
verticalSeparator: '\+\+\+\+\+'
theme: 'moon'
revealOptions:
    transition: 'fade'
---

### ITSE-1042 Intermediate Python
<span style="font-family:Helvetica Neue; font-weight:bold; color:#e49436">Class 9: Chapter 15/16/17: Classes and Objects/Functions/Methods</span>
<br /><br />

-----

##### Chapter 15: Classes and Objects

+++++

##### 15.1 Programmer-defined types

+++++

We've used many of python's built-in-types, but now we are going to make our own. As an example, we will create a type called Point that represents a point in two-dimensional space.

+++++

There are several ways we might represent points in Python:
- We could store the coordinates separately in two variables, x and y.
- We could store the coordinates as elements in a list or tuple.
- We could create a new type to represent points as objects.

Note:
In mathematical notation, points are often written in parentheses with a comma separating the coordinates. For example, (0,0) represents the origin, and (x,y) represents the point x units to the right and y units up from the origin.

+++++

Creating a new type is more complicated than the other options, but it has advantages that will be apparent soon.

```python
class Point:
    """Represents a point in 2-D space."""
```

Note:
A programmer-defined type is also called a class. A class definition looks like this:

+++++

Defining a class named Point creates a class object.

```python
class Point:
    """Represents a point in 2-D space."""

blank = Point() # <__main__.Point object at 0xb7e9d3ac>
```

Note: Creating a new object is called instantiation, and the object is an instance of the class.

+++++

##### 15.2 Attributes

You can assign values to an instance using dot notation:

```python
blank.x = 3.0
blank.y = 4.0

print('(%g, %g)' % (blank.x, blank.y))
# '(3.0, 4.0)'
distance = math.sqrt(blank.x**2 + blank.y**) # 5.0
```

Note:
This syntax is similar to the syntax for selecting a variable from a module, such as math.pi or string.whitespace. In this case, though, we are assigning values to named elements of an object. These elements are called attributes.
You can use dot notation as part of any expression.

+++++

You can pass an instance as an argument in the usual way. For example:

```python
def print_point(p):
    print('(%g, %g)' % (p.x, p.y))

print_point(blank)
# (3.0, 4.0)
```

+++++

##### 15.3  Rectangles

+++++

Imagine you are designing a class to represent rectangles. What attributes would you use to specify the location and size of a rectangle? You can ignore angle; to keep things simple, assume that the rectangle is either vertical or horizontal.

+++++

There are at least two possibilities:
- You could specify one corner of the rectangle (or the center), the width, and the height.
- You could specify two opposing corners.

Note:
At this point it is hard to say whether either is better than the other, so we’ll implement the first one, just as an example.

+++++

```python
class Rectangle:
    """Represents a rectangle. 

    attributes: width, height, corner.
    """
```

Note:
The docstring lists the attributes: width and height are numbers; corner is a Point object that specifies the lower-left corner.

+++++

To represent a rectangle, you have to instantiate a Rectangle object and assign values to the attributes:

```python
box = Rectangle()
box.width = 100.0
box.height = 200.0
box.corner = Point()
box.corner.x = 0.0
box.corner.y = 0.0
```

Note:
The expression box.corner.x means, “Go to the object box refers to and select the attribute named corner; then go to that object and select the attribute named x.”

+++++

##### 15.4 Instances as return values

+++++

Functions can return instances. Take a look at this example:

```python
def find_center(rect):
    p = Point()
    p.x = rect.corner.x + rect.width/2
    p.y = rect.corner.y + rect.height/2
    return p
center = find_center(box)
print_point(center)       # (50, 100)
```

Note:
For example, find_center takes a Rectangle as an argument and returns a Point that contains the coordinates of the center of the Rectangle:
The code at the end passes box as an argument and assigns the resulting Point to center:

+++++

##### 15.5 Objects are mutable

+++++

You can change the state of an object by making an assignment to one of its attributes.

```python
box.width = box.width + 50
box.height = box.height + 100
```

Note:
This modifies the values of width and height and changes the size of a rectangle without changing its position.

+++++

You can also write functions that modify objects.

```python
def grow_rectangle(rect, dwidth, dheight):
    rect.width += dwidth
    rect.height += dheight

box.width = 150.0
box.height = 300.0
grow_rectangle(box, 50, 100)
print(box.width, box.height)    # (200.0, 400.0)
```

Note:
Grow_rectangle takes a Rectangle object and two numbers, dwidth and dheight, and adds the numbers to the width and height of the rectangle.
The bottom is an example. Inside the function, rect is an alias for box, so when the function modifies rect, box changes.

+++++

##### 15.6 Copying

+++++

Copying an object is often an alternative to aliasing. The copy module contains a function called copy that can duplicate any object:

```python
p1 = Point()
p1.x = 3.0
p1.y = 4.0

import copy
p2 = copy.copy(p1)

print_point(p1)   # (3, 4)
print_point(p2)   # (3, 4)
p1 is p2          # False
p1 == p2          # False
```

Note:
Aliasing can make a program difficult to read because changes in one place might have unexpected effects in another place. It is hard to keep track of all the variables that might refer to a given object.
p1 and p2 contain the same data, but they are not the same Point.

+++++

If you use copy.copy to duplicate a Rectangle, you will find that it copies the Rectangle object but not the embedded Point.

```python
box2 = copy.copy(box)
box2 is box                  # False
box2.corner is box.corner    # True
```

Note:
This operation is called a shallow copy because it copies the object and any references it contains, but not the embedded objects.
For most applications, this is not what you want. In this example, invoking grow\_rectangle on one of the Rectangles would not affect the other, but invoking move\_rectangle on either would affect both! This behavior is confusing and error-prone.

+++++

Fortunately, the copy module provides a method named deepcopy that copies not only the object but also the objects it refers to, and the objects they refer to, and so on.

```python
box3 = copy.deepcopy(box)
box3 is box                   # False
box3.corner is box.corner     # False
```

Note:
box3 and box are completely separate objects.

+++++

<pre class="stretch"><code class="python" data-trim data-noescape>
#!/usr/bin/env python3

# Exercise 15.1
#
# 1. Write a definition for a class named Circle with attributes center and 
# radius, where center is a Point object and radius is a number.
#
# 2. Instantiate a Circle object that represents a circle with its center at 
# (150, 100) and radius 75.
#
# 3. Write a function named point_in_circle that takes a Circle and a Point and 
# returns True if the Point lies in or on the boundary of the circle.
#
# 4. Write a function named rect_in_circle that takes a Circle and a Rectangle 
# and returns True if the Rectangle lies entirely in or on the boundary of the 
# circle.
#
# 5. Write a function named rect_circle_overlap that takes a Circle and a 
# Rectangle and returns True if any of the corners of the Rectangle fall inside 
# the circle. Or as a more challenging version, return True if any part of the 
# Rectangle falls inside the circle.
</code></pre>

-----

##### Chapter 16: Classes and Functions

+++++

##### 16.1 Time

+++++

Let's define a class called Time that records the time of day.

```python
class Time:
    """Represents the time of day.
       
    attributes: hour, minute, second
    """
```

+++++

We can create a new Time object and assign attributes for hours, minutes, and seconds:

```python
time = Time()
time.hour = 11
time.minute = 59
time.second = 30
```

+++++

##### 16.2  Pure functions

+++++

Now we will write two functions that add time values. They demonstrate two kinds of functions: pure functions and modifiers. 

```python
def add_time(t1, t2):
    sum = Time()
    sum.hour = t1.hour + t2.hour
    sum.minute = t1.minute + t2.minute
    sum.second = t1.second + t2.second
    return sum
```

Note:
The function creates a new Time object, initializes its attributes, and returns a reference to the new object. This is called a pure function because it does not modify any of the objects passed to it as arguments and it has no effect, like displaying a value or getting user input, other than returning a value.

+++++

To test this function, we’ll create two Time objects: start contains the start time of a movie, like Monty Python and the Holy Grail, and duration contains the run time of the movie, which is one hour 35 minutes.

```python
start = Time()
start.hour = 9
start.minute = 45
start.second =  0

duration = Time()
duration.hour = 1
duration.minute = 35
duration.second = 0

done = add_time(start, duration)
print_time(done)  # 10:80:00
```

Note:
The result, 10:80:00 might not be what you were hoping for. The problem is that this function does not deal with cases where the number of seconds or minutes adds up to more than sixty. When that happens, we have to “carry” the extra seconds into the minute column or the extra minutes into the hour column.

+++++

Here’s an improved version:

```python
def add_time(t1, t2):
    sum = Time()
    sum.hour = t1.hour + t2.hour
    sum.minute = t1.minute + t2.minute
    sum.second = t1.second + t2.second

    if sum.second >= 60:
        sum.second -= 60
        sum.minute += 1

    if sum.minute >= 60:
        sum.minute -= 60
        sum.hour += 1

    return sum
```

Note:
Although this function is correct, it is starting to get big. We will see a shorter alternative later.

+++++

##### 16.3 Modifiers

+++++

Sometimes it is useful for a function to modify the objects it gets as parameters. Functions that work this way are called modifiers.

+++++

increment, which adds a given number of seconds to a Time object, can be written naturally as a modifier:

```python
def increment(time, seconds):
    time.second += seconds

    if time.second >= 60:
        time.second -= 60
        time.minute += 1

    if time.minute >= 60:
        time.minute -= 60
        time.hour += 1
```

Note:
The first line performs the basic operation; the remainder deals with the special cases we saw before.
Is this function correct? What happens if seconds is much greater than sixty?
How would we fix this?
In that case, it is not enough to carry once; we have to keep doing it until time.second is less than sixty. One solution is to replace the if statements with while statements. That would make the function correct, but not very efficient. As an exercise, write a correct version of increment that doesn’t contain any loops.

+++++

Anything that can be done with modifiers can also be done with pure functions. In fact, some programming languages only allow pure functions. There is some evidence that programs that use pure functions are faster to develop and less error-prone than programs that use modifiers. But modifiers are convenient at times, and functional programs tend to be less efficient.

+++++

<pre class="stretch"><code class="python" data-trim data-noescape>
#!/usr/bin/env python3

# Exercise 16.1
#
# 1. Write a function called mul_time that takes a Time object and a number and
# returns a new Time object that contains the product of the original Time and 
# the number.
# 
# Then use mul_time to write a function that takes a Time object that represents
# the finishing time in a race, and a number that represents the distance, and 
# returns a Time object that represents the average pace (time per mile).
#
</code></pre>

-----

##### Chapter 17: Classes and Methods

Note:
Up 'til now we have not really been doing any object oriented programming as we have not discovered the relationship with the types we created and the functions. We intend to fix that with this chapter.

+++++

##### 17.1 Object-oriented features

+++++

17.1  Object-oriented features

+++++

Python is an object-oriented programming language and has the following defining characteristics of such:

- Programs include class and method definitions.
- Most of the computation is expressed in terms of operations on objects.
- Objects often represent things in the real world, and methods often correspond to the ways things in the real world interact.

Note:
For example, the Time class defined in Chapter 16 corresponds to the way people record the time of day, and the functions we defined correspond to the kinds of things people do with times. Similarly, the Point and Rectangle classes in Chapter 15 correspond to the mathematical concepts of a point and a rectangle.

+++++

So far, we have not taken advantage of the features Python provides to support object-oriented programming. These features are not strictly necessary; most of them provide alternative syntax for things we have already done. But in many cases, the alternative is more concise and more accurately conveys the structure of the program.

Note:
For example, in Time1.py there is no obvious connection between the class definition and the function definitions that follow. With some examination, it is apparent that every function takes at least one Time object as an argument.

+++++

This observation is the motivation for methods; a method is a function that is associated with a particular class. We have seen methods for strings, lists, dictionaries and tuples. In this chapter, we will define methods for programmer-defined types.

+++++

Methods are semantically the same as functions, but there are two syntactic differences:

- Methods are defined inside a class definition in order to make the relationship between the class and the method explicit.
- The syntax for invoking a method is different from the syntax for calling a function.

Note:
In the next few sections, we will take the functions from the previous two chapters and transform them into methods. This transformation is purely mechanical; you can do it by following a sequence of steps. If you are comfortable converting from one form to another, you will be able to choose the best form for whatever you are doing.

+++++

##### 17.2 Printing objects

+++++

```python
class Time:
    """Represents the time of day."""

def print_time(time):
    print('%.2d:%.2d:%.2d' % (time.hour, time.minute, time.second))

start = Time()
start.hour = 9
start.minute = 45
start.second = 00
print_time(start)
```

Note:
In Chapter 16, we defined a class named Time and in Section 16.1, you wrote a function named print_time:
To call this function, you have to pass a Time object as an argument.

+++++

To make print_time a method, all we have to do is move the function definition inside the class definition. 
class Time:
    def print_time(time):
        print('%.2d:%.2d:%.2d' % (time.hour, time.minute, time.second))

Note:
Notice the change in indentation.

+++++

Now there are two ways to call print_time. The first (and less common) way is to use function syntax:

```python
Time.print_time(start)
```

The second way is to use the method syntax:
```python
start.print_time()
```

Note:
In the first use of dot notation, Time is the name of the class, and print_time is the name of the method. start is passed as a parameter.
The second (and more concise) way is to use method syntax. In this use of dot notation, print_time is the name of the method (again), and start is the object the method is invoked on, which is called the subject.
Inside the method, the subject is assigned to the first parameter, so in this case start is assigned to time.

+++++

By convention, the first parameter of a method is called self, so it would be more common to write print_time like this:

```python
class Time:
    def print_time(self):
        print('%.2d:%.2d:%.2d' % (self.hour, self.minute, self.second))
```

Note:
The reason for this convention is an implicit metaphor:
The syntax for a function call, print\_time(start), suggests that the function is the active agent. It says something like, “Hey print_time! Here’s an object for you to print.”
In object-oriented programming, the objects are the active agents. A method invocation like start.print_time() says “Hey start! Please print yourself.”

+++++

##### 17.3 Another example

+++++

Here’s a version of increment (from Section 16.3) rewritten as a method:

```python
class Time:
    def increment(self, seconds):
        seconds += self.time_to_int()
        return int_to_time(seconds)
        
start.print_time()              # 09:45:00
end = start.increment(1337)
end.print_time()                # 10:07:17
```

Note:
This version assumes that time\_to\_int is written as a method. Also, note that it is a pure function, not a modifier.
The subject, start, gets assigned to the first parameter, self. The argument, 1337, gets assigned to the second parameter, seconds.

+++++

##### 17.4 A more complicated example

+++++

Rewriting is_after (from Section 16.1) is slightly more complicated because it takes two Time objects as parameters.

```python
class Time:
    def is_after(self, other):
        return self.time_to_int() > other.time_to_int()

end.is_after(start)    # True
```

Note:
In this case it is conventional to name the first parameter self and the second parameter other:
To use this method, you have to invoke it on one object and pass the other as an argument

+++++

##### 17.5 The init method

The init method (short for “initialization”) is a special method that gets invoked when an object is instantiated. Its full name is \_\_init\_\_. 

```python
class Time:
    def __init__(self, hour=0, minute=0, second=0):
        self.hour = hour
        self.minute = minute
        self.second = second
```

Note:
An init method for the Time class might look like this
It is common for the parameters of __init__ to have the same names as the attributes. The statement self.hour = hour stores the value of the parameter hour as an attribute of self.
The parameters are optional, so if you call Time with no arguments, you get the default values.

+++++

```python
class Time:
    def __init__(self, hour=0, minute=0, second=0):
        self.hour = hour
        self.minute = minute
        self.second = second

time = Time()
time.print_time()      # 00:00:00
time = Time (9)
time.print_time()      # 09:00:00
time = Time(9, 45)
time.print_time()      # 09:45:00
```

+++++

17.6  The \_\_str\_\_ method

\_\_str\_\_ is a special method, like \_\_init\_\_, that is supposed to return a string representation of an object.

```python
class Time:
    def __str__(self):
        return '%.2d:%.2d:%.2d' % (self.hour, self.minute, self.second)

time = Time(9, 45)
print(time)          # 
```

Note:
For example, here is a str method for Time objects:
When you print an object, Python invokes the str method:

+++++

When you write a new class, you should start by writing \_\_init\_\_, which makes it easier to instantiate objects, and \_\_str\_\_, which is useful for debugging.

+++++

##### 17.7  Operator overloading

+++++

By defining other special methods, you can specify the behavior of operators on programmer-defined types. 

Here is what the definition might look like:

```python
class Time:
    def __add__(self, other):
        seconds = self.time_to_int() + other.time_to_int()
        return int_to_time(seconds)

start = Time(9, 45)
duration = Time(1, 35)
print(start + duration)    # 11:20:00

```

Note:
For example, if you define a method named \_\_add\_\_ for the Time class, you can use the + operator on Time objects.

+++++

##### 17.8 Type-based dispatch

+++++

In the previous section we added two Time objects, but you also might want to add an integer to a Time object.
```python
class Time:
    def __add__(self, other):
        if isinstance(other, Time):
            return self.add_time(other)
        else:
            return self.increment(other)

    def add_time(self, other):
        seconds = self.time_to_int() + other.time_to_int()
        return int_to_time(seconds)

    def increment(self, seconds):
        seconds += self.time_to_int()
        return int_to_time(seconds)

start = Time(9, 45)
duration = Time(1, 35)
print(start + duration)  # 11:20:00
print(start + 1337)      # 10:07:17
```

Note:     
The following is a version of __add__ that checks the type of other and invokes either add_time or increment.
The built-in function isinstance takes a value and a class object, and returns True if the value is an instance of the class.
If other is a Time object, __add__ invokes add_time. Otherwise it assumes that the parameter is a number and invokes increment. This operation is called a type-based dispatch because it dispatches the computation to different methods based on the type of the arguments.

+++++

Unfortunately, this implementation of addition is not commutative. If the integer is the first operand, you get

```python
print(1337 + start)   # TypeError: unsupported operand type(s) for +: 'int' and 'instance'
```

+++++

The problem is, instead of asking the Time object to add an integer, Python is asking an integer to add a Time object, and it doesn’t know how. But there is a clever solution for this problem: the special method __radd__, which stands for “right-side add”. This method is invoked when a Time object appears on the right side of the + operator. Here’s the definition:

```python
class Time:
    def __radd__(self, other):
        return self.__add__(other)
        
print(1337 + start)    # 10:07:17
```

+++++

##### 17.9 Polymorphism

+++++

Type-based dispatch is useful when it is necessary, but (fortunately) it is not always necessary. Often you can avoid it by writing functions that work correctly for arguments with different types.

+++++

Many of the functions we wrote for strings also work for other sequence types. 

```python
def histogram(s):
    d = dict()
    for c in s:
        if c not in d:
            d[c] = 1
        else:
            d[c] = d[c]+1
    return d

t = ['spam', 'egg', 'spam', 'spam', 'bacon', 'spam']

histogram(t)            # {'bacon': 1, 'egg': 1, 'spam': 4}
```

Note:
For example, in Section 11.2 we used histogram to count the number of times each letter appears in a word.
This function also works for lists, tuples, and even dictionaries, as long as the elements of s are hashable, so they can be used as keys in d.

+++++

Functions that work with several types are called polymorphic. Polymorphism can facilitate code reuse.


For example, the built-in function sum, which adds the elements of a sequence, works as long as the elements of the sequence support addition.

```python
t1 = Time(7, 43)
t2 = Time(7, 41)
t3 = Time(7, 37)
total = sum([t1, t2, t3])
print(total)              # 23:01:00
```

Note:
Since Time objects provide an add method, they work with sum:

+++++

In general, if all of the operations inside a function work with a given type, the function works with that type.

The best kind of polymorphism is the unintentional kind, where you discover that a function you already wrote can be applied to a type you never planned for.

+++++

<pre class="stretch"><code class="python" data-trim data-noescape>
#!/usr/bin/env python3

# Exercise 17.1
#
# 1. Download the code from this chapter from :
#    http://thinkpython2.com/code/Time2.py. 
# Change the attributes of Time to be a single integer representing seconds 
# since midnight. Then modify the methods (and the function int_to_time) to work
# with the new implementation. You should not have to modify the test code in 
# main. When you are done, the output should be the same as before.
#

</code></pre>

+++++

##### PyCon 2012 - Stop Writing Classes - Jack Diederich

Classes are great but they are also overused.  This talk will describe examples of class 
overuse taken from real world code and refactor the unnecessary classes, exceptions, and 
modules out of them.
Category - Education

https://youtu.be/o9pEzgHorH0
https://www.youtube.com/watch?v=o9pEzgHorH0&feature=youtu.be


+++++

Homework is 16.2/17.2

