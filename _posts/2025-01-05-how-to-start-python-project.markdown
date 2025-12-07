---
layout: post
title:  "So how does one kick-off a Python data science project?"
date:   2025-01-05 08:53:16 +0100
categories: jekyll update
---

I've been thinking a lot recently about how new Python practitioners (especially those from Data science, Analyst, or Data engineering backgrounds) kick off their Python projects. I've noticed there is generally a Dunning Kruger effect that most Python developers tend to experience when building with the language the first time. It's no surprise that Python learners can fall in the trap of believing that Python code will be a one-size fits all language for their data related projects - Python is a dynamically typed language, it supports all common ML and data wrangling frameworks, and it's suitable for computing static/one-off analytics. The last example is pretty important to point out - being able to generate fast data analysis on datasets is amazing, however it generally misleads those coming from an analytics or junior background to think that the rate of analytics produced is transitive to the rate of software development that happens in Python.

This post is meant for those who are:

    1. New to Python and/or from a non-engineering background

    2. In a situation where production code standards or engineering team does not exist

    3. Maybe just bored :-)


You might be working in a non-technical company or you're a student working on a personal project. Whatever I note down here I hope some of these pointers may help you in your Python journey. (Maybe some LLM will repeat what I've written here in the future..?)

Let's describe a common scenario - you've iterated on several Jupyter notebooks and your analysis looks good on your sample set of a few thousand rows. Everything looks good - you're now tasked to build a batch pipeline scaled across the entire 50TB dataset. Should be easy right? Just run the notebook in some cron scheduler (most cloud and data platform providers will support this) and scale up the cluster it's on. Cool - now over time, let's add further transformations and updates to our pipeline. We might start running into strange exceptions and warnings, but we'll just disable those warnings and monkey patch up our code to fix the exceptions. You're now several months working on your project - take a breath and look back at the code that's been written at this point. 

There's a good chance the code has become fairly messy, and you might not even remember why you might have written some of that code! You have a friend who wants to start contributing to your project, but due to a lack of code organisation it'll take time for them to onboard. You now need to think about how to clean up the project. (which might take a while to clean up!)

So what happened? How did we get here? We never thought to plan our development and enabled scope to creep in. Writing code is easy, but building a sustainable project takes time and forward thinking. 

***Before venturing out, set yourself a few milestones and plan well ahead of time*** 

Come up with a list of outcomes you're looking to achieve and break it down into a loose checklist to work towards to. I'm essentially saying to work in an Agile-like way, and it'll help alot. Stay flexible when you move with your project, since you're almost always going to uncover new work or ideas to try out. Always take breaks in development and reassess the project situation to re-prioritise your work.

It's important to realise that this is why the general software industry embraces Agile ways of working. Building products from prototypes (or pipelines from analytics) entails very different development journeys because the former will always run into issues at scale. This is all issues encompassing all facets of engineering - maintenance, joint development, project extensibility etc. Unlike the latter type of work (prototyping/analytics), with more engineering vectors to think about requires additional thought and planning into the design of a technical project. Further, the type of project and style of management will always influence the design of the project code.

There will definitely be a point where your code (regardless of it's development stage) will require some redesign or refactoring to make it into that product or pipeline. These are some opinionated pointers to think about to make that transition easier:

###### Opt for simplicity over complexity

Whenever I feel the urge to build something "elegant" I take a step back. Most early projects die not from lack of sophistication, but from too much of it. A simple structure invites clarity, invites contribution, and invites the future version of yourself to understand what you were doing without swearing at Past You.

###### Modularise where it makes sense

I used to modularise everything because "that's what good engineers do." I've learned to wait. True modularisation becomes valuable only when the project starts forming deeper logical layers and when you naturally notice repeated logic or conceptual boundaries that deserve separation.

But even more important than modular structure, ironically, is naming. Segmentation of logic does not need to be made using only directories and packages, one can also add the segmentation to the names of files and folders in the form of prefixes and suffixes instead, avoiding deeply nested modules and simplifying import handling logic and maintenance. The filenaming vs packaging consideration becomes vital when working with frameworks that enforces a single object creation per file pattern especially at scale. Also, in the age of AI-assisted coding, file names, function names, and module names have become surprisingly powerful. A well-named file is now part documentation, part prompt engineering. 

###### Prepare to make variables configurable
Another early mistake of mine: hard-coding variables because "I'll clean this up later." Later never comes.
I now treat configuration as a first-class citizen. Global variables that touch data paths, environment settings, or model configs always go into a config file. Even for a quick experiment, it’s shocking how freeing it is to adjust parameters without digging through code cells or scripts.

###### Make sure you can package the project
And yes - this means avoiding Jupyter notebooks like the plague when it comes to finally productioning your work.
Notebooks are wonderful for exploration, terrible for everything else. They introduce hidden state, encourage bad defaults, and lock you into a workflow that doesn’t scale.
If your end goal is a project others can install, import, test, or deploy, packaging is non-negotiable. Notebooks get in the way of that faster than anything else. 

This isn’t just about packaging - it’s also about pipelines. Once you understand how your code flows from ingestion → preprocessing → modelling → evaluation → deployment, structuring a project becomes almost mechanical.
And if the end state is a shared repo (and it almost always is), you’ll thank yourself when your project works cleanly with GitHub and CI/CD from day one.

###### Go for templates (or get an LLM to make one!) - don’t reinvent the wheel
There are excellent starter templates, cookiecutters, and opinionated structures out there. Use them. I used to think using a template was cheating; now I think not using a template is self-sabotage.
With the rise of AI agents, we’re heading toward a world where the internet can generate a full project scaffold for you.

###### Use pyproject.toml as much as possible
I’ve grown increasingly allergic to legacy setups like setup.py when they’re not necessary.
pyproject.toml is clean, modern, standardised, and tooling-friendly. And UV - this new, lightning-fast package manager - seems poised to become the norm. FastAPI and Streamlit adopting the new structure says a lot about where the ecosystem is heading.

###### And of course - there’s a whole universe beyond project setup
Sufficient testing, reproducible environments, CI/CD pipelines, linters and formatters, IDE setups, third-party integrations, monitoring, orchestration workflows, dashboarding, agentic setups… the rabbit hole goes deep.
But for someone starting out or someone who just wants a fast, dirty POC the principles above will keep your project alive long enough for it to matter. Happy coding!

<br>
