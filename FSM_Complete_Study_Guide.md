# Finite State Machines — Complete Study Guide
*Digital Logic and Computer Design — built from your lecture slides*

> Terminology, examples, and numbers below are taken directly from your slides (traffic-light controller, divide-by-3 counter, robotic-snail pattern detector, keypad-lock derivation). Anywhere I add something the slides don't state, it's explicitly marked **"Additional context."**

---

## 1. What is an FSM?

**Concept:** An FSM is the standard way to design any digital circuit whose behavior depends on *history* — not just today's inputs, but what has happened before. A vending machine, a traffic light, a lock, a CPU control unit — all remember "where they are" and react differently to the same input depending on that memory.

**Formal definition (from your slides):** A Finite State Machine (FSM) is a model used to design synchronous sequential circuits. It works by moving between a limited (finite) number of states based on the current inputs and the present state of the circuit. A circuit containing *k* registers can have 2ᵏ different states, because each register stores one bit of information. The FSM can be in only one state at a time.

**Key Point (slide):** An FSM is *a digital circuit that stores its current state and changes to the next state on every clock edge.* "Finite" means it has only a limited number of possible states.

**Intuition:** Think of a state as a labeled room in a building. Inputs are doors you can walk through. On every "tick" of a clock, you either stay in your room or walk through exactly one door into a new room, based on which doors are open (current inputs) and which room you're currently in. You are *always* in exactly one room — never between rooms, never in two rooms at once.

**Why "synchronous"?** Because state changes only happen on a clock edge — not the instant an input changes. This is what makes FSM behavior predictable and analyzable; combinational logic alone can't "remember" — you need memory elements (flip-flops) plus a clock.

**Exam perspective:** You will very often be asked to state the definition, explain why *k* flip-flops give 2ᵏ states (and the reverse: given N states, how many flip-flops are the *minimum*), and to distinguish an FSM from plain combinational logic (no memory) or an unclocked latch circuit (not synchronous).

**Common mistakes:**
- Saying an FSM "can be in multiple states" — it cannot; it is in exactly one state at a time.
- Confusing "number of states" with "number of flip-flops." They're related by 2ᵏ ≥ (number of states), not equal to it.

---

## 2. Components of an FSM

**Formal definition (slide):** An FSM consists of:
- **M Inputs** — signals received from the external system.
- **N Outputs** — signals produced by the circuit.
- **k-bit State Register** — stores the current state of the FSM.
- **Clock Signal (CLK)** — controls when the state changes.
- **Optional Reset Signal** — returns the FSM to its initial state.

**How it works:** Every one of these five pieces has a specific job:
| Component | Job |
|---|---|
| Inputs | Tell the FSM what's happening in the outside world right now |
| State register | Physically stores *which* state you're in, using flip-flops |
| Clock | The "heartbeat" — the only moment state is allowed to change |
| Outputs | What the FSM tells the outside world, computed from state (and maybe inputs) |
| Reset | Forces a known starting state — essential, because flip-flops power up in an unpredictable state |

**Exam perspective:** A common exam question gives you a word description and asks you to *list* M, N, k before doing anything else — this is literally Step 1 of the "derive an FSM" and "design an FSM" procedures you'll see later. Practice pulling these five items out of a paragraph.

**Common mistakes:** Forgetting Reset is optional in principle but almost always included in practice (because otherwise the starting state is undefined) — many students skip discussing it and lose easy marks.

---

## 3. Basic FSM Structure

**Concept:** An FSM is built from exactly three functional blocks wired in a loop.

**Formal definition (slide):** A Finite State Machine consists of three main components:
1. **Next State Logic** — computes the next state of the FSM (combinational logic).
2. **State Register (Memory)** — stores the present state of the FSM; updates the stored state on every clock edge (CLK).
3. **Output Logic** — generates the output signals of the FSM (combinational logic).

**How it works, step by step:**
1. Next-State Logic looks at the *current state* (fed back from the register) and the *current inputs*, and computes what the *next state* should be — this is pure combinational logic (AND/OR/NOT gates), no memory.
2. On the next active clock edge, the State Register (a bank of k flip-flops) *latches* that next-state value, and it becomes the new current state.
3. Output Logic looks at the current state (Moore) or current state + inputs (Mealy) and computes the outputs — again pure combinational logic.

**Intuition:** This is exactly a feedback loop: `state → (next-state logic) → next-state → (register, waits for clock) → new state → feeds back to next-state logic`. The register is the *only* place "memory" lives; everything else is stateless combinational logic that could be redrawn as a truth table or K-map at any instant.

**Exam perspective:** You'll be asked to label a block diagram with these three blocks, or to identify which part of a given circuit is "next-state logic" vs "output logic" vs "state register" (this is literally Step 1 of deriving an FSM from a schematic — see Section 19).

**Common mistakes:** Thinking the flip-flops "do the computing." They don't — they only store. All computation (next-state and output) is done by combinational gates.

---

## 4. How an FSM Operates on Each Clock Edge

**Working Principle (slide):** On every clock edge, the FSM:
1. Reads the current state.
2. Accepts the current inputs.
3. Computes the next state.
4. Updates the state register.
5. Produces the required outputs.

**How it works:** Steps 1–3 (reading state, reading inputs, computing next-state) happen *continuously* in the combinational next-state logic — it's always evaluating, all the time, based on whatever the state register currently holds and whatever the inputs currently are. Step 4 — actually updating the register — only happens at the clock edge. Step 5 (producing outputs) also happens continuously from combinational output logic, but *what* that output depends on differs by FSM type (see Sections 5–7).

**Intuition:** Between clock edges, the "next state" wire is like a proposal sitting there, fully computed and ready. The clock edge is the moment that proposal gets "accepted" and becomes the official current state.

**Exam perspective:** Timing-diagram questions rely on this: outputs (Moore) only change *after* a clock edge that changes the state; Mealy outputs can glitch/change the instant an input changes, even between clock edges. Be ready to sketch a waveform showing this difference.

---

## 5. Moore Machine

**Concept:** In a Moore machine, the output is a property of *which room you're in* — it doesn't matter which door you're about to walk through.

**Formal definition (slide):** Output depends only on the current state. Changes in input do not immediately affect the output. Output changes only after the state changes.

**Formula:** `Output = f(Current State)`

**Components involved:** Output logic in a Moore machine takes *only* the state-register bits as input — the FSM's inputs never feed the output logic directly.

**How it works, step by step:**
1. FSM sits in some state.
2. Output logic reads only the state bits and computes the output — this output is stable and constant as long as the FSM stays in that state.
3. Even if an input changes right now, the output doesn't move until the *next* clock edge causes the state itself to change.

**Example (from slides — traffic light controller):** In State S0, `LA = green, LB = red`. This output is written *inside* the state circle in the diagram — that is the visual signature of a Moore machine. As long as the FSM is in S0 (even for many clock cycles, e.g., while `TA = 1`), the lights stay exactly green/red.

**Exam perspective:** If you see outputs labeled *inside* the state circles of a diagram, it's Moore. If asked "will the output change immediately when input X changes," the Moore answer is always **no** — it waits for the next clock edge and only changes if the state actually changes.

**Common mistakes:** Assuming output changes the instant the state-defining input changes — it only changes after the *clock edge* actually moves the FSM into a new state.

---

## 6. Mealy Machine

**Concept:** In a Mealy machine, the output is a property of both *which room you're in* and *which door you're currently reaching for* — it can react instantly.

**Formal definition (slide):** Output depends on both the current state and the current inputs. Output can change immediately when the input changes. Generally requires fewer states than a Moore machine.

**Formula:** `Output = f(Current State, Current Input)`

**Components involved:** Output logic takes *both* state-register bits *and* the current input signals.

