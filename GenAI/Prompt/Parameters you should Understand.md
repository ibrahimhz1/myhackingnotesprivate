---
title: Parameters you should Understand
date: 2026-06-27
tags:
  - 
publish: false
draft: false
description: ""
---
---

Parameters you should understand: 

1. Temperature 
	>Low (0-0.3) - deterministic, repeatable, extraction, classification, compliance 
	>Higher (0.7+): varied, creative - brainstorming, copy.

2. Max Tokens
	>Caps response length. Set it generously enough to finish. but use it to prevent runaway output and control on high-volume jobs

3. Stop Sequences
	>Tell the model where to halt (Eg: `</json>`) useful when you generate structured blocks and want a clean cut-off



---

### Evaluate prompts like software

#### Build a small test set
- 10-30 real, varied inputs - include the hard and weird ones
- for each, note the ideal output or acceptance criteria 
- re-run the set after every prompt change
-
#### Score against a rubric
- Correctness, format, compliance, completeness, tone, safety 
- Pass/fail per criterion beats a vague "feels better"


A/B your prompts 
Run two prompt versions on the same inputs and compare scores. Keep the winner, log what changed and why 

Let a model help grade
for subject criteria, a separate "judge" prompt with a clear rubric can score outputs at scale - spot-check it against human judgement 

### Common pitfalls and anti-patterns 
1. Burying the instruction : the actual tasks is hidden in paragraph four. Lead with it, repeat critical rules at the end of long prompts 
2. Negative -only guidance : "Don't be formal" without saying what to be. State the target, not just the anti-target. 
3. No examples for nuanced tasks : If the standard is hard to be described show it 
4. conflicting instructions: Be thorough but use one sentence. "Thhe Model "
