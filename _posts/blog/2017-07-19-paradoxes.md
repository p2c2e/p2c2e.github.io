---
layout: post
title: "Interesting Stories in Computer Science"
modified:
categories: blog
excerpt: "Learning Computer Science could be so much fun if only we focused on the stories.."
tags: [technology, compsci, physics, pigeon-hole, paxos]
comments: true
image:
  feature:
date: 2016-07-19
---
When joining college, I decided to do Physics as my Masters major due to my love of Astronomy and Astrophysics. Something about gazing at
the stars and putting ourselves in perspective was always thrilling. It is another matter that the 'course work' was not all blue skies!

Similarly, the reason for taking an active interest in Computer Science was manifold - the instant gratification of seeing your program
obey your instructions, the level playing field that it provides regardless of the discipline of study etc. One of the arcane reasons
is the abundance of myths, anecdotes, pseudo "laws" surrounding computer science. I am listing a few below that you might like:

# Buridan's Principle or more fondly, Dilemma of [Buridan's Ass](https://en.wikipedia.org/wiki/Buridan%27s_ass)
The story is about a Donkey that dies of starvation because he is equidistant from both hay and water and cannot make up his mind which
way to go. Though originally used as an analogy by Aristotle, it has since been used to illustrate similar non-sensical and seemingly
intractable problems. See the Wiki link for more history.

In the context of computers, Leslie Lamport published a report in 1984. [You can find it here](http://research.microsoft.com/en-us/um/people/lamport/pubs/buridan.pdf).

From my understanding of it, it talks about decision systems taking more than expected time due to 'indecision points', where two equally
possible states are making it hard to decide, requiring a need for arbiters.
It has been variously extended to explain thread dead-locks due to resource contention etc.

# Pigeon hole principle
This seemingly innocuous principle states that : If there are say 10 pigeons and 9 pigeon holes (exactly fits one pigeon each), there will always be 1 pigeon left-over after they pigeons are accommodated.

This is a principle from Discrete Mathematics. Though not directly relatable to computer science, it is very useful
in proving algorithm bounds, limits on solutions ('no more than X satisfy some criteria') etc.

In the realm of computers, some examples of use include:

- Sudokus where we can solve some boxes by ruling out others
- Bounds on compression algorithms etc. : No lossless compression algorithm can reduce the sizes of all n-bit files, because there are 2^n different n-bit files and only 2^(n−1) possible files of length less than n bits. Or in other words, there is always a few files for which any lossless algorithm will likely result in the new file with same / larger size!

# Paxos (another Leslie Lamport paper from Microsoft Research)

Rarely have I come across research papers that read like novellas. "[The Part Time Parliament](http://research.microsoft.com/en-us/um/people/lamport/pubs/lamport-paxos.pdf)" is refreshing to read.
Highly recommended for leisure reading as well.

The Paxos problem and the corresponding algorithm address challenges in building fault-tolerant distributed systems. The story is about legislators that are free to come / leave the parliament at will. But, for some work to get done, there is a minimum quorum required. It is also applicable for work distribution, election of a node for some work etc.
