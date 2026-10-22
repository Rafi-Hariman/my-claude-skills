---
name: humanizer
description: >-
  Edit prose to remove generic, formulaic, promotional, or chatbot-like wording while
  preserving meaning, evidence, register, and the writer's voice. Use when the user
  asks to humanize, de-AI, naturalize, tighten, or review prose for recurring LLM-style
  habits. This is an editorial skill, not an AI-authorship detector and not a method
  for evading detection systems.
license: MIT
metadata:
  version: "3.0.0"
---

# Humanizer

Edit the text for a human reader. Prefer precise meaning, natural rhythm, and the
writer's established voice over a checklist of words that supposedly sound human or
AI-generated.

The pattern catalog in Wikipedia's "Signs of AI writing" is useful as diagnostic
evidence, not as a style specification. A listed pattern is a reason to inspect a
passage, not a reason to rewrite it automatically. Do not infer authorship from prose
style.

## Editorial contract

Unless the user asks for substantive editing, preserve:

- factual claims and their degree of certainty;
- names, dates, numbers, measurements, rankings, quotations, citations, and links;
- technical meaning and domain terminology;
- the writer's position, including uncertainty or mixed judgment;
- document purpose, audience, and level of formality;
- intentional voice markers that are supported by a writing sample.

Never invent evidence, anecdotes, opinions, first-person experience, sources, or
personal details to make prose seem more human.

A humanizer improves expression. It does not manufacture humanity.

## Authority order

When style instructions conflict, use this order:

1. the user's explicit instructions for the current text;
2. a representative writing sample supplied by the user;
3. the conventions required by the document type or repository;
4. the source text's established register and terminology;
5. this skill's default editorial guidance.

Do not flatten a distinctive but intentional style merely because it resembles a
known LLM tendency.

## Modes

### Direct rewrite

Default when the user asks to humanize pasted prose. Return the revised text. Explain
changes only when the user requests an audit, rationale, or comparison.

### Review

Use when the user asks what sounds artificial, generic, promotional, or overwritten.
Identify the passages and explain the editorial problem without claiming they prove AI
authorship.

### File edit

When the user asks to edit a file, change only prose within the requested scope.
Preserve code, frontmatter, structured data, identifiers, link destinations, and other
machine-significant content unless the user explicitly asks to change them.

### Embedded edit

When another workflow uses this skill to edit a commit message, pull request,
document section, release note, or similar artifact, return only the text required by
that workflow.

## Workflow

### 1. Establish the text's job

Determine what the text is trying to do and who will read it. Editing a technical
runbook, an executive memo, a personal essay, a product page, and a support message
requires different choices.

Do not add personality where neutrality is part of the job.

### 2. Capture the voice before changing it

If a writing sample is available, inspect stable features rather than copying surface
quirks mechanically:

- sentence-length distribution and cadence;
- directness and level of formality;
- preferred vocabulary and contractions;
- paragraph density;
- use of first person;
- punctuation habits;
- tolerance for fragments, asides, repetition, humor, or technical shorthand.

Match recurring tendencies, not isolated accidents. A sample outranks generic advice
such as avoiding a particular punctuation mark.

If no sample exists, default to plain, direct prose appropriate for the document.

### 3. Find passages that need editorial work

Look for clusters of problems, not isolated tokens.

#### Generic abstraction

Revise wording that replaces a concrete fact with broad significance, symbolism, or
importance. Prefer what happened, what changed, who acted, and why it matters in the
specific context.

Common symptoms include inflated claims about a "pivotal" moment, "evolving
landscape", "enduring legacy", or broad implications that the evidence does not
support.

#### Promotional language

Remove sales language when the document is supposed to inform rather than persuade.
Words such as `stunning`, `renowned`, `groundbreaking`, `vibrant`, or `commitment to`
are not errors by themselves. Change them when they substitute praise for information.

#### Unsupported attribution

Do not preserve vague authority merely because it sounds cautious. `Experts say`,
`observers note`, and `industry reports show` require an identifiable source when the
claim depends on that authority. If the source text does not provide one, preserve the
underlying supported fact or flag the attribution instead of inventing a source.

#### Formulaic reasoning

Inspect repeated rhetorical templates such as:

- `not just X, but Y`;
- automatic groups of three;
- fake `from X to Y` ranges;
- stock challenge/future-outlook sections;
- staged objections no reader raised;
- rejected alternatives that add no information;
- repeated "deeper truth" framing;
- strings of dramatic fragments or punchlines.

Rewrite the argument directly. Keep a pattern when it performs a real rhetorical job.

#### Mannered prose

Prefer literal wording when metaphor, flourish, aphorism, or a clever turn of phrase
makes the reader work harder without adding meaning. Do not replace a precise statement
with a slogan.

This applies especially to phrases that turn ordinary claims into sayings, metaphors,
or miniature revelations.

#### Chatbot residue

Remove assistant-facing scaffolding from standalone prose when it is not part of the
document: greetings, praise, "here's what you need to know", "let's dive in", offers
to continue, knowledge-cutoff commentary, or generic closing optimism.

Keep real warnings, scope statements, legal notices, named objections, and useful
limitations.

#### Structural monotony

Fix structure when it harms reading:

- several sentences with identical openings;
- uniform mid-length sentence cadence;
- unnecessary heading restatement;
- a list where continuous prose is easier to follow;
- excessive inline mini-headings;
- repeated conclusion sentences saying the same thing.

Do not introduce random irregularity merely to look human. Rhythm should follow the
content.

