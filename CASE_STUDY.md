# Case study: building iDrone from zero

How the three steps of [iLearn](./README.md) produced a working hand-gesture-controlled drone, built by someone who had never soldered, never used a multimeter, and didn't know what a DAC was.

The README is the manifesto. This is the receipts.

---

## Starting position

- Software background: Python, a code editor (Cursor). That's it.
- Hardware background: zero. No electronics. No soldering. Never read a circuit diagram.
- Trigger: saw a 2018 Arduino "mind-control drone" project. Wanted a modernized version.
- Time horizon: months. I didn't rush.

The final build: MacBook webcam tracks my hand, Python script sends commands over USB to a Raspberry Pi Pico, the Pico drives a DAC chip soldered into a hacked toy drone controller, the drone hovers and obeys my hand.

I learned the whole thing step-by-step.

---

## Step 0. Mental model before buying anything

First prompt, weeks before I bought a single part:

> *"How should i start learning how to build a drone from scratch with no prior experience in hardware."*

AI pointed me at simulators, kits before custom builds, soldering practice, basic electronics (voltage, current, spec sheets).

I bought none of it. I just read and asked questions. When I finally did buy parts, I knew what they were for.

This was step 1 of the framework in its purest form: dump the goal, get the landscape.

---

## Step 1. The keystone, used on turn one

When I was ready to scope the actual build, I dumped the rough idea plus a transcript from a different AI chat into Claude and wrote what became my most-used prompt:

> *"Heres a convo with another AI chatbot about this project, help me write a PRD given the context. **Ask me follow up questions for full understanding if needed.** The referenced 2018 arduino build is from [link]."*

That one phrase, *ask follow-up questions for full understanding*, changed the dynamic. AI stopped guessing what I wanted and started interrogating me until I knew what I wanted.

The PRD that came out of it was specific in ways I couldn't have specified myself:

- Discrete 3-zone yaw input (left/center/right) instead of continuous proportional control, because **discrete is more forgiving for a beginner pilot.** I never would have proposed that. AI surfaced it by asking.
- Proper DAC over PWM plus RC filter. Cleaner voltages, no ripple. I didn't know either of those words yet.
- Indoor only, altitude-hold drone, future BCI phase deferred.

I didn't have these opinions. The interview produced them.

---

## Step 2. Two architecture pivots, no sunk-cost guilt

**Pivot 1:** Eye-tracking (MediaPipe Face Mesh) became phone gyroscope tilt. Triggered before I bought hardware. Eye-tracking was cool, gyroscope was simpler and just as expressive.

**Pivot 2:** Phone gyroscope became MacBook hand tracking, throttle only. Triggered *after most of the hardware was built and wired.* I threw out three axes of working DAC control because hand tracking was a cleaner demo.

The pivot was prompted exactly the way step 2 of the framework says:

> *"I want to modify this drone project. Instead of controlling… via raspberry pi and DAC, i want to just control throttle via handtracking on my macbook webcam. **Interview me for full understanding.** Would i still need a rasberry pi?"*

The keystone phrase again. Not just for the first prompt. Every scope change deserves an interview.

---

## Step 3. Hardware bring-up, LEGO mode

After the design converged, I asked for step-by-step instructions detailed enough to follow with no prior context. It felt like **following LEGO instructions.** I wasn't designing anymore. I was complying.

Wired the Pico to the DAC on a breadboard:

- GP4 to SDA, GP5 to SCL, 3V3 to VCC, GND to GND
- DAC at I²C address `0x60`

First milestone: `i2c.scan()` returned `[96]`. The Pico saw the DAC. I had no idea what I'd just done, but I'd done it.

Then I asked: *"What is this thing called?"* pointing at the breadboard wiring. AI taught me "breadboard prototyping" and "I²C bus" as terms. I learned vocabulary on purpose so I could later read docs, search forums, and write better prompts.

---

## Step 4. The mV moment

Second milestone: setting the DAC to 2048 should produce 1.65 V on the output pin.

My multimeter read **001.6 mV.**

AI's first response assumed display ambiguity. Maybe it was actually 1.6 V displayed weirdly. I pushed back with one word:

> *"mV."*

That's it. AI went back, re-read its own code, and found a **bit-packing bug.** Config bits were colliding with data bits, accidentally enabling an internal voltage reference and zeroing the value. Fixed version: 1.65 V exactly.

I would have missed that bug if I'd said "it didn't work" or rounded to "1.6." Reporting literally, with the unit, is what let AI debug itself.

