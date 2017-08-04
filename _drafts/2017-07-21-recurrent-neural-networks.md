---
layout: post
title:  "Untitled RNN Post"
date:   2017-7-21 14:00:00 -0500
comments: true
---

## 1. What is a recurrent neural network?

## 2. How can we make it do amusing things?

Karpathy demonstrates the potential of char-rnn by training it on Shakespeare plays. The results are very cool, but ultimately of limited interest to most because the neural network cannot understand the most important patterns in a play. It can only consider about 50-100 characters at a time, which is enough to "understand" how the dialogue should be formatted, but not enough to understand that a play is divided into scenes, each scene has a limited and consistent number of characters, the lines these characters speak have related content, and so forth. Humans cannot interpret the output in the same way that they interpret the source data.

If you want to use a neural network to generate entertaining content, I think you should look for training data with the following characteristics:

**The data follows a consistent format** – consistent enough for machines to recognize, not just humans. If you gather data of the same type from multiple sources, you must make sure to regularize the format.

**The data comes in small and self-contained units.** A neural network that can "remember" more than about 200 characters at a time is impractical to run on a typical desktop computer, and if you don't want to wait several days for results, you may want to stay below 100. This doesn't mean the network can't produce structures that are longer than that; it can learn that B follows A and C follows B, even if the entire A > B > C sequence is longer than its sequence length. But it won't recognize any relationship between A and B that is not mediated by C. (I'll point out an example of a situation like this later on.)

**The range of possible data is relatively small.** It can't be *too* small, of course, or the output will be boring, but there must be limitations of some kind. There are actually two reasons for this. First, of course, it's easier for the network to find patterns in a more homogenous dataset. Feeding it arbitrary English text will largely produce gibberish; a more consistent body of English text (e.g., only Shakespeare plays, as above) will allow it to pick up words and phrases.

But the words and phrases don't have any meaning, which leads us to the second reason for this requirement. Of course, in one sense of the word "meaning," this is obvious; we know there's no real understanding here. More precisely, I should say that we can't interpret that output. When machine-generated text is funny, it's because we *can* interpret it, even though we know that it has no meaning. If the format is very specific and limited, much of the work is done for us; we already know what information each part of the output would normally convey.

## 3. How did *I* make it do amusing things?

[A list of video game titles](https://pastebin.com/DG1CsVXk), with a few specific pieces of additional data, satisfies these requirements. The network can master the format easily, and if it generates "Foo (Bar, 2001) (Baz)," we already know that it's describing a game called *Foo* published by Bar for the Baz platform in 2001.

Of course, I didn't explicitly apply the above criteria when I decided to use it; that would be pretty strange, since I was primarily trying to entertain myself. But to some extent I had become aware of those criteria while using neural networks with other types of data, which is why I recognized the potential here. (There's an additional reason: video game titles are ridiculous, or at least they seem so when taken out of context. I've done similar things with books, and although the results are pretty cool they haven't gotten anything like the same amount of attention.)

Yet even with such a limited format, the neural networks could still surprise me. I started out with a small 64-cell network, which was enough for it to figure out the basic format. When I realized how fast torch-rnn was running, I increased the size to 128 cells, and then it did something rather striking: it recognized the relationship between the platform and the year. No more Commodore 64 games with 2007 release dates, or iOS games in 1983 – it very consistently gives plausible years for the console or operating system it names.

When I increased the size again, to 256 cells, it abruptly began generating lists of games that all began with the same letter. It took me a moment to remember that the original list was in alphabetical order, and it was trying to emulate *that*. When I randomized the order of the rows, I found that the last 30,000 row or so didn't follow the same format as the rest, and had only titles. The network had never generated games in that format before, but after the shuffling, it began to do so.

As it turns out, the order of the data matters, and patterns near the beginning are more likely to be learned. It's a good idea to randomize the order whenever possible.

I tried other variations – increasing the number of layers or the sequence length, for example. I'm not sure if it made a difference. Sometimes it seemed that it did; I think it might eventually have done better than chance at associating publishers with platforms. But I can't confirm this. I'd like to know how to isolate individual neurons, the way Karpathy does in the paper above, so I could get more insight into what it's doing. But even that would only explain so much.
