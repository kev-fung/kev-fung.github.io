---
layout: post
title:  "How Code Evolves: On Frameworks, Teams, and the Dev Ceiling"
date:   2026-03-21 09:00:00 +0000
categories: jekyll update
---

Code is not written in a vacuum. It is written by people — people with specific mental models, specific blind spots, specific horizons of understanding. Over time, a codebase comes to reflect the ceiling of its authors. Not their floor, not their aspirations, but the habitual level at which they naturally operate. This is not a criticism. It is an observation about how things grow.

The question *"how will this codebase look in one year?"* is fundamentally a question about the people who will be touching it.

---

###### The data scientist's mental model

Data scientists arrive at Python through a particular door. They come, usually, from statistics, from Excel, from R, from Jupyter notebooks. They think *imperatively*. First do this, then do that, then print the result. They do not naturally think in terms of dependency graphs, lazy evaluation, type systems, or side-effect-free functions. They think in procedures.

This is not a deficiency. Imperative thinking is the most natural cognitive form of computation — it mirrors how we ourselves move through the world. We don't think: *"declare a function that, when given tomorrow morning, returns coffee."* We think: *"wake up, go to kitchen, make coffee."*

The problem emerges when these practitioners encounter frameworks designed by engineers who think differently — who think declaratively, functionally, who see a pipeline not as a sequence of steps but as a graph of dependencies to be resolved. The framework assumes a mental model its users do not have. And what follows from that mismatch is, almost always, the same.

---

###### The framework trap

Take Flyte as a concrete example. Flyte is genuinely impressive engineering. It handles distributed execution, data provenance, reproducibility, fault tolerance — things a data scientist would never think to build, or even know to want. But it makes assumptions. It assumes you understand that when you pass a large DataFrame between tasks, Flyte is serialising it to object storage and back. It assumes you understand that the DAG is constructed at definition time, not at runtime. It assumes you understand why you cannot arbitrarily call tasks within loops or conditionals without thought.

For an engineer, these assumptions are unremarkable background knowledge. For a data scientist who has never had to reason about distributed state, they are invisible walls. The code seems to work — until it doesn't. And when it doesn't, the author doesn't know why, so they patch. And patch again. They write a helper function that re-reads data from S3 because they didn't realise Flyte was already moving it. They wrap their entire workflow in a single monolithic task because the task boundary abstraction felt like friction. They orchestrate things that should not be orchestrated, because the framework gives them the power to do so and no intuition about when they shouldn't.

Over time, the codebase grows in the shape of these patches. It accretes cargo cult patterns — code that was copied because it worked, without anyone knowing why. The framework is visible everywhere, in every decorator and type annotation, but it is being used as scaffolding, not as architecture. The framework's design intent has been quietly abandoned. What remains is the syntax without the semantics.

---

###### Code grows in the shape of its authors

This is a deeper generalisation of Conway's Law. Conway observed that organisations design systems that mirror their communication structure. But there is a more fundamental version: *teams build systems that mirror their cognitive structure.* The code reflects not just how the team communicates, but how the team thinks.

A team of data scientists will build a pipeline that looks like a very long notebook. A team of engineers will build something modular, typed, testable. A mixed team with unclear ownership will build something that looks like both at the same time — which is often the worst outcome, combining the inflexibility of over-engineering with the fragility of under-engineering.

The point is not to judge any of these outcomes as moral failures. The point is to notice that code evolution is not a purely technical process. It is a human process. If you want to understand why a codebase looks the way it does, don't read the commits — read the team.

And if you want to predict how a codebase will evolve, ask: what is the team's engineering ceiling? What do they naturally reach for when under pressure, when deadlines are close, when understanding is incomplete? That is what the code will become.

---

###### Meeting people where they are

What follows from this, for those who build frameworks and for those who lead teams?

For framework designers: **the framework should be intuitive at the level where most of its users naturally operate.** This does not mean simplifying away power. It means providing a natural entry point that maps to the user's existing mental model, while making the path to greater sophistication visible and navigable.

Python itself is a masterclass in this philosophy. You can write Python like pseudocode on day one. Years later, you can be writing metaclasses, async generators, descriptors. The floor is low; the ceiling is high; the slope between them is gentle. The language does not demand that you understand everything before you can do anything useful.

