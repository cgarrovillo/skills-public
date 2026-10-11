---
name: prosncons
description: Scan the relevant chat and directly referenced documents to compare the pros, cons, risks, unknowns, and tradeoffs of a decision in a mandatory comparison table. Use when the user invokes /prosncons or $prosncons, weighs alternatives, compares approaches, evaluates whether to proceed, or requests a recommendation whose reasoning must be explicitly explained.
---

# Pros and Cons

Turn the available context into transparent decision support. Always show the comparison in a table. Recommend an option only when justified, and always explain why it is recommended.

## Gather the decision context

1. Read the full relevant conversation, not only the latest message. Treat later corrections and narrowed requirements as authoritative.
2. Identify every document, attachment, local file, URL, or other artifact directly referenced in the relevant chat. Open and inspect each accessible source before evaluating the options.
3. State any source that could not be accessed. Do not imply that unread material was reviewed.
4. State the decision in one sentence. Identify the credible options, including the status quo when it is a real alternative.
5. Extract the user's goals, constraints, evaluation criteria, time horizon, risk tolerance, and deal-breakers. Label assumptions that could affect the result.

## Evaluate the options

1. Evaluate every option against the same relevant criteria. Consider benefits, costs, effort, time, risk, reversibility, opportunity cost, maintenance burden, and second-order effects where applicable.
2. Distinguish verified facts from estimates, predictions, and inferences. Verify unstable or high-stakes claims when tools and policy permit.
3. Weight considerations by their importance to the user's goals, their likelihood, and their reversibility. Do not choose an option by counting bullets.
4. Surface dependencies, failure modes, unknowns, and the evidence most likely to change the result.
5. Avoid false balance. Explain when one criterion or deal-breaker materially outweighs several minor benefits.

## Always provide a comparison table

Include a Markdown table even for a two-option decision. Use one row per option by default:

```markdown
| Option | Pros | Cons | Risks / unknowns | Best fit when |
|---|---|---|---|---|
| Option A | ... | ... | ... | ... |
| Option B | ... | ... | ... | ... |
```

Adapt the columns when the decision requires specific criteria such as cost, delivery time, security, or reversibility, but always retain explicit pros and cons. Keep entries concrete: explain for whom, compared with what, and why each point matters.

## Explain every recommendation

Recommend an option only when the evidence and the user's priorities support it. When making a recommendation, include all of the following:

1. Name the recommended option directly.
2. Explain the decisive reasons, connecting each reason to the user's stated goals, constraints, or weighted criteria.
3. Explain the tradeoffs being accepted and why they are worth accepting.
4. Explain why the strongest alternative is less suitable in this context.
5. State the confidence level and the assumptions behind it.
6. State the strongest reason the recommendation could be wrong and what new evidence would change it.
7. Give the next action or a low-cost experiment that can reduce the remaining uncertainty.

Do not provide a bare verdict such as "Option A is best." The reasoning is part of the recommendation, not optional supporting detail.

When the evidence is insufficient, do not force a recommendation. Say why the choice cannot yet be justified, identify the smallest missing information needed, and still provide the comparison table.

Keep routine decisions concise. Expand the comparison and reasoning for costly, consequential, irreversible, or high-uncertainty decisions.
