---
title: 2018 Resume Continuous Deployment Pipeline
date: 2018-03-11 15:54:12
tags:
  - linux
  - markdown
  - aws
  - travis
---
In my previous post [2018 Website Continuous Deployment Pipeline](https://damir.tech/2018-website-pipeline) I forgot to include one crutial part of my website - my [Resume](https://damir.tech/resume/) Page.

<!-- more -->

# Intro

I wanted a fresh resume. My previous where total and utter garbage in regards to formatting and readability, so it was time for a new one. I was also getting sick of editing my resume page and forgetting to update my PDF resume, and vise versa, I needed a way to edit once and update both my resume page on my website and my resume PDF.

# Solution

Only 2 requirements had to be fulfiled:

- the generated PDF had to be readable and clean
- there had to be an option to generate from markdown

I stumbled upon a python tool called [grip](https://github.com/joeyespo/grip) which can generate a `.html` page from a markdown file and I also knew of a tool to generate a `.pdf` from a `.html` named [wkhtmltopdf](https://github.com/wkhtmltopdf/wkhtmltopdf). So my first idea was to somehow use these two to generate a PDF but I ran into too many issues and decided to persue another route.

[markdown-resume](https://github.com/there4/markdown-resume) was the solution. It satisfies all my requirements and adds multiple themes as an extra. This along with [travis ci](https://travis-ci.org) seemed to offer me everything I needed.

# Flow

To sum up the general workflow of adding and editing my resume

- `git push` locally edited page to my github [resume](https://github.com/ddulic/resume) repo
- travis ci is set up to watch the repo and on a commit execute the build details in `.tavis.yaml` in the root of the project
- travis adds the header and footer as well as the download button needed for the page to be full
- travis generates the `.pdf` from the markdown file and pushes them to [hexo-website](https://github.com/ddulic/hexo-website) which triggers the repo's travis build

# Setup

Just as my last post, everything is open and can be found on my github, along with the basic understanding of how everything works which is explained in this post you should have no problem 1:1 copying and modifying to fit your needs.

## AWS

Unlike the last part, the AWS setup requires the least work and explanation since it just hooks into the previous AWS setup.

## Docker

I was in the process conteineratzing various services at my job, so why not conteinerize markdown-resume while at it. After all, practice makes perfect :)

But it was easier said then done... markdown-resume is in PHP and I had some formating problems in the resulting PDF. It's been years since I touched PHP to be able to fix it my self, luckily someone helped me with my [issue](https://github.com/there4/markdown-resume/issues/65).

The final Dockerfiles can be found in my [fork](https://github.com/ddulic/markdown-resume) of the project which includes the fixes described above.

# Final Thoughts

It is so relaxing to be able to edit once and update both my resume PDF and my resume page. The next step is to update my Linkedin profile via [api](https://developer.linkedin.com/docs/guide/v2/people/profile-edit-api#) 🤔
