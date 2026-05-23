# iLearn

**3 steps to learn anything (and build it) with AI.**

---

I previously built a hand-gesture-controlled drone with zero hardware experience. No electronics, no soldering, no clue what a DAC was. The video went viral on **[Instagram (2.6M+ views)](https://www.instagram.com/p/DX7eG0mR4I6/?hl=en)** and **[TikTok (1.3M+ views)](https://www.tiktok.com/@pattssun/video/7636158433917488398)**, so I wrote up how I did it: **[iDrone GitHub repo](https://github.com/pattssun/iDrone)**

The drone isn't the point. The method is. I've used the same 3 steps to learn everything hard I've picked up. **This repo compiles the exact AI prompts I've used.**

---

## The framework

1. **Dump** what you want to build.
2. **"Interview me for full understanding."**
3. **LEGO instructions.** Follow them. Screenshot-debug when stuck.

Most people use AI as an oracle. Ask, accept, get stuck. I use AI as a tutor that interviews me until my idea is sharp enough to execute, then walks me through it like LEGO steps.

---

## Step 1. Dump

Write what you want to build. Half-formed is fine. The mistake here is waiting until you "understand it well enough to ask." You don't need to. Dump the rough version and let AI sharpen it with you. Here's my prompt👇:

```
I want to build [rough description]. Here's everything I know so far:
[braindump: links, references, half-thoughts, what inspired this].

I have no background in [domain]. Help me turn this into a real plan.
```

---

## Step 2. "Interview me for full understanding"

One phrase changes everything. AI stops assuming. You stop hunting for answers and start getting your idea pressure-tested. Here's my prompt👇:

```
[Your dump or your question.]

Interview me for full understanding before answering.
Ask me questions one at a time if the answers depend on each other.
```

### Variations I use a lot

**When given a binary choice, get the full option space first👇:**

```
Walk me through the options. Don't narrow yet, I want to see them all.
```

**When you suspect AI is wrong, make it argue against itself👇:**

```
Make the strongest case against this answer. What would someone who disagrees say?
What are the failure modes of this approach?
```

**When the project is bloating👇:**

```
What's the smallest version of this that still proves the idea works?
List everything we could build, then tell me the 20% that delivers 80%.
```

**When you don't know what you don't know👇:**

```
What are the things a beginner in [domain] would not even think to ask?
List the unknowns I'm not aware of.
```

---

## Step 3. LEGO instructions and screenshot-debug

Once the idea is sharp, ask for the build plan in LEGO form👇:

```
Give me step-by-step build instructions for this, detailed enough to follow
with no prior context. Number each step. Tell me what to do, what to buy,
what to check before moving on.
```

Then follow them. When you get stuck, and you will, send a screenshot👇:

```
[Screenshot or photo of the thing you don't understand.]

Here's what I'm seeing literally: [exact text / exact reading / exact error].
What is this? What do I do?
```

For the drone, it was photos of the breadboard, photos of the PCB, photos of the multimeter. For software, it's screenshots of error messages, dashboards, weird UI. For anything intimidating, screenshot the thing that's confusing you.

### When AI's suggestions stop working, force state enumeration

If you're three rounds deep and not progressing, paste this👇:

```
This is taking too long. Let's reduce assumptions as much as possible.
Ask me yes/no questions about the exact state of things until you can
identify the bottleneck. Don't suggest fixes yet.
```

This single prompt unblocked the hardest moment of the drone build (the binding crisis). It forces AI off speculation and onto facts.

### After it works, ask why

Don't skip this. Doing teaches you the *what*. This prompt teaches you the *why*👇:

```
It works. Explain what we just did and why, in plain language.
What is this technique actually called?
```

---

## Your turn

Pick something you've wanted to build but felt locked out of. Open a new chat with your favorite AI. Paste this👇:

```
I want to build [the thing]. Here's everything I know so far: [dump].
I have no background in [domain].

Interview me for full understanding. Ask me one question at a time.
Don't propose a plan until you've understood what I actually want.
```

That's step 1 and step 2. Step 3 happens when you've talked enough that you're ready to be told what to do. Good luck!

---

## Proof: how this built a drone from zero

If you want to see the three steps in action, the real prompts, the wrong answers, the pivots, the binding crisis that almost killed the project, read **[CASE_STUDY.md](./CASE_STUDY.md)**.

Or just go look at the drone: **[github.com/pattssun/iDrone](https://github.com/pattssun/iDrone)**.

---

*Built something with this? Tag me @patsunrick on [Instagram](https://www.instagram.com/patsunrick/?hl=en), @pattssun on [TikTok](https://www.tiktok.com/@pattssun)/[Twitter](https://x.com/pattssun). I want to see what you make.*