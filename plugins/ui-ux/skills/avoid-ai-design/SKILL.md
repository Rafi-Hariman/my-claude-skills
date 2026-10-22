---
name: avoid-ai-design
description: >-
  Audit existing frontend UI for generic, template-like, or context-insensitive design
  decisions, then optionally revise the smallest justified set of visual choices while
  preserving product behavior, accessibility, brand constraints, and the existing
  design system. Use when the user asks to de-slop a UI, make an AI-generated frontend
  feel less generic, audit visual design quality, or remove repetitive generated-design
  defaults. Supports audit-only and rewrite modes. Do not infer that a UI was created
  by AI from visual style alone.
version: 1.0.0
license: MIT
metadata:
  author: Rafi Hariman
  tags: design ui frontend visual-audit
---

# Avoid AI Design

Improve an existing interface by replacing unjustified defaults with decisions that
fit the product, audience, content, brand, and interaction model.

This is an editorial/refactoring skill for visual design. It is not an AI-authorship
detector, a novelty generator, or a catalog of forbidden colors, fonts, libraries, or
CSS classes.

A pattern is a finding only when it weakens the interface in context. A gradient,
Inter, shadcn/ui, cards, Lucide icons, a centered hero, glass effects, or a common
Tailwind utility can all be appropriate. Their presence does not prove low-quality or
AI-generated design.

## Modes

### Audit

Use when the user asks to inspect, scan, review, flag, or explain the design without
editing files.

Return evidence-based findings and leave code unchanged.

### Rewrite

Use when the user asks to improve, de-slop, redesign within the current product
constraints, or fix the identified visual problems.

Audit first, then edit only justified findings.

If the user's wording does not clearly authorize changes, default to audit rather than
assuming permission to rewrite the interface.

## 1. Establish design context

Read the relevant project files before judging the UI. Determine, when available:

- product purpose and primary user task;
- page/component role;
- target audience and usage context;
- existing brand guidelines or visual identity;
- design tokens, theme variables, typography, spacing, and component primitives;
- framework and styling stack;
- existing design-system/component-library constraints;
- responsive behavior and supported viewport range;
- accessibility requirements and interaction conventions;
- real content or representative product data.

Use repository evidence before inventing a new direction. Existing brand and design
system decisions outrank this skill's taste.

Do not assume that a default-looking primitive is a mistake when consistency with the
rest of the application is the intended requirement.

## 2. Inspect source and rendered output

Design quality cannot be established from class names alone.

Read the relevant source for:

- repeated layout primitives;
- token usage and arbitrary values;
- typography and hierarchy decisions;
- component-library defaults and overrides;
- interaction/focus/hover/disabled states;
- responsive rules;
- animation/motion behavior;
- semantics and accessibility implementation;
- copy that affects layout or hierarchy.

When a rendering or screenshot workflow is already available, inspect the interface at
representative viewport sizes and relevant states. Prefer project-native preview/test
tooling over adding a new renderer.

A useful visual inspection covers, when applicable:

- desktop and narrow/mobile layout;
- default state;
- keyboard focus;
- hover/pressed/selected state;
- loading, empty, validation, and error states;
- reduced-motion behavior when motion exists.

Do not install screenshot/browser dependencies solely for this audit unless the user
authorizes the dependency/tooling change.

When rendering is unavailable, distinguish:

- **Observed in code**: directly supported by source;
- **Visual inference**: likely visual effect that has not been rendered;
- **Unverified**: requires pixels or interaction to judge responsibly.

Never state an inferred visual defect as if it were observed.

## 3. Audit the interface by design function

Do not scan for a blacklist. Evaluate whether the interface makes coherent decisions.

### Product and content fit

Ask whether visual choices follow from the actual subject matter, product category,
audience, and content.

Flag generic treatment when the same composition could be moved to an unrelated
product with almost no change. Typical evidence includes placeholder hierarchy,
meaningless decorative labels, stock section choreography, or a visual identity that
has no relation to the product.

Do not demand novelty. Familiar patterns are valuable when familiarity improves task
completion or comprehension.

### Visual hierarchy

Check whether attention follows the intended order of importance through scale,
contrast, placement, grouping, spacing, and typography.

Look for concrete problems such as:

- every surface competing at the same visual weight;
- every heading treated as a hero;
- primary and secondary actions appearing equivalent;
- information split into cards without a content reason;
- oversized marketing treatment surrounding low-priority content;
- important data visually weaker than decorative chrome.

