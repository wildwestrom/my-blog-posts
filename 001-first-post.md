---
title: My First Blog Post
subtitle: An outline of my attempt at creating my own static site generator.
date: 2021-04-15
tags:
  [
    programming,
    clojure,
    clojurescript,
    CI/CD,
    jamstack,
    tailwindcss,
    react,
    reagent,
    raspberry-pi,
    shadow-cljs
  ]
author: Christian Westrom
id: first-blog-post
---

# Inspiration

I decided I wanted to create my own website. Now how was I going to do that? Well with clojure of course! I decided that the most maintainable way to create a static site was by using a static-site generator. So I got some inspiration from other static site generators. Special thanks to [yosevu](https://github.com/yosevu/shadow-static) for his static site generator that makes use of shadow-cljs and Tailwind CSS. My design is mostly based off his project.

# Choice of stack

So what's my current stack?

- Clojure/ClojureScript
- shadow-cljs
- Reagent
- Tailwind CSS
- Gitlab CI-CD
- Caddy
- Rasberry Pi OS

# CI/CD Pipelines

This was a real pain to figure out. For someone just getting into programming I think it took way too long just to figure out how to build a project and deploy it by using ssh. Here's a working iteration of the script:

```yaml
stages:
  - deploy

workflow:
  rules:
    - if: '$CI_COMMIT_BRANCH'

deploy:
  stage: deploy
  image: clojure:openjdk-17-tools-deps-alpine
  before_script:
    - apk add openssh-client
    - eval $(ssh-agent -s)
    - echo "$PI_SSH_PRIVATE_KEY" | tr -d '\r' | ssh-add -
    - mkdir -p ~/.ssh
    - chmod 700 ~/.ssh
  script:
    - apk add yarn
    - yarn install
    - yarn release
    - ssh -q -p $PI_SSH_PORT -o "StrictHostKeyChecking=no" deployer@westrom.xyz "rm -rf /var/www/westrom.xyz/html/*"
    - scp -q -P $PI_SSH_PORT -o "StrictHostKeyChecking=no" -r public/* deployer@westrom.xyz:/var/www/westrom.xyz/html
  artifacts:
    paths:
      - public
  rules:
    - if: '$CI_COMMIT_BRANCH == "master"'
```

As someone who never delved into other CI/CD technologies, Gitlab seems to do a pretty good job. Not only is Gitlab a pretty nice git repository, having integrated CI/CD helps a lot. I think their site navigation could be improved, but it's pretty nice once it's up and running. I currently have this set to copy the static content to my raspberry pi. I'm using caddy to serve the static pages and keep my TLS/HTTPS certificates up to date.

# Fast Feedback

The REPL is indispensible when it comes to programming. That's one of the main reasons I love clojure so much. There's nothing like typing `, e e` in spacemacs and seeing an S-expression evaluate on the spot. It just makes the process of writing code so much more enjoyable. I wanted to get fast feedback in as many areas as possible, so I'm centering my project around this goal. Shadow-cljs was an obvious choice for this. The npm support and compilation is such a breeze. To this end, I also opted to use Tailwind CSS. This allows me to write my CSS directly into my reagent components.

# To be improved

Obviously there's much to be desired:

- The CI script is quite inefficient. Although it uses an image of alpine linux to start, every time it must install npm and yarn as well.
- I would also rather use rsync instead of scp so I can just transfer the deltas rather than the entire project.
- Reagent is great because of its beautiful hiccup syntax, however I don't even know how any of this react magic works. I might just try to figure out how to use plain hiccup so I don't have a bunch of javascript to weigh me down. If there's a way to compile reagent cljs code to static HTML pages, then that would be awesome.
