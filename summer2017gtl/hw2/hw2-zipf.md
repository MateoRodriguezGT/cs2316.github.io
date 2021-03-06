---
layout: sabic-homework
title: Homework 2 - Zipf's Law
---

# Homework 2 - Zipf's Law

## Introduction

In this homework you will practice

- data structures,
- control structures,
- functional programming, and
- using modules that you learn on your own from documentation.

## General Instructions

**This is an individual assignment.**

Collaboration at a reasonable level will not result in substantially similar code. Students may only collaborate with fellow students currently taking Intro to Computing, the TA's and the lecturer. Collaboration means talking through problems, assisting with debugging, explaining a concept, etc. You should not exchange code or write code for others.

Notes:

- Include a comment with your name and GTID at the top of all Python files.
- *Do not wait until the last minute* to do this assignment in case you run into problems.
- Pay close attention to whether problems require you to print or return the results! Printing instead of returning or vice versa will result in a point deduction.
- Name all functions as specified in the instructions.
- Unless otherwise stated, you can assume inputs will be valid in this assignment (i.e. error checking is not required).
- In a Python module you must define a value (such as a function) before referencing it. So if you call function A from function B, the definition of function A must come before the definition of function B in the file.

## Problem Descrtiption

You're a curious linguist with computer hacking skills and you want to see, roughly, if [Zipf's Law](https://en.wikipedia.org/wiki/Zipf%27s_law) holds for texts contained in files lying around on your disk.

## Solution Description

Write a Python module in `hw2.py` that includes the following functions.

```Python
def normalize_text(text):
    """Return copy of text in lowercase with punctuation removed.

    Parameters:
    text: str - text to normalize

    Return: str which is copy of text converted to lowercase with
    punctuation (the chars in string.punctuation) removed.

    Usage Examples:
    >>> normalize_text("Numchuk skills, bow hunting skills, computer hacking skills...")
    'numchuk skills bow hunting skills computer hacking skills'
    """
```

```Python
def mk_word2count(text):
    """Return a dictionary mapping words in text to their count in text. Be sure to account for newlines in the text!

    Parameters:
    text: str - string containing words separated by spaces

    Return: char_dict: dict - dictionary whose keys are words and
    associated values are the number of times the word appears in text

    Usage Examples: (Note technique for testing dict equality.)

    >>> mk_word2count('the butcher the baker the candlestick maker') == {'butcher': 1, 'baker': 1, 'candlestick': 1, 'the': 3, 'maker': 1}
    True
    """
```

```Python
def dict2tuples(word_dict, key=None):
    """Convert a str:int dictionary to a sorted list of (str, int) tuples, optionally with a key

    Parameters:
    word_dict: dict[str -> int]
    key: (optional) a key function to extract the element of the tuples by which to sort

    Return: a list[(str, int)], sorted in descending order, optionally by a key

    Usage Examples:
    >>> dict2tuples({'a': 2, 'b': 5, 'c': 1}, key=lambda t: t[1])
    [('b', 5), ('a', 2), ('c', 1)]
    """
    return sorted([(k, v) for k, v in word_dict.items()], key=key, reverse=True)
```

```Python
def normalize_counts(tuples, max_value=100):
    """Normalize the second values in tuples.

    Parameters:
    tuples: Sequence[(str, int)] - (word, count) tuples
    max_value: int - the max value of the normalized counts (min value is 0)

    Return: Sequence[(str, int)] with same first elements as tuples
    but whose second elements are normalized to the range 0 to
    max_value.

    Usage Examples:
    >>> wctups = [('a', 200), ('the', 180), ('an', 160), ('shenannigans', 50)]
    >>> normalize_counts(wctups, 100)
    [('a', 100), ('the', 90), ('an', 80), ('shenannigans', 25)]
    """
```

```Python
def word_hist(bar_list):
    """Create a text-based bar chart from bar_list.

    Parameters:
    bar_list: Sequence[(str, int)] - (label, length) tuples

    Return: list[str] with one line per tuple in bar_list. Each line --
    a str in the returned list -- has the right-aligned label, a |
    character, then length Xs

    Usage Examples:
    >>> from pprint import pprint
    >>> pprint(word_hist([('a', 10),('the', 9),('an', 8),('shenannigans', 2)]))
    ['           a | XXXXXXXXXX',
     '         the | XXXXXXXXX',
     '          an | XXXXXXXX',
     'shenannigans | XX']
    """
    max_len = len(max(bar_list, key=lambda t: len(t[0]))[0])
    return ["{word} | {bars}".format(word=w.rjust(max_len), bars="X"*length)
            for w, length in bar_list]
```

### `main`

Structure your main method as we have been taught:

```Python
def main(args):
    # code intended to be executed when run as a script

if __name__=="__main__":
   import sys
   main(sys.argv)
```

The user may supply one to three command line arguments to **your** Python script. The first argument to the `python` interpreter, `sys.argv[0]`, is the name of your script, i.e.,  `args[0] = "hw2.py"`, so there will be 1 to 4 arguments in `sys.argv`. `sys.argv`, a list of strings, should be passed as-is to the `main()` function to minimize confusion.

  * The first argument will be the file name of Python script (ie. `args[0] = "hw2.py"`).
  * The second argument, if supplied, must be the name of a text file to read and analyze.
    + If the user supplies a file name on the command line and the file does not exist, you may simple allow the program to exit due to the missing file and let Python report that the file was not found.
    + If the user does not supply a file name on the command line, prompt the user for the file name. If the file does not exist, tell the user the file doesn't exist and prompt the user repeatedly until they enter the name of a file that exists.
  * The third argument, if supplied, is the maximum bar length for the word frequency histogram.
  * The fourth argument, if supplied, is the number of lines of the bar graph to display.

Here's a snippet of code that checks for the existence of a file:

```Python
import os.path
os.path.exists("file_name.txt") # returns True if file_name.txt exists
```

Once you have a valid file name, read the file contents into a string. Here's a snippet of code that opens a file for reading as text and reads the file contents into a `str` variable:

```Python
infile = open(file_name, 'r') # opens file_name as readable file object infile
text = infile.read()          # dumps text data from infile into text variable
infile.close()                # closes infile
```

Once you read the contents of the file into a `str`, use the functions you created above to:

- normalize the text to remove punctuation and make all letters lowercase,
- create a dictionary mapping words from the text to the occurence counts,
- create a list of tuples whose first elements are words from the word count dictionary, and whose second elements are the associated counts -- this list should be sorted in descending order by word count, e.g., largest count first,
- normalize the counts in the list of word count tuples to the maximum bar length of your histogram,
- create a histogram using your `word_hist` function, and
- print the first `num_lines` of the histogram, where `num_lines` is the number of lines of the histogram to display.

Here's a sample program run, using the file [i-have-a-dream.txt](i-have-a-dream.txt):

```sh
$ python hw2.py i-have-a-dream.txt 80 80
           the | XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
            of | XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
            to | XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
           and | XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
             a | XXXXXXXXXXXXXXXXXXXXXXXXXXXX
            be | XXXXXXXXXXXXXXXXXXXXXXXXX
            we | XXXXXXXXXXXXXXXXXXXXXXX
          will | XXXXXXXXXXXXXXXXXXXX
          that | XXXXXXXXXXXXXXXXXX
            is | XXXXXXXXXXXXXXXXX
            in | XXXXXXXXXXXXXXXXX
          this | XXXXXXXXXXXXXXX
       freedom | XXXXXXXXXXXXXXX
            as | XXXXXXXXXXXXXXX
          from | XXXXXXXXXXXXX
          have | XXXXXXXXXXXXX
           our | XXXXXXXXXXXXX
          with | XXXXXXXXXXXX
             i | XXXXXXXXXXX
         negro | XXXXXXXXXX
           not | XXXXXXXXXX
           one | XXXXXXXXXX
           let | XXXXXXXXXX
           day | XXXXXXXXX
          ring | XXXXXXXXX
         dream | XXXXXXXX
          come | XXXXXXX
        nation | XXXXXXX
         every | XXXXXXX
           for | XXXXXX
            go | XXXXXX
          back | XXXXXX
         today | XXXXXX
           are | XXXXXX
          must | XXXXXX
     satisfied | XXXXXX
            by | XXXXXX
           you | XXXXXX
         their | XXXXXX
       justice | XXXXXX
          able | XXXXXX
          when | XXXXX
           all | XXXXX
            it | XXXX
        cannot | XXXX
           men | XXXX
         white | XXXX
          long | XXXX
           now | XXXX
           but | XXXX
         there | XXXX
      together | XXXX
          time | XXX
         which | XXX
            on | XXX
         faith | XXX
      children | XXX
       america | XXX
            my | XXX
          free | XXX
         check | XXX
           has | XXX
         shall | XXX
         great | XXX
           new | XXX
         years | XXX
           who | XXX
            an | XXX
          into | XXX
            so | XXX
         black | XXX
          hope | XXX
       hundred | XXX
   mississippi | XXX
            up | XXX
            us | XXX
          down | XXX
         until | XXX
      mountain | XXX
         later | XXX
```

Note that the full output would be very long and words with low rank would have no `X`s.

## Submission Instructions

Attach your `hw2.py` file to your T-Square HW2 assignment submission.

## Verify Your T-Square Submission!

Practice safe submission! Verify that your HW files were truly submitted correctly, the upload was successful, and that your program runs with no syntax or runtime errors. It is solely your responsibility to turn in your homework and practice this safe submission safeguard.

- After uploading the files to T-Square you should receive an email from T-Square listing the names of the files that were uploaded and received. If you do not get the confirmation email almost immediately, something is wrong with your HW submission and/or your email. Even receiving the email does not guarantee that you turned in exactly what you intended.
- After submitting the files to T-Square, return to the Assignment menu option and this homework. It should show the submitted files.
- Download copies of your submitted files from the T-Square Assignment page placing them in a new folder.
- Re-run and test the files you downloaded from T-Square to make sure it's what you expect.
- This procedure helps guard against a few things.

    - It helps insure that you turn in the correct files.
    - It helps you realize if you omit a file or files. Missing files will not be given any credit, and non-compiling homework solutions will receive few to zero points. Also recall that late homework will not be accepted regardless of excuse. Treat the due date with respect.  Do not wait until the last minute! (If you do discover that you omitted a file, submit all of your files again, not just the missing one.)
    - Helps find syntax errors or runtime errors that you may have added after you last tested your code.
