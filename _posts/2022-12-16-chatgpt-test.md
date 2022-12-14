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

Hello Skynet? 🤖

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

## Why is this useful?

With this command line utility we can now start playing, for example, I asked ChatGPT to explain the code snippet above (I highglighted and copied it into my clipboard, then used the `pbpaste` MacOS command to feed it into our command line utility):

`python main.py "Explain this code snippet: $(pbpaste)"`

Here is what ChatGPT had to say:

---

This code imports the `ChatGPT` class from the `chatgpt_wrapper` module and creates an instance of the `ChatGPT` class, which is stored in the `bot` variable. Then, the code enters a loop that iterates over the response stream returned by the `ask_stream` method of the `bot` instance. For each response in the stream, the code writes the response to the standard output stream and flushes the output stream, which ensures that the response is immediately displayed to the user.

`ChatGPT` is likely a class that wraps a GPT-3 language model, allowing you to easily ask it questions and receive responses. The `ask_stream` method takes a query string as input and returns a stream of responses that the GPT-3 model generates in response to the query. The responses are generated incrementally, so they can be immediately displayed to the user as they are generated, rather than waiting for the entire response to be generated before displaying anything.

The code uses the `sys.argv[1]` list to access the first command-line argument passed to the script, which is assumed to be the query string to be passed to the `ask_stream` method. This allows the user to specify the query when running the script from the command line, for example:

```
python my_script.py "What is the capital of France?"
```

In this case, the query string "What is the capital of France?" would be passed to the `ask_stream` method, and the resulting stream of responses would be displayed to the user as they are generated.

---

That's pretty impressive if you ask me!

ChatGPT is skilled in Python, but let's try a similar thing with a bit of Terraform, here is the snippet I'm feeding in:

```
locals {
  bucket_name = "bart-parka-blog-assets"
}

module "s3" {
  source = "terraform-aws-modules/s3-bucket/aws"
  bucket = local.bucket_name
  tags   = var.tags

  versioning = {
    enabled = false
  }

  attach_policy                         = true
  policy                                = data.aws_iam_policy_document.bucket_policy.json
  attach_deny_insecure_transport_policy = true

  # Allow deletion of non-empty bucket
  force_destroy = true

  # S3 bucket-level Public Access Block configuration
  block_public_acls       = false
  block_public_policy     = false
  ignore_public_acls      = false
  restrict_public_buckets = false
}
```

---

This code defines a Terraform module named "s3". The module creates an S3 bucket with the name specified in the `bucket_name` local variable. The bucket's tags, versioning settings, and access policies are specified by the `tags`, `versioning`, `attach_policy`, `policy`, `attach_deny_insecure_transport_policy`, `force_destroy`, `block_public_acls`, `block_public_policy`, `ignore_public_acls`, and `restrict_public_buckets` variables, respectively. These variables are passed to the module when it is called elsewhere in the Terraform configuration.

---

Not quite as good, but still sensical!

I think there are plenty of potential use cases here - to be continued!

---

P.S. I used [this article](https://medium.com/@pczarkowski/how-to-make-an-animated-gif-of-your-terminal-commands-62b08dfb6089) to produce the animated command line action gifs.

Install the following:

1. [ttyrec](https://github.com/mjording/ttyrec)
2. [ttygif](https://github.com/icholy/ttygif)
3. [gifsicle](https://github.com/kohler/gifsicle)