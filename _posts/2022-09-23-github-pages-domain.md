---
title: "GitHub Pages and Custom Domains"
date: 2022-09-23T17:52
tagline: "How to attach a custom domain to your GitHub Page"
header:
  overlay_image: /assets/docker-terraform.jpg
  overlay_filter: 0.3 # same as adding an opacity of 0.5 to a black background
  caption: "Photo credit: [**Bart Parka**](https://www.instagram.com/bart_parka/)"
categories:
  - blog
tags:
  - GitHub Pages
  - Route53
  - DNS
---

Been a while! I thought I'd resurrect the blog and treat myself to my own domain. Linking them turned out to be quite easy - read below.

## Custom Domain on AWS

I chose to register my new domain using AWS and Route53, it was really easy:

* Follow the steps outlined in the documentation [here](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/domain-register.html#domain-register-procedure). I initially wanted an `.io` domain but damn they're expensive (apparently only because they are trendy). I settled for `bartparka.com`.
* Once you have your domain and it has been successfully registered, you will need to create two records if you want both `example.com` and `www.example.com` to correctly resolve/redirect to your blog pages.
* The steps are outlined [here](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site), but in essence you need to create two records (see screenshot below):
  * A record - pointing at GitHub Pages ips in above link.
  * CNAME record - pointing at the `<user>.github.io` address of your GitHub pages url.
* Once done, navigate to your repo -> Settings -> Code and automation -> Pages and change the Custom domain to match your newly created domain (see screenshot below).
* You may have to wait a few minutes to tick Enforce HTTPS.

<figure>
  <img src="/assets/images/2022-09-23-github-pages-domain/aws-screenshot.png">
  <figcaption>Route53 Records - you will have to create the A and CNAME records.</figcaption>
</figure>

<figure>
  <img src="/assets/images/2022-09-23-github-pages-domain/github-screenshot.png">
  <figcaption>GitHub Pages config page - change the Custom domain to match your newly acquired domain.</figcaption>
</figure>

That's it! 
