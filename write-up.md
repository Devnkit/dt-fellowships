# Write-Up: Design Rationale for The Daily Reflection Tree

**Candidate:** [Your Name]
**Assignment:** DT Fellowship — Daily Reflection Tree

---

## Why These Questions

The central design challenge wasn't finding "good" questions — it was finding questions where *all the options are honest*. A question fails if any of its options feel obviously shameful to pick. When that happens, the employee self-selects the flattering path, and the tree gives them a reflection they already held. That's not reflection — it's confirmation.

Each question went through the following test: *Would a tired, frustrated, self-aware person realistically pick any of these options?* The options that passed were kept; those that felt like moral traps were rewritten.

**On Axis 1 (Locus):** The opening question asks for a single word to describe the day — not a rating, not a score. This matters because the word they choose is emotionally uncurated. "Frustrating" and "Productive" surface two very different orientations before any structured question has been asked. The branch on A1_D1 routes them into different second questions, so someone who had a hard day isn't being asked to reframe it before they've been seen.

The second Axis 1 question ("Think of one specific moment...") is designed to find the crack of agency even in an external-locus path. Option C ("I blamed the situation or the system") is deliberately non-shaming — it's an accurate description of how most people respond to systemic failures. Picking it doesn't make you a victim. It makes you honest. The reflection that follows names this gently.

**On Axis 2 (Orientation):** The entitlement literature (Campbell et al., 2004) makes clear that entitlement is largely invisible to the person holding it — it feels like justice, not greed. This means questions that directly surface entitlement ("Did you feel you deserved more?") are ineffective; the person simply says no. The approach here is indirect: ask about the *interaction* first, then ask about the *motivation* second. Someone who describes helping a colleague is then asked *why* — this probes whether the contribution orientation is genuine or transactional.

The follow-up question for the entitlement branch asks *how they responded* to feeling unrecognized — not whether they were right to feel it. This separates the feeling (valid) from the behavior (observable and improvable), which is the difference between reflection and judgment.

**On Axis 3 (Radius):** The self-transcendence literature (Maslow, 1969; Batson, 2011) suggests that widening concern isn't a moral upgrade — it's a cognitive shift. The question "Whose presence comes up most?" is phenomenological: it asks what *actually happened* in the employee's mental experience, not what should have happened. This makes it harder to game. The options progress from narrow (just me) to wide (customers/mission), and the routing distinguishes people who held others in mind but didn't act from those who acted — because both are meaningfully different from self-absorption.

---

## How the Branching Was Designed

The tree uses a two-question structure per axis: a **broad intake question** followed by a **probing follow-up**. This gives each axis two signal opportunities, so the dominant direction is always based on at least two data points rather than a single choice.

The **decision nodes** are invisible to the employee. This was a deliberate choice: if a user knew the tree was routing them based on their previous answer, they might try to "correct" for it. The transparency of the experience is in the reflections, not the routing.

The **summary key** (8 possible combinations of 3 binary axes) uses pre-written templates with a closing paragraph tailored to the exact combination of signals. The most important design constraint here was that none of the eight summaries should feel like punishment. Even `external+entitlement+self` ends with: *"Just notice it, rest, and come back tomorrow with one small thing you can own."* — an actionable forward direction, not a verdict.

**Trade-offs made:**

- *Depth vs. breadth:* I chose to go two questions deep on each axis rather than one question per axis across more axes. This produces a richer signal but makes the session about 8–10 minutes long — possibly too long for a daily ritual. With more time, I would user-test session length to find the right depth.

- *Binary poles vs. continuous spectrum:* The signals tally toward two poles per axis, which simplifies the summary but loses nuance. A spectrum score (e.g., axis1: 0.6 internal) would be more accurate but harder to template into meaningful summaries. The binary approach was chosen for the deterministic constraint.

- *No free text:* The assignment required fixed options only — and I found this constraint generative. It forced me to design options that genuinely span the spectrum, rather than relying on free text to capture edge cases. The hardest work in this project was writing option sets where every option is realistic.

---

## Psychological Sources

| Source | How it informed the design |
|--------|--------------------------|
| Rotter, J.B. (1966). *Generalized expectancies for internal versus external control of reinforcement.* Psychological Monographs. | Defined the internal/external locus of control construct. Axis 1 questions were designed to surface behavioral indicators (how you respond to setbacks) rather than beliefs (do you think you control your life), which Rotter's scale showed are more reliable signals. |
| Dweck, C.S. (2006). *Mindset: The New Psychology of Success.* | The growth mindset framing — that seeing choices and room to improve is a learnable orientation, not a personality trait — shaped the reflection text tone. Reflections avoid saying "you ARE an internal/external person" and instead describe behavior: "you stayed close to your choices today." |
| Campbell, W.K., Bonacci, A.M., Shelton, J., Exline, J.J., & Bushman, B.J. (2004). Psychological entitlement. *Personality and Social Psychology Bulletin.* | Entitlement is invisible to the holder. Axis 2 questions avoid directly asking "did you feel entitled" and instead surface the orientation through behavior (how you responded, what was behind your help). |
| Organ, D.W. (1988). *Organizational Citizenship Behavior.* | OCB's concept of discretionary prosocial effort shaped the "contribution" pole options — specifically the idea that genuine contribution goes beyond role requirements and isn't motivated by reciprocity. |
| Maslow, A.H. (1969). The farther reaches of human nature. *Journal of Transpersonal Psychology.* | The often-omitted peak of Maslow's hierarchy. Self-transcendence — orienting toward what the world needs from you — is the conceptual anchor for Axis 3. The reflection for the altrocentric path references this shift: "from 'what's happening to me?' to 'what's needed here?'" |
| Batson, C.D. (2011). *Altruism in Humans.* Oxford University Press. | Perspective-taking as a cognitive act, not just a feeling. Axis 3 questions are designed to surface whether the employee is engaging in genuine perspective-taking (imagining another's situation) vs. empathy-as-sympathy (feeling bad for them but staying self-referential). |

---

## What I'd Improve With More Time

**Session cadence data.** The tree can't currently learn from patterns across sessions. With a lightweight session log, the summary could reference yesterday's reading ("You called yesterday 'Productive' too — but the reflection path looked different"). This would make the tool dramatically more powerful over time.

**Adaptive question depth.** If an employee gives two strong internal-locus signals in a row, the tree could shorten Axis 1 and spend more time on the axes with more ambiguity. Currently the structure is symmetric regardless of clarity.

**The mid-range options.** Several questions have options that sit at neither pole clearly — "It just fell into place on its own" (A1_Q_HIGH) is ambiguous between genuine external locus and deserved humility. I routed it toward the external path, but it warrants deeper design thinking and possibly splitting into two options.

**Testing with actual tired people.** The acid test for any reflection tool is whether it works at 9pm after a hard day. I'd want to pilot this with five real employees for a week and see which questions get clicked through without reading, which options feel missing, and which reflections land or fall flat. All of that is invisible in the design phase.