Flyte's `@task` and `@workflow` decorators do something clever in this respect. They let you write what looks like ordinary Python functions. You call tasks as if they were functions. The graph is implicit in the call structure. This is an imperative shell around a declarative core — and it works, for beginners, precisely because it honours their mental model.

But it breaks down at the edges. When data movement becomes non-obvious. When a team begins orchestrating things that should not be orchestrated, because no one told them they shouldn't. When the lack of engineering instinct leads to workflows that do too much, that are too stateful, that conflate computation with coordination.

A framework should not just have a low entry floor. ***It should have guard rails that make misuse visible before it compounds.*** It should surface complexity at the moment it becomes relevant — not before, not after. It should fail loudly when an anti-pattern is forming, rather than silently permitting it until the codebase is beyond saving. The best frameworks are opinionated enough that a user who follows them somewhat blindly still ends up somewhere reasonable.

---

###### The question of agents

Now we introduce a new variable. Agents.

If the problem with human data scientists is that they write code at the ceiling of their understanding, what happens when that ceiling is replaced by an AI that has no ceiling — but also no intuition?

An agent writing code within a framework like Flyte will not struggle with the mental model gap. It understands DAGs. It understands serialisation. It understands type systems and functional purity. In one sense, it is the ideal framework user. In another sense, it is an entirely new kind of risk.

An agent optimises for the specification it is given. Give it a task with a narrow success criterion, and it will satisfy that criterion by whatever means are available. It will make architectural decisions that are locally optimal but globally incoherent. It will produce code that is technically correct but conceptually alien to the human team that must maintain it. It has no stake in the codebase's long-term intelligibility. It does not have to live with what it builds.

The deeper problem: agents do not accumulate tacit knowledge. A human engineer, over months of working in a system, develops a *feel* — for what belongs in a task versus a workflow, for what data should be passed as typed arguments versus read directly from storage, for what failure modes to anticipate, for what complexity is acceptable and what will become a maintenance nightmare. This feel is hard to articulate. It is almost impossible to write into a spec. But it is what keeps a codebase sane over time.

An agent without this feel, given too much freedom, will build something that works but that no one can reason about. This is not a hypothetical failure mode. It is the natural outcome of autonomous code generation without sufficient architectural constraint. The agent will go — if not literally *crazy* — then deeply, quietly strange.

The implication is not that agents should be kept away from code. It is that **the specification you give an agent must include not just what the code should do, but what the code should look like.** Style is not decoration. In a codebase that will be read and maintained by humans, style is substance.

This means: give agents discovery hooks — ways to understand context, existing conventions, the vocabulary of the system, before they generate. Give them an aesthetic target. Tell them whether to write imperatively or functionally, when to abstract and when to keep things inline, what complexity level is appropriate for the humans who will inherit this work. Without this, you will get code that is correct but cultureless.

---

###### What this means, practically

For **framework designers**: bake taste into the framework. Make the idiomatic path the easy path. Make anti-patterns expensive or, where possible, impossible. Don't just provide flexibility — provide guidance. A framework is not just a set of tools; it is an encoded set of judgements about how work should be done. Those judgements should be visible.

For **team leads and engineering managers**: know your team's engineering ceiling, and design your system to live comfortably below it. This is not lowering your standards. It is matching your architecture to your capability. A beautiful architecture that your team cannot maintain is not beautiful — it is a liability. The system you build should be the simplest system that solves the problem correctly, given the hands that will maintain it.

For **those working with agents on codebases**: treat the agent as a very capable junior engineer with no cultural context and no memory. It needs onboarding. It needs style guides. It needs enough discoverable structure in the existing codebase that it can infer what belongs and what doesn't. The better your code is organised before the agent touches it, the better the code will look after.

---

###### The shape of the ceiling

The evolution of code is not driven by logic alone. It is driven by the humans — and now the agents — who write it, and by the frameworks that shape what is easy and what is hard to do. Good framework design does not fight the cognitive grain of its users; it works with it, while gently extending it. Good team design does not pretend that everyone has more engineering knowledge than they do; it builds systems that are robust at the level of understanding that will actually be applied.

***The most durable codebases are not the most sophisticated. They are the most honest*** — honest about the team that built them, honest about the framework they inhabit, honest about the level of understanding that can realistically be brought to bear.

The engineering ceiling is not a flaw to be overcome through clever abstraction. It is a constraint to be designed around. And the first step in designing around a constraint is acknowledging that it exists.

<br>
