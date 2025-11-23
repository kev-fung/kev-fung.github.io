---
layout: post
title:  "So how does one kick-off a python data science project?"
date:   2025-01-05 08:53:16 +0100
categories: jekyll update
---

I've been thinking a lot about how new Python developers/engineers, analysts, or simply fans of statistically inclined Amazonian snakes kick off their first data science projects recently. Most of us start the same way: with very little experience, very little time, and a burning need to get a POC off the ground yesterday.
And honestly? There’s no universally "correct" way to structure a project. But after witnessing numerous projects either grow gracefully or collapse under their own weight over the last 5 years - a little foresight can extend a project's lifespan dramatically especially in a professional setting.

If anyone is the same, we're usually all excited to get stuck into building that cool machine learning idea still burning in our minds... There is an urge to immediately get stuck into the code to see results. You think knowing Python and a Jupyter notebook is enough to get started, and you're confident in the setup you currently have. But it's a big mistake. You're a month working into the project now, and you start producing many different iterations of the same notebook. Duplicated cells, cryptic comments, unclear variable names, and maybe even a hacked up dashboard you decided to build with FastAPI exists now because it got too confusing trying to visualise all the plots you have sprawled across your notebooks.

So what do I suggest to minimise this pitfall? **Set breakpoints to stop and plan for the moment - move slow to move fast.** This is a crucial point for new developers and data scientists to realise. One must take a step back, think and observe on the state of their project, then take the time out to make the many refactoring or design changes they now need for their project. Essentially - make it a habit to iteratively plan and prioritise your work as you move along with your idea. It won't be fun, but self management is a must. 

Some further pointers I think about now when I start to work on a new Python project:

###### Opt for simplicity over complexity

These days, whenever I feel the urge to build something "elegant" I take a step back. Most early projects die not from lack of sophistication, but from too much of it. A simple structure invites clarity, invites contribution, and invites the future version of yourself to understand what you were doing without swearing at Past You.

###### Modularise where it makes sense

I used to modularise everything because "that's what good engineers do." I've learned to wait. True modularisation becomes valuable only when the project starts forming deeper logical layers and when you naturally notice repeated logic or conceptual boundaries that deserve separation.
But even more important than modular structure, ironically, is naming. Segmentation of logic does not need to be made using only directories and packages, one can also add the segmentation to the names of files and folders in the form of prefixes and suffixes instead, avoiding deeply nested modules and simplifying import handling logic and maintenance. Also, in the age of AI-assisted coding, file names, function names, and module names have become surprisingly powerful. A well-named file is now part documentation, part prompt engineering. 

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
I’ve grown increasingly allergic to legacy setups like setup.py or requirements.txt when they’re not necessary.
pyproject.toml is clean, modern, standardised, and tooling-friendly. And UV - this new, lightning-fast package manager - seems poised to become the norm. FastAPI and Streamlit adopting the new structure says a lot about where the ecosystem is heading.

###### And of course - there’s a whole universe beyond project setup
Sufficient testing, reproducible environments, CI/CD pipelines, linters and formatters, IDE setups, third-party integrations, monitoring, orchestration workflows, dashboarding, agentic setups… the rabbit hole goes deep.
But for someone starting out or someone who just wants a fast, dirty POC the principles above will keep your project alive long enough for it to matter. Happy coding!

<br>
