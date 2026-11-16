# Copilot prompts for the QA + Avo Assure workflow
Copy one prompt at a time into Copilot Chat (agent mode, Playwright MCP on). Replace the parts in `<ANGLE BRACKETS>`. Run the stages in order; each stage ends with something you review before the next.

Rules that apply to every prompt (paste once at the start of a session):

```
Use the avo-assure and qa-web-testing skills. Before any click, drag or typing in Avo Assure, read
skills/avo-assure/references/verified-procedures.md and follow it exactly. Do not explore the UI.
I log in myself; never type credentials. Work only in project <PROJECT>. Do not delete or rename
anything you did not create unless I ask. After every save, check the toast and re-read the result.
If a step does not behave as documented, stop and tell me; do not try other routes.
```

---

## Stage 1. Understand the feature (no Avo yet)
```
Feature to test: <FEATURE NAME>. Sources: <USER STORY / REQUIREMENT TEXT OR LINK>, test environment
<APP URL> (not production). I will log in to the app now.
Explore only this feature with Playwright MCP, read-only: list the pages, fields, buttons, validations,
messages and business rules you see. Do not submit anything that changes data without asking me.
Output: a short test basis (what the feature does, rules, open questions for me).
```

## Stage 2. Manual test cases
```
From the approved test basis for <FEATURE NAME>, write manual test cases using the qa-web-testing
templates: positive, negative, boundary, empty/required, and role or permission cases where they apply.
Each case: ID, title, preconditions, steps, test data, expected result, priority.
Output as a table in <FILE NAME>.md. List anything you could not cover and why.
```

## Stage 3. Choose what to automate
```
Score every manual test case in <FILE NAME>.md with the automation scoring in qa-web-testing
(score out of 14: 10+ automate now, 7-9 later, 6 or less stay manual).
Output: a table with ID, score, decision, one-line reason. Wait for my approval of the list.
```

## Stage 4. Avo design (on paper, before touching Avo)
```
For the approved automation list, write the Avo Assure design: test case folder and names
(TC_<Area>_<NNN>_<Behavior>), step groups, which groups are reusable, the elements needed per page
(repository per page, names like txt_, btn_, lnk_, ddl_, chk_, rdo_, lbl_), the data table columns,
and every step as Element | Action | Input | Output using skills/avo-assure/references/action-syntax.md.
Mark which elements already exist in repository <REPO PATH> and which I must capture.
Output the design in <DESIGN FILE>.md and the list of elements I need to capture. Stop there.
```

## Stage 5. I capture elements (you do this in the Avo client)
No prompt. Capture the listed elements into the repository and save. Then run Stage 6.

## Stage 6. Rename captured elements
```
I am logged in to Avo Assure. Follow skills/avo-assure/references/rename-workflow.md.
Project <PROJECT>, repository <FOLDER> > <REPOSITORY>. Product page for matching: <APP PAGE URL>.
Read the element list, match each element to the page by its properties, and show me a table:
old name | type | new name | evidence. Wait for my approval, then rename one at a time, Save every
5 renames, and finish with a reload and a full re-read. Report any name that did not stick.
```

## Stage 7. Data table
```
Follow section 6e of verified-procedures.md. In project <PROJECT>, Manage Integrations >
Test Data Management, create folder <FOLDER> and data table DT_<AREA>_<PURPOSE> with columns
<COLUMN LIST> and these rows: <ROWS, or "from <DESIGN FILE>.md">. Use test data only, prefixed QA_
where the app stores it. Save, then re-read the table and show it to me.
```

## Stage 8. Reusable step groups
```
Follow section 6 of verified-procedures.md. In project <PROJECT>, build the reusable group(s)
from <DESIGN FILE>.md: <GROUP NAMES>, in Reusable Library folder <LIBRARY FOLDER>.
If a group with the same purpose already exists, tell me and reuse it instead of creating a new one.
Save, reload, re-read the steps and show them to me.
```

## Stage 9. Test cases and steps
```
Follow the build order in section 6g of verified-procedures.md. In project <PROJECT>, folder <FOLDER>,
create test case(s) <TEST CASE IDS> from <DESIGN FILE>.md: add reused groups first, then the own
step group, then the steps (element from repository, then Action, then Input). For data-driven cases
link the data table on the Get Param row and use |Column| inputs between Start Loop and End Loop.
Add step details (description, expected, pass, fail) for the verification steps.
Save, reopen each test case by its link, re-read all steps, and give me a table per test case
plus anything that differs from the design.
```

## Stage 10. E2E flow (only when test cases must run as one chain)
```
Follow section 6f of verified-procedures.md. In project <PROJECT>, E2E Flows folder <FOLDER>,
create flow E2E_<NAME> with these test cases in this order: <LIST>. Save, reopen, confirm the order.
```

## Stage 11. I run Debug (you do this in the Avo client)
No prompt. Run Debug on each test case and note failures. Then run Stage 12.

## Stage 12. Fix failures
```
Debug result for <TEST CASE>: step <N> failed with "<ERROR TEXT>". Screenshot/log: <PASTE OR ATTACH>.
Find the likely cause (element identifier, wrong action, timing, data) using the failure table in
verified-procedures.md and action-syntax.md. Propose the smallest fix and wait for my OK before
changing anything in Avo. After the change, save and re-read the step.
```

## Stage 13. Report
```
Summarise the work on <FEATURE NAME>: manual cases written, cases automated (with Avo paths),
cases left manual and why, data tables and reusable groups created, open issues, and what was not
verified. Keep it to one page for my team lead.
```

---

## Maintenance prompts
**App changed, element broke**
```
Element <ELEMENT NAME> in <REPOSITORY> no longer matches after an app change. Open <APP PAGE URL>,
find the element, and propose new identifier values. Wait for my OK, then update the properties
per section 3 of verified-procedures.md, Save Properties, then repository Save. Do not edit steps.
```
**Add one case to an existing data-driven test**
```
Add this row to data table <DATA TABLE PATH>: <VALUES BY COLUMN>. Save and re-read the table.
```
**Something in Avo behaves differently from the doc**
```
Stop. Describe exactly what you clicked, what the doc said would happen, and what happened instead.
Append it to skills/avo-assure/references/site-notes.md under today's date so the team sees it.
```
