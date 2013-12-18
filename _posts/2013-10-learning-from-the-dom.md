---
layout: post
title: "Learning from the DOM: How can we do question answering on the web?"
---


What if you could query the DOM to learn about the content of a page similar to how

Google might?

Now extend this to an automated approach that didn't take a huge amount of processing power.


It's an interesting concept I'm sure many programmers might like for crawling the web.

Let's start with a graph to get a good overview. 


![Visualuze the attributes]({{ site.url }}/images/learning-from-dom.png)



You'll notice how all of the graphs are by and large split in to 2 groups.

I used ([Expectimax](http://en.wikipedia.org/wiki/Expectation%E2%80%93maximization_algorithm)) to cluster the dom elements according to different attributes.

The core idea is fairly simple. First let's consider how the DOM is structured. It's a tree which has a recursive structure.

We're trying to infer content based on the text of each dom element. One neat thing about how dom manipulation works with text:

When you call element.text() in any html parsing library or in normal javascript you will get the immediate textnodes of that child as

well as those of the children's children. First we need to elminiate the noise by only considering nodes that only have one or 2 children.

This will allow us to better localize the query to a specific region. From there we can establish an answer to the query.

How do we pick a region to help best answer a question now?

From here let's only consider DOM elements that were in the most relevant cluster. The easiest way is to score an indivudal node as the highest.

The highest sum of the 4 attributes mentioned earlier is a good estimator. From that general region we can establish an answer to the question.

Now here's where having the DOM structure comes in handy. Let's consider different kinds of questions people might want to answer.


                1. Count of items
                2. List of items
                3. A specific answer


A relatively naive query language with special keywords is enough to get us by for now; ( I plan on expanding this to something more formal as the need arises)

Now how do we answer questions? Let's cover the 4 attributes that help us arrive at an answer. The core idea is actually pretty simple.

          1. Normalize query terms and words on the page.
          2. Use similarity and distance metrics to determine the most relevant dom element.
          3. Using A* search search the possible nodes in that cluster querying each dom element to determine what the most likely dom element is