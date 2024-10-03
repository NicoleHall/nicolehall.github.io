---
layout: post
title: Key Learnings About Python from Leetcode's Roman to Integer Challenge
image: "/posts/i_grind.jpg"
tags: [Python, Leetcode]
---

### Key Learnings from Leetcode's Roman to Integer Challenge

Everyone says "Grind Leetcode". Ok, fine 👩‍🎓

### Romans to Integer

The challenge can be found [here](https://leetcode.com/problems/roman-to-integer/description/)
This post is about what I've learned by doing the challenge. My initial solution was incorrect. For my personal style of learning, I find it useful to compare my assumptions about Python and my incorrect solution with working examples from other programmers: [solution video](https://www.youtube.com/watch?v=_5MYW7n1U-I).

I started my development career with Ruby. I’ve always appreciated the descriptive method names. `"string".reverse` does exactly what you think it will. I was curious about whether Python had a similar feature. A string can be reversed using the slice syntax: `"example_string"[::-1]`. But it's not terribly readable.

Python has a `reversed` method that takes a string as the argument. But it doesn't return a reversed string. It returns an iterator object. Which means attempting to reverse `"NICOLE"` like this won't work:

```python
reversed_string = reversed("NICOLE")
print(reversed_string)
```

You won't get the string back. It prints an iterator object to the console: `<reversed object at 0x31bf035b0>`. To convert it back into a string use `join()`:

```python
print(''.join(reversed_string))  # => ELOCIN
```

Readability is nice. But the slicing approach is more straightforward and does not require type conversions which may impact performance.

### Strings Are Iterable

Something I didn't know about Python prior to this challenge is that strings are iterable. This means you don't have to do that Ruby-like thing of using `"string".chars`. The string can be iterated over directly.

Here's an example of iterating over a string in Python:

```python
upcases = ""
for letter in "nicole":
    upcases += letter.upper()
print(upcases) # => NICOLE
```

Unlike Ruby, you can't directly save the result of a for loop to a variable in Python. To save the results of a loop in a list, you'll need to use list comprehension. You must use it if you want to create a new list by performing an operation on each element of the thing you're iterating over:

```python
upcases = [letter.upper() for letter in "nicole"]
```

`upcases` will be a list object.

### Back to the Leetcode Roman Numerals Challenge

With a better understanding of how strings work, it's time to tackle the Roman to Integer problem. Once the string is reversed, it can be iterated over. Start by adding a dictionary to the method with the keys and values from the challenge description. The next step is instantiating a `sum` variable to 0 and a `last` variable to "I". The `last` variable is to track the last seen numeral to avoid complications with indexing, especially at the first index where the current value is uncertain. By initializing the variable last to "I," we can safely assume we're starting with the smallest value, ensuring no issues arise as we process the string.

The next step involves using an if-else statement to iterate over the passed string. If the current element is greater than the previous one (stored in the `last` variable), you'll adjust the sum by subtracting `last` from the current element. Otherwise, you'll add the current element to the sum. Here is the completed solution:

```python
class Solution(object):
    def romanToInt(self, s):
        roman_table = {
            "I": 1,
            "V": 5,
            "X": 10,
            "L": 50,
            "C": 100,
            "D": 500,
            "M": 1000
        }
        num = 0
        last = "I" # "I" is the smallest Roman numeral. Any numeral encountered later will either be equal to or larger than "I."

        # the slicing below means that both start and stop are omitted, so Python defaults to the entire string
        for numeral in s[::-1]:
            if roman_table[numeral] < roman_table[last]: # that means one of the special cases is happening -- like IV.
                num -= roman_table[numeral] # if so, subtract the current element from the num value which is instantiated as 0
            else:
                num += roman_table[numeral] # this means we are in the normal case where the current numeral should be added to num
            last = numeral
        return num

print(Solution().romanToInt("MCMXCIV"))   # => 2016
```

### Alternative Approach

This challenge can also be solved with a substring replacement solution. Python's `string.replace()` method can handle substrings. For example, if I encounter a substring like "IV", I can create a key-value pair that sets "IV" to its logical translation of "IIII". The special cases are also listed in the challenge description. I added the conversions of special cases as a dictionary:

```python
conversions = {"IV": "IIII", "IX": "VIIII", "XL": "XXXX", "XC": "LXXXX", "CD": "CCCC", "CM": "DCCCC"}
```

The logic here is to tell the program to replace the odd-ball values with values consistent with the `roman_table` dictionary. By replacing substrings, I can convert the string as it's passed in based on the `converstions` key-value pairs and then sum it all up. Here is the completed solution that manipulates the string as it's passed in:

```python
class Solution(object):
    def romanToInt(self, s):
        roman_table = {
            "I": 1,
            "V": 5,
            "X": 10,
            "L": 50,
            "C": 100,
            "D": 500,
            "M": 1000
        }

        conversions = {"IV": "IIII", "IX": "VIIII", "XL": "XXXX", "XC": "LXXXX", "CD": "CCCC", "CM": "DCCCC"}

        for k,v in conversions.items():
            new_s = s.replace(k,v) # swap out odd ball roman numerals to be consistent with roman_table

        amounts = []
        for element in new_s:
            amounts.append(roman_table[element])
        return sum(amounts)
print(Solution().romanToInt("MCMXCIV"))  # => 2016
```

---

The run time of the second solution is faster. Lookups in dictionary data structure allow for quick access to values. Once the hash code is computed, the dictionary can access the memory location directly where the value is stored. This direct way of finding values speeds up the retrieval process.
