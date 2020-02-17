+++
title = "List Comprehensions"
date = "2017-09-23T14:10:29Z"
description = ""
slug = ""
tags = ["Python"]
+++

List comprehensions are used for creating new list from other iterables.

As list comprehension returns list, they consists of brackets containing the expression which needs to be executed for each element along with the for loop to iterate over each element.

Basic syntax:

```
new_list = [expression for_loop_one_or_more condtions]
```

A few examples:

### Find squares of a number using for loop.
```
numbers = [1, 2, 3, 4]
squares = []

for n in numbers:
  squares.append(n**2)

print(squares)  # Output: [1, 4, 9, 16]
```

### Finding squares using list comprehensions

```
numbers = [1, 2, 3, 4]
squares = [n**2 for n in numbers]

print(squares)  # Output: [1, 4, 9, 16]
```

Here, square brackets signifies that the output is a list. n**2 is the expression that gets executed for each element and for n in numbers is used to iterate over each element. In other words, execute n**2 (expression) for each element in numbers.

### Find common numbers from two list using for loop.
```

list_a = [1, 2, 3, 4]
list_b = [2, 3, 4, 5]

common_num = []

for a in list_a:
  for b in list_b:
    if a == b:
      common_num.append(a)

print(common_num)  # Output [2, 3, 4]
```

### Find common numbers from two list using list comprehension

```

list_a = [1, 2, 3, 4]
list_b = [2, 3, 4, 5]

common_num = [a for a in list_a for b in list_b if a == b]

print(common_num) # Output: [2, 3, 4]
```

### Return numbers from the list which are not equal as tuple:

```

list_a = [1, 2, 3]
list_b = [2, 7]

different_num = [(a, b) for a in list_a for b in list_b if a != b]

print(different_num) # Output: [(1, 2), (1, 7), (2, 7), (3, 2), (3, 7)]
```

Here as we are returning a list of tuples, tuples must be in parenthesis else an error will occur. In the above example, tuple with a and b will be printed such that a and b are not same.

List comprehensions can also be used to iterate over strings as shown below:

```
list_a = ["Hello", "World", "In", "Python"]

small_list_a = [str.lower() for str in list_a]

print(small_list_a) # Output: ['hello', 'world', 'in', 'python']
```

Above example converts each of the string from *list_a* to smaller case.

Just like tuples list comprehensions can be used to produce list of list as shown below.

```
list_a = [1, 2, 3]

square_cube_list = [ [a**2, a**3] for a in list_a]

print(square_cube_list) # Output: [[1, 1], [4, 8], [9, 27]]
```
Here we use *list_a* to create *square_cube_list* which is a list of list in Python containing squares and cubes of the numbers from *list_a*
