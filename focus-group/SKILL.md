---
name: focus-group
description: Use this skill when the user wants a multi-perspective critique of an idea, not a single recommendation. Trigger for requests like focus group, panel, devil’s advocate, red-team, stress test, poke holes, pressure-test, gut-check, “argue with me,” or “give me different perspectives” on a product, strategy, architecture, roadmap, pricing, or life/business decision. Prefer this skill when the user explicitly worries about sycophancy, confirmation bias, or “people telling me what they want to hear.” Do not trigger for simple one-voice pros/cons, generic summaries, pure editing tasks, customer-research study logistics, or supportive roleplay.
---

# Focus Group
Convene a panel of five distinct personas to evaluate an idea, let them react to each other, then synthesize an honest, well-rounded verdict.

## Why this exists
A single assistant can drift toward agreement and validation, especially when the user signals a preferred answer. This skill counters that with structure:
- Assigned stances create real tension.
- Blind-parallel Round 1 reduces anchoring. Note the honest limit: the panelists share one base model, so these are **decorrelated stances, not decorrelated minds**. The structure reduces anchoring; it cannot manufacture priors the model lacks, and five voices can still be wrong in the same direction. Treat convergence as a weaker signal than the polish suggests.
- Evidence requirements reduce vibe-only takes.
- Skew disclosure plus confidence calibration improves honesty.
- Explicit updating prevents stubbornness without forcing fake consensus.

The output should map real convergence and real disagreement, not manufacture harmony.

## When to use
Use this for any idea or decision the user wants tested rather than validated: product features, architecture choices, pricing, GTM plans, strategy, career moves, or life/business tradeoffs.

## Input: the idea
The idea to evaluate is `$ARGUMENTS`.
If `$ARGUMENTS` is empty or vague, ask for the specific idea and key context before running the panel.

If the user requests custom personas or a smaller panel, honor that; otherwise use the default five personas below.

## The panel (default cast of five)
1. **Champion** — lens: upside, leverage, best plausible outcome.  
   Style: energetic but substantive.  
   Skew: over-optimism.
2. **Contrarian** — lens: challenge the premise itself, invert the framing, argue the opposite case ("what if the problem isn't real, or you're solving the wrong one?").  
   Style: provocative and first-principles.  
   Skew: disagreeableness / contrarianism for its own sake.
3. **Pragmatist** — lens: feasibility, effort, dependencies, execution reality.  
   Style: concrete and checklist-minded.  
   Skew: overweights near-term practicality.
4. **Strategist** — lens: long-term positioning, second-order effects, optionality.  
   Style: big-picture and scenario-driven.  
   Skew: abstraction bias.
5. **User Advocate** — lens: user value, adoption friction, behavior vs intention.  
   Style: empathetic and concrete.  
   Skew: overweights user comfort.

## Ground rules for every panelist
- Serve the idea, not the proposer’s ego.
- Do your homework before taking a stance.
- Reasons over vibes: include mechanisms, examples, or evidence.
- Own your skew.
- Calibrate confidence (low / medium / high + why).
- Update when another argument is stronger.
- Do not collapse disagreement just to be agreeable.

## How to run it in Claude Code
Round 1 must be **genuinely independent** — one model writing five personas in the same context has already anchored on persona 1 by the time it writes persona 2. So run the rounds differently:

- **Round 1: parallel subagents.** Spawn five subagents in a single message (one per persona) so they run concurrently and blind to each other. Give each subagent only its own persona brief (lens, style, skew), the idea, and the shared context — never another persona's output. Each returns its Round-1 block verbatim.
- **Round 2 and synthesis: single-agent.** Once all five Round-1 blocks are back, do the cross-pollination and moderator synthesis yourself in the main context, where shared visibility is exactly what you want.

### Round 1 — Independent takes (parallel subagents)
Each persona subagent:
1. Researches its lens **only if the idea has external facts to check** (e.g. a tech/market claim). For self-contained decisions (a career move, an internal call), skip research and reason from the given context — do not manufacture authoritative-sounding "research."
2. Keeps research proportionate; avoid rabbit holes.
3. Produces an independent take without any knowledge of the other personas.
4. Keeps each field to a few tight sentences — no padding.

Round-1 output for each persona:

```text
Position: support / oppose / mixed — one-line verdict
What I checked / found: sources, evidence, and key takeaways
Strongest points (from my lens): ...
Key concerns / risks: ...
Confidence: low / medium / high — because ...
My skew here: ... (and where it might be distorting me)
Questions I'd want answered: ...
```

### Round 2 — Cross-pollination (single-agent)
After all five Round-1 subagent takes are back:
1. Create a short digest of the five takes.
2. For each persona, produce an updated reaction in light of the others:

```text
Where I now agree / what I've updated (and why): ...
Where I still disagree (and why I'm holding): ...
New risks or opportunities the others surfaced: ...
Updated confidence: low / medium / high
```

### Synthesis (moderator)
Combine Round 1 + Round 2 into the final structured recommendation. The moderator's instinct is to resolve everything into a tidy, reassuring middle — resist it. Specifically:
- If any Round-2 dissent survived, the **"Where the panel still disagrees"** section must NOT be empty. Name the live tension and give the best argument on each side. Never smooth genuine disagreement into a single verdict just to look conclusive.
- A one-sided verdict is allowed. If the panel is uniformly negative (or positive), say so plainly — "drop it" or "proceed" is a valid moderator read. Do not manufacture artificial balance.
- Flag correlated-error risk: if all five converged, note that shared-base-model agreement is weaker evidence than it appears, rather than presenting it as strong triangulated consensus.

## Output format
Use this exact structure:

```text
# Focus Group: <idea in one line>

## Verdicts at a glance
- Champion — <final lean> (confidence: <>)
- Contrarian — <final lean> (confidence: <>)
- Pragmatist — <final lean> (confidence: <>)
- Strategist — <final lean> (confidence: <>)
- User Advocate — <final lean> (confidence: <>)

## Where the panel converged
<points of genuine agreement after Round 2>

## Where the panel still disagrees
<live tensions, with best argument on each side>

## Biggest risks surfaced
## Biggest opportunities surfaced
## Questions the idea still needs to answer

## Moderator's balanced read
<honest bottom line: proceed / proceed with conditions / reshape / drop, with reasoning and uncertainty explicitly acknowledged>
```

## Quality bar
- Distinct voices, not five paraphrases.
- Specific evidence over generic claims.
- No flattery padding.
- No forced consensus.

## Examples
- `/focus-group should we replace our REST API with GraphQL for the mobile app?`
- `/focus-group should I leave my job to go full-time on my side project?`
- `/focus-group poke holes in this: we drop the free tier and go sales-led only.`
