# Lesson 8 - Intro to Python for Java Programmers

Today we do a quick review of Python for Java Programmers.

Here is a nice online intro to [Python for Java Programmers](http://python4java.necaise.org/Main/TableOfContents)
(We will mostly follow the main structure of this intro).

Here is the [Official Python tutorial](https://docs.python.org/3/tutorial/) which you should skim
and here is [Google's short Python class](https://developers.google.com/edu/python) if you want another intro.
and here is a [Python Cheat Sheet for Java Developers](https://github.com/akashp1712/awesome-python-cheatsheets#1-python-for-java-developers-1)

Today, we will go over the main differences between Python and Java and give you practice writing Python code.
Tomorrow, we'll show how to create and use Classes in Python.

## Statement syntax and semicolons
Java requires semicolons to terminate statements, but allows statements to span many lines.
Python requires semicolons to separate statements on the same line 
but doesn't require semicolons if statements are on separate lines.
Here is an example... but if you remove the semicolon after print(3) it will generate a syntax error
``` python
print(1) 
print(2); 
print(3); print(4)
print(5); print(6);
```

## Comments
Python uses the hashtag to indicate that the rest of a line is a comment
``` python
print('hello')  # this is a comment
```

## Statement blocks are indicated using indentation
This is the biggest syntactical difference between Python and other OOP and imperative languages.
Java uses curly braces {...} to indicate statement blocks such as while bodies, and if-then-else blocks.
Python uses a "colon" (:) at the end of the enclosing statement 
and [indentation](https://docs.python.org/3/reference/lexical_analysis.html#indentation) afterwards (typically a tab or 4 spaces).
``` python
for i in [2,3,5,7,11]:  # for loops can iterate over a list of Python values
    print('i=',i)
    if i%2==0:
        print(i,'is an even prime') # strings can use either ' or "
    else:
        print(i, "is an odd prime") 
    print('-'*10)  # you can multiply a string by an integer to repeat it
print('bye')
```

## identifiers
[Python identifiers](https://docs.python.org/3/reference/lexical_analysis.html#keywords) are similar to Java. They are case sensitive, can be of any length, and
consist of letters, digits, and underscores, starting with a letter or underscore.

Python 3.11 does not allow the following key words to be identifiers:
``` python
False      await      else       import     pass
None       break      except     in         raise
True       class      finally    is         return
and        continue   for        lambda     try
as         def        from       nonlocal   while
assert     del        global     not        with
async      elif       if         or         yield
```

## Functions
Python functions are defined using the def keyword 
``` python
def functionname(arg1, arg2, ...):
   '''  doc string comment describing what the function does
        this can span multiple lines
   '''
   statement1
   statement2
   ....
   statementn
   return(expression)  # return statements are optional, and NONE will be return if no return is given
```
Here are some examples:
``` python
'''
demo04 is a module 
that contains a function is_prime(n) 
that returns True if n is a positive prime, False otherwise
author: Tim Hickey
date: 2023-01-31
'''
def is_prime(n):
    ''' returns True if n is a positive prime, False otherwise 
    '''
    for i in range(2,n):
        if n%i== 0:
            return False  # if n is divisible by 2<=i<n it is not prime
        elif i*i>n:
            return True  # any proper divisor must be less than sqrt(n)
    return n>=2  # if n is 2 we never enter the loop

if __name__ == '__main__':  # this only runs if this file is run directly
    print('the primes < 100 are')
    for i in range(-3,100):
        if is_prime(i):
            print(i, end=', ')  #don't add a newline after printing, add a comma space instead
```

