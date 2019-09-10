---
layout: post
title:  "Java Speed Reader"
date:   2019-08-08
desc: "This post outlines the features in my basic desktop speed reader program and how I built it."
keywords: "blog"
categories: [Java, Other Work]
tags: [java,projects]
icon: icon-java
---
During my sophomore year, I took a class on data structures. At the time, I was also interested in natural language processing (NLP), which is a broad field that encompasses a long history of traditional algorithms as well as new machine-learning based algorithms to parse and understand human language. Thus, I came up with an idea that would combine these two interests.

One of the major tasks in NLP is sentence parsing. This process consists of labelling the function of every word and its relationship to other words. This results in a tree structure being generated. Stanford has a very good parser that they published [here](https://nlp.stanford.edu/software/lex-parser.shtml). A parse of an example sentence is shown below.

![parse](/static/files/speedreader1.png)

I had previously heard of speed readers that showed one word at a time in quick succession. They claimed this method helped with both comprehension and reading speed. Inspired by this, I thought I could use NLP to remove unneeded words from text, beyond just common words like "the" or "a." Thus, I would be both shortening text and increasing reading speed.

In order to remove unnecessary words, I relied on the tree structure that was generated above. First, I parsed the text into sentences. I then went through each sentence and did the following procedure:
1. Generate the tree parsing for that sentence.
2. Go through each word, remove it from the sentence, then generate a new semantic tree.
3. Find the word that, when removed, created the least decrease in "similarity" between the original tree and the new tree it created.
4. Remove that word from the sentence, then repeat. Continue this process until the "similarity" between the tree and the original tree has gone below a certain threshold value.
5. Remove some common words like "and," "the," and "a."

This is a fairly simple algorithm, but I have yet to say what tree "similarity" means. This is an integral part of the algorithm, and, in my opinion, what really defines how it functions (along with the parser). For my similarity calculation, I simply checked how many nodes overlapped and had the same data (they had to have the same position and data in their respective trees) and divided that by the number of total nodes of the original tree. There is no one way to define tree similarity, so this could be changed to a number of other things.

With the threshold (which I dubbed the calibration constant) set to 0.95, the tree algorithm alone reduces the number of words by about 20% and the removal of very common words reduces it by another 10%, meaning a total of 30% reduction in the number of words. That means a 42% increase in reading speed!

I have not done any extensive or formal testing on interpretability, but I can offer my own opinion. I think that the text is just barely readable enough for a casual reading, but it would definitely be lacking if you wanted to get past any surface level plot in the reading.

The algorithm is also not the most efficient. I am basically brute forcing the approach, but this is largely because I can't at all characterize what the parser will do to a word. If removing a word one time decreases the similarity by a lot, removing it a few iterations down the line may not decrease similarity so much. Thus, I was unable to think of any particularly good optimizations. Also, the parser takes up the majority of the run time. It is a complex piece of code. It works at the rate of about 34 words/second, so running the algorithm on a 6000 word article takes about three minutes. This could most easily be improved by loading while the user is reading.

I then built a fairly crude GUI for the user to interact with. Here are some of the pages:

![sr2](/static/files/speedreader2.png)

**_This is the main screen. There are options to load the text from a .txt file or copy and paste it directly in._**

![sr3](/static/files/speedreader3.png)

**_This is the main reading screen. You can pause with the space bar and go forward or backwards by one word using the arrow keys. The screen turns red when paused._**

There are a few things I still would like to add to this project. Here are some of my ideas:
- A calibration mode that calculates the user's optimal speed
- Speed adjustment in the actual reader
- A better core algorithm; I have tried to think of a way to do this task using machine learning, but I really would need to first formalize the problem a bit more then take a deeper dive to see what the current problems are, what potential solutions are (both learning and non-learning solutions), and try those out to see what works
- Showing the past few and upcoming few words on the main page in a smaller text
- Accepting more formats as input, such as webpage or ebook formats
- A better UI and more accessibility and customization options
- Optimize the algorithm a bit more
- Dynamic loading of the shortened text

The code will be posted soon.
