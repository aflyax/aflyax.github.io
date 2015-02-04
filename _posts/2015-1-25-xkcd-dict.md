---
layout: post
title: XKCD → Python → Julia
permalink: xkcd-Julia
comments: true
tags: [julia, python, dictionaries, functions, index]
---

In [Better Way to Read the News](http://interactivepython.org/runestone/static/everyday/2013/11/1_news.html), a chapter from the online version of *Everyday Python* book, we find a function that implements the map from thix XKCD cartoon:

![](http://imgs.xkcd.com/comics/substitutions.png)

<!-- more -->

Original Python code:

``` python
def changeWord(word):
    if word in boring_news:
        idx = boring_news.index(word)
        return xkcd[idx]
    else:
        return word
```

I made a few changes to translate the function to Julia:

<code data-gist-id="b694d0d8c3714d582af6" data-gist-hide-footer="true" data-gist-hide-line-numbers="true"></code>

Out:

```
elf-lord
python
```

Translating the following function from Python to Julia was quite easy. It's not just an exercise of looking up a difference in syntax, but also in using functional vs. object-oriented dot-notation.

Python:

``` python
article = '''Senator johnson was caught stealing a smartphone on election
night.  witnesses say that he allegedly took the smartphone from a kindly
old lady while she was washing her electric car. republican and democrat
congressional leaders have vowed to hold hearings.  Senator johnson could
not be reached for comment.'''

def translateNews(article, boringWords, funWords):
    article = article.lower()
    for idx in range(len(boringWords)):
        article = article.replace(boringWords[idx],funWords[idx])
    return article

print(translateNews(article,boring_news,xkcd))
```

Julia:

<code data-gist-id="12c7ef888b1ab3e73a2f" data-gist-hide-footer="true" data-gist-hide-line-numbers="true"></code>

Out:

```
elf-lord johnson was caught stealing a pokedex on eating contest night.
dudes I know say that he allegedly took the pokedex from a kindly old
lady while she was washing her atomic cat. orc and hobbit
river spirits have vowed to hold hearings.  elf-lord johnson could
not be reached for comment.
```