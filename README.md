# iLearn

**Three steps to learn anything (and build it) with AI.**

---

A few months ago I built a hand-gesture-controlled drone with zero hardware experience. No electronics, no soldering, no clue what a DAC was. The video went viral so I wrote up how I did it: **[iDrone](https://github.com/pattssun/iDrone)**.

The drone isn't the point. The method is. I've used the same three steps to learn everything hard I've picked up. This repo is that method, written down.

If you're staring at something you want to build but feel locked out of, this is for you.

---

## The framework

Three steps. In order. That's it.

1. **Dump** what you want to build.
2. **"Interview me for full understanding."**
3. **LEGO instructions.** Follow them. Screenshot-debug when stuck.

Most people use AI as an oracle. Ask, accept, get stuck. I use AI as a tutor that interviews me until my idea is sharp enough to execute, then walks me through it like LEGO.

The whole unlock is step 2.

---

## Step 1. Dump

Write what you want to build. Rough is fine. Half-formed is fine. You're not pitching, you're starting a conversation.

The mistake here is waiting until you "understand it well enough to ask." You don't need to. Dump the rough version and let AI sharpen it with you.

```
I want to build [rough description]. Here's everything I know so far:
[braindump: links, references, half-thoughts, what inspired this].

I have no background in [domain]. Help me turn this into a real plan.
```

Two things I do here:

- **Build the mental model before buying anything.** My first drone conversation bought zero parts. I just read and asked questions for weeks. When I finally did buy stuff, I knew what it was for.
- **Tell AI you're a beginner.** It's not an apology. It makes AI translate everything into plain language and end-to-end signal flow.

---

## Step 2. "Interview me for full understanding"

This is the keystone. One phrase changes everything:

> **"Interview me for full understanding."**

Drop it in and the dynamic flips. AI stops assuming. It starts asking. You stop hunting for answers and start getting your idea pressure-tested.

I use it constantly. First turn, every scope change, anytime AI is filling in gaps with guesses. From my actual first drone prompt:

> *"Heres a convo with another AI chatbot about this project, help me write a PRD given the context. **Ask me follow up questions for full understanding if needed.**"*

The prompt:

```
[Your dump or your question.]

Interview me for full understanding before answering.
Ask me questions one at a time if the answers depend on each other.
```

### Variations I use a lot

**When given a binary choice, get the full option space first:**

```
Walk me through the options. Don't narrow yet, I want to see them all.
```

**When you suspect AI is wrong, make it argue against itself:**

```
Make the strongest case against this answer. What would someone who disagrees say?
What are the failure modes of this approach?
```

**When the project is bloating:**

```
What's the smallest version of this that still proves the idea works?
List everything we could build, then tell me the 20% that delivers 80%.
```

**When you don't know what you don't know:**

```
What are the things a beginner in [domain] would not even think to ask?
List the unknowns I'm not aware of.
```

The trap: writing "interview me" and then answering your own questions in the same prompt. Let AI ask. Answer short. Loop.

---

## Step 3. LEGO instructions and screenshot-debug

Once the idea is sharp, ask for the build plan in LEGO form:

```
Give me step-by-step build instructions for this, detailed enough to follow
with no prior context. Number each step. Tell me what to do, what to buy,
what to check before moving on.
```

Then follow them. Don't think, comply. You're not designing anymore, you're executing.

When you get stuck, and you will, send a screenshot plus the symptom:

```
[Screenshot or photo of the thing you don't understand.]

Here's what I'm seeing literally: [exact text / exact reading / exact error].
What is this? What do I do?
```

Most people don't know this is available. For the drone, it was photos of the breadboard, photos of the PCB, photos of the multimeter. For software it's screenshots of error messages, dashboards, weird UI. For anything intimidating, screenshot the thing that's confusing you.

**Report exactly what you see, including the units.** My multimeter read "001.6 mV" when AI expected 1.65 V. I told it "mV." It found a bug in its own code. If I'd said "it didn't work" or rounded to "1.6," we'd still be stuck.

> Summarize and AI guesses. Report literally and AI debugs.

### When AI's suggestions stop working, force state enumeration

If you're three rounds deep and not progressing, paste this:

```
This is taking too long. Let's reduce assumptions as much as possible.
Ask me yes/no questions about the exact state of things until you can
identify the bottleneck. Don't suggest fixes yet.
```

This single prompt unblocked the hardest moment of the drone build (the binding crisis). It forces AI off speculation and onto facts.

### After it works, ask why

Don't skip this. Doing teaches you the *what*. This prompt teaches you the *why*:

```
It works. Explain what we just did and why, in plain language.
What is this technique actually called?
```

You're not just learning to build one thing. You're learning vocabulary you can search, read docs in, and use to ask better questions next time.

---

## Mindset

A few things that aren't prompts but matter:

- **Pivot without sunk-cost guilt.** I threw out three axes of working DAC code mid-build because hand tracking was a cleaner demo. If a pivot makes the thing better, pivot. Even if you already soldered it.
- **Pick the harder path when learning is the goal.** I bought the toy drone that needed soldering and reverse-engineering instead of the one with a clean SDK. The friction was the point. Pick "shippable" when you want to ship. Pick "hard" when you want to learn.
- **Do, then understand.** You don't need to understand the theory before acting. Follow LEGO instructions to something working, then ask "explain what we just did and why." Doing and understanding are two passes, not one.

---

## Your turn

Pick something you've wanted to build but felt locked out of. Open a new chat with your favorite AI. Paste this:

```
I want to build [the thing]. Here's everything I know so far: [dump].
I have no background in [domain].

Interview me for full understanding. Ask me one question at a time.
Don't propose a plan until you've understood what I actually want.
```

That's step 1 and step 2. Step 3 happens when you've talked enough that you're ready to be told what to do.

You can stop reading here. The whole framework fits on one screen on purpose.

---

## Proof: how this built a drone from zero

If you want to see the three steps in action, the real prompts, the wrong answers, the pivots, the binding crisis that almost killed the project, read **[CASE_STUDY.md](./CASE_STUDY.md)**.

Or just go look at the drone: **[github.com/pattssun/iDrone](https://github.com/pattssun/iDrone)**.

---

*Built something with this? Tag me. I want to see what you make.*
