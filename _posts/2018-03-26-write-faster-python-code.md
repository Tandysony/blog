---
layout: post
title: Write Faster Python Code
tags: [Python]
---

After I stepped into Python world, often had I been told that `Python is slow, especially the Django Framework.` The first question bump into my mind is `Slow? In terms of what?` If we are talking about _learning a new language_ and _prototyping_, this is definitely NOT true. Python is easy to use and fun to use, and can be used to [do almost anything you want](https://www.python.org/about/apps/): website development, web scraping, data science, machine learning, desktop apps/games, etc. There are tons of well-maintained third-party libraries and packages at your disposal, to speed up your development process.

Back to the topic, is Python really that slow? You might be writing slow Python code. The following are some tips on how much time you would save if writing pythonic code the right way.

> NOTE: All the following code snippets are running with Python 3.6 and Jupyter Notebook, on a 2011 13-inch MacBook Pro (2.3G Core i5, 8G 1333MHz DDR3, and macOS High Sierra).

## Tips

### 1. `list()` vs `[]`

Both are used to create a list.

```python
import timeit
%timeit list()
# 666 ns ± 9.91 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)

%timeit []
# 110 ns ± 2.97 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)
```

Apparently, list comprehension `[]` is **`6.6 times faster`** than `list()`.

Therefore, **use list comprehension `[]` to create a list**.

### 2. `dict()` vs `{}`

Both are used to create a dictionary.

```python
import timeit
%timeit dict()
# 576 ns ± 18.3 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)

%timeit {}
# 230 ns ± 20 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)
```

As can be seen, dictionary comprehension `{}` is **`2.5 times faster`** than `dict()`.

Therefore, **use dictionary comprehension `{}` to create a dictionary**.

### 3. Checking for an empty list

There are three commonly used ways to check an empty list: `if len(a_list)==0`, `if a_list==[]` and `if not a_list`. Let look at how much time each of they costs.

```python
a_list=[1,2,3]

%timeit if len(a_list)==0: pass
# 253 ns ± 4.13 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)

%timeit if a_list==[]: pass
# 155 ns ± 2.63 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)

%timeit if not a_list: pass
# 73.1 ns ± 1.78 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)
```

From the results, we can see that `if len(a_list)==0` is the slowest. `if a_list==[]` is **`63% fast`**, and `if not a_list` is **`3.5 times faster`**.

Therefore, **use `if not a_list` to check an empty list.**

### 4. Checking for `True` or `False`

There are three commonly used way to check for `True` or `False`, and I will use checking for `False` as an example: `if var==False`, `if var is False` and `if not var`. Let look at how much time each of they costs.

```python
var = False

%timeit if var==False: pass
# 158 ns ± 16.2 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)

%timeit if var is False: pass
# 143 ns ± 18.4 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)

%timeit if not var: pass
# 92.1 ns ± 8.76 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)
```

From the results, we can see that `if var==False` is the slowest. `if var is False` is **`10% fast`**, and `if not var` is **`72% faster`**.

Therefore, **use `if not var` to check for `True` or `False`.**

### 5. Filtering a list

When filtering a list, the first method will be the `filter` function in python, for most of the Python users. Is it the fastest? Let's checkout.

```python
import random
THOUSAND_NUMBERS = random.sample(range(1, 10000), 1000)

# -- method one --
oddNumList = []
def get_odd_numbers():
    for element in THOUSAND_NUMBERS:
        if element % 2:
            oddNumList.append(element)

%timeit get_odd_numbers()
# 252 µs ± 8.6 µs per loop (mean ± std. dev. of 7 runs, 1000 loops each)

# -- method two --
%timeit list(filter(lambda x: x%2, THOUSAND_NUMBERS))
# 371 µs ± 11.5 µs per loop (mean ± std. dev. of 7 runs, 1000 loops each)

# -- method three --
%timeit [item for item in THOUSAND_NUMBERS if item%2]
# 154 µs ± 8.91 µs per loop (mean ± std. dev. of 7 runs, 1000 loops each)
```

From the result, you might be surprised: The `filter` function is the slowest, even slower than the `for` loop version. List comprehension, as we have illustrated earlier, is the fastest. And it is **`2.4 times faster`** then the `filter` function.

Therefore, **when filtering a list, try the list comprehension way, rather than the `filter` and `for` loop, as it is much faster and syntactically beautiful.**

### 6. Testing Membership: set vs list

Assume that we have a huge amount of data available, and we are going to further test its membership. We can make a list to hold the data, like the `THOUSAND_NUMBERS` in the previous example. Here are some test results.

```python
import random
THOUSAND_NUMBERS = random.sample(range(1, 10000), 1000)
MILLION_NUMBERS = random.sample(range(0, 1000001), 1000000)

%timeit if 22 in THOUSAND_NUMBERS: pass
# 38.7 µs ± 4.42 µs per loop (mean ± std. dev. of 7 runs, 10000 loops each)

%timeit if 22 in MILLION_NUMBERS: pass
# 82.9 ms ± 13.3 ms per loop (mean ± std. dev. of 7 runs, 10 loops each)
```

Apparently, it takes significantly much more time if the list grow (`38.7 µs` vs `82.9 ms`, **`µs` vs `ms`** here), with a complexity of `O(n)`. Look at the following test with set:

```python
# convert the list to a set
MILLION_SET = set(MILLION_NUMBERS)

%timeit if 22 in MILLION_SET: pass
# 138 ns ± 1.81 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)

# converting takes time as well
%timeit set(MILLION_NUMBERS)
# 227 ms ± 11.5 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
```

From the results, testing membership in a set **destroys** that of a list: `138 ns` vs `82.9 ms`, `600,000 times faster`. Please do not forget that, covering a list to a set takes much time.

If we have data initialized as a **set** rather than a **list**, we can avoid converting to save us much time in testing membership. Please keep in mind, **elements in a set are unique**. If your data have duplicates and the duplication matters, please do NOT use set.

Other than list and set, dictionary is an option as well. It is a hash table-bash data structure. This makes adding, retrieving and updating elements very fast. It is not the concern here.

Therefore, **if data duplication does not matter for you, use set for membership testing, otherwise, use list.**

### 6. Sorting a list

There are two ways to sort a list: the built-in `.sort()` function of the list object, or the `sorted()` method in Python. Let's see how they perform:

```python
%timeit sorted(MILLION_NUMBERS)
# 1.31 s ± 22.7 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)

%timeit MILLION_NUMBERS.sort()
# 39.6 ms ± 4.99 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
```

Apparently, the built-in `.sort()` function is `33.1 times faster`.\

Therefore, **when sorting a list, use its built-in `.sort()` function.**

## Summary

Those tips help you writing cleaner and faster python code. There are other approaches and tools to optimize performance for very specific topics, but beyond the topic of this subject. For example, we can use `prefetch_related`, `select_related` and `Django Debug Toolbar` for optimize a Django app. I will write another blog in this regard later.
