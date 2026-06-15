# Day 5 — Prompt Engineering & Roadmap Generation

## What I built
Two 30-day learning roadmaps using Claude, then compared them to understand
how context affects AI output quality.

## Output 1 — Generic roadmap (Python, beginner)
**Prompt context:** Topic, time available, experience level only  
**Result:** A solid but generic Python roadmap for anyone picking those options  
**Screenshot:** [add screenshot here]

## Output 2 — Personalized roadmap (SWE interview prep)
**Prompt context:** Current skills, goal, target role, learning style, time  
**Result:** DSA + system design roadmap tailored to my actual background  
**Screenshot:** [add screenshot here]

## Comparison findings

| Factor | Output 1 | Output 2 |
|---|---|---|
| Personalization | Low — built for any beginner | High — used my actual skill stack |
| Relevance | Generic Python basics | Interview-specific (LeetCode, system design) |
| Resources | General tutorials | NeetCode, Gaurav Sen, ByteByteGo |
| Projects | Learning exercises | Portfolio-ready, interview-aligned |
| Would I follow it? | No — too basic for my level | Yes — jumps straight to what matters |

## Key learnings

1. **Context is the prompt.** The structure of both prompts was identical.
   What changed was the specificity of the input — and the output quality
   jumped dramatically as a result.

2. **Vague goals produce vague plans.** "Prepare for a job" gave a
   generic roadmap. "Software Engineer targeting DSA + system design
   interviews" gave a specific, actionable one.

3. **AI skips what it knows you already know** — but only if you tell it.
   Output 2 skipped Python basics entirely because I listed my existing skills.
   Output 1 started from scratch.

4. **Structured prompts outperform conversational ones** for planning tasks.
   Including fields like Current Skills, Goal, Experience Level, and Learning
   Style acts like a form — it forces completeness and removes ambiguity.

## What I'll do differently
- Always include current skills, target outcome, and constraints in planning prompts
- Specify the *type* of job/goal, not just "get a job"
- Use the personalized roadmap (Output 2) as my actual 30-day study plan

## Resources from today
- NeetCode: https://neetcode.io
- Gaurav Sen (System Design): https://youtube.com/@gkcs
- ByteByteGo: https://youtube.com/@ByteByteGo
- Pramp (mock interviews): https://pramp.com
