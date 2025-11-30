---
layout: post
title: "Code-Execution MCP - Validating Savings"
excerpt: "Anthropic suggested a technique they call Code Execution with MCP, as a more efficient way to use Tokens/context. This is an attempt to validate the idea"
categories: articles
tags: [technology, ai, mcp, llm, tokens, agentic-ai, python]
author: sudharsan
comments: true
share: true
image:
  feature: so-simple-sample-image-3.jpg
---

## Summary
Most recent language models have become very adept at generating and using code. In a recent blog, Anthropic suggested a technique they call 'Code Execution with MCP', as a more efficient way to use Tokens/context.
If you are not familiar with their post, here is the link : [Code execution with MCP: building more efficient AI agents \\ Anthropic](https://www.anthropic.com/engineering/code-execution-with-mcp) 

In this article, I share how I went about valiating the idea. Finally confirming that this 
is a useful technique only under certain conditions/trade-offs: large number of MCP servers installed, robust code-execution sandbox, complicated tasks that require multi-step MCP calls etc. I think we may save token-costs at the risk of more requests & time (depends on the task relationship to MCP tools)

## Can we really save so many tokens? With what other trade-off?

The basic premise is to avoid sending _all_ the tools/MCPs available at the client side to the LLM to allow it to pick what to use. This not only uses up a lot of context to list the tools, but it also results in context bloat when we have a multi-step task and we store and process intermediate results back and forth between tool invocations and we have them polluting the history as well.

What was startling was the amount of savings they mentioned - in the order of 90+% (validate). I wanted to try this out for myself. 

As any good engineer would do, I set out to find out 'someone else' who has already solved it and just test it out. The blog itself mentions 'Cloudflare Code Mode' as an example. It turns out, what Cloudflare described in _their_ blog was implemented with their own SDK. See: [Code Mode: the better way to use MCP](https://blog.cloudflare.com/code-mode/) ; To make matters worse, it was for Typescript (so was the blog written by Anthropic). While familiar with Typescript, I rather fancied some Python implementation to allow me to use the approach in my own code.

Bit of googling, led me to [MCP Code Execution in Practice: Improving Claude Code Project Structure](https://jangwook.net/en/blog/en/mcp-code-execution-practical-implementation/) ([JANGWOOK KIM](https://www.linkedin.com/feed/#)) - once again describing some implementation (not in public domain) and written in Typescript.

Looks like, I have to write some code after all :(

## The Code design

The key building blocks of a basic implementation would be:
- Pick any Agentic framework : I chose Pydantic AI
- Provide it with a mechanism to search file system : This is essentially part of a delicate System Prompt
- Provide it with a mechanism to execute code (Python in our case) : System Prompt also includes instruction on how to use the execute_code function 
- Provide it access to a bunch of MCPs : I selected few simple ones - including the filesystem and time operations
- Finally, a Task that is worthy of all this complexity !!! - Count the number of files in a folder and could only .py and the number of lines in all the files, then save the results into a file with the 'current datetime as a suffix' 


## The Implementation details

The actual sample implementation is in pydantic_main.py - which just demonstrates the bare minimum to use such an agent with 'code execution' capabilities.

Then there is the token_savings_demo.py code, that demonstrates using plain MCP vs code-execution MCP. It prints the savings and lot of intermediate contents for debug purposes. For the relatively trivial task given to the Agent, it showed a savings of between 50-90% token savings.

---
The code, as of today, does the bare minimum to help understand the flow. There are potentially many enhancements that we can do. For e.g. the 'search_tools' tool mentioned in the Anthropic article (maybe doign some smart search), add the ability to persistent code that resulted in successful outcomes (remembered Skills), security aspects of a Sandbox etc.

---


## Some key observations:
- At the expense of context/token savings, the whole task becomes 'chatty' with the LLM - there are more back-n-forth to 'discover' servers and tools
- For all this to make sense, we need a complex-enough task, that can be decomposed into smaller steps and those steps can in-turn be represented within a single block of code.
- There should enough number of MCP servers configured on the client application that would have taken up token - with the new approach that won't need to be sent to the LLM at once.
- This requires quite a bit of scaffolding - for .e.g to generate the MCP Server access 'wrappers' that will be used in the generated code. Similarly, a way to pass intermediate code execution values across code invocations till a task is completed (multiple code execution steps)

All said and done, it was an interesting weekend trying to get this working. Hope this helps you understand how 'small and incremental' changes can make a big difference in cost/effciency
The code I wrote is available in the repo here: [pydantic_mcp_code_execution](https://github.com/p2c2e/pydantic_mcp_code_execution) 
