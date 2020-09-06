---
title: "Initial thoughts on functional programming via learning Clojure"
date: 2020-09-05T19:37:30-08:00
categories:
  - blog
tags:
  - functional_programming
  - clojure
---

My programming learning journey has been a series of pitfalls and missteps. I spent my first formative years in InfoSec learning the linux commandline and by extension Bash and then learning Python. These early failures and successes were written in a very declarative manner, i.e do foo and then modify bar with results.  As other engineers came in behind me and used my code I realized the error in my ways and tried to do better. In my search to better myself and the code that I'm writing 
I've started learning Clojure via what is looking to be a very good book titled ["Clojure for the Brave and True"](https://www.braveclojure.com/clojure-for-the-brave-and-true/).  Clojure is a functional programming language with roots in Lisp built on top of the Java Virtual Machine (JVM). 
Functional programming is a paradigm where functions are treated as first class citizen and where the program is written with pure functions that when given an argument always return the same results (i.e. do no have state).  Writing a program in this way minimizes state issues and other side effects while allowing you as the developer to write simple unit tests for these pure functions.


```python
import re

bot_urls = ['https://correct.horse', 'https://battery.staple', 'http://bobby.tables', 'http://elderly.goats']
bot_urls_defanged = []

for i in bot_urls:
    i = re.sub(r'(http)', 'hxxp', i)
    bot_urls_defanged.append(i)

print('bot urls defanged: %s' %(bot_urls_defanged))
#=> prints bot urls defanged: ['hxxps://correct.horse', 'hxxps://battery.staple', 'hxxp://bobby.tables', 'hxxp://elderly.goats'] to STDOUT.
```
This code uses a for loop to iterate over the urls in the list and replace the string http with hxxp.  It has undesirable qualities... like change the value of i in place (state).


Compared to the clojure equivalent

```clojure
(defn defang_url
    [url]
    (clojure.string/replace url "http" "hxxp"))

(map defang_url ["https://correct.horse", "https://battery.staple", "http://bobby.tables", "http://elderly.goats"])
```

This code uses map to create a vector using the function `defang_url` and the vector of urls.  It is very succinct and doesn't alter or modify any variables.

I will try to include more examples on my clojure journey and my plan is once I'm done with this book to start working on Rust (another functional programming language) and hopefully build some usefully security tools using Rust.

Below is some talks that gave me inspiration to start learning functional programming languages.

* [QuakeCon 2013 Keynote - John Carmack ](https://www.youtube.com/watch?v=W3RsQCGiTgA)
* [DOES19 - Fabulous Fortunes, Fewer Failures, and Faster Fixes from Functional Fundamentals - Scott Havens](https://www.youtube.com/watch?v=FskIb9SariI)