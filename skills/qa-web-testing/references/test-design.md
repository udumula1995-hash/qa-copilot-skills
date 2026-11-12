# Test design techniques

Apply to every input field, rule and workflow. Name the technique in each test condition.

## Equivalence partitioning (EP)
Split each input into groups the system treats the same; test one value per group.
Always include: a valid group, each distinct invalid group, empty, and wrong type.
Example, Age accepts 18 to 65: valid 30; invalid below 10; invalid above 80; empty; "abc".

## Boundary value analysis (BVA)
Test at each edge and just outside it. For a range min..max: min-1, min, min+1, max-1,
max, max+1. For length limits: max-1, max, max+1 characters, and zero.
Dates: first and last valid day, the day before and after, month end, leap day, time zone edge.

## Decision table
Use when several conditions combine into different outcomes (discount rules, eligibility,
role + status). List conditions as columns, write one row per meaningful combination,
state the expected outcome for each. Collapse rows where a condition does not matter.

## State transition
Use for anything with a status (order: Draft > Submitted > Approved > Shipped).
Test every valid transition once, and every invalid transition that a user could attempt
(approve a cancelled order, edit after submit, back button after submit, double submit).

## Pairwise
Use when many independent options multiply (browser x role x language x plan).
Cover every pair of values at least once instead of every combination.

## Use case / end-to-end flow
One main success path and each alternate and exception path from the story.

## Error guessing (experience-based)
Empty and whitespace-only; leading/trailing spaces; very long input; special characters
(' " < > & % \ /); Unicode and emoji; numbers as text, negative, zero, decimals, leading
zeros; copy-paste with line breaks; double click on submit; refresh during submit; back
after submit; two tabs editing the same record; session timeout mid-form; slow network.

## Role and permission matrix
Rows = actions, columns = roles. Each cell allowed or denied. Test every denied cell that
matters by trying the action through the UI and by opening the URL directly.

## Prioritising
Priority = impact if it fails x likelihood of failing.
- High: core flow, money, data integrity, security and access, legal.
- Medium: common alternate paths, validation of important fields.
- Low: cosmetics, rare paths, convenience features.
