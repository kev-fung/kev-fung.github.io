---
layout: post
title:  "Binary Search Beauty [Problem Solving]"
date:   2024-06-08 08:53:16 +0100
categories: jekyll update
---

Binary search is as the name implies, it searches for something by splitting and only considering either left or right halfs of an array. But how to properly understand this?

In short:

Binary search finds the MINIMUM value that satisfies the condition you provide it.

The classic binary search problem where it asks to find a value in an array is one of the biggest pitfalls into thinking that there is some notion of a mid-way value to return, and it also makes you think there's a need to always sort the value. But this is WRONG THINKING for binary search.


In reality, binary search only considers a binary condition - true or false. And depending on which, we will traverse down either the left or right parts. 

However - how do we return the value that we want? If we only consider left and right parts how do we also include the middle part (the value which we check)? 

This is when we must determine if we should include the middle part as always the "left part" or the "right part".

Imagine a problem to find the first "X" in this array of X and O.

OOOOOXXXX

Now say we run our binary search and eventually we reach some point where we're only considering the subarray:

OXXXXXX

If we look at this, our midpoint is X (and is also the global answer to our asked problem) we should see that if we attempt to include this X to the right part of the midpoint, we're going to be way off from the true answer! Therefore it only makes sense to consider our target/ midway-value to be in the left half always.

We'll always look to search like this OX, OOX, OOOX, OOOOX, OOOOOX. 

Now we realise this, this means that our implementation will look something like:

If condition==True:
    update right pointer to equal to mid-way point. (i.e. only consider our left part + midway value now).
Else:
    update left pointer to equal mid-way point + 1. (i.e. only consider our right part exc. Midway value now). 


Why does searching for values in sorted array work with binary search?

We just need to reframe the problem. In reality, if we apply the notion of some binary domain to our problem, we're basically saying:

Let's split our array into numbers smaller than or equal to the number of our interest, and numbers larger than the number of our interest.

e.g. I want to find 4 in 1234567:

1 2 3 4 5 6 7
OOO X X X X

This is the same problem as what we detailed earlier.



<!-- You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated. This is another one!


Jekyll requires blog post files to be named according to the following format:

`YEAR-MONTH-DAY-title.MARKUP`

Where `YEAR` is a four-digit number, `MONTH` and `DAY` are both two-digit numbers, and `MARKUP` is the file extension representing the format used in the file. After that, include the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/ -->
