---
layout: post
title: "Github page and custom domain"
date: 2023-09-16 10:00:00
categories: Hosting
tags: dns_record github_action  # TAG names should be lowercase
image:
    path: /assets/img/headers/github_page.webp
---

## ***Coming Soon***

```bash
$ gem install bundler jekyll
$ jekyll new myblog
$ cd myblog
$ bundle exec jekyll serve
```
## Text and Typography
```bash
<hr style="border:2px solid blue">
```
## Prompts

```bash
<!-- markdownlint-capture -->
<!-- markdownlint-disable -->
> An example showing the `tip` type prompt.
{: .prompt-tip }

> An example showing the `info` type prompt.
{: .prompt-info }

> An example showing the `warning` type prompt.
{: .prompt-warning }

> An example showing the `danger` type prompt.
{: .prompt-danger }
<!-- markdownlint-restore -->
```

## Dark/ Light mode and Shadow
```bash
The image below will toggle dark/light mode based on theme preference, notice it has shadows.

![light mode only](/posts/20190808/devtools-light.png){: .light .w-75 .shadow .rounded-10 w='1212' h='668' }
![dark mode only](/posts/20190808/devtools-dark.png){: .dark .w-75 .shadow .rounded-10 w='1212' h='668' }
```
## Video
```bash
{% include embed/youtube.html id='Balreaj8Yqs' %}
```
## Footnote

```bash
## Footnote

Click the hook will locate the footnote[^footnote], and here is another footnote[^fn-nth-2].
```
## Reverse Footnote
```bash
## Reverse Footnote

[^footnote]: The footnote source
[^fn-nth-2]: The 2nd footnote source
```

