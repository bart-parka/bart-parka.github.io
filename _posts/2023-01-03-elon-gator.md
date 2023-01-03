---
title: "The Elon-Gator"
date: 2023-01-03T16:22
tagline: "Feeding Tweets to ChatGPT - what could go wrong!"
header:
  overlay_image: https://bart-parka-blog-assets.s3.eu-west-2.amazonaws.com/images/overlays/glacier.jpg
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
  - Twitter
---

Happy new year all!

Before Christmas I had this crazy idea to feed tweets to ChatGPT. Twitter can be an incredibly toxic place and, given the character limit, many things you read lack context. My idea was to pull recent tweets from some of my favourite accounts and ask ChatGPT to give me more background.

[Skip to the good part](/blog/elon-gator/#elon-gator) or read on:

As you are about to see, this works relatively well for certain types of tweets but fails completely when the content of the tweet is an external link or image for example (I believe ChatGPT has been trained on data up to 2021 so it won't be very good at explaining current events, and I think at the moment it is prevented from following links to retrieve additional info.)

I was able to put together a very rough example by using the Twitter API, and the ChatGPT Python wrapper discussed in the previous post (the ChatGPT wrappers are the best I could find until OpenAI release a proper API, which no doubt will be paid for). The `elongate.py` (pardon the pun) in the example code linked above generates a markdown file which I can paste in here directly. Notice also the few prompt examples I included in the script, the quality of ChatGPT's outputs depends a lot on the prompt you provide, which has spawned a whole new field of "[prompt-engineering](https://en.wikipedia.org/wiki/Prompt_engineering)".

If you are interested in using it yourself you will have to:

1. Sign up for Twitter API (you can find instructions [here](https://developer.twitter.com/en/docs/twitter-api/getting-started/getting-access-to-the-twitter-api))
2. Sign up for ChatGPT (read my previous [post](https://bartparka.com/blog/chatgpt-test/))
3. Export a TOKEN environment variable with your Twitter bearer token, generated in step 1.

I will be making periodic enhancements to the rough Python script I shared here. I think one area where this technology has huge potential is in enabling "journalism". For example consuming a bunch of short-form text messages (tweets) in order to write a coherent and engaging long-form article.

It's hardly been a few weeks we have had trial access and people have already built products using this technology, despite the flakey work-around APIs:

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">It’s done!! 🚀🚀🚀🚀<br><br>Introducing Addy - Your AI email assistant 🤖<br><br>Addy is powered by ChatGPT and can write emails for you 10x faster in your preferred style and tone.<br><br>If you want early access, please retweet and comment below and I’ll send you an invite code! <a href="https://t.co/uikj2Irywi">pic.twitter.com/uikj2Irywi</a></p>&mdash; Michael Vandi (@michael_vandi) <a href="https://twitter.com/michael_vandi/status/1607422866416619520?ref_src=twsrc%5Etfw">December 26, 2022</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Anyway, below you can see the output of the elon-gator and it's obvious weaknesses at the moment. Enjoy - the rest of this post was auto-generated.

Elon-Gator
=========

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">After he discovered his principle of buoyancy, Archimedes allegedly yelled out &quot;Eureka!&quot; and ran naked through the city of Syracuse. The discovery was that important. The story goes that Archimedes made his great breakthrough when he noticed the water rise as he got into the tub.</p>&mdash; World of Engineering (@engineers_feed) <a href="https://twitter.com/engineers_feed/status/1605223509420302336?ref_src=twsrc%5Etfw">December 20, 2022</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

---
The story of Archimedes shouting "Eureka!" and running naked through the city of Syracuse is a well-known tale that has been passed down through history. According to the story, Archimedes was a Greek mathematician and scientist who lived in the city of Syracuse in the 3rd century BC. He is known for his contributions to mathematics, physics, and engineering, and is considered one of the greatest mathematicians of all time.

The story goes that Archimedes was tasked with determining whether a crown was made of pure gold or if it was a forgery made of a cheaper metal that had been coated in gold. Archimedes struggled to find a way to determine the composition of the crown, but one day, while getting into the tub, he noticed that the water level rose as he got in. This gave him the idea to measure the volume of the crown by submerging it in water and comparing its volume to the amount of water that was displaced. When he realized that the crown's volume was greater than it should have been if it were made of pure gold, he realized that it must have been a forgery.

Overjoyed at his discovery, the story goes that Archimedes shouted "Eureka!" (which means "I have found it!" in Greek) and ran naked through the streets of Syracuse, so excited that he forgot to put on his clothes. This story has become famous as a symbol of the excitement and joy that can come from making a great scientific discovery. However, it should be noted that the story may be more legend than fact, and there is no historical record to verify whether it actually happened.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">This has always been true, and we wanted to make it more explicit. If you have any proofs of applied extraordinary ability outside of academic credentials, our careers page now has a VIP-Access section: <a href="https://t.co/4J5rnpiL9S">https://t.co/4J5rnpiL9S</a></p>&mdash; tobi lutke (@tobi) <a href="https://twitter.com/tobi/status/1605217906853400577?ref_src=twsrc%5Etfw">December 20, 2022</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

---
This tweet appears to be referencing a company or organization that is looking to hire individuals with exceptional abilities or talents, and is willing to consider candidates who do not have traditional academic credentials. The tweet suggests that the company has made this policy more explicit by adding a "VIP-Access" section to their careers page specifically for candidates who can demonstrate their exceptional abilities through other means.

It is not uncommon for companies to look for candidates who possess specialized skills or expertise that may not necessarily be reflected in their academic credentials. For example, someone who has spent years working in a particular industry or who has developed a unique skill set through self-study or other non-traditional means may be a valuable asset to an organization, even if they do not have a traditional degree in that field. By including a VIP-Access section on their careers page, the company is likely trying to make it easier for such candidates to apply and be considered for positions, and to make it clear that they are open to considering candidates with a diverse range of backgrounds and experiences.


<blockquote class="twitter-tweet"><p lang="en" dir="ltr">“A good scientist is a person with original ideas. A good engineer is a person who makes a design that works with as few ideas as possible. There are no prima donnas in engineering.”<br>- Freeman Dyson</p>&mdash; World of Engineering (@engineers_feed) <a href="https://twitter.com/engineers_feed/status/1605208887573073921?ref_src=twsrc%5Etfw">December 20, 2022</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

---
This tweet is a quote from Freeman Dyson, a British-American theoretical physicist and mathematician who was known for his contributions to quantum electrodynamics and his work on solid-state physics, nuclear engineering, and astrophysics. In the quote, Dyson makes a distinction between the roles of scientists and engineers, stating that scientists are known for their original ideas, while engineers are known for their ability to create designs that work efficiently and effectively with as few ideas as possible.

Dyson's quote suggests that scientists are typically focused on generating new ideas and theories, while engineers are more concerned with practical application and implementation. He also implies that engineers are more focused on efficiency and effectiveness, and that they do not have the same ego or desire for personal recognition that some scientists may have.

It's worth noting that this quote may be oversimplifying the roles of scientists and engineers, and that there is a lot of overlap between the two fields. Many scientists also have practical skills and are concerned with the practical applications of their research, and many engineers also have creative ideas and are involved in the development of new technologies. However, Dyson's quote does highlight some of the key differences between these two fields and the different approaches that scientists and engineers may take to problem-solving.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Friends were asking what’s behind these tweets and encouraged me to share the answer <a href="https://t.co/BzOmu0m8dn">https://t.co/BzOmu0m8dn</a> <a href="https://t.co/ETfETqTgHp">pic.twitter.com/ETfETqTgHp</a></p>&mdash; tobi lutke (@tobi) <a href="https://twitter.com/tobi/status/1605191046782140416?ref_src=twsrc%5Etfw">December 20, 2022</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

---
It is not possible for me to provide context for this tweet as it is not clear what the tweet is referring to or what the content of the linked articles might be. Without more information, it is difficult to accurately interpret or respond to the tweet. In order to provide a more helpful response, it would be helpful to have more information about the specific tweets and articles that are being referenced.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Life tip: If you ever forget your WiFi password or you want to get your school WiFi password etc. just type this command into the command line of a computer already connected to that WiFi: <br><br>netsh wlan show profile WiFi-name key=clear</p>&mdash; World of Engineering (@engineers_feed) <a href="https://twitter.com/engineers_feed/status/1605139144929329153?ref_src=twsrc%5Etfw">December 20, 2022</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

---
This tweet is providing a tip for how to retrieve the password for a WiFi network using the command line on a computer that is already connected to that network. The tip suggests using the "netsh wlan show profile" command followed by the name of the WiFi network and the "key=clear" parameter to display the password for the network.

The "netsh" command is a utility in Windows operating systems that allows you to view and modify the network configuration of your computer. The "wlan" subcommand allows you to manage wireless LAN (local area network) settings, and the "show profile" subcommand displays the configuration information for a specific wireless profile. By specifying the name of the WiFi network and the "key=clear" parameter, you can view the password for that network in clear text.

This tip can be useful if you have forgotten the password for a WiFi network that you are already connected to, or if you need to retrieve the password for a WiFi network in order to connect another device to it. It is important to note that this tip is specific to Windows operating systems, and the exact steps and commands may vary depending on your specific version of Windows and the configuration of your computer.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Geometry reasoning test. <a href="https://t.co/53aTtuPFjx">pic.twitter.com/53aTtuPFjx</a></p>&mdash; World of Engineering (@engineers_feed) <a href="https://twitter.com/engineers_feed/status/1605085742916567042?ref_src=twsrc%5Etfw">December 20, 2022</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

---
It is not possible for me to provide background on this tweet as it does not provide any context or information about the content of the linked website. The tweet simply includes a link to a website with the label "Geometry reasoning test," which suggests that the website may be a test or assessment related to geometry and reasoning skills.

Without more information, it is not possible to accurately interpret or respond to the tweet. It would be helpful to have more information about the specific test or assessment being referred to, as well as the purpose and format of the test. Geometry is a branch of mathematics that deals with the study of shapes, sizes, and the properties of space. Reasoning skills refer to the ability to think logically and analytically, and are important for solving problems and making decisions. A test that assesses geometry reasoning skills may involve questions or tasks that require the use of spatial reasoning, problem-solving, and logical thinking skills.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">On Dec 20, 1966, J.R. Biard receives patent for Light-emitting diods. <a href="https://t.co/VDUsjji4ep">pic.twitter.com/VDUsjji4ep</a></p>&mdash; World of Engineering (@engineers_feed) <a href="https://twitter.com/engineers_feed/status/1605045072382332929?ref_src=twsrc%5Etfw">December 20, 2022</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

---
This tweet is referring to the fact that on December 20, 1966, J.R. Biard was granted a patent for light-emitting diodes (LEDs). LEDs are a type of electronic component that produces light when an electric current passes through it. They are known for their energy efficiency, long lifespan, and small size, and are widely used in a variety of applications including lighting, displays, and signaling.

J.R. Biard, who was a researcher at Texas Instruments at the time, was awarded the patent for his work on developing a practical and efficient method for producing visible light using semiconductor devices. This work laid the foundation for the modern LED industry and led to the widespread adoption of LEDs in a variety of applications.

Today, LEDs are an important part of many products and technologies, and are used in everything from television displays and computer monitors to automotive lighting and traffic signals. They are known for their energy efficiency, durability, and long lifespan, and have revolutionized the way we use and consume light.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Space fact:<br><br>Neptune takes nearly 165 Earth years to make one orbit of the Sun.</p>&mdash; World of Engineering (@engineers_feed) <a href="https://twitter.com/engineers_feed/status/1604999522438074368?ref_src=twsrc%5Etfw">December 20, 2022</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

---
This tweet is stating a fact about the planet Neptune, which is the eighth and outermost planet in the solar system. The tweet mentions that Neptune takes nearly 165 Earth years to make one orbit around the Sun, meaning it takes approximately 165 years for Neptune to complete one full revolution around the Sun.

Neptune is known for its long orbit, which is significantly longer than the orbits of the other planets in the solar system. It is located approximately 2.8 billion miles (4.5 billion kilometers) from the Sun, on average, and takes about 248.09 Earth years to complete one orbit around the Sun. This means that a year on Neptune is about four times longer than a year on Earth.

Neptune is also known for its bluish color, which is caused by the presence of methane in its atmosphere. It is a gas giant planet, meaning it is composed primarily of hydrogen and helium and has no solid surface. It is the fourth largest planet in the solar system, and is known for its strong winds and extreme weather conditions.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">„Don’t be careful, be competent“ — <a href="https://twitter.com/TomCruise?ref_src=twsrc%5Etfw">@TomCruise</a></p>&mdash; tobi lutke (@tobi) <a href="https://twitter.com/tobi/status/1604984705178296329?ref_src=twsrc%5Etfw">December 19, 2022</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

---
This tweet appears to be quoting actor Tom Cruise, who is known for his roles in films such as "Mission: Impossible," "Top Gun," and "Jerry Maguire." The quote, "Don't be careful, be competent," suggests that it is more important to be skilled and capable at something than it is to be cautious or careful.

It is not clear what context or situation the quote is being used in, or what specific message or advice Tom Cruise is trying to convey. Without more information, it is difficult to accurately interpret the quote. However, it could be interpreted as a call to focus on developing skills and expertise rather than simply being careful or avoiding risk. In some cases, being competent and confident in one's abilities can be more important than being careful, as it may allow a person to take on challenges and achieve success more effectively. 

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Still the best website: <a href="https://t.co/FKmVXLg0pI">https://t.co/FKmVXLg0pI</a><br><br>You can literally do everything there.</p>&mdash; tobi lutke (@tobi) <a href="https://twitter.com/tobi/status/1604967476479000576?ref_src=twsrc%5Etfw">December 19, 2022</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

---
It is not possible for me to provide background on this tweet as it does not provide any context or information about the content of the linked website or what specific features or capabilities the website has. The tweet simply includes a link to a website and the statement that it is "the best website" and that "you can literally do everything there."

Without more information, it is not possible to accurately interpret or respond to the tweet. It would be helpful to have more information about the specific website being referred to and the purpose or functions of the website. Without this information, it is not possible to accurately assess the capabilities or usefulness of the website.  
