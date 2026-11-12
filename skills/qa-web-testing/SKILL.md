---
name: qa-web-testing
description: End-to-end QA for a web application. Explore the app in a browser, design and write manual test cases, decide which cases are worth automating, and produce Avo Assure automation designs for the avo-assure skill to build. Use when asked to test a feature or page, write or review test cases, build a smoke or regression suite, decide what to automate, or write a bug report.
---

# QA for web applications

You act as a senior QA engineer. The work runs in six phases. Each phase writes one file, so
work is never repeated and the user can stop or redirect after any phase.

Output folder: `qa/<feature-name>/` in the workspace.

| Phase | Output file | Reference to read (only when you reach the phase) |
|---|---|---|
| 1. Test basis | `01-test-basis.md` | `references/exploration.md` |
| 2. Test conditions | `02-test-conditions.md` | `references/test-design.md`, `references/web-checklists.md` |
| 3. Manual test cases | `03-manual-test-cases.md` | `references/templates.md` |
| 4. Automation selection | `04-automation-candidates.md` | `references/automation-selection.md` |
| 5. Avo Assure designs | `05-avo-designs.md` | the `avo-assure` skill |
| 6. Build and report | summary in chat, `bugs.md` if any | the `avo-assure` skill, `references/templates.md` |

If the user asks for only one thing (for example "write test cases for this story"), run only
the phases needed and skip the rest.

## Rules that decide quality

1. **Observed behaviour is not expected behaviour.** What the app does today may be a bug.
   Every expected result carries a source: `REQ` (requirement, story, acceptance criteria),
   `STD` (a general convention, such as a mandatory field showing an error), or `OBS`
   (only observed in the app). `OBS` items are listed as questions for the user and are
   never presented as confirmed.
2. **Never invent requirements, field rules, limits or error messages.** If a rule is
   unknown, write the test with the value marked `<confirm>` and add it to Open questions.
3. **One objective per test case.** A case that checks two things hides which one failed.
4. **Concrete data, not descriptions.** Write `Qty = 0`, not "an invalid quantity".
5. **Cases must be independent.** Each one states its own preconditions and can run alone.
6. **Risk decides depth.** Spend most cases on what breaks the business if it fails (money,
   data loss, access control, core flow), least on cosmetics.
7. **No duplicate coverage.** Before writing a case, check that no existing case already
   covers the same condition with an equivalent value.

## Safety rules for testing in a browser

- Test only the environment named in the avo-assure skill's `references/site-notes.md` or
  given by the user. If the URL looks like production, stop and ask.
- The user logs in. Never type, store or echo credentials.
- Ask before any action that cannot be undone or that reaches outside the app: delete,
  payment, sending email or SMS, bulk update, changing another user's data, admin settings.
- Create test data with the prefix `QA_` so it can be found and cleaned up.
- Input-validation checks use harmless probe strings only (see web-checklists). Do not run
  attack tooling, guess passwords, or try to reach data you were not given access to.

## Phase 1: Test basis

Gather what the feature should do and what the app actually contains.
- Inputs, in order of authority: user story and acceptance criteria, design or spec, the
  user's explanation, the running app.
- If a browser tool is available and the user is logged in, explore the feature following
  `references/exploration.md`. If not, work from the documents and say that nothing was
  verified in the app.
- Write `01-test-basis.md`: scope (in and out), user roles, page and element inventory,
  business rules with their source, risks ranked High/Medium/Low, open questions.
- Show the open questions and risks to the user. Continue without waiting only if the user
  asked for an unattended run; mark every assumption you made.

## Phase 2: Test conditions

List what to test before writing any steps. One line per condition, tagged with technique,
risk and source. Apply the techniques in `references/test-design.md` to every input and
rule, then sweep `references/web-checklists.md` for the relevant page types.
Aim for full coverage of rules and risks with the fewest conditions.

## Phase 3: Manual test cases

Turn each condition into one test case using the template in `references/templates.md`.
- Order: smoke (core happy path) first, then functional positive, negative, boundary,
  then role/permission, then UI and usability.
- Add a traceability table: requirement or rule -> test case IDs. Any rule with no case
  is a gap to fix or to list as out of scope.
- Finish with a count by type and priority.

## Phase 4: Automation selection

Score every manual case with `references/automation-selection.md` and write
`04-automation-candidates.md` with three lists: Automate now, Automate later, Keep manual,
each with the reason. Check exclusions first; they override the score.

## Phase 5: Avo Assure designs

Load the `avo-assure` skill. For each "Automate now" case write a design in that skill's
automation design format (Element, Action, Input Value, Output Value, Expected Result).
- Split into reusable test step groups by screen or business action. Login, navigation and
  logout are written once and reused.
- Cases that differ only in data become one design plus a data table
  (Get Param, Start Loop, End Loop).
- Every design ends with at least one verification step; a design with no verify action
  proves nothing.
- List the elements needed, marked: exists / create manually / user must capture.
- Keep the manual case ID in the Avo test case name so the two stay traceable.

## Phase 6: Build and report

- With the user's go-ahead, build the designs in Avo Assure using the `avo-assure` skill.
  Without a browser tool, hand over `05-avo-designs.md` as the build sheet.
- If exploration or test design exposed a defect, write it in `bugs.md` using the bug
  template. Report only what you reproduced, with exact steps and data.
- Final summary: cases written, split by automation decision, what was built in Avo Assure,
  elements awaiting capture or mapping, open questions still unanswered.
