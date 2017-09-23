---
layout: post
title: "Insights using a Graph Database"
modified:
categories: blog
excerpt: "I have been playing around with Graph Databases and Machine Learning in the context of HR / People development"
tags: [technology, graphdb, neo4j, cypher, hr]
comments: true
image:
  feature:
date: 2017-09-20
---

# Introduction

As part of ongoing efforts towards learning (Sharpen the Saw!), I have been playing around with Graph Databases and Machine Learning
in the context of HR / People development.

In case of Graph Databases, I picked Neo4J to do some experiments - partly because it is quite easy to setup, is free etc.

Sources / types of data include:
* People records (Joining, Total Expr, Promotions, Titles, City)
* Skill data (raw skills information, grouping, certifications, social endorsements)
* Team data (name, members, skills required in general)
* Performance data - ratings, feedback, coding behaviours
* Internal Knowledge management / Expert Forums - to seek out skills

# Rough Schema

Here is roughly how the relationship are setup - for lack of a Schema.

![Rough Schema](/images/graph_schema.png?=250x)

# The Interesting part:
Obviously, I am doing this for the insights. Here are a some interesting questions I was able to answer:

1. Find all Junior managers managing teams remotely - they may need support to be successful
1. If a Person knows "X" Technology, what are technologies are they likely to knows
1. Which people know "X" technology, but are part of teams that don't require that skill? Sub-optimal use
1. Find employees "similar" to a given person - This helps in mitigating key-person risk, succession planning
1. Find the "Best Fit" team for a given person
1. Given open-positions in the organization, who can be tapped internally? Job mobility.
1. What are some pre-requisites for a Certification?
1. If I want to work in Team X or for a particular function (say: business analysis), what certifications are useful?

# Is this really easy to implement?

If I can whip up some code to quickly answer these questions, why is it that larger organiztions are not using such approaches already?

Answer is simple - This is not a technolgy problem. Getting quality data requires connecting disparate systems that hold useful information. And for the most part, they are unreliable and "point-in-time".

Sometimes, it is possible to infer information - For example,

What skills are expected in a particular team? We could get this from the team management or infer from the most common skills in the team.

# Some ideas for implementation
- Using internal and external social platforms to capture and validate skills and endorsements
- Gamification to collect and cleanse data
- Make these insights part and parcel of day to day team and individual activities, feedback sessions, internal mobility, hiring, learning etc.
