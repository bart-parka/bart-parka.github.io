---
title: "Blog Upgrade - Moving Images to S3"
date: 2022-09-26T17:52
tagline: "Moving Photos and Images out of the Git Repo for this site"
header:
  overlay_image: https://bart-parka-blog-assets.s3.eu-west-2.amazonaws.com/images/overlays/lacblanc.jpg
  overlay_filter: 0.3 # same as adding an opacity of 0.5 to a black background
  caption: "Photo credit: [**Bart Parka**](https://www.instagram.com/bart_parka/)"
  actions:
  - label: "See Example Code"
    url: "https://github.com/bart-parka/terraform-images-s3"
categories:
  - blog
tags:
  - GitHub Pages
  - S3
  - AWS
---

Moving photos and pictures out of the Git Repo to S3 Storage.

## Why?

I'm planning on making this blog more "photography" focused in the future - as that is one of my main hobbies. Keeping all the assets stored inside the same Git Repo may be convenient but it's not sustainable in the long run. The obvious choice was to start storing them in S3. A few things to note:

* The S3 bucket is publicly accessible - so that I can easily link the objects in post front matter, for example:
  ```
  header:
  overlay_image: https://bart-parka-blog-assets.s3.eu-west-2.amazonaws.com/images/overlays/lacblanc.jpg
  ```
  Obviously please never make your S3 buckets public - unless you know what you're doing (or don't care)!

## Terraform Repo

The example code repo looks as follows:

```
.
├── README.md
├── images
│   ├── 2021-07-22-docker-inspec
│   ├── 2022-09-23-github-pages-domain
│   └── overlays
├── main.tf
├── providers.tf
├── terraform.tfvars
└── variables.tf

```

I put the images folder in `.gitignore` as to not commit the image files to git, but the TF code will:

1. Create a public S3 Bucket
2. Upload all the images to the bucket, maintaining the folder structure

## Future Improvements

* The photos/pictures take a while to load - I need to consider potentially using a CDN to cache these assets and improve user experience.
* Also, I am not sure of GitHub Pages performance. While it's a quick and easy solution at some point in the future I may end up running this blog off of an EC2 instance or similar to ensure it is performant.
* I need to compress the images properly - no need for them to be super high quality for the blog.