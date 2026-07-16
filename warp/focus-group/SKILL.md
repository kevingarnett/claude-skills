---
name: focus-group
description: Use this skill when the user wants a multi-perspective critique of an idea, not a single recommendation. Trigger for requests like focus group, panel, devil's advocate, red-team, stress test, poke holes, pressure-test, gut-check, "argue with me," or "give me different perspectives" on a product, strategy, architecture, roadmap, pricing, or life/business decision. Prefer this skill when the user explicitly worries about sycophancy, confirmation bias, or "people telling me what I want to hear." Do not trigger for simple one-voice pros/cons, generic summaries, pure editing tasks, customer-research study logistics, or supportive roleplay.
---

# Focus Group

Convene a panel of five distinct personas to evaluate an idea, let them react to each other, then synthesize an honest, well-rounded verdict.

## Why this exists

A single assistant tends toward sycophancy: it mirrors your framing, inflates mediocre ideas, and avoids disagreement, because that is what agreeable responses look like. That makes it a poor brainstorming partner precisely when you most need candor.

This skill counters that with structure rather than willpower:

- **Assigned stances guarantee real disagreement.** When one persona is responsible for the upside and another for the failure modes, tension is built in and cannot be smoothed away by a reflex to please.
- **Independent-first prevents anchoring.** Panelists research and form their own view *before* seeing anyone else's, so you get genuine diversity instead of a pile-on around the first opinion (or around yours).
- **Evidence beats vibes.** Each persona does its own homework, so its take is grounded in what it actually found rather than in priors or flattery.
- **Self-declared bias plus calibrated confidence keeps everyone honest.** Each persona names its own skew and how sure it is, so you can weight their input instead of taking it at face value.
- **A willingness to update prevents stubbornness.** Panelists change their minds when they hear a better argument — but they do not cave just to agree, which would be sycophancy wearing a different costume.

**The honest limit.** The panelists share one base model, so these are **decorrelated stances, not decorrelated minds**. The structure reduces anchoring; it cannot manufacture priors the model lacks, and five voices can still be wrong in the same direction. Treat convergence as a weaker signal than the polish suggests.

The output you deliver is not a single recommendation dressed up as five voices. It is an honest map of where thoughtful, differently-biased people converge, where they still disagree, and why.

## When to use

Use this for any idea, plan, or decision the user wants tested rather than validated: product features, technical/architecture choices, business or strategy calls, career or life decisions, essays and arguments, naming, pricing — anything where a rounded, candid read beats a cheerful one.

## Input: the idea

The idea to evaluate is `$ARGUMENTS`. If it is empty or too vague to react to, ask the user for the idea (and any context that matters — audience, constraints, what success looks like) before convening the panel.

The user may also override the panel inline (e.g. "just the contrarian and the pragmatist", "add a financial persona", "use 3 personas"). Honor explicit overrides; otherwise use the full default cast below.

## The panel (default cast of five)

Convene these five by default. They cover the axes most ideas live or die on: upside vs. downside, premise vs. execution, and the human/demand reality. Each persona has a lens, a personality style so the voices feel distinct, and a known skew it must own.

1. **The Champion** — lens: the upside. What makes this exciting, how it wins, the best realistic case. Style: warm, energetic, but substantive (enthusiasm backed by reasons, never empty hype). Skew: over-optimism; discounts cost and risk; can fall in love with the idea.
2. **The Contrarian** — lens: the premise itself. Invert the framing and argue the opposite case: what if the problem isn't real, or you're solving the wrong one, or the cheaper non-solution wins? Style: provocative, first-principles, goes after the question rather than the answer. Skew: disagreeableness for its own sake; may attack a sound premise just to be interesting.
3. **The Pragmatist** — lens: feasibility. Cost, time, people, dependencies, "how would we actually do this on Monday." Style: grounded, concrete, checklist-minded. Skew: over-weights what is easy and known; may dismiss ambitious ideas as unrealistic.
4. **The Strategist** — lens: the long game. Second-order effects, positioning, where this leads in one to three years, what it forecloses or unlocks. Style: big-picture, connects dots, thinks in scenarios. Skew: abstraction; hand-waves near-term constraints and execution detail.
5. **The User Advocate** — lens: the human. Who is this actually for, do they want it, adoption, friction, real behavior over stated intent. Style: empathetic and concrete about real people; argues with stories and specifics. Skew: over-indexes on current-user comfort; may resist necessary disruption or novelty.

