---
title: "ChatGPT vs. GPT-3"
date: 2023-01-24T16:00
tagline: "Feeding tweets to OpenAI's two language models."
header:
  overlay_image: https://bart-parka-blog-assets.s3.eu-west-2.amazonaws.com/images/overlays/lacblanc.jpg
  overlay_filter: 0.3 # same as adding an opacity of 0.5 to a black background
  caption: "Photo credit: [**Bart Parka**](https://www.instagram.com/bart_parka/)"
  actions:
  - label: "See Example Code"
    url: "https://github.com/bart-parka/chatgpt-tests"
categories:
  - blog
tags:
  - GPT-3
  - OpenAI
  - Twitter
  - Elon-Gator
---

At the moment building anything on top of the ChatGPT beta is cumbersome due to no official API being available (I have been using the Python wrapper as described in one of my previous posts). This will no doubt be monetised once ChatGPT is out of beta. OpenAI do support an API for their previous models (GPT-3) with a free-tier quota.

**Info**: See this article for a [nice write up](https://dev.to/ben/the-difference-between-chatgpt-and-gpt-3-19dh) of differences (and similarities) between ChatGPT and GPT-3.
{: .notice--info}

**Info**: Since I wrote this post OpenAI have released a limited invite only [paid ChatGPT offering](https://analyticsindiamag.com/openai-releases-paid-chatgpt-professional/).
{: .notice--info}

I thought it may be interesting to see the outputs produced by the older model in comparison to ChatGPT when feeding in tweets using the Elon-gator script I built in one of my previous posts. I am finding it impossible to log on to ChatGPT at the moment as it seems to be permanently at capacity - but I've managed to run the elon-gator both using ChatGPT and GPT-3 to compare (see below).

This also sets me up nicely for when OpenAI finally release API support for ChatGPT, which one can assume will be similar if not the same as the existing API for other models.

I'll be using Python in my examples and you can sign up for an OpenAI account yourself to re-create this example:

1. Generate API Key on [OpenAI website](https://beta.openai.com/account/api-keys)
2. [Example Authentication](https://beta.openai.com/docs/api-reference/authentication)
3. The `openai.organization` header is only required if you're a member of multiple orgs

Here is the function which will explain/contextualise a tweet:

```python
def elongate_tweet_gpt3(text):

    COMPLETIONS_MODEL = "text-davinci-003"
    COMPLETIONS_API_PARAMS = {
        # We use temperature of 0.0 because it gives the most factual answer.
        "temperature": 0.0,
        "max_tokens": 300,
        "model": COMPLETIONS_MODEL,
    }

    # Removing any non-printable characters before feeding to ChatGPT, not sure if required
    printable = set(string.printable)

    question_and_tweet = "{}".format(random.choice(questions)) + ''.join(filter(lambda x: x in printable, text))

    logging.debug("GPT-3 question and tweet:\n" + question_and_tweet)
    response = openai.Completion.create(
        prompt=question_and_tweet,
        **COMPLETIONS_API_PARAMS,
    )
    output = response["choices"][0]["text"].strip(" \n")
    print(output)  # This step seems to have fixed ratelimiter errors, maybe need a backoff or sleep here
    return output
```

where `questions` are a list of possible prompts to use, something like:

```python
questions = [
    "Can you explain this tweet: ",
    "Can you expand on this tweet: ",
    "Can you give me background on this tweet: ",
    "Can you give some context for this tweet: "
]
```

I have generally found ChatGPT to produce more verbose output than GPT-3 (no surprise I guess), that's even with the [temperature](https://algowriting.medium.com/gpt-3-temperature-setting-101-41200ff0d0be) set to 1 - meaning the most "creative" response.

There are a few improvements I've made to the `elongate.py` since my last post:

1. Added a [TinyDB](https://tinydb.readthedocs.io/en/latest/) to cache tweets and gpt responses - I only have free-tier quotas for both APIs so want to limit requests if possible.
2. Added arguments so you can produce the GPT response from cached tweets
3. The ChatGPT and GPT-3 responses are also cached so can be retrieved later

Future enhancements I'd like to implement:

* The tool should be able to follow links in tweets, scrape the content, and 'elongate' accordingly.
* The tool should be able to filter out tweets that can't/haven't been contextualised properly.
* We can provide context/teach the model, there is a good example of this [here](https://github.com/openai/openai-cookbook/blob/main/examples/Question_answering_using_embeddings.ipynb).

Elon-Gator
=========

Below are some examples of GPT-3 vs. ChatGPT output. The below was generated automatically.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">some remote, but mostly in person.<br><br>imo most tech companies who rushed to full remote permanently made a big mistake, and the cracks are starting to show. (works for some!)<br><br>hard work is even less fashionable than in-person work, but i still really believe in that too! <a href="https://t.co/3cMz461UBK">https://t.co/3cMz461UBK</a></p>&mdash; Sam Altman (@sama) <a href="https://twitter.com/sama/status/1617629569628123136?ref_src=twsrc%5Etfw">January 23, 2023</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

## ChatGPT

This tweet expresses the opinion that many technology companies that quickly transitioned to full remote work during the pandemic made a mistake, as the challenges and limitations of remote work are becoming more apparent. The tweet suggests that some companies may be better suited to remote work, but that for many, in-person work is still more effective. The tweet also mentions the importance of hard work, suggesting that it is not as popular as it once was, but that the author still values it. The tweets end with the link of an article or a tweet discussing the topic.

## GPT-3

The tweet is referring to the trend of many tech companies transitioning to full-time remote work due to the COVID-19 pandemic. The author of the tweet is suggesting that this transition was a mistake for many tech companies, as the transition has caused cracks to begin to form. The author believes in the value of in-person work, but also acknowledges that remote work works for some people.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">If you&#39;re interested in DAOs and network states, here are some good concepts to read about:<br><br>- community organizing<br>- mutual aid<br>- dual power<br>- shadow cabinet<br><br>Media is the air game, but organizing is the ground game.</p>&mdash; Balaji (@balajis) <a href="https://twitter.com/balajis/status/1617497898606030849?ref_src=twsrc%5Etfw">January 23, 2023</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

## ChatGPT

This tweet is providing a list of concepts related to decentralized autonomous organizations (DAOs) and network states that the author believes are worth reading about. The concepts listed are:

- Community organizing: This refers to the process of bringing people together to take collective action to address shared problems or goals.

- Mutual aid: This refers to the practice of people coming together to provide each other with the resources they need to survive and thrive.

- Dual power: This refers to the existence of both a government and a parallel organization, representing the people and acting in their interests, that can serve as a check on the power of the government.

- Shadow cabinet: This refers to a group of opposition politicians in a parliamentary system who are appointed to mirror the roles of the government cabinet, and who are ready to take office if their party comes to power.

The tweet implies that these concepts are important for understanding how decentralized autonomous organizations (DAOs) and network states work. The author also suggests that media is the air game, meaning that it is important for getting the message out, but organizing is the ground game, meaning that it is important for building the infrastructure and support needed to achieve the goals of the movement.

The tweet is likely aimed at people who are interested in decentralized autonomous organizations (DAOs) and network states, it suggests that in order to understand how these organizations work, it is important to understand how to organize people and resources at the grassroots level.

## GPT-3

A DAO (decentralized autonomous organization) is a structure that operates with greater autonomy and transparency than a traditional corporation. It can be used to more accurately and efficiently reflect the community’s interests, such as by voting on the distribution of resources or allocating resources to complete tasks. Network states are similar in concept to organizational networks, but strive to decentralize the power of the elites who dominate many organizational networks and offer more power to the people.

To gain a better understanding of the concepts behind these DAOs and network states, readers may want to explore the various types of organizing models, such as community organizing, mutual aid, dual power, and shadow cabinet. Community organizing is a practice of empowering communities to bring about change. Mutual aid emphasizes cooperation between individuals and groups, often through bartering and sharing resources. Dual power is a concept closely analogous with dual sovereignty, meaning that the people can have a degree of self-rule while still working within the larger framework of a state or governance. Shadow cabinet is a body of representatives from the opposition party that hold government positions in shadow to those actually in power.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">The Qanat Firaun (Gadara Aqueduct) is a testament to the remarkable engineering capabilities of the <a href="https://twitter.com/hashtag/Roman?src=hash&amp;ref_src=twsrc%5Etfw">#Roman</a> <a href="https://twitter.com/hashtag/Empire?src=hash&amp;ref_src=twsrc%5Etfw">#Empire</a> who constructed it, and the <a href="https://twitter.com/hashtag/Persians?src=hash&amp;ref_src=twsrc%5Etfw">#Persians</a>, who are the first known culture to have developed qanats. <a href="https://t.co/tLpLJP7wK3">https://t.co/tLpLJP7wK3</a></p>&mdash; Ancient Origins (@ancientorigins) <a href="https://twitter.com/ancientorigins/status/1617764134363320322?ref_src=twsrc%5Etfw">January 24, 2023</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

## ChatGPT

The Qanat Firaun, also known as the Gadara Aqueduct, is an ancient water management system that was built by the Romans during their occupation of the area that is now modern-day Jordan. The aqueduct was used to transport water from underground sources to the city of Gadara, which was a major center of the Roman province of Arabia. The tweet is highlighting the remarkable engineering capabilities of both the Romans and the Persians with respect to the aqueduct. It also points out that the Persians are the first known culture to have developed Qanats, which are underground channels for water. The Qanat Firaun is considered an important historical artifact and an example of ancient engineering. The tweet contains a link which probably leads to some information related to the aqueduct or the history of the area.

## GPT-3

This tweet describes the Qanat Firaun, which is an ancient Gadara Aqueduct built by the Roman Empire. The tweet emphasizes the impressive engineering capabilities of both the Romans and the Persians, noting that the Persians were the first known culture to have developed Qanats.