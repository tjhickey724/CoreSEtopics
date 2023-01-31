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
Python uses a "colon" (:) at the end of the enclosing statement and indentation afterwards (typically a tab or 4 spaces).
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



