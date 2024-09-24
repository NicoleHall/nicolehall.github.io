---
layout: post
title: Finding Prime Numbers with Python
image: "/posts/coach_prime_numbers.jpg"  
tags: [Python, Primes]
---

I am learning Python. This post is about a function that will find prime numbers between 0 and n. If the 100 was passed to the function, it would find all the prime numbers below 100.

A Prime Number is a number that can only be divided wholly by itself and one. 7 is a prime number because no other numbers apart from 7 or 1 divide cleanly into it. A prime number has exactly two divisors; 1 and itself. 

---

The first step is to create a variable that will act as the upper limit of the range of numbers. If n is set to 20, the program will find all prime numbers that are equal to or smaller than 20

```ruby
n = 20
```

The smallest true Prime number is 2, therefore the program will check every integer between 2 and n to determine if it is prime. Python has a .range() method. The first argument is the beginning of the range and the second argument is the upper limit. Python's range logic is not inclusive of the upper. To include the upper limit, n should be increased by 1. This will instruct the program to create a range that is 2 through 21. 20 is included. 21 is excluded.

Python has choices for data structures; List, Set, Tuple and Dictionary. The best option for this program is to use a set. I will use set's .difference() method to compare two sets in a later step

```ruby
number_range = set(range(2, n+1))
```

A list data structure can be used to hold the prime numbers as they are discovered as the program loops through each integer between 2 and 20.

```ruby
primes_list = []
```