Prefer correcting hierarchy over adding decoration.

### Layout and composition

Inspect alignment, grouping, whitespace, density, section rhythm, and responsive
behavior.

A familiar container or grid is not a defect. Flag it when the composition ignores the
content or produces monotonous repeated modules with no hierarchy.

For applications and dashboards, prioritize scanning, comparison, density, and stable
interaction over novelty. For marketing/editorial surfaces, more expressive
composition may be justified when it reinforces the subject.

### Typography

Judge typography by role and execution, not font popularity.

Check:

- hierarchy and contrast between text roles;
- readable line length and line height;
- weight/style usage;
- content density;
- heading/body relationship;
- consistency with brand and language requirements;
- font loading/performance constraints where relevant.

Do not replace Inter, system fonts, Roboto, or another common family solely because it
is common. Replace a type choice when it is generic *for this context*, conflicts with
the brand, produces weak hierarchy, or fails the content requirements.

### Color and material

Evaluate palette hierarchy, contrast, semantic color use, surface relationships, and
brand fit.

Do not assign severity based on hue. Purple, blue, cream, black, acid green, gradients,
and translucent surfaces are legitimate design materials.

Flag them when they are repeated as unexplained styling reflexes or when they weaken
readability, hierarchy, consistency, or brand fit.

### Components and repeated primitives

Inspect whether repeated cards, pills, icon tiles, borders, shadows, radius values,
arrow max-width shells, or component-library defaults encode real hierarchy.

A repeated primitive is valid when it represents repeated information or interaction.
It becomes a design problem when all content is forced into the same primitive despite
different semantics.

Do not rewrite shadcn/ui, Material UI, Bootstrap, Chakra, Mantine, or another design
system merely to hide its origin. Preserve the library when it supports the product.
Customize only the layer necessary to achieve the intended identity and hierarchy.

### Interaction and feedback

Review hover, focus, active, selected, disabled, loading, success, error, disclosure,
and navigation states that matter to the audited surface.

Motion should communicate change, continuity, priority, or feedback. Decorative motion
may be appropriate but should not become the primary source of perceived quality.

Respect `prefers-reduced-motion` when non-essential motion exists. Reduced motion does
not require all useful state feedback to disappear.

### Accessibility

Accessibility is a quality floor, not an anti-slop aesthetic.

Preserve or improve:

- semantic controls;
- keyboard operation;
- visible focus;
- accessible names;
- state and relationship semantics;
- text/non-text contrast as applicable;
- focus visibility and non-obscuration;
- target size requirements where applicable;
- reduced-motion preferences.

Do not claim WCAG conformance solely from reading a color token or CSS class. Use
appropriate measurement/testing when conformance is part of the request.

### Copy and interface language

Review visible copy only when it contributes to generic design treatment or usability.
Prefer task language that tells the user what an action does.

Do not rewrite product claims, positioning, terminology, or brand voice merely to make
the page sound less AI-generated. Preserve meaning and factual claims.

## 4. Classify findings by confidence and impact

Do not use `P0/P1/P2` to mean "how AI-looking this feels." That framing confuses taste
with product risk.

For each finding, record:

### Confidence

- **Observed**: verified in rendered output or interaction;
- **Code-certain**: directly established by source;
- **Inferred**: plausible visual consequence, not rendered;
- **Unverified**: evidence is insufficient.

### Impact

- **Critical**: blocks or seriously harms use, accessibility, or comprehension;
- **High**: materially damages hierarchy, task clarity, responsive behavior, or brand
  consistency;
- **Medium**: repeated design choice weakens coherence or distinctiveness;
- **Low**: polish issue with limited user impact;
- **Preference**: subjective alternative without demonstrated product benefit.

Do not edit preference-only findings unless the user explicitly wants aesthetic
exploration.

## 5. Form a design hypothesis

In rewrite mode, summarize the few design decisions that should govern the correction.
Base them on evidence from the product rather than choosing from a list of aesthetic
personas.

A design hypothesis should usually answer:

- what should receive attention first;
- how dense or spacious the surface should be;
- what existing visual identity/tokens should dominate;
- what typography role needs correction;
- where expression is valuable and where restraint is better;
- one product-specific detail worth emphasizing, if the product provides one.

Example:

```text
Prioritize the comparison table over marketing chrome; keep the existing blue brand
palette; reduce card fragmentation; use the current type family with a stronger scale;
reserve motion for filter/result transitions; carry the product's map-grid motif into
section dividers.
```