**How it works, step by step:**
1. FSM sits in some state.
2. Output logic continuously reads both the state bits *and* the live input signals.
3. The instant an input changes (even without a clock edge), the output can change too, because the combinational output logic reacts immediately.

**Example (from slides — snail pattern detector):** Transitions are labeled `Input / Output`, e.g. `1/1` means "on input 1, produce output 1 during this transition," written **on the transition arrow**, not inside a circle — that's the visual signature of a Mealy machine.

**Exam perspective:** If outputs are labeled on the *arrows* (as `input/output`), it's Mealy. Mealy machines typically need *fewer states* than an equivalent Moore machine for the same problem, because Mealy can encode some of the "memory" directly into which transition just occurred, rather than needing a dedicated state.

**Common mistakes:** Forgetting that a Mealy output can produce a brief output "glitch" if the input changes asynchronously between clock edges — output is *not* synchronized to the clock the way Moore's is.

---

## 7. Moore vs Mealy Machines

**Comparison table (from slides):**

| Moore Machine | Mealy Machine |
|---|---|
| More states | Fewer states |
| Stable output | Faster response |
| Output depends only on state | Output depends on state and input |
| One clock delay | Immediate output |

**Guidance (slide):** Choose a **Moore** machine when *stable outputs* are important. Choose a **Mealy** machine when *faster output response* is required.

### Worked comparison example: the robotic snail (from your slides)

**Scenario (slide):** Alyssa P. Hacker owns a pet robotic snail controlled by an FSM. The snail moves over a paper tape of 1s and 0s. At every clock cycle it moves forward one position and remembers the last two bits it read. The snail smiles when a specific bit pattern is detected. Input: `A` = current bit read. Output: `Y` — `Y = 1` → snail smiles, `Y = 0` → does not smile. (The pattern being detected, from the diagrams, is **two consecutive 1s**, i.e. "11".)

**Moore design (3 states):**
```
        0            1
 →(S0)------>(S1)------>(S2)
   |0  ↺1     |0  ↺0    |1
   ^__________|_________|
        (1 on S2→S0, 0 on S1→S0)
```
- S0 = "no relevant history / last bit was 0" → output 0
- S1 = "just saw one 1" → output 0
- S2 = "just saw two 1s in a row" → output 1
- Self-loops: S0 stays on input 1→S0 (wait — re-check with your actual diagram logic below)

Using the **exact** diagram from your slides: S0 --0--> S1, S0 --1--> S0 (self-loop), S1 --1--> S2, S1 --0--> S0, S2 --1--> S2 (self-loop), S2 --0--> S1. Outputs: S0:0, S1:0, S2:1.

**Mealy design (2 states, from slides):**
- S0 --0/0--> S1, S0 --1/0--> S0 (self-loop)
- S1 --0/0--> S1 (self-loop), S1 --1/1--> S0

Same job, done with **one fewer state**, because the "third state" of Moore (S2, meaning "just completed the pattern") is replaced in Mealy by simply emitting a 1 *on the transition* rather than needing a dedicated state to hold that fact.

**Moore transition/output table (Table 3.11 / 3.12):**

| Current State S | Input A | Next State S′ |
|---|---|---|
| S0 | 0 | S1 |
| S0 | 1 | S0 |
| S1 | 0 | S1 |
| S1 | 1 | S2 |
| S2 | 0 | S1 |
| S2 | 1 | S0 |

| Current State S | Output Y |
|---|---|
| S0 | 0 |
| S1 | 0 |
| S2 | 1 |

**Binary-encoded Moore table (Table 3.13 / 3.14), with S0=00, S1=01, S2=10:**

| S₁ | S₀ | A | S₁′ | S₀′ |
|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 1 |
| 0 | 0 | 1 | 0 | 0 |
| 0 | 1 | 0 | 0 | 1 |
| 0 | 1 | 1 | 1 | 0 |
| 1 | 0 | 0 | 0 | 1 |
| 1 | 0 | 1 | 0 | 0 |

**Moore equations (Equation 3.3.1, from slide):**
```
S₁′ = S₀·A
S₀′ = A̅
Y   = S₁
```

**Mealy table (Table 3.15 — symbolic, and 3.16 — one state bit, since Mealy needs only 2 states = 1 flip-flop):**

| Current State S | Input A | Next State S′ | Output Y |
|---|---|---|---|
| S0 | 0 | S1 | 0 |
| S0 | 1 | S0 | 0 |
| S1 | 0 | S1 | 0 |
| S1 | 1 | S0 | 1 |

**Additional context:** the slides don't print the simplified Mealy equations explicitly, but they follow directly by inspection from Table 3.16 (state bit Q, Q=0 for S0, Q=1 for S1):
```
Q′ = A̅          (next state is 1 only when A = 0, regardless of current state)
Y  = Q · A       (output is 1 only when current state is S1 AND input is 1)
```

**Exam perspective:** A very common question type: "Convert this Moore machine to an equivalent Mealy machine" or vice-versa, and/or "how many states/flip-flops does each need?" Always check: Mealy usually needs ≤ states than Moore for the same behavior.

**Common mistakes:**
- Mixing up which one has "immediate" output — it's Mealy.
- Assuming Mealy is *always* better because it uses fewer states — Moore's stability (no glitches, output synchronized with clock) is often preferred in real hardware, e.g., driving other synchronous logic.

---

## 8. FSM Design Example — Traffic Light Controller (the running example)

**Problem statement (slide):** Ben notices a busy intersection between Academic Avenue and Bravado Boulevard. Students move between Dormitories and Labs on Academic Ave; football players travel between Athletic Fields and the Dining Hall on Bravado Blvd. Since people aren't paying attention, accidents have occurred. The Dean asks Ben to design an automatic traffic light controller using traffic sensors to control the lights and ensure safe, smooth flow.

This single example is used across your slides to show the **entire pipeline**:
`problem statement → states → state diagram → state transition table → state encoding → next-state equations → output equations → circuit`

### Step 1 — Identify inputs, outputs, states (slide)
- **2 Outputs:** `L_A` (controls Academic Ave. light), `L_B` (controls Bravado Blvd. light)
- **2 Sensors (inputs):** `T_A` (detects vehicles on Academic Ave.), `T_B` (detects vehicles on Bravado Blvd.)
- **Clock (CLK):** the controller checks traffic and updates signals every 5 seconds.
- **Reset:** initializes the FSM to its starting state when turned on.

### Step 2 — State Transition Diagram (Section 9 below has the general method; here is the result)
4 states, each with fixed `(L_A, L_B)` outputs — this is a **Moore machine** (outputs written inside the circles):

| State | L_A (Academic Ave.) | L_B (Bravado Blvd.) |
|---|---|---|
| S0 | green | red |
| S1 | yellow | red |
| S2 | red | green |
| S3 | red | yellow |

**Transitions (from the diagram):**
- Reset → S0
- S0 → S1 if `T_A = 0`; S0 → S0 (stay) if `T_A = 1`
- S1 → S2 automatically after 5 seconds (no input condition — pure timing)
- S2 → S3 if `T_B = 0`; S2 → S2 (stay) if `T_B = 1`
- S3 → S0 automatically after 5 seconds

**Intuition for why:** S0 is "Academic Ave has a green light — keep it green as long as traffic (`T_A=1`) is still flowing." Once traffic clears (`T_A=0`), move to S1 (yellow, a fixed 5-second warning), then S2 gives Bravado Blvd. the green (again held open as long as `T_B=1`), then S3 is Bravado's yellow warning, then back to S0. This is a repeating cycle that treats both roads fairly and responds to actual traffic.

### Step 3 — State Transition Table (Table 3.1, slide)