Before implementing the looping logic, it is valuable to consider the needed logic that should execute in each iteration of the loop. Set has a .pop() method. This method will remove the first number from the number_range set. The first number from the number_range set is 2. This will be the first number in the range that will be either identified as prime or not prime. If it is prime, it will be added to the list called primes_list. If it is not a prime, it will remain in the number_range set and be [garbage collected](https://stackify.com/python-garbage-collection/) by Python.

```ruby
print(number_range)
>>> {2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20}
```
If we use pop, and assign this to the object called **prime** it will remove, or pop, the first element from the set called **number_range**, and place it into the variable called **prime**

```ruby
prime = number_range.pop()
print(prime)
>>> 2
print(number_range)
>>> {3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20}
```

The first value in the range, which is 2, is already known to be a prime number. This is becuase there is no smaller number that 2 could divide evenly into it. Given that it is confirmed to be a prime, it is added to the list of primes.

```ruby
primes_list.append(prime)
print(primes_list)
>>> [2]
```

A clever implementation will now be used to check the remaining number range for non-primes. For the prime number just checked (in this first case, the number 2), all its multiples will be generated up to the upper range (in this example, 20). 

Once again, a set data structure will be used instead of a list data structure to hold the range of multiples. In Python, a set has function to compare two sets, which will be implemented in a later step. 

```ruby
multiples = set(range(prime*2, n+1, prime))
```

It's important to remember that the syntax for creating a range is `range(start, stop, step)`. For the starting point, there's no need to include the original number, as it has already been identified as a prime. Instead, the range of multiples should begin at 2 times the given number, which represents the first multiple. For example, if the number is 2, the first multiple will be 4. If the number being checked is 3, the first multiple would be 6 and so on.

The range should stop at 20, but it needs to include 20. n+1 is used so that 20 will be included. The default functionality of .range() is to exclude the number provided in the `stop` argument.

To generate multiples of the given number, the range should increment in steps equal to that number, allowing it to capture each multiple effectively. Therefore, the step value should be set to the prime number itself.

The set of multiples looks like this:

```ruby
print(multiples)
>>> {4, 6, 8, 10, 12, 14, 16, 18, 20}
```

The comparison function available to objects of the set type that was mentioned earlier is called **difference_update**. This function removes any values from the number range that are multiples of the number we just checked. The reason for doing this is because if a number is a multiple of anything other than 1 or itself then it is **not a prime number** and can be removed from the `number_range` set.

Here are the two sets. If a number appears in the `mulitples` set, it should be removed from the `number_range` set.

```ruby
print(number_range)
>>> {3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20}

print(multiples)
>>> {4, 6, 8, 10, 12, 14, 16, 18, 20}
```

**difference_update** function will modify the `number_range` set to exclude a value that also appears in the `multiples` set.

```ruby
number_range.difference_update(multiples)
print(number_range)
>>> {3, 5, 7, 9, 11, 13, 15, 17, 19}
```
The `number_range` set now excludes values that were present in the `multiples` set. The values in the `multiples` set are necessarily not prime numbers because they have   three divisors: 1, themselves and the number that divides them evenly.

When examining the modified `number_range`, all values that were also present in the multiples set have been removed, because the necessarily are not prime.
```ruby
number_range >>> {3, 5, 7, 9, 11, 13, 15, 17, 19}
```
 
This is amazing!  We've made a massive reduction to the pool of numbers that need to be tested so this is really efficient. It also means the smallest number in our range *is a prime number* as we know nothing smaller than it divides into it...and this means we can run all that logic again from the top!

Whenever you can run sometime over and over again, a while loop is often a good solution.

Here is the code, with a while loop doing the hard work of updated the number list and extracting primes until the list is empty.

Let's run it for any primes below 1000...

```ruby
n = 1000

# number range to be checked
number_range = set(range(2, n+1))

# empty list to append discovered primes to
primes_list = []

# iterate until list is empty
while number_range:
    prime = number_range.pop()
    primes_list.append(prime)
    multiples = set(range(prime*2, n+1, prime))
    number_range.difference_update(multiples)
```

Let's print the primes_list to have a look at what we found!

```ruby
print(primes_list)
>>> [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101, 103, 107, 109, 113, 127, 131, 137, 139, 149, 151, 157, 163, 167, 173, 179, 181, 191, 193, 197, 199, 211, 223, 227, 229, 233, 239, 241, 251, 257, 263, 269, 271, 277, 281, 283, 293, 307, 311, 313, 317, 331, 337, 347, 349, 353, 359, 367, 373, 379, 383, 389, 397, 401, 409, 419, 421, 431, 433, 439, 443, 449, 457, 461, 463, 467, 479, 487, 491, 499, 503, 509, 521, 523, 541, 547, 557, 563, 569, 571, 577, 587, 593, 599, 601, 607, 613, 617, 619, 631, 641, 643, 647, 653, 659, 661, 673, 677, 683, 691, 701, 709, 719, 727, 733, 739, 743, 751, 757, 761, 769, 773, 787, 797, 809, 811, 821, 823, 827, 829, 839, 853, 857, 859, 863, 877, 881, 883, 887, 907, 911, 919, 929, 937, 941, 947, 953, 967, 971, 977, 983, 991, 997]
```

Let's now get some interesting stats from our list which we can use to summarise our findings, the number of primes that were found, and the largest prime in the list!

```ruby
prime_count = len(primes_list)
largest_prime = max(primes_list)
print(f"There are {prime_count} prime numbers between 1 and {n}, the largest of which is {largest_prime}")
>>> There are 168 prime numbers between 1 and 1000, the largest of which is 997
```

Amazing!

The next thing to do would be to put it into a neat function, which you can see below:

```ruby
def primes_finder(n):
    
    # number range to be checked
    number_range = set(range(2, n+1))

    # empty list to append discovered primes to
    primes_list = []

    # iterate until list is empty
    while number_range:
        prime = number_range.pop()
        primes_list.append(prime)
        multiples = set(range(prime*2, n+1, prime))
        number_range.difference_update(multiples)
        
    prime_count = len(primes_list)
    largest_prime = max(primes_list)
    print(f"There are {prime_count} prime numbers between 1 and {n}, the largest of which is {largest_prime}")
```

Now we can jut pass the function the upper bound of our search and it will do the rest!

Let's go for something large, say a million...

```ruby
primes_finder(1000000)
>>> There are 78498 prime numbers between 1 and 1000000, the largest of which is 999983
```

That is pretty cool!

I hoped you enjoyed learning about Primes, and one way to search for them using Python.

---

###### Important Note: Using pop() on a Set in Python

In the real world - we would need to make a consideration around the pop() method when used on a Set as in some cases it can be a bit inconsistent.

The pop() method will usually extract the lowest element of a Set. Sets however are, by definition, unordered. The items are stored internally with some order, but this internal order is determined by the hash code of the key (which is what allows retrieval to be so fast). 

This hashing method means that we can't 100% rely on it successfully getting the lowest value. In very rare cases, the hash provides a value that is not the lowest.

Even though here, we're just coding up something fun - it is most definitely a useful thing to note when using Sets and pop() in Python in the future!

The simplest solution to force the minimum value to be used is to replace the line...

```ruby
prime = number_range.pop()
```

...with the lines...

```ruby
prime = min(sorted(number_range))
number_range.remove(prime)
```

...where we firstly force the identification of the lowest number in the number_range into our prime variable, and following that we remove it.

However, because we have to sort the list for each iteration of the loop in order to get the minimum value, it's slightly slower than what we saw with pop()!