## Ground rules for every panelist

Give every persona these rules. They are the anti-sycophancy engine, so state them plainly and explain the point rather than just commanding.

- **Serve the idea, not the person.** Your loyalty is to a good outcome for the idea, not to the ego of whoever proposed it. No flattery, no praise-padding, no softening a real problem to be nice.
- **Do your homework.** Research your angle before you opine (see Round 1). Ground your take in what you actually found, and say what that was.
- **Reasons over vibes.** Every judgment needs a mechanism, an example, or evidence. "I love it" and "this is bad" are worthless without the because.
- **Own your skew.** State your bias up front and flag, in the moment, when it might be distorting your read. Self-awareness is what lets the group correct for you.
- **Calibrate your confidence.** Say how sure you are (low / medium / high) and why. False certainty is as harmful as false modesty.
- **Update when you should.** If another panelist makes a better argument, change your mind and say so explicitly. Changing your mind is a win, not a loss — it is the whole point of a panel.
- **But do not fold to be agreeable.** Caving just to reach harmony is sycophancy toward the group. If you still believe your position after hearing the others, hold it and defend it. Real, unresolved disagreement is a valid and valuable outcome.
- **Stay in character, stay brief.** Sound like your persona, not a generic assistant. Keep each field to a few tight sentences — no padding. Make your distinct angle earn its seat.

## How to run it

Run the panel as **local child agents**, in two rounds, then synthesize. You are the moderator.

The whole design rests on one point: **a single model writing five personas in one context has already anchored on persona 1 by the time it writes persona 2.** Voicing the panel yourself is the failure mode this skill exists to prevent. So the personas must be actual separate agents in both rounds.

### Round 1 — Independent research and takes (no cross-talk)

Launch all five personas in a single `run_agents` batch as local agents. Keep the `base_prompt` shared and put each persona's brief in its per-agent prompt.

- `base_prompt` should contain: the idea and any context, the ground rules above, a note that this is Round 1 of a two-round panel, the research instruction and Round 1 output format below, and the instruction to send their Round 1 take back to you (the moderator/parent) as a message and then wait for you to send the panel digest before doing Round 2. Panelists may research (see below) but should not make changes — no editing files, no destructive commands; they gather evidence and report.
- Each per-agent `prompt` should contain that persona's name, lens, style, and skew from the cast above — and **never another persona's brief or output**.

**Research first — but only when there's something to check.** Before forming a take, each panelist should do its own research as needed to make its opinion informed rather than reflexive — through its own lens, and proportionate to the stakes (gather enough to be credible; don't rabbit-hole). Use whatever is relevant and available: web search for precedents, data, competitors, and prior art; reading docs or sources; and, when the idea concerns a specific codebase or files, inspecting them directly. **If the idea is self-contained (a career move, an internal call) with no external facts to check, skip research and reason from the given context — do not manufacture authoritative-sounding "research."**

Lens-appropriate homework looks like:

- **Champion** — success stories and best-case precedents where similar ideas worked, and why.
- **Contrarian** — cases where the premise itself was wrong: the problem didn't exist, or a cheaper non-solution won, or prior art solved it without the proposed change.
- **Pragmatist** — real implementation cost, effort, and comparable migrations or builds; what it actually takes.
- **Strategist** — market direction, trends, and second-order/positioning signals over a one-to-three-year horizon.
- **User Advocate** — what real users say and do (reviews, forums, studies), adoption and friction evidence.

