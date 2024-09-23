---
layout: post
title:  "Understanding Binary Search [Problem Solving]"
date:   2024-06-08 08:53:16 +0100
categories: jekyll update
---

Binary search is as the name implies, it searches for something by splitting and only considering either the left or right halves of an array. Should be simple, but can be confusing so - how do we properly understand this? 

In short:

Binary search finds the MINIMUM value that satisfies the condition you provide it.

Why?

The classic binary search problem where it asks to find a value in a sorted array is one of the biggest pitfalls into thinking that the implementation must separate the search into three part, the left, the middle, and the right part. This is the WRONG THINKING for binary search, and it does not generalise to harder problems requiring binary search.

The generalised implementation of binary search only needs to split the search into two parts, the left and the right - because binary search only considers a binary condition - true or false. Depending on which, we will traverse down either left or right parts. This is obvious.

The crux of the issue comes however, when we ask ourselves, how do we return the value we want? If we only consider left and right parts how do we also include the middle part (the value which we check)? The simple implementation would have just returned the middle part, but this time we only have the left and right parts.

This is when we must determine if we should include the middle part as always the "left part" or the "right part".


Imagine a problem to find the first "X" in this array of X and O.

**OOOOOXXXXXXX**

Now say we run our binary search and eventually we reach some point where we're only considering the subarray:

**OXXXX**

If we look at this, our current midpoint is X (which is also the global answer to our example problem - but not the first X in this subarray of X's!) and we should see that because we're in the middle of the right part, we can do better and keep looking to the left of this current search, and as we search through these X's one of these visited X's must be the final answer! Therefore it obvious to consider our target/midway-value to be in the right half always. This is why we next: 1) look at the left part 2) include this midway-value for our search. 

We'll always look to search like this OX, OOX, OOOX, OOOOX, OOOOOX. 

Now we realise this, our implementation will look something like:

```
If condition==True:
    update right pointer to equal to mid-way point. 
    # (i.e. only consider our left part + midway value now).
Else:
    update left pointer to equal mid-way point + 1. 
    # (i.e. only consider our right part exc. midway value now). 
```

Conversely another insight here is when condition==False, our midpoint is currently in the OOO part, and we obviously don't want to include this current midpoint in our search. Thus we exclude it from our next search by shifting left pointer to midpoint + 1. 

When the target value doesn't exist, this algorithm will just return the next best value that meet's the condition.

Why does searching for values in sorted array work with binary search?

We just need to reframe the problem. In reality, if we apply this notion of some binary domain to our problem (e.g. OOOOXXXXXs), we're basically saying:

Let's split our array into numbers smaller than the number of our interest, and numbers larger or equal to the number of our interest.

e.g. I want to find 4 in 1234567:

```
1 2 3 4 5 6 7
OOO X X X X
```

So our solution to this problem boils back to what we've just described. 



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