This is the line from the README: *summarize and AI guesses; report literally and AI debugs.* This is where I learned it.

---

## Step 5. Photo-debugging the controller PCB

Mapping the toy drone's controller PCB was where the photo trick did the heaviest lifting.

The goal: find which pads on the PCB matched throttle, yaw, pitch, roll. The method: multimeter plus my phone camera plus AI.

Same cycle, repeated dozens of times:

1. Hit a wall. Wrong reading, mystery pad, can't tell which side of the PCB I was looking at.
2. Snap a photo with my phone.
3. Describe the symptom in one sentence: *"The pad next to L3 reads 290 mV at rest. What is this?"*
4. AI identifies the pad, diagnoses the wire, or proposes the next test.
5. Run the test. Report back numerically.

Result:

| Channel  | Wiper pads | Min (mV) | Rest (mV) | Max (mV) |
| -------- | ---------- | -------- | --------- | -------- |
| Throttle | L2/L3      | 292      | 310       | 366      |
| Yaw      | L5/L6      | 290      | 310       | 366      |
| Pitch    | R4/R5      | 290      | 309       | 365      |
| Roll     | R2/R3      | 289      | 308       | 365      |

I caught one important PCB-orientation gotcha along the way: viewed from the back, left/right are mirrored. AI flagged it. I verified with a continuity check (B− to battery negative) before soldering. **Do, then understand.** I didn't need to understand the mirror at first, I just needed to be told to check.

---

## Step 6. The binding crisis (state enumeration saved this)

I soldered the four DAC wires plus ground to the PCB. Tried to fly. The controller would not bind to the drone.

I tried things. They didn't work. I tried more things. They didn't work. AI's suggestions were drifting into guesswork. Three rounds in, I pasted this:

> *"This takes too much time since i have to solder/desolder the axis pins for a connection to form. **lets try to reduce assumptions as much as possible, ask me questions to clear them up so we can identify the bottleneck.**"*

AI stopped suggesting fixes and started asking five specific yes/no state questions: *"What is the exact physical state of the left joystick right now? Is the right joystick installed? Are R2 and R4 tabs continuous with their pads?"*

I answered concretely. The bottleneck was identified in two turns.

The diagnosis was beautiful and counterintuitive. The toy controller's chip uses **pulsed ADC sampling.** During flight, the DAC can overpower the joystick pot. During binding, the chip's impedance check needs the pot's reference circuit physically intact. DAC alone can never bind.

The fix: hybrid setup. Keep the left joystick fully installed (binding needs it). Reinstall the right joystick but cut the wiper tabs of R2 and R4 so the DAC owns pitch/roll cleanly during flight while the reference circuit stays intact.

I knew none of those words (pulsed ADC, reference circuit, impedance check) at the start. I learned them by needing them.

That one prompt saved the project. It's why "force state enumeration" is in step 3 of the framework.

After it worked, I asked the retrospective:

> *"It works. Can you explain what we just did and why."*

Doing first. Understanding second. That's the order.

---

## Step 7. Hand tracking and ship

The final pivot. Hand tracking for throttle only. Physical joysticks for pitch, roll, yaw. Webcam to MediaPipe to hand-openness score (0.0 to 1.0) to Python to Pico to DAC channel 0 to blue wire on the L2 throttle pad.

Closed fist is throttle 0. Half-open hand is hover (DAC value 2048, the 1.65 V midpoint I'd hard-won earlier). Fully spread is throttle high.

Hand leaves the frame, throttle cuts off. Keyboard `K` killswitch. Hover is the default. Fail-safe by design.

I documented everything in `HAND_TRACKING_THROTTLE_PRD.md` and handed it to a coding agent to implement. The PRD included the hard-won gotchas (neutral DAC is 2048 not 4095, MicroPico kills `main.py` on connect and I lost hours to that, arrow keys could accidentally trigger the bind command) so the next person doesn't lose the same hours.

---

## What I want you to notice

The drone is impressive. The method is replicable.

I didn't:

- Have hardware experience
- Take a course
- Find a mentor
- Get lucky

I did:

- Ask AI to interview me, on the first turn and every turn after
- Report observations literally, units included
- Send photos when I was stuck
- Force state enumeration when AI started guessing
- Pivot architectures twice with no guilt
- Pick the harder path (toy drone plus DAC) over the easier one (Tello plus SDK) because I wanted to learn
- Ask "what is this called?" and "explain what we just did and why" after every milestone

Every one of those is from the [iLearn framework](./README.md). The drone is the proof. Now go pick your own thing.
