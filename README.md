# The Daily Reflection Tree
**DT Fellowship Assignment — Role Simulation**

A deterministic end-of-day reflection tool built as a decision tree.
No LLM at runtime. Every conversation path is pre-encoded. Same answers → same reflection, every time.

---

## Repository Structure

```
/tree/
  reflection-tree.json     ← Part A: the full tree data (47 nodes)
  tree-diagram.md          ← Part A: visual diagram (Mermaid)
/agent/
  agent.py                 ← Part B: Python CLI that walks the tree
/transcripts/
  persona-1-transcript.md  ← Victim / Entitled / Self-Centric path
  persona-2-transcript.md  ← Victor / Contributing / Altrocentric path
write-up.md                ← Design rationale (< 2 pages)
README.md                  ← This file
```

---

## What It Does

The tool walks an employee through a structured reflection conversation each evening, covering three psychological axes in sequence:

| Axis | Spectrum | Psychology |
|------|----------|------------|
| **Locus** | External (Victim) ↔ Internal (Victor) | Rotter (1966), Dweck (2006) |
| **Orientation** | Entitlement ↔ Contribution | Campbell et al. (2004), Organ (1988) |
| **Radius** | Self-Centric ↔ Altrocentric | Maslow (1969), Batson (2011) |

Each axis contains 2 question nodes, 2+ decision nodes, and 2+ reflection nodes. The tree ends with a personalised summary drawn from 8 possible closing templates.

---

## Part A: Reading the Tree

Open `tree/reflection-tree.json`. Each node has:

| Field | Purpose |
|-------|---------|
| `id` | Unique node identifier |
| `parentId` | Tree parent (used for child-following logic) |
| `type` | `start` / `question` / `decision` / `reflection` / `bridge` / `summary` / `end` |
| `text` | What the employee sees. `{A1_OPEN.answer}` is interpolated with prior answers. |
| `options` | For `question` nodes: array of choosable strings. For `decision` nodes: routing rule string. |
| `target` | Explicit next-node override (used by `reflection` and `bridge` nodes). |
| `signal` | What this node records in state, e.g. `axis1:internal`. |

### Decision Routing Format

Decision nodes use a pipe/colon/semicolon mini-language:
```
answer=Option A|Option B:TARGET_NODE_1;answer=Option C|Option D:TARGET_NODE_2
```
The engine checks the most recent answer against each rule left-to-right and routes to the first match.

### Summary Interpolation

The summary node uses:
- `{A1_OPEN.answer}` — replaced with the employee's actual answer at that node
- `{axis1.dominant}` — replaced with the dominant pole (`internal` or `external`)
- `{axis1.summary}` — replaced from `summary_templates.axis1`
- `{closing_reflection}` — replaced from `summary_templates.closing` using the key `axis1_dominant+axis2_dominant+axis3_dominant`

---

## Part B: Running the Agent

### Requirements
- Python 3.7+
- No external dependencies

### Run

```bash
# From the repo root:
python agent/agent.py
```

The agent automatically locates `tree/reflection-tree.json` relative to the script. If it can't find it, it will tell you the expected path.

### What the agent does

1. Loads the JSON tree into a node index
2. Starts at the `START` node
3. For each node, renders text (with interpolation) and handles input based on type:
   - **start / bridge** — prints text, auto-advances
   - **question** — prints options, waits for numeric input, records answer + signal
   - **decision** — parses routing rules, routes silently (invisible to user)
   - **reflection** — prints reframe text, waits for Enter
   - **summary** — prints personalised closing, waits for Enter
   - **end** — prints goodbye, exits
4. All branching is lookup-based. Zero LLM calls.

---

## Tree Statistics

| Metric | Value |
|--------|-------|
| Total nodes | 47 |
| Question nodes | 12 |
| Decision nodes | 12 |
| Reflection nodes | 17 |
| Bridge nodes | 3 |
| Summary nodes | 1 |
| End nodes | 1 |
| Axes covered | 3 (Locus, Orientation, Radius) |
| Possible summary outcomes | 8 |
| Approximate unique full paths | ~32 |

---

## Sample Transcripts

See `/transcripts/` for two full walkthroughs:

- **persona-1** — Ananya: external locus, entitlement orientation, self-centric radius. Arrives defensive after a frustrating day.
- **persona-2** — Rohan: internal locus, contribution orientation, altrocentric radius. Arrives tired but grounded after a mixed day.

Both include the full console output and a node-by-node path trace.

---

## Design Philosophy

> The questions are the product. A technically perfect tree with shallow questions loses to a rough tree with questions that make someone pause.

The primary design goal was writing options that are all *honest* to pick — no moral traps, no obvious flattering choices. The reflections never grade the employee. Someone on the external/entitlement/self end of the spectrum leaves the session with awareness, not shame.

See `write-up.md` for full design rationale, psychological sources, and what would be improved with more time.

---

## Guardrails Against AI Hallucination

Per the assignment brief, this tool was *built with* AI assistance but does not *call* any AI at runtime.

During the design process, the following guardrails were applied:
- All psychological frameworks were traced to primary sources (Rotter 1966, Dweck 2006, Campbell et al. 2004, Organ 1988, Maslow 1969, Batson 2011) and not accepted from AI summaries alone
- Every question was reviewed for whether it could produce false positives (options that feel shameful and get avoided, producing an inaccurate signal)
- The routing logic is fully auditable from the JSON — any path can be traced without running code
- Reflection text was reviewed to ensure it reframes without moralizing — the tone is a "wise colleague," not a therapist or a coach
