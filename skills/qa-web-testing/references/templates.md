# Templates

## Manual test case
```
ID:            TC_<Module>_<NNN>
Title:         Verify that <actor> can/cannot <action> when <condition>
Requirement:   <story or rule ID>          Source: REQ | STD | OBS
Type:          Smoke | Positive | Negative | Boundary | Permission | UI
Priority:      High | Medium | Low
Technique:     EP | BVA | Decision table | State transition | Use case | Error guessing
Preconditions: <state, role, existing data>
Test data:     <concrete values>
Steps:
  1. <one action>
  2. <one action>
Expected result:
  1. <observable result for step 1, if any>
  2. <observable result>
Postconditions / cleanup: <data to remove>
Automation:    <filled in phase 4: Automate now | Later | Manual, with reason>
```
Writing rules: steps start with a verb; name controls by their on-screen label in quotes;
expected results are observable facts (text shown, record created, URL), never "works
correctly"; no step depends on another test case having run.

For many cases, use one table instead:
| ID | Title | Preconditions | Test data | Steps | Expected result | Type | Priority | Source |

## Test condition (phase 2)
| # | Condition | Technique | Risk | Source |
|---|---|---|---|---|
| 1 | Quantity accepts 1 to 99 | BVA | High | REQ AC-3 |

## Traceability
| Requirement / rule | Test case IDs | Gap? |
|---|---|---|

## Automation candidates (phase 4)
| ID | Title | Score /14 | Decision | Reason | Blockers |
|---|---|---|---|---|---|

## Bug report
```
Title:        <what is wrong, where> (not "X does not work")
Environment:  <URL, build if known, browser, role>
Severity:     Critical | Major | Minor | Trivial      Priority: High | Medium | Low
Preconditions:
Steps to reproduce:
  1.
  2.
Test data:
Expected result:   <with source: REQ / STD>
Actual result:     <exact message text or behaviour>
Reproducibility:   Always | Sometimes (n of m tries)
Evidence:          <screenshot, console error, request that failed>
Related test case: <ID>
```
Severity guide: Critical = data loss, security hole, core flow blocked, no workaround.
Major = important function wrong, workaround exists. Minor = small function or validation
issue. Trivial = cosmetic.

## Open questions
| # | Question | Why it matters | Assumption used meanwhile |
|---|---|---|---|
