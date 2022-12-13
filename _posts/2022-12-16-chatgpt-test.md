---
title: "Playing around with ChatGPT"
date: 2022-12-12T17:22
tagline: "Hello Skynet? 🤖"
header:
  overlay_image: https://bart-parka-blog-assets.s3.eu-west-2.amazonaws.com/images/overlays/docker-terraform.jpg
  overlay_filter: 0.3 # same as adding an opacity of 0.5 to a black background
  caption: "Photo credit: [**Bart Parka**](https://www.instagram.com/bart_parka/)"
  actions:
  - label: "See Example Code"
    url: "https://github.com/bart-parka/chatgpt-tests"
categories:
  - blog
tags:
  - ChatGPT
  - OpenAI
---

## Register

* Register fot ChatGPT on the [OpenAI website](https://openai.com/blog/chatgpt/), it is free at the moment! 
* You can use your Microsoft/Google account for this.
* You can start playing around immediately [here](https://chat.openai.com/chat)
* Note this service is super busy, with something like a million users registering in 5 days. [Read more](https://www.forbes.com/sites/ariannajohnson/2022/12/07/heres-what-to-know-about-openais-chatgpt-what-its-disrupting-and-how-to-use-it/).

## Python Wrapper

I wanted to make something useable on the command-line so went searching. People did not disappoint - turns out a wrapper already exists for Python, you can get it [here](https://github.com/mmabrouk/chatgpt-wrapper)

You run an interactive session on command line:
* `pip install git+https://github.com/mmabrouk/chatgpt-wrapper` 
* I had to run `playwright install` before running below install command
* `chatgpt install`
* A browser window will open where you can log in using your OpenAI account.
* Once logged in, I had to run `session` command in the interactive session. 
* You can then interact with ChatGPT normally.

This is nice, but what I really want is some sort of command line tool that makes my life easier as a developer. GitHub Copilot already does much of this I hear, in terms of code completion etc.. I've never tested it myself so can't comment but what I would like to try and achieve here is a utility that helps me in my day job, for example in writing Terraform scripts.

To do this I'm going to use the ChatGPT class provided in [mmabrouk's](https://github.com/mmabrouk/chatgpt-wrapper) wrapper. You can achieve a "streaming" experience where the response is streamed in a "typing" like manner:

```python
from chatgpt_wrapper import ChatGPT
import sys

bot = ChatGPT()

for iterator in bot.ask_stream(sys.argv[1]):
    sys.stdout.write(iterator)
    sys.stdout.flush()
```

Which you can then use from the command line:

![Alt Text](https://bartparka.com//assets/images/chatgpt.gif)

P.S. I used [this article](https://medium.com/@pczarkowski/how-to-make-an-animated-gif-of-your-terminal-commands-62b08dfb6089) to produce the animated command line action gifs.

Install the following:

1. [ttyrec](https://github.com/mjording/ttyrec)
2. [ttygif](https://github.com/icholy/ttygif)
3. [gifsicle](https://github.com/kohler/gifsicle)