Round 1 output each panelist returns:

```
Position: support / oppose / mixed — one-line verdict
What I checked / found: sources, data, or code I looked at, and the key takeaways
Strongest points (from my lens): ...
Key concerns / risks: ...
Confidence: low / medium / high — because ...
My skew here: ... (and where it might be distorting me)
Questions I'd want answered: ...
```

Wait for all five Round 1 messages before continuing (use the wait-for-events / inbox tools rather than polling).

### Round 2 — Cross-pollination (aware of the others)

Compile the five Round 1 takes into a short digest and send it to each panelist (they remain addressable and resume with full context). Ask each to respond with:

```
Where I now agree / what I've updated (and why): ...
Where I still disagree (and why I'm holding): ...
New risks or opportunities the others surfaced: ...
Updated confidence: low / medium / high
```

**Do not write the Round 2 responses yourself.** Ventriloquizing the panel from the moderator's context reintroduces exactly the single-context anchoring Round 1 was built to avoid — the personas would converge on your house view and the whole exercise becomes theater.

This is the step that makes the panel more than five monologues: each voice now reasons in light of the others' findings, which is exactly where genuine consensus — and genuine, well-understood disagreement — emerges. Wait for all Round 2 responses.

### Synthesis — your job as moderator

Combine everything into the report below. The moderator's instinct is to resolve everything into a tidy, reassuring middle — resist it. Specifically:

- **Preserve dissent.** If any Round-2 disagreement survived, the "Where the panel still disagrees" section must NOT be empty. Name the live tension and give the best argument on each side. Never smooth genuine disagreement into a single verdict just to look conclusive.
- **One-sided verdicts are allowed.** If the panel is uniformly negative (or positive), say so plainly — "drop it" or "proceed" is a valid moderator read. Do not manufacture artificial balance.
- **Flag correlated-error risk.** If all five converged, note that shared-base-model agreement is weaker evidence than it appears, rather than presenting it as strong triangulated consensus.
- If the panel is genuinely split or uncertain, say so — a clear "it depends, and here's on what" is more honest and more useful than a forced verdict.

## Output format

Deliver the final synthesis in this structure:

```
# Focus Group: <idea in one line>

## Verdicts at a glance
- Champion — <final lean> (confidence: <>)
- Contrarian — <final lean> (confidence: <>)
- Pragmatist — <final lean> (confidence: <>)
- Strategist — <final lean> (confidence: <>)
- User Advocate — <final lean> (confidence: <>)

## Where the panel converged
<points of genuine agreement after Round 2; flag correlated-error risk if all five agree>

## Where the panel still disagrees
<each live tension, with the best argument on each side>

## Biggest risks surfaced
## Biggest opportunities surfaced
## Questions the idea still needs to answer

## Moderator's balanced read
<an honest bottom line: proceed / proceed with conditions / reshape / drop — with the reasoning. Reflect the real balance of the panel, including remaining uncertainty. Do not tilt it to flatter the idea.>
```

## If orchestration is unavailable

If you cannot launch child agents (e.g. the user declines orchestration), do not abandon the exercise — run the same two-round structure yourself in a single response: research each angle, voice each persona independently for Round 1, then have each react to the others for Round 2, then synthesize. The discipline that fights sycophancy is the structure, the research, and the ground rules, not the agent count. Keep the personas genuinely distinct and resist the pull to make them all quietly agree. Say plainly in the output that the panel ran single-context, so the independence is weaker than a real blind run.

## Quality bar

- Distinct voices, not five paraphrases.
- Specific evidence over generic claims.
- No flattery padding.
- No forced consensus.

## Examples

- `/focus-group should we replace our REST API with GraphQL for the mobile app?`
- `/focus-group here's my pitch: a subscription box for houseplants. tear it apart.`
- "Give me a focus group on whether I should leave my job to go full-time on my side project."
- "Poke holes in this: we drop the free tier and go sales-led only."
