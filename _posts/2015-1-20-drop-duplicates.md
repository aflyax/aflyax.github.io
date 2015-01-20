---
layout: post
title: Dropping duplicates in Python and Julia
permalink: drop-duplicates
comments: true
tags: [julia, python, arrays, sorting, duplicates]
---
In Python, if we try to remove duplicates from a list, we can convert the list to a set and then back to a list:

``` python
a = rand(15)*10
a = a.astype(int)
print(a)
list(set(a))
```
Out:

```
[3 5 6 3 7 6 9 4 4 4 1 7 3 2 1]
[1, 2, 3, 4, 5, 6, 7, 9]
```

What if I don't want to change the order? From this Stack Overflow [response](http://stackoverflow.com/questions/480214/how-do-you-remove-duplicates-from-a-list-in-python-whilst-preserving-order):

``` python
a = rand(15)*10
a = a.astype(int)
print(a)

def foo(seq):
    seen = set()
    seen_add = seen.add
    return [ x for x in seq if not (x in seen or seen_add(x))]

foo(a)
```
Out:

```
[2 8 1 9 1 0 5 6 7 6 6 9 3 6 3]
[2, 8, 1, 9, 0, 5, 6, 7, 3]
```

In Julia, it's a little more straightforward. You can use `union(var)` to drop duplicates and `sort(union(var))` to sort the result. `union(a,b)` finds the union between two variables and can be used to drop duplicates between them.

``` julia
a = int(rand(10)*15)

println("a: \t\t", a)
println("union: \t\t", union(a))
println("sort union: \t", sort(union(a)))

b = int(rand(10)*15)
println("b: \t\t", b)
println("common(a,b): \t", sort(union(a,b)))
```
Out:

```
a: 	 	     [3,9,9,8,0,5,5,1,13,3]
union: 	 	 [3,9,8,0,5,1,13]
sort union:  [0,1,3,5,8,9,13]
b: 		     [3,13,12,14,12,1,4,6,14,2]
common(a,b): [0,1,2,3,4,5,6,8,9,12,13,14]
```