#### Verb and clause inflation

Prefer a simple verb when a longer construction adds nothing: `is`, `has`, `uses`,
`shows`, `causes`, `requires`, `removes`.

Cut empty throat-clearing and stacked qualifiers. Preserve hedging when uncertainty is
substantive.

#### Formatting residue

Remove decorative bolding, emojis, or generated-chat formatting when it conflicts
with the target document. Preserve formatting that carries hierarchy or meaning.

Do not enforce straight quotes, sentence-case headings, or a ban on dashes unless the
user, writing sample, target style guide, or file format calls for it.

### 4. Rewrite by meaning unit

Do not perform blind word substitution. A weak paragraph usually needs a paragraph
rewrite, not a thesaurus pass.

For each passage:

1. identify its concrete claim or purpose;
2. remove unsupported interpretation and redundant framing;
3. choose the simplest wording that preserves the intended register;
4. preserve useful specificity;
5. vary sentence structure only where the content naturally benefits from it;
6. reconnect sentences so the paragraph reads as one thought rather than patched
   fragments.

Avoid replacing one stock phrase with another.

### 5. Run a fidelity check

Compare the revision with the source before returning it.

Check every material item:

- fact;
- name;
- number or measurement;
- date;
- quotation;
- citation and link target;
- attribution;
- ranking or comparison;
- modal strength (`is`, `may`, `likely`, `must`);
- negation;
- technical term;
- scope or exception.

If the rewrite strengthens, weakens, adds, or removes a claim unintentionally, correct
it.

### 6. Read for voice and rhythm

Read the revised passage as continuous prose. Remove any new repetition, clipped
cadence, over-edited stiffness, or conspicuous rhetorical pattern introduced by the
rewrite.

The final text should sound authored, not "humanized".

## Diagnostic reference

Use these as warning signs only when context supports the diagnosis:

- exaggerated significance without evidence;
- promotional adjectives in neutral prose;
- vague attribution;
- superficial `-ing` clauses that imply analysis without providing it;
- repeated abstract words such as `landscape`, `pivotal`, `showcase`, `underscore`,
  `tapestry`, or `interplay`;
- repeated avoidance of simple copular verbs;
- negative parallelism used as a default rhetorical frame;
- forced triplets;
- synonym cycling for the same subject;
- fake ranges;
- excessive passive voice when the actor matters;
- repeated stylistic dashes or parenthetical interruptions;
- decorative bolding, emoji, or excessive micro-headings;
- chatbot greetings, assurances, transition announcements, and continuation offers;
- stacked hedging;
- generic optimistic endings;
- faux-candid hooks;
- unsupported objections and fake alternatives;
- dramatic fragment sequences;
- aphorisms or metaphors used in place of literal explanation.

Never scan-replace these words or structures globally. Technical uses remain valid.
For example, `highlight`, `key`, `gate`, `real-time`, and `data-driven` can be the most
precise terms in context.

## False-positive discipline

Do not treat polished grammar, formal vocabulary, Markdown, curly quotes, em dashes,
passive voice, lists, or a single stock phrase as proof of AI writing.

Do not tell a user that a passage was generated by AI based on style alone. Human and
model writing overlap, and stylistic cues are probabilistic.

Do not deliberately insert typos, grammar errors, slang, random fragments, personal
anecdotes, or unusual punctuation to defeat an AI detector. Detector evasion is not a
quality criterion and can damage the text.

If the user's actual goal is detector evasion, redirect the task toward improving
clarity, fidelity, and authentic voice instead of optimizing for a detector score.

## Document-specific guidance

### Technical and professional prose

Prioritize accuracy, explicit actors, literal wording, stable terminology, and
verification details. Do not add casual personality to code documentation, incident
reports, security findings, contracts, or runbooks unless the existing style supports
it.

### Marketing and product copy

Persuasion is allowed when persuasion is the purpose. Remove generic hype, not all
sales language. Preserve claims that are supported by product evidence and retain the
brand's actual voice.

### Personal and opinion writing

Preserve first person, uncertainty, humor, irregular rhythm, and genuine asides when
they belong to the writer. Do not invent any of them.

### Academic and sourced writing

Preserve citation relationships and epistemic strength. Do not simplify specialist
terminology merely because it sounds formal. Never invent a source to repair vague
attribution.

### Wikipedia-style prose

When the target is Wikipedia, apply the project's own neutral-tone, sourcing, and
formatting conventions in addition to the diagnostic patterns. The "Signs of AI
writing" page is an advice page and field guide, not a universal prose style guide.

## Output

Return the final rewrite by default.

If the user explicitly asks for an audit or comparison, return:

1. the revised text;
2. a concise list of the material edits and why they were made;
3. any passage that could not be safely rewritten without missing information.

Do not automatically return a hidden draft, an AI-pattern score, or a checklist of all
possible tells. The user asked for better prose, not a performance about the editing
process.

## Final check

Before returning the result, verify that:

- the meaning and evidence are intact;
- no fact, opinion, source, anecdote, or personal experience was invented;
- the register fits the document and audience;
- the writer's sample was respected when supplied;
- generic or mannered wording was changed only where it improved the prose;
- repeated structure was fixed without manufacturing randomness;
- formatting still serves the target medium;
- no claim about AI authorship or detector success was made from stylistic evidence.

## Reference

Primary diagnostic reference: Wikipedia, "Signs of AI writing", maintained by
WikiProject AI Cleanup. Treat it as a descriptive field guide whose indicators require
context, not as a prescriptive checklist or authorship test.
