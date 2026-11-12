# Exploring the application efficiently

Goal: build an accurate page and element inventory with the fewest browser calls.

## Method
1. Start at the feature's entry page. Take ONE structured page snapshot (accessibility
   tree), not a screenshot. Use screenshots only to judge layout or visual defects.
2. From the snapshot record, per page: URL or route, page title, purpose, every input
   (label, type, required marker, placeholder, default, max length if exposed), every
   button and link with its destination, tables (columns, sort, filter, pagination),
   messages shown.
3. Follow each navigation path of the feature once. Do not re-snapshot a page already
   recorded unless its state changed (after submit, after an error, another role).
4. For each form, make at most three passes: valid submit, empty submit (to reveal
   required-field messages), one invalid value per validated field. Record exact
   message text.
5. Note every state: empty list, one item, many items, loading, error, no permission.
6. Stop when the inventory covers the scope. Exploration is not test execution.

## What to record in 01-test-basis.md
```
## Page: <name>  (route: /path)
Purpose:
Roles that can open it:
Inputs:
| Label | Type | Required | Rules seen | Source |
Actions:
| Control | Result | Needs confirmation? |
Messages seen: "<exact text>" (when)
States: empty / populated / error
```

## Limits
- Stay inside the feature's scope and the test environment.
- Do not click destructive or outbound actions (delete, pay, send, publish) without asking.
- If login expires, a CAPTCHA appears, or the same step fails twice, stop and tell the user.
- Everything learned here is `OBS` until a requirement confirms it.