Do not force a "signature detail" when the product does not need one.

If a full ground-up visual concept is required rather than refinement of an existing
interface, route or compose with the repository's dedicated frontend-design skill when
available. Avoid maintaining two competing comprehensive design systems.

## 6. Choose rewrite depth

### Surgical

Use for:

- components inside an established design system;
- settings/forms/admin surfaces;
- isolated hierarchy, typography, color, spacing, or state problems;
- interfaces whose identity is already coherent.

Preserve structure and tokens unless the finding directly implicates them.

### Structural

Use when repeated layout/component decisions prevent clear hierarchy but product
architecture and behavior remain valid.

Examples include excessive card fragmentation, inappropriate grid repetition, or a
page shell that obscures the primary task.

### Conceptual redesign

Use only when the user requested a redesign or the existing visual concept itself is
the problem. This may overlap a frontend-design skill and is a materially broader
change than de-slopping.

Do not silently escalate a surgical audit into a full redesign.

## 7. Implement with minimum disruption

When edits are authorized:

- preserve props, public component contracts, state, routing, data flow, forms, and
  user-visible behavior unless the user requested a behavior change;
- preserve framework and component-library architecture;
- reuse existing tokens before introducing arbitrary values;
- avoid unrelated code cleanup;
- avoid new dependencies when CSS/current primitives are sufficient;
- disclose any new font, icon, animation, or component dependency before introducing
  it when it changes project setup or network/runtime behavior;
- preserve accessibility semantics while changing visual structure;
- retain copy meaning and factual claims.

Do not "solve" generic design by randomly changing colors, fonts, radii, or spacing.
Each material edit should connect to a finding and the design hypothesis.

## 8. Validate the result

After editing, use project-native checks when available:

1. relevant unit/component tests;
2. type/lint/build checks affected by the edit;
3. render/screenshot at representative viewports;
4. keyboard interaction and focus state;
5. responsive overflow/layout;
6. reduced-motion behavior when motion changed;
7. accessibility tooling already present in the project.

Then re-audit the same findings.

Success means:

- the original material findings are resolved or consciously retained;
- hierarchy matches content importance;
- visual decisions are coherent with product/brand context;
- functionality and accessibility were preserved;
- the result does not merely swap one fashionable template for another;
- no unnecessary redesign or dependency was introduced.

A UI does not fail merely because another designer could make a different valid
choice.

## 9. Output

### Audit mode

Report findings ordered by product impact:

```text
Location: <file/component/region>
Finding: <specific design problem>
Evidence: <render/source observation>
Confidence: Observed | Code-certain | Inferred | Unverified
Impact: Critical | High | Medium | Low | Preference
Why it matters: <effect on hierarchy, task, brand, accessibility, or coherence>
Recommendation: <smallest justified correction>
```

Finish with an overall assessment:

- **Distinct and coherent**;
- **Sound but generic in identified areas**;
- **Materially template-driven**;
- **Insufficient evidence without render/context**.

Do not produce an "AI probability" or claim authorship.

### Rewrite mode

Return:

1. the material audit findings;
2. the design hypothesis;
3. files changed and why;
4. validation actually performed and outcomes;
5. unresolved or unverified visual/accessibility items.

Do not dump every catalog match when several instances are the same root problem.
Group repeated symptoms so the report remains actionable.

## 10. Boundaries

Do not:

- classify a design as AI-generated from visual style alone;
- ban specific colors, fonts, libraries, border radii, shadows, gradients, or layout
  primitives globally;
- treat novelty as a usability requirement;
- optimize against a static anti-pattern catalog at the expense of product fit;
- change a design system locally when the problem is system-wide without surfacing the
  scope mismatch;
- replace one generic trend with another to create superficial distinctiveness;
- add motion because a page feels plain;
- sacrifice familiar interaction patterns for visual novelty;
- claim accessibility conformance without appropriate verification;
- manufacture findings when the UI is already coherent and intentional.

A clean audit with no material changes is a valid outcome.

## 11. Completion check

Before finishing, verify that:

- the product/audience/task context was considered;
- existing brand and design-system constraints were respected;
- source-only findings are distinguished from rendered observations;
- every edit maps to an evidence-based finding;
- preference is separated from defect/impact;
- responsive, interaction, and accessibility implications were considered;
- no functionality or content meaning changed unintentionally;
- no dependency or broad redesign was introduced silently;
- the final assessment discusses design quality, not presumed AI authorship.