| Current State S | T_A | T_B | Next State S′ |
|---|---|---|---|
| S0 | 0 | X | S1 |
| S0 | 1 | X | S0 |
| S1 | X | X | S2 |
| S2 | X | 0 | S3 |
| S2 | X | 1 | S2 |
| S3 | X | X | S0 |

`X` = **don't care**: the value of that input does not affect the next state for that row. E.g., in S1 the FSM *always* moves to S2 regardless of `T_A` or `T_B` (it's purely timer-driven); in S3 it always returns to S0 regardless of sensors.

### Step 4 — State and Output Encoding (Table 3.2, 3.3)

| State | Encoding S₁S₀ |
|---|---|
| S0 | 00 |
| S1 | 01 |
| S2 | 10 |
| S3 | 11 |

| Output | Encoding L₁L₀ |
|---|---|
| green | 00 |
| yellow | 01 |
| red | 10 |

### Step 5 — Binary-Encoded State Transition Table (Table 3.4) and Output Table (Table 3.5)

| S₁ | S₀ | T_A | T_B | S₁′ | S₀′ |
|---|---|---|---|---|---|
| 0 | 0 | 0 | X | 0 | 1 |
| 0 | 0 | 1 | X | 0 | 0 |
| 0 | 1 | X | X | 1 | 0 |
| 1 | 0 | X | 0 | 1 | 1 |
| 1 | 0 | X | 1 | 1 | 0 |
| 1 | 1 | X | X | 0 | 0 |

| S₁ | S₀ | L_A1 | L_A0 | L_B1 | L_B0 |
|---|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 1 | 0 |
| 0 | 1 | 0 | 1 | 1 | 0 |
| 1 | 0 | 1 | 0 | 0 | 0 |
| 1 | 1 | 1 | 0 | 0 | 1 |

### Step 6 — Next-State Equations (Section 21 explains how to derive/simplify these)

```
S₁′ = S̅₁S₀ + S₁S̅₀T̅_B + S₁S̅₀T_B
S₀′ = S̅₁S̅₀T̅_A + S₁S̅₀T̅_B
```
Simplified (the middle two terms of S₁′ combine, since they differ only in `T_B`, one true one complemented — a classic K-map/Boolean-algebra reduction: `X·T̅ + X·T = X`):
```
S₁′ = S₁ ⊕ S₀        (this is the simplified/reduced form)
S₀′ = S̅₁S̅₀T̅_A + S₁S̅₀T̅_B
```

**How this simplification works, step by step:** Take `S₁S̅₀T̅_B + S₁S̅₀T_B`. Factor out the common term `S₁S̅₀`: you get `S₁S̅₀(T̅_B + T_B) = S₁S̅₀·1 = S₁S̅₀`. So `S₁′ = S̅₁S₀ + S₁S̅₀`, which is exactly the XOR of S₁ and S₀ — hence `S₁′ = S₁ ⊕ S₀`. This is a good general exam trick: whenever two product terms differ in only one complemented/uncomplemented literal and are otherwise identical, they combine and that literal disappears (this *is* what a K-map grouping of two adjacent 1-cells does visually).

### Step 7 — Output Equations
```
L_A1 = S₁
L_A0 = S̅₁S₀
L_B1 = S̅₁
L_B0 = S₁S₀
```
**How to read off an output equation directly from the output table:** for `L_A0`, look at every row where `L_A0 = 1`: only `(S₁,S₀) = (0,1)`. That single row gives the product term `S̅₁S₀` directly. When only one row is "1," the equation is just that row's minterm — no K-map needed.

### Step 8 — Circuit Implementation (Figure 3.26, slide)
The complete circuit has three stages wired in the standard FSM loop:
1. **(a) State register:** two D-flip-flops (or similar), inputs `S₁′, S₀′`, outputs `S₁, S₀`, clocked by CLK, with asynchronous Reset.
2. **(b) Next-state logic:** `T_A, T_B, S₁, S₀` feed AND/OR/XOR gates that implement the equations above, producing `S₁′, S₀′`, which feed back into the register's inputs.
3. **(c) Output logic:** `S₁, S₀` feed AND/NOT gates implementing `L_A1, L_A0, L_B1, L_B0`.

**Exam perspective:** You may be asked to draw this block-level circuit (register + 2 gate stages) rather than transistor-level gates — know the *shape*: inputs → next-state logic → register (with CLK, Reset) → feedback loop to next-state logic, and register → output logic → outputs.

---

## 9. State Transition Diagrams — How to Construct One

**Concept:** The state transition diagram is the *graphical* first draft of your FSM — you build it directly from the English problem description, before writing any table or equation.

**Formal definition (slide):** Each circle represents a state, and the directed arrows represent the transitions between states based on the inputs.
- **States:** Circles
- **Transitions:** Arcs (arrows)
- In a **Moore** FSM, outputs are labeled *inside* each state circle.
- In a **Mealy** FSM, outputs are labeled *on* each transition arrow, as `input/output`.

**How to construct it, step by step (using the traffic-light example as the running illustration):**
1. **List every distinct situation/behavior** the system must be in. Each becomes one circle. (Traffic light: 4 distinct light-color combinations → S0, S1, S2, S3.)
2. **Draw a Reset arrow** pointing into the designated starting state (S0 here) from outside the diagram — this shows where the FSM begins.
3. **For each state, ask: "under what condition do I leave, and where do I go?"** Draw one arrow per possible transition, labeled with the input condition that causes it (e.g., `T_A = 0`).
4. **Don't forget self-loops** — a state can transition back into itself under some condition (e.g., S0 → S0 when `T_A = 1`, meaning "stay green while traffic is still flowing").
5. **Check every state has an outgoing arrow for every possible input combination** (this guarantees the FSM is fully specified — no dead ends).
6. Label outputs: inside circles for Moore, on arrows for Mealy.

**Example, built up exactly as your slides do it, one arrow at a time:**
- Start: circle "S0" with `L_A: green, L_B: red`, Reset arrow pointing in.
- Add: S0 →(T_A=0)→ S1 `(L_A: yellow, L_B: red)`, plus self-loop S0→S0 on `T_A=1`.
- Add: S1 →(after 5s)→ S2 `(L_A: red, L_B: green)`.
- Add: S2 →(T_B=0)→ S3 `(L_A: red, L_B: yellow)`, plus self-loop S2→S2 on `T_B=1`.
- Add: S3 →(after 5s)→ S0, closing the cycle.

**Exam perspective:** You'll often be given a paragraph description (like the parade-mode extension in Section 18) and asked to draw the diagram from scratch. Always explicitly write out: number of states, what triggers each transition, and what the self-loop conditions are, before drawing.

**Common mistakes:**
- Leaving a state with an undefined transition for some input combination (incomplete FSM).
- Forgetting the Reset arrow — examiners often check this is present and points to the correct initial state.
- Mixing Moore-style (in-circle) and Mealy-style (on-arrow) output labeling in the same diagram.

---

## 10. State Transition Tables — How to Construct One

