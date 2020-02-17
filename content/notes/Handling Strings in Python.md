+++
title = "Handling unicode strings in Python"
date = "2017-08-05T14:10:29Z"
description = ""
slug = ""
tags = ["Python"]
+++
---
![Logo](https://imgs.xkcd.com/comics/the_history_of_unicode.png)
## Summary
In this post I will try to explain how to handle unicode strings in `python 2 and 3`.

Let’s start shall we?

## ASCII strings
So let’s start with the `ASCII` strings, Have a look at hi.txt

Let’s see what does it hold

```
azhar at MacbookPro in ~/unicode
$ cat hi.txt
hi
```
Nice and easy. It contains, two characters `h` and `i`

Size ?

```python
azhar at MacbookPro in ~/unicode
$ du -a -b hi.txt
2   hi.txt
```
This means that the file is of `2 bytes`. Now what do these `2 bytes` hold inside them? Let’s do a `hexdump`
```python
azhar at MacbookPro in ~/unicode
$ hexdump hi.txt
0000000 6968                                   
0000002
```
If you look over to the [ASCII](http://www.asciitable.com) table and look out for the hex representations, you will see that the letter `h` is represented by `68` and `i` is represented by `69`

Let’s see how `python2` handles this. Firing up the `interpreter`
```python
>>> with open('hi.txt') as f:
...   content = f.read()
...
>>> content
'hi'
>>> type(content)
<type 'str'>
>>>
```
Now I probably should reiterate the fact that

> Every character in a string is a single byte

And that the ASCII table translates each byte value to a unique character. the file contains an `ASCII` string of exactly two characters. So it does makes sense. Let’s dig a little further.
```python
>>> len(content)
2
>>> content[0]
'h'
>>> content[1]
'i'
>>>
```
So this confirms that the `x[0]` contains `h` and `x[1]` contains `i`


## Enter Unicode
So how many characters does the `ASCII` representation able to represent? Doing the math, 256(`2^8`) would be the maximum number of characters that the `ASCII` table can represent. Just giving a heads up here, `Chinese` has a lot more than `256` characters. So how would you handle `chinese` as well as the characters on your keyboard?

Have a look at chinese.txt

```python
azhar at MacbookPro in ~/unicode
$ cat chinese.txt
hi猫
```
So it contains three character namely `h`, `i` and `猫`. Size?

```python
azhar at MacbookPro in ~/unicode
$ du -a -b chinese.txt
5   chinese.txt
```
`5 bytes`. Let’s see what does each byte contain

```python
azhar at MacbookPro in ~/unicode
$ hexdump chinese.txt
0000000 68 69 e7 8c ab
0000005
```
The relevant thing to note here are the five `hexadecimal` numbers `69`, `68`, `e7`, `8c` and `ab`

So five numbers, 5 bytes. Good so far? Now how do we interpret these numbers? We will have a look at the [Unicode UTF-8](https://en.wikipedia.org/wiki/UTF-8) table.

In this table, `68` is the character `h`, `69` is the character `i`, and the three-byte sequence `e7`, `8c`, `ab` is the character `猫`. To recap, `h` is one byte, `i` is one byte, but `猫` is three bytes.

A point to note here is that, the Unicode UTF-8 table is a superset of the ASCII table, so that’s the reason `h` and `i` are represented by the same characters in both.

## Handling unicode strings in `python2`
```python
>>> with open('chinese.txt') as f:
...   content = f.read()
...
>>> content
'hi\xe7\x8c\xab'
>>> len(content)
5
>>>
```

What was all that? `h` and `i` are represented just fine but when it comes to the chinese character, it shows me hexdecimal numbers. And how does it return me `5` as the string lenght, when we know perfectly well that there are just `3` characters in that file?

It turns out that the python `str` doesn’t store a `string` but a stream of `bytes` in it. Digging further.

```python
>>> x[0]
'h'
>>> x[1]
'i'
>>> x[2]
'\xe7'
>>> x[3]
'\x8c'
>>> x[4]
'\xab'
```
The `hi` is returned prefectly fine as those are ASCII characters, but when it comes to the chinese character, it is represented by UTF-8 unicode. But since `str` object in `python2` just stores a sequece of bytes, it has no way of deciding to group these 3 characters to represent the chinese character. So we see them as the hexadecimal numbers.

So how should we deal with this.

## `decode()` to the rescue
```python
>>> utf_content = content.decode('utf-8')
>>> utf_content
u'hi\u732b'
>>> type(utf_content)
<type 'unicode'>
>>> len(utf_content)
3
>>> utf_content[0]
u'h'
>>> utf_content[1]
u'i'
>>> utf_content[2]
u'\u732b'
>>>
```
So the `decode()` tells python to convert the string `content` into a `UTF-8` string. I know, the name is confusing as hell. But let’s leave that for another day.

I we call the `print` statement now. Let’s see what we get

```python
>>> print utf_content
hi猫
>>>
```
So there you go.

## Word of caution

Weird things happen in `python2` if you think that `str` is a `string`. To be safe, convert the `str` object to `utf-8` format immediately by doing a `decode('utf-8')`. Then work with your `unicode` object and not the `str`.

> In python2, a unicode object type represents real strings whereas the str object is a sequece of bytes.

So when you are done precessing your `unicode` object and now you want to write it down to a file or a database. First convert it back to a sequence of `bytes` (`str` object) using the `encode()` method.

```python
>>> str_content = utf_content.encode('utf-8')
>>> type(str_content)
<type 'str'>
>>> str_content
'hi\xe7\x8c\xab'
>>> content == str_content
True
>>>
```
Now you will be able to write this content to a file or database as directly doing so with a `unicode` object would have given you some wierd errors.

Okay, okay. I will show that to you

```
>>> with open('myfile.txt', 'w') as f:
...   f.write(utf_content)
...
Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
UnicodeEncodeError: 'ascii' codec can't encode character u'\u732b' in position 2: ordinal not in range(128)
>>>
```
Now doing the same with the `str` object
```
>>> with open('myfile.txt', 'w') as f:
...   f.write(str_content)
...
>>>
```

## Handling unicode strings in `python3`
Python3 makes handling of unicode strings easy.

One of the significant changes being that, `str` now stores unicode `strings` and not a sequence of `bytes`

Let’s see how it handles the chinese.txt file

```
azhar at MacbookPro in ~/unicode
$ python3
Python 3.4.2 (default, Jun 19 2015, 11:34:49)
[Clang 10.0.0 (clang-1000.11.45.5)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> with open('chinese.txt') as f:
...   content = f.read()
...
>>> type(content)
<class 'str'>
>>> len(content)
3
>>> content[0]
'h'
>>> content[1]
'i'
>>> content[2]
'猫'
```
So everything works out of the box (Going with the Batteries included philosophy of `python`).

Now what if I wanted to interpret the contents of it as `bytes`.

You can do so by passing the argument `rb` when opening the file
```
>>> with open('chinese.txt', 'rb') as f:
...   content = f.read()
...
>>> type(content)
<class 'bytes'>
>>> content
b'hi\xe7\x8c\xab'
```
So now you have got the default behaviour of `python2`.

Converting it into `utf-8`
```
>>> content.decode('utf-8')
'hi猫'
```
So to sum it up

In `python3`, `str` represents `unicode` string while the `bytes` type represent the sequence of `bytes`

For further reading, I would really, really suggest you have a look on the content written by these guys

* [Joel Spolsky](https://www.joelonsoftware.com/2003/10/08/the-absolute-minimum-every-software-developer-absolutely-positively-must-know-about-unicode-and-character-sets-no-excuses/)

* [Ned Batchelder](https://nedbatchelder.com/text/unipain.html)

* [Philip Guo](http://pgbovine.net/unicode-python.htm)

on this topic