**Concept:** The state transition table is a *mechanical translation* of the diagram into rows and columns — the "bridge between the graphical FSM representation and the digital hardware implementation" (your slides' own words).

**How to construct it, step by step:**
1. Make columns: **Current State**, one column per **input**, **Next State** (and a separate **Output** table with **Current State** and one column per output — for Moore; for Mealy, put Output in the *same* table alongside Next State, since it depends on the input too).
2. For each state, and for **every relevant combination of inputs**, write down the next state exactly as the diagram shows (Reset arrow → initial row is redundant to include, since Reset is handled separately at the register).
3. Use **X (don't care)** whenever a particular input doesn't affect the outcome for that row, **as long as you've already covered the case(s) where it does matter.** E.g., in S1 of the traffic light example, the FSM always goes to S2 regardless of both sensors — one row with `T_A=X, T_B=X` correctly covers all 4 real input combinations.
4. Double check every state/input combination is accounted for (no missing rows, no contradictory rows).

**Worked example (traffic light, Table 3.1):**

| Current State S | T_A | T_B | Next State S′ |
|---|---|---|---|
| S0 | 0 | X | S1 |
| S0 | 1 | X | S0 |
| S1 | X | X | S2 |
| S2 | X | 0 | S3 |
| S2 | X | 1 | S2 |
| S3 | X | X | S0 |

**Exam perspective:** Examiners love to test whether you use "X" correctly — using X when the input actually *does* matter is a serious error (it silently drops real behavior); failing to use X when you could (writing out all 4 explicit rows instead of 1) isn't "wrong," just less efficient — but you must show you understand which inputs are truly irrelevant in that state.

**Common mistakes:** Writing a next-state table where a `(state, input)` combination has two different "next state" entries (non-determinism) — an FSM must be fully deterministic: exactly one next state for every current state + input combination.

---

## 11. State and Output Encoding

**Concept:** So far your states have friendly names (S0, S1, S2...) — but hardware doesn't understand names, only bits. Encoding is how you translate the symbolic FSM into something flip-flops and gates can actually implement.

**Formal definition (slide):** Before implementing the FSM using digital hardware, the states and outputs must be represented in binary form. This process is called **Encoding**.
1. **State Encoding** — each state is assigned a unique binary code.
2. **Output Encoding** — each output value (e.g., traffic light color) is also represented using binary values.

**Why Encoding? (slide):**
- Converts symbolic states into binary values.
- Makes the FSM compatible with digital circuits.
- Simplifies the derivation of Boolean equations.

**Key Observation (slide):** Every state and output now has a unique binary representation. Encoding transforms the FSM from a conceptual model into a form that can be implemented using digital hardware.

**How it works, step by step:**
1. Count the number of states, N.
2. Decide *which encoding scheme* to use (Binary or One-Hot — Sections 12–13).
3. Assign each symbolic state a unique bit pattern under that scheme.
4. Rewrite your state transition table and output table using these bit patterns instead of names — this produces the "binary-encoded" versions of your tables (e.g. Table 3.4/3.5 in your traffic-light example).

**Exam perspective:** Expect direct questions like "Given 6 states, how many bits are needed for Binary Encoding? For One-Hot?" (see the worked formulas in Section 21), and "Rewrite this state table using the given encoding."

---

## 12. Binary Encoding

**Concept:** Assign states consecutive binary numbers, just like counting: 00, 01, 10, 11, ...

**Formal definition / example (Table from slide, traffic light):**

| State | Binary Code |
|---|---|
| S0 | 00 |
| S1 | 01 |
| S2 | 10 |
| S3 | 11 |

**Advantages (slide):**
- Uses fewer state bits and requires fewer flip-flops.
- Efficient hardware utilization.

**How many bits does Binary Encoding need?** For N states, you need `k = ⌈log₂ N⌉` bits (the smallest integer k such that 2ᵏ ≥ N). For 4 states, k=2 (2² = 4, exact fit). For 5 states, you'd still need k=3 (2² = 4 is not enough), leaving 3 unused/"don't care" codes.

**Exam perspective:** This is one of the most commonly quizzed numeric facts — see the two quiz questions in Section 21/23 (5 flip-flops → 32 states max with Binary Encoding; 8 states → 3 flip-flops needed with Binary Encoding).

---

## 13. One-Hot Encoding

**Concept:** Instead of counting in binary, give every state its own personal flip-flop, which is 1 only when the FSM is in that exact state — like a row of light bulbs where exactly one is ever lit.

**Formal definition (slide):** In One-Hot Encoding, each state is assigned a separate bit. Only one bit is HIGH (1) at any given time, while all other bits remain LOW (0). For K states, K bits are required.

**Example (slide, for 3 states):**

| State | Encoding |
|---|---|
| S0 | 001 |
| S1 | 010 |
| S2 | 100 |

**Advantages (slide):** Simpler combinational logic and faster implementation.
**Disadvantages (slide):** Requires more flip-flops and higher hardware cost.

**Why is the logic simpler?** Because "am I in state S1?" is answered directly by reading a single wire (bit 1 is HIGH), with no decoding needed. In Binary Encoding, you often need an AND/NOT combination of multiple bits just to detect "am I in state S1" (e.g. `S̅₁S₀`), whereas One-Hot gives you that state's identity "for free" as a single bit.

**Exam perspective:** For N states, One-Hot always needs exactly **N** flip-flops (compare with Binary's `⌈log₂N⌉`) — this trade-off (fewer flip-flops vs. simpler gates) is a favorite comparison question; see Section 15.

---

## 14. Divide-by-3 Counter — Full Worked Example

**Problem (slide):** To understand how different state-encoding techniques are applied, consider designing a **Divide-by-3 Counter** — a sequential circuit that reduces the frequency of the input clock by a factor of 3.

**System description (slide):** The FSM has one output (Y) and three states S0, S1, S2. Output Y becomes HIGH (1) for one clock cycle out of every three clock cycles.

**Operation (slide):** The counter continuously cycles: `S0 → S1 → S2 → S0 → ...` This repeating sequence divides the clock frequency by 3. (From the diagram: `Y=1` only in S0; `Y=0` in S1 and S2.)

### Step 1 — State transition table (Table 3.6)

| Current State | Next State |
|---|---|
| S0 | S1 |
| S1 | S2 |
| S2 | S0 |

### Step 2 — Output table (Table 3.7)

| Current State | Output |
|---|---|
| S0 | 1 |
| S1 | 0 |
| S2 | 0 |

### Step 3 — Binary encoding version (Table 3.9), S0=00, S1=01, S2=10

| S₁ | S₀ | S₁′ | S₀′ |
|---|---|---|---|
| 0 | 0 | 0 | 1 |
| 0 | 1 | 1 | 0 |
| 1 | 0 | 0 | 0 |

**Additional context** (not stated but a good check): the row `(1,1)` is never a valid current state, since only 3 of the 4 possible 2-bit codes are used — this is exactly the kind of "unused state" situation covered in Section 16.

### Step 4 — One-Hot version (Table 3.10), S0=001, S1=010, S2=100

| S₂ | S₁ | S₀ | S₂′ | S₁′ | S₀′ |
|---|---|---|---|---|---|
| 0 | 0 | 1 | 0 | 1 | 0 |
| 0 | 1 | 0 | 1 | 0 | 0 |
| 1 | 0 | 0 | 0 | 0 | 1 |

### Step 5 — Implementation (Figure from slide)
- **Binary implementation:** two flip-flops; next-state logic built from AND/OR gates computing `S₁′, S₀′`; output `Y` is a simple function of `(S₁,S₀)`.
- **One-Hot implementation:** three flip-flops, one per state, wired so each one's "set"/"reset" inputs directly reflect "was the previous state the one that leads to me," and `Y` can literally be *read directly off* the S0 flip-flop's output (since Y=1 exactly when in S0) — no extra output gate needed at all in this case!

**Exam perspective:** This is the cleanest example in your slides to practice building *both* encodings for the same FSM side-by-side and comparing gate count / flip-flop count — expect a similar "design a divide-by-N counter" question on the exam (see Practice Problems).

---

## 15. Binary vs. One-Hot: Direct Implementation Comparison

Using the Divide-by-3 Counter above as the concrete case:

| Aspect | Binary Encoding | One-Hot Encoding |
|---|---|---|
| Flip-flops needed | `⌈log₂N⌉` (2 for 3 states) | N (3 for 3 states) |
| Next-state logic | More complex (decoding needed) | Simpler (often direct wiring) |
| Speed | Slower (more gate levels) | Faster (fewer gate levels) |
| Hardware cost | Lower (fewer flip-flops) | Higher (more flip-flops) |
| Output decoding | May need extra gates | Often free (read the relevant bit directly) |

**Exam perspective:** A guaranteed question type: "An FSM has N states. Compare the flip-flop requirement of Binary vs One-Hot encoding," or "Why would a designer choose One-Hot despite using more flip-flops?" (Answer: speed — modern FPGAs have abundant flip-flops but gate delay matters more, so One-Hot is often preferred there.)

---

## 16. State Encoding Considerations

**Concept:** Choosing an encoding isn't just "Binary vs One-Hot" as two fixed recipes — in general, you could assign *any* unique bit pattern to *any* state, and different assignments change the resulting circuit.

**Formal statement (slide):** State Encoding is the process of assigning binary codes to the states of an FSM. The choice of encoding directly affects the hardware implementation, including the number of logic gates, propagation delay, and overall circuit complexity. Different state encodings can produce different circuit implementations, numbers of logic gates, propagation delays, and hardware complexity. Therefore, selecting an appropriate encoding helps in designing an efficient FSM.

**Why is finding the "best" encoding hard? (slide):** Finding the optimal encoding is difficult because it requires checking all possible encoding combinations, which becomes impractical as the number of states increases (this grows combinatorially — for N states there are N! possible ways to assign N distinct k-bit codes, ignoring symmetric equivalents). Instead, designers usually choose a suitable encoding based on the application, or use CAD (Computer-Aided Design) tools to help select an efficient one.

**Exam perspective:** A conceptual question like "why don't designers always search for the mathematically optimal encoding?" is testing whether you understand this combinatorial-explosion argument — you don't need to compute N!, just explain *why* exhaustive search doesn't scale.

---

## 17. Factoring State Machines

**Concept:** Just like you'd break a big program into functions, you can break one huge FSM into several smaller, cooperating FSMs.

**Formal definition (slide):** As FSM complexity increases, designing and maintaining it becomes difficult. Instead of designing one large FSM, the system can be divided into multiple smaller FSMs that work together. This design approach is called **Factoring of State Machines**.

**How does it work? (slide):**
- A complex FSM is split into smaller interacting FSMs.
- The output of one FSM can be used as the input to another FSM.
- This creates a hierarchical and modular design.

**Intuition:** One FSM's output effectively becomes another FSM's "mode signal" input — like a manager FSM deciding *which mode* the system is in, and a worker FSM that behaves differently based on that mode signal, without needing to duplicate all of the worker's states for every mode.

---

## 18. Factored vs. Unfactored FSMs — Parade Mode Example

**Extended problem (slide):** Take the traffic light controller and add a **Parade Mode**, which keeps the Bravado Blvd. light green while a parade is in progress.

**Additional inputs (slide):**
- **P (Parade)** — enables Parade Mode.
- **R (Resume)** — exits Parade Mode.

**Controller behavior (slide):**
- When `P = 1`, the controller enters Parade Mode.
- During Parade Mode, the controller follows its normal sequence until `L_B` becomes green.
- Once `L_B` is green, it remains green throughout the parade.
- When `R = 1`, the controller exits Parade Mode and resumes normal traffic operation.

**Objective (slide):** Design the controller using: (a) a single FSM (**Unfactored**), and (b) multiple interacting FSMs (**Factored**).

### Unfactored FSM (single, large FSM)
**Approach (slide):** A single FSM controls both normal operation and parade mode.
- **State organization:** S0–S3 → Normal Mode; S4–S7 → Parade Mode.
- The two halves of the diagram are almost identical, except that during Parade Mode the controller *remains in State S6* while `L_B` stays green (instead of cycling on).
**Characteristics (slide):** One large FSM, more states, complex transitions, difficult to modify and debug.

### Factored FSM (two cooperating FSMs)
**Approach (slide):** Break the controller into two smaller FSMs.
1. **Mode FSM:** tracks whether the controller is in Normal Mode or Parade Mode; produces output **M**.
2. **Lights FSM:** controls the traffic lights, using traffic sensors (`T_A, T_B`) **and** the Mode signal **M** produced by the Mode FSM.

Notice: the Lights FSM doesn't need to know *why* it's in parade behavior — it just reads `M` as another input, exactly like it reads `T_A` and `T_B`. This is the essence of factoring: the Mode FSM's output feeds the Lights FSM's input.

### Factored vs. Unfactored comparison (slide table)

| Unfactored FSM | Factored FSM |
|---|---|
| Single large FSM | Multiple smaller FSMs |
| More states | Fewer states per FSM |
| Complex transitions | Simpler transitions |
| Difficult to debug | Easier to debug |
| Less modular | Highly modular |
| Harder to modify | Easy to extend |

**Exam perspective:** You may be asked to sketch *both* versions of a modified FSM (e.g., add a new mode to the traffic-light example) and argue for factoring — the standard answer is always "fewer total states per machine, easier to verify each piece independently, easier to extend later."

---

## 19. Deriving an FSM from a Schematic — Keypad-Lock Example

This is the **reverse process**: you're handed a *circuit*, not a word problem, and must reconstruct what FSM it implements.

**General steps (slide):**
1. Examine the circuit and identify Inputs, Outputs, State bits.
2. Write the next-state and output equations.
3. Create the next-state and output tables.
4. Remove unreachable states.
5. Assign symbolic names to valid states.
6. Draw the state transition diagram.
7. Describe the overall function of the FSM.

### Scenario (slide)
Alyssa P. Hacker discovers her keypad lock has been rewired, and the original unlock code no longer works. She finds the circuit diagram (Figure 3.35) attached to the keypad and suspects it's FSM-controlled. She derives the FSM from the circuit to find the correct unlock sequence.

### Step 1 — Identify Inputs, Outputs, State Bits, and read off equations directly from the gates
From the circuit (Figure 3.35 — two AND gates with some inverted inputs, feeding a 2-bit state register with Reset, output taken from `S₁`):
- **Inputs:** A₁, A₀ (a 2-bit code, e.g. entered on a keypad)
- **Output:** Unlock
- **State bits:** S₁, S₀

**How you read equations off a schematic:** trace each gate. A bubble on an input to an AND gate means that input is *complemented* before the AND. Multiply together whatever feeds each AND gate (respecting bubbles) to get that gate's output expression; OR together multiple AND-gate outputs if they feed a common OR gate before the flip-flop input.

### Step 2 — Create the Next-State and Output Tables (Tables 3.17, 3.18)

**Table 3.17 — Next-state table, all 16 combinations of (S₁,S₀,A₁,A₀):**

| S₁ | S₀ | A₁ | A₀ | S₁′ | S₀′ |
|---|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 0 | 0 |
| 0 | 0 | 0 | 1 | 0 | 0 |
| 0 | 0 | 1 | 0 | 0 | 0 |
| 0 | 0 | 1 | 1 | 0 | **1** |
| 0 | 1 | 0 | 0 | 0 | 0 |
| 0 | 1 | 0 | 1 | **1** | 0 |
| 0 | 1 | 1 | 0 | 0 | 0 |
| 0 | 1 | 1 | 1 | 0 | 0 |
| 1 | 0 | 0 | 0 | 0 | 0 |
| 1 | 0 | 0 | 1 | 0 | 0 |
| 1 | 0 | 1 | 0 | 0 | 0 |
| 1 | 0 | 1 | 1 | 0 | 0 |
| 1 | 1 | 0 | 0 | 0 | 0 |
| 1 | 1 | 0 | 1 | **1** | 0 |
| 1 | 1 | 1 | 0 | 0 | 0 |
| 1 | 1 | 1 | 1 | 0 | 0 |

**Table 3.18 — Output table:**

| S₁ | S₀ | Unlock |
|---|---|---|
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 1 |
| 1 | 1 | 1 |

**⚠️ A note on the printed equations, in fairness to you as a student:** the slide text prints the next-state equations as `S₁' = S₀A₁A₀` and `S₀' = S̄₁S₀A₁A₀`, and `Unlock = S₁`. If you actually multiply those literals out and compare row-by-row against Table 3.17 above (which is also from your slides), they don't quite match — the table shows `S₁'=1` only when `A₁=0` (not A₁=1), and `S₀'=1` only when `S₀=0` (not S₀=1). This looks like a small transcription slip in the printed formulas (a missing complement bar) rather than an error in the table. **Reading the equations correctly off the table itself** (which is the authoritative, checkable source):
```
S₁′ = S₀ · A̅₁ · A₀
S₀′ = S̅₁ · S̅₀ · A₁ · A₀
Unlock = S₁          ← this one matches Table 3.18 exactly, no issue
```
This is a good habit to build for your exam: whenever you're given both an equation and a truth table for the same function, **spot-check the equation against a couple of table rows** before trusting it — a fast way to catch printing/transcription errors before they cost you marks downstream.

### Step 3 — Reduce the State Tables: Remove Unreachable States
**Concept (slide):** Some state combinations are never reached during operation — these are called Unreachable States. Removing them simplifies the FSM.

Looking at the **Next-State** column of Table 3.17, the only next-state values that ever appear are `00`, `01`, and `10`. The code `11` never appears as anyone's next state — meaning, starting from Reset (which clears the register to `00`), the FSM can *never* reach state `11`. So `11` is unreachable and gets dropped, leaving the **Reduced tables (Table 3.19, 3.20)** with just the 3 reachable current-states (`00,01,10`) as rows (each with all 4 input combinations, i.e. 12 rows total, all consistent with the corrected equations above).

### Step 4 — Assign Symbolic Names to Valid States

| Binary State | Symbolic Name |
|---|---|
| 00 | S0 |
| 01 | S1 |
| 10 | S2 |

### Step 5 — Draw the State Transition Diagram
Using `A` to mean the 2-bit value `A₁A₀` read as a number (0–3):
```
        Reset
          ↓
        ┌────┐  A=3  ┌────┐  A=1  ┌────┐
   ┌───→│ S0 │──────→│ S1 │──────→│ S2 │
   │    │ (0)│        │ (0)│        │ (1)│
   └────┘ A≠3└────────┘ A≠1└────────┘
          ↑______________________________|
```
- Reset → S0
- S0 → S1 when `A = 3` (i.e., `A₁A₀ = 11`); S0 stays at S0 when `A ≠ 3`
- S1 → S2 when `A = 1` (i.e., `A₁A₀ = 01`); S1 returns to S0 when `A ≠ 1`
- S2: Unlock = 1 (output). From S2 the circuit returns to S0 (ready for the next attempt).

**Overall Behaviour (slide):** The FSM unlocks the door only after detecting the correct input sequence. After unlocking, it returns to the initial state, ready for the next input sequence.

**Putting it together — this tells Alyssa the code!** Entering `A = 3` (binary `11`) first, then `A = 1` (binary `01`) next, drives the FSM S0→S1→S2, at which point Unlock = 1. **That two-step sequence, "3 then 1," is the rewired unlock code.**

**Exam perspective:** "Derive the FSM from schematic" questions are graded step-by-step (exactly the 7 steps above) — even if your final diagram is right, you can lose marks for skipping the "identify unreachable states" step or for not explicitly writing the equations before the table. Always show all 7 steps in order.

**Common mistakes:**
- Forgetting to check for unreachable states — including a "dead" state in your final diagram is a common and costly error.
- Not verifying your equations against the truth table before proceeding (see the note above — this exact scenario is worth practicing).

---

## 20. Complete FSM Design Procedure (Review)

**General steps (slide, "FSM Review"):**
1. Identify the inputs and outputs.
2. Draw the State Transition Diagram.
3. Create: State Transition Table, Output Table.
4. Select a suitable State Encoding.
5. Write: Next-State Equations, Output Equations.
6. Design the circuit schematic.

**Note the symmetry with Section 19:** designing an FSM (word problem → circuit) is exactly the *forward* direction of this list; deriving an FSM from a schematic (Section 19) is the *reverse* — circuit → equations → tables → (remove unreachable states) → symbolic states → diagram → behavior description. Knowing both directions cold is the single best exam-prep habit for this topic.

---

## 21. Important Formulas and Exam Points

**Core formulas:**
- States possible with *k* flip-flops (any encoding, upper bound): `2^k`
- Minimum flip-flops needed for N states with **Binary Encoding**: `k = ⌈log₂ N⌉`
- Flip-flops needed for N states with **One-Hot Encoding**: exactly `N`
- Moore output: `Output = f(Current State)`
- Mealy output: `Output = f(Current State, Current Input)`

**Worked quiz examples (from your slides):**
1. *"An FSM is implemented using 5 flip-flops with Binary Encoding. What is the maximum number of states that can be represented?"*
   Each flip-flop stores 1 bit → number of flip-flops = 5 → maximum states = `2⁵ = 32`. **Answer: D) 32.**
2. *"An FSM has 8 states. Using Binary Encoding, how many flip-flops are required?"*
   You need the smallest k with `2^k ≥ 8`. `2³ = 8` exactly. **Answer: B) 3.**
3. *"In Example [keypad lock], the output Unlock is given by `Unlock = S₁`. This indicates the output depends on..."*
   Since it's a function of state bits only, no input terms — **Answer: B) Current state only** (confirms this circuit implements a Moore-type output).

**Key exam checklist:**
- Can you state the FSM definition and the three structural blocks from memory?
- Can you build a state diagram directly from a paragraph?
- Can you convert diagram → table → binary-encoded table → equations → circuit, and also go in reverse?
- Do you know when to use X (don't care) in a table, and why?
- Can you compute flip-flop counts for both Binary and One-Hot given N states?
- Can you explain, in one line each, why Mealy needs fewer states, why One-Hot is faster but costlier, and why factoring helps maintainability?

---

## 22. Common Mistakes (Consolidated)

- Believing an FSM can be in more than one state simultaneously — it cannot.
- Confusing "number of states" with "number of flip-flops" (they're related by 2ᵏ ≥ N, not equal).
- Mixing up Moore (output inside the state circle) and Mealy (output on the transition arrow) diagram conventions.
- Assuming Moore output reacts instantly to input changes — it only changes after a clock edge moves the state.
- Using "X" (don't care) in a transition table when the input actually does matter for that row — this silently deletes real behavior.
- Leaving a state/input combination unspecified in a transition table (non-total FSM) or giving it two different next states (non-deterministic FSM).
- Forgetting the Reset arrow in a state diagram, or pointing it at the wrong state.
- Skipping the "remove unreachable states" step when deriving an FSM from a schematic.
- Trusting a printed Boolean equation without spot-checking it against the accompanying truth table (see Section 19's worked caution).
- Believing there is always one uniquely "best" state encoding to search for — exhaustive search is impractical as N grows; practical designs pick a reasonable scheme (Binary/One-Hot) or use CAD tools.

---

## 23. Practice Problems

*Attempt all of these yourself first — the Answer Key is in the separate section below. Don't scroll ahead!*

### A. Conceptual Questions (5)
1. Explain, in your own words, why an FSM is called "synchronous," and why this matters for predictability of behavior.
2. Why does a circuit with *k* flip-flops have at most 2ᵏ possible states? Under what circumstances would it have *fewer* than 2ᵏ *reachable* states?
3. Explain why a Mealy machine can typically implement the same behavior as a Moore machine using fewer states. Use the robotic-snail example to illustrate.
4. What does "factoring" a state machine mean, and what specific problem does it solve as FSMs grow large? Use the parade-mode traffic light as your example.
5. Explain the difference between the *forward* FSM design procedure and the *reverse* "derive FSM from schematic" procedure. Which steps are shared, and which are unique to each?

### B. Multiple-Choice Questions (5)
1. In a Moore machine, the output changes:
   A) The instant an input changes B) Only after a clock edge changes the state C) Never D) Only during Reset
2. For an FSM with 6 states using One-Hot encoding, how many flip-flops are required?
   A) 3 B) 4 C) 6 D) 2
3. For an FSM with 6 states using Binary encoding, how many flip-flops are required (minimum)?
   A) 6 B) 2 C) 3 D) 5
4. Which of these is an advantage of One-Hot encoding over Binary encoding?
   A) Fewer flip-flops B) Simpler, faster combinational logic C) Lower hardware cost D) Fewer states
5. In the traffic light controller, which signal determines *when* (not *whether*) the FSM moves from S1 to S2?
   A) T_A B) T_B C) A fixed 5-second timer D) Reset

### C. State-Diagram / State-Table Questions (5)
1. Draw the Moore state diagram for a sequence detector for "11" (robotic snail example) from scratch, labeling all states, transitions, and outputs, without looking back at Section 7.
2. Convert your Moore diagram from Q1 into an equivalent Mealy diagram, and explain why it needs one fewer state.
3. Build the full state transition table (with binary encoding) for the Divide-by-3 counter, starting only from the English description in Section 14 (don't copy the table — derive it).
4. For the keypad-lock example, given only the *unreduced* Table 3.17 (Section 19), identify which state code is unreachable and explain how you know.
5. Sketch (diagram only, no equations required) the Unfactored FSM state diagram structure for the Parade Mode traffic light (S0–S3 normal, S4–S7 parade) based on the description in Section 18.

### D. Numerical / Design Questions (5)
1. An FSM needs to represent 10 distinct states. How many flip-flops are required using (a) Binary Encoding, (b) One-Hot Encoding?
2. Derive the simplified next-state equation `S₁′` for the traffic-light controller from its two unreduced product terms, showing the Boolean-algebra factoring step explicitly (Section 8, Step 6).
3. For the Mealy snail detector (Section 7), verify by truth-table substitution that `Q′ = A̅` and `Y = Q·A` reproduce every row of Table 3.16.
4. Design a Moore FSM (states, diagram, and binary-encoded transition table) for a "divide-by-4 counter" whose output Y is HIGH for one cycle out of every 4 (analogous to Section 14, but for 4 states).
5. Given the keypad-lock corrected equations `S₁′ = S₀A̅₁A₀` and `S₀′ = S̅₁S̅₀A₁A₀`, compute the next state for `(S₁,S₀,A₁,A₀) = (0,1,0,1)` and confirm it matches Table 3.17.

### E. Difficult University-Exam-Style Problems (3)
1. **(Full design)** A campus vending machine accepts only exact change using two coin types: a 5-rupee coin (input `F`) and a 10-rupee coin (input `T`). An item costs 10 rupees. Design a Moore FSM: identify states (in terms of "money inserted so far"), draw the state diagram, build the state table, choose Binary encoding, and write the next-state and output ("Dispense") equations. (Hint: this is structurally similar to the traffic-light pipeline in Section 8 — follow the same 8 steps.)
2. **(Full derivation)** You are given a circuit with two state bits (S₁,S₀), one input B, and next-state equations `S₁′ = S₁B + S₀B̅` and `S₀′ = S̅₁S̅₀B`, output `Z = S₁S₀`. Following the 7-step reverse procedure of Section 19, build the next-state table, identify any unreachable states, assign symbolic names, and draw the final diagram. Describe in one sentence what the FSM appears to do.
3. **(Comparative design)** Take the robotic-snail "detect 11" Moore FSM from Section 7. Now suppose the requirement changes to "detect 101" (three-bit pattern). Design the new Moore FSM (states, diagram, transition/output table) and the equivalent Mealy FSM, and state clearly how many states each needs and why the difference (if any) appears.

---

## Answer Key & Explanations

### A. Conceptual — Answers
1. "Synchronous" means all state changes are gated by the clock edge — state can only change at those discrete instants, never in between. This makes behavior predictable and analyzable: at any moment between edges, you know for certain the FSM is stably sitting in one well-defined state, with no in-between/glitchy states to worry about, unlike a purely combinational or unclocked circuit.
2. Each flip-flop stores 1 bit, and k independent bits can represent 2ᵏ distinct combinations — hence at most 2ᵏ states are representable. Fewer than 2ᵏ states may actually be *reachable* if some bit-patterns are simply never produced as a next-state by the design (exactly what happened with code `11` in the keypad-lock example — Section 19) — those are the "unreachable states" that get removed during reduction.
3. Mealy can emit an output *during a transition* rather than needing a dedicated state to represent "pattern just completed." In the snail example, Moore needs a separate S2 state purely to mean "the 11 pattern was just seen" so it can emit Y=1 while in that state; Mealy instead emits `1` directly on the transition arrow S1→S0 labeled `1/1`, collapsing that information into the transition itself and needing only 2 states total.
4. Factoring splits one FSM into cooperating smaller FSMs, each handling one concern (e.g., a Mode FSM tracking Normal/Parade, and a Lights FSM handling the actual light sequencing, reading the Mode FSM's output as an extra input). This solves the "one giant, hard-to-debug/hard-to-extend FSM" problem — in the parade example, the Unfactored version needed to double its state count (S0–S7) to add one new feature, while the Factored version added the feature with a small, separate 2-state Mode FSM instead.
5. Forward design goes: inputs/outputs → diagram → table → encoding → equations → circuit (problem to hardware). Reverse derivation goes: circuit → equations (read off gates) → tables → remove unreachable states → symbolic names → diagram → behavior description (hardware back to problem). The shared core is the middle: both use "table ↔ equations ↔ diagram" as the common representations; what's unique is direction — forward starts from a description and ends at a circuit, reverse starts from a circuit and ends at a description, and only the reverse process needs an explicit "remove unreachable states" step, since real, unplanned circuits often contain unused codes.

### B. Multiple-Choice — Answers
1. **B** — output only updates after a clock edge changes the state (Moore's defining property).
2. **C) 6** — One-Hot always needs exactly N flip-flops for N states.
3. **C) 3** — smallest k with 2^k ≥ 6 is k=3 (2³=8 ≥ 6; 2²=4 < 6).
4. **B** — simpler, faster combinational logic is One-Hot's advantage (at the cost of more flip-flops, i.e., NOT A or C).
5. **C** — the slide states S1→S2 happens "automatically after 5 seconds," a pure timer transition, independent of T_A or T_B.

### C. State-Diagram / Table — Answers
1. Same structure as Section 7's Moore diagram: S0 (output 0, "no progress") --1--> S0 self-loop is wrong — recheck against your slide-exact version: S0--0-->S1, S0--1-->S0(self); S1--1-->S2, S1--0-->S0; S2--1-->S2(self), S2--0-->S1. Outputs: S0:0, S1:0, S2:1.
2. Mealy version: S0--0/0-->S1, S0--1/0-->S0(self); S1--0/0-->S1(self), S1--1/1-->S0. It needs one fewer state because Moore's S2 ("pattern just completed, holding output 1") is unnecessary in Mealy — the `1` output is emitted directly on the S1→S0 transition instead of requiring a dedicated state to "hold" that information for one cycle.
3. From "S0→S1→S2→S0 repeating, Y=1 only in S0": table is S0→S1 (Y=1 at S0), S1→S2 (Y=0), S2→S0 (Y=0) — matches Table 3.6/3.7 exactly (Section 14). With binary encoding S0=00,S1=01,S2=10: (00→01), (01→10), (10→00).
4. Code `11` is unreachable: scanning the Next-State (`S₁′,S₀′`) column of Table 3.17, the value `11` never appears anywhere — every row produces `00`, `01`, or `10` as the next state, so starting from Reset (`00`), the FSM can never land on `11`.
5. Structurally: two 4-state rings, S0–S3 (normal) and S4–S7 (parade), nearly mirroring each other's internal transitions (green→yellow→green→yellow pattern on each road), except that the parade-mode ring's state analogous to "Bravado green" (S6) has a self-loop that holds indefinitely instead of timing out, and `P`/`R` provide the only cross-links between the two rings (P moves you from the normal ring into the parade ring; R moves you back).

### D. Numerical / Design — Answers
1. (a) Binary: `k=⌈log₂10⌉=4` (2³=8 <10 ≤ 2⁴=16). (b) One-Hot: exactly 10 flip-flops.
2. `S₁S̅₀T̅_B + S₁S̅₀T_B` — factor out the common `S₁S̅₀`: `= S₁S̅₀(T̅_B + T_B) = S₁S̅₀ · 1 = S₁S̅₀`. Combined with the remaining term `S̅₁S₀`, full equation: `S₁′ = S̅₁S₀ + S₁S̅₀ = S₁⊕S₀`.
3. Table 3.16 rows: (Q=0,A=0)→(Q′=1,Y=0): A̅=1 ✓, Q·A=0 ✓. (Q=0,A=1)→(Q′=0,Y=0): A̅=0 ✓, Q·A=0 ✓. (Q=1,A=0)→(Q′=1,Y=0): A̅=1 ✓, Q·A=0 ✓. (Q=1,A=1)→(Q′=0,Y=1): A̅=0 ✓, Q·A=1 ✓. All four rows match.
4. 4 states S0–S3 in a ring (S0→S1→S2→S3→S0), Y=1 only at S0 (or wherever you choose to define the "divide" pulse — must be exactly 1 of the 4 states), binary encoding S0=00,S1=01,S2=10,S3=11, transition table `00→01, 01→10, 10→11, 11→00`, and Y = a single minterm equation identifying your chosen "pulse" state (e.g. `Y=S̅₁S̅₀` if S0 is the pulse state).
5. `(S₁,S₀,A₁,A₀)=(0,1,0,1)`: `S₁′ = S₀·A̅₁·A₀ = 1·1·1 = 1`. `S₀′ = S̅₁·S̅₀·A₁·A₀ = 1·0·0·1 = 0`. So next state = `(1,0)`. Checking Table 3.17, row `(0,1,0,1)` indeed shows `S₁′=1, S₀′=0`. ✓ Matches.

### E. Difficult Problems — Answers
1. States (money inserted): S0 ("₹0"), S1 ("₹5"), S2 ("₹10 or more, dispense"). Diagram: S0--F-->S1, S0--T-->S2; S1--F-->S2, S1--T-->S2 (₹5+₹10=15≥10, dispense, and give ₹5 change — note, exact-change assumption in the prompt means you can simplify by assuming no change-giving is required, or extend for extra credit); S2--(any)-->S0 (reset after dispensing, or hold — design choice, state clearly). Binary encoding S0=00,S1=01,S2=10. Dispense = S₁ (state S2's high bit), following the exact same pattern as `Unlock = S₁` in the keypad-lock example (Section 19) — a good example of how one FSM "pattern" (a counting-up-to-threshold FSM with a single high-order output bit indicating "reached threshold") recurs across different problems.
2. Next-state table (all 4 combos of S₁,S₀,B): compute each row from the given equations. You'll find `S₁′=1` only for `(S₁,S₀,B)=(1,0,1)` [from S₁B] and `(0,1,0)` [from S₀B̅]; `S₀′=1` only for `(0,0,1)`. Every state (00,01,10,11) actually appears as a next-state at least once here (verify this yourself row-by-row) — if you find all 4 are reachable, there's nothing to remove, and you'd assign symbolic names S0=00,S1=01,S2=10,S3=11 and draw all 4 states with their transitions. Output `Z=S₁S₀` is 1 only in state 11 (S3) — describing the FSM as "a circuit that outputs 1 exactly when in its 4th/highest state," structurally a small binary counter or sequence-based indicator (exact description depends on which transitions you find dominant — this is intentionally open-ended, matching real exam derivation questions).
3. Moore "detect 101": needs 4 states (S0: no relevant match, S1: saw "1", S2: saw "10", S3: saw "101", output 1) — because you must distinguish "saw 1, then 0" (2 characters matched) from "saw just 1" (1 character matched), which Mealy can often compress. Mealy: typically 3 states (tracking "no match," "matched 1," "matched 10") with the output `1` emitted directly on the transition that completes the "1" from "10". Moore needs one more state than Mealy here, for the same underlying reason as the "11" case: Moore needs a dedicated state to *hold* the completed-match output for one full cycle, while Mealy emits it in-transition.

---

## One-Page Revision Sheet

**Definitions**
- FSM: synchronous circuit that stores current state, moves to next state on each clock edge; "finite" = limited states.
- 3 blocks: Next-State Logic (combinational) → State Register (memory, clocked) → Output Logic (combinational).
- 5 parts: M inputs, N outputs, k-bit state register, CLK, (optional) Reset.

**Moore vs Mealy**

| | Moore | Mealy |
|---|---|---|
| Output = f(...) | Current State only | Current State + Input |
| Output label position | Inside state circle | On transition arrow |
| States needed | More | Fewer |
| Output timing | 1 clock delay, stable | Immediate, can glitch |
| Use when | Stability matters | Speed matters |

**Binary vs One-Hot**

| | Binary | One-Hot |
|---|---|---|
| Flip-flops for N states | ⌈log₂N⌉ | N |
| Combinational logic | More complex | Simpler |
| Speed | Slower | Faster |
| Hardware cost | Lower | Higher |

**Flip-flop / state relationships**
- k flip-flops → max 2ᵏ states.
- N states, Binary encoding → min flip-flops = ⌈log₂N⌉.
- N states, One-Hot encoding → exactly N flip-flops.
- Some binary codes may be **unreachable** — always check the next-state table before finalizing a design derived from a circuit.

**FSM design steps (forward):** Inputs/Outputs → State Diagram → State Table + Output Table → Choose Encoding → Next-State & Output Equations → Circuit.

**Deriving FSM from schematic (reverse):** Circuit → Identify I/O/state bits → Equations → Tables → Remove unreachable states → Symbolic names → Diagram → Describe behavior.

**Factoring:** Split one large FSM into smaller cooperating FSMs; one's output feeds another's input; improves modularity/debuggability at the cost of inter-FSM coordination.

**Common exam traps**
- Output changing "immediately" — true only for Mealy, never Moore.
- Using "X" (don't care) where the input actually matters.
- Forgetting to remove unreachable states when deriving from a circuit.
- Confusing states-count with flip-flop-count (2ᵏ relationship, not equality).
- Not verifying a printed equation against its own truth table (equations can contain typos — the table is usually the more reliable ground truth).

**Running examples map (for quick recall of "which problem is this?")**
- **Traffic light controller** → full forward design pipeline (Moore, 4 states, Binary encoding).
- **Divide-by-3 counter** → Binary vs One-Hot side-by-side comparison.
- **Robotic snail** → Moore vs Mealy comparison (pattern "11").
- **Keypad lock** → reverse derivation (circuit → FSM), unreachable-state removal.
- **Parade-mode traffic light** → Factored vs Unfactored FSM design.
