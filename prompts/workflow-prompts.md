# Copilot prompts: 3-stage QA workflow (Jira > manual cases > discovery > Avo Assure)
Copy one prompt into Copilot Chat (agent mode). Replace the parts in `<ANGLE BRACKETS>`.
Each stage writes files that the next stage reads, so keep them in one folder: `qa/<TICKET>/`.

---

## Stage 1. Manual test cases from the Jira ticket (Zephyr import CSV)
Needs: Jira MCP. Run when the ticket is ready for test design.

```
Use the qa-web-testing skill and the Jira MCP.

1. Read Jira ticket SMRTBSKT-<NUMBER>: summary, description, acceptance criteria, test case
   description field, comments, linked tickets, and every attachment (open images, documents and
   mockups; tell me about any attachment you could not open).
2. List each acceptance criterion with an ID (AC1, AC2, ...) and any rule that is unclear or missing.
   Ask me those questions first and wait, unless there are none.
3. Write manual test cases covering every acceptance criterion: positive, negative, boundary,
   empty/required fields, and role or permission cases where they apply. One expected result per step.
4. Output format: open <PATH TO ZEPHYR TEMPLATE CSV> and read its header row. Write the test cases to
   qa/SMRTBSKT-<NUMBER>/manual-test-cases.csv using EXACTLY those column names, in the same order,
   with the same delimiter, quoting and encoding as the template. Do not add, rename or drop columns.
   If the template has example rows, copy their style (how steps are split into rows, how priority,
   labels, status and folder are written). Leave a column empty when you have no value; never invent
   values for IDs or users. Put the ticket key SMRTBSKT-<NUMBER> in the column the template uses for
   the linked issue or coverage.
5. Check the file: same column count on every row, commas and line breaks inside cells quoted, no
   blank rows. Then give me a traceability table: acceptance criterion > test case names, and list
   any criterion with no test case.

Do not open the application or Avo Assure in this stage.
```

If the template changes later:
```
The Zephyr template changed. Read the header row of <PATH TO NEW TEMPLATE CSV> and rewrite
qa/SMRTBSKT-<NUMBER>/manual-test-cases.csv to match it exactly (names, order, format). Show me which
columns were added, removed or renamed and how you mapped the old data. Do not change test content.
```

---

## Stage 2. Discover the flow in the application and design the automation
Needs: Playwright MCP, the build deployed to a test environment. Run when development is complete.

```
Use the qa-web-testing and avo-assure skills and the Playwright MCP.
Input: qa/SMRTBSKT-<NUMBER>/manual-test-cases.csv. Application: <TEST ENVIRONMENT URL> (never
production). I will log in myself; do not type credentials. Ask before any step that creates,
changes or deletes real data, and prefix any data you create with QA_.

1. Walk through each manual test case in the application, one at a time. For every step record:
   the page, the control used, what you entered, and what the application showed.
   If the application behaves differently from the expected result, do not adapt the test: log it
   as a possible defect or an outdated test case and carry on.
2. For every control you used, record the element details Avo needs: a name in our convention
   (txt_, btn_, lnk_, ddl_, chk_, rdo_, img_, tbl_, lst_, lbl_ + purpose), element type, page URL,
   id, name attribute, CSS selector, relative XPath, visible label. Prefer stable ids over
   position-based XPath. Group elements by page (one repository per page).
3. Score each test case for automation with the qa-web-testing scoring (out of 14: 10+ automate,
   7-9 later, 6 or less manual) and give a one-line reason.
4. For the cases to automate, design the Avo structure:
   - Reusable step groups: any sequence used by 2 or more test cases (login, navigation, common
     setup). Name them <Area>_<Action>.
   - Test cases TC_<Area>_<NNN>_<Behavior>, each as an ordered list of step groups.
   - Every step as: Element | Action | Input | Output, using the exact action names and input
     syntax in skills/avo-assure/references/action-syntax.md.
   - Data tables: where the same steps run with different data, one table DT_<Area>_<Purpose>
     with column names and rows; steps use |Column| inputs between Start Loop and End Loop.
5. Write these files in qa/SMRTBSKT-<NUMBER>/:
   - discovery-notes.md (flow per test case, differences from expected results, questions)
   - automation-selection.md (score table)
   - elements.csv (repository, element name, type, url, id, name, css, xpath, label)
   - avo-design.md (reusable groups, test cases, steps, data tables)
6. Stop and show me a summary: what will be automated, what is reusable, what stays manual, and
   every difference you found. Do not open Avo Assure in this stage.
```

---

## Stage 3. Build everything in Avo Assure
Needs: Playwright MCP, Avo Assure open. Run after I approve the Stage 2 design.

```
Use the avo-assure skill and the Playwright MCP. Before any click, drag or typing, read
skills/avo-assure/references/verified-procedures.md and follow it exactly; do not explore the UI.
I am logged in to Avo Assure at <AVO URL>. Project: <PROJECT>. Never type credentials. Do not delete
or rename anything you did not create unless it is in the approved rename list. After every save,
check the toast and re-read the result. If a step does not behave as documented, stop and tell me.

Inputs: qa/SMRTBSKT-<NUMBER>/avo-design.md and elements.csv.
Follow the build order in section 6g of verified-procedures.md:

1. Check what exists first: list the element repositories, reusable groups, data tables and test
   cases in the project that match the design. Reuse what exists; show me the list of what you
   will create and what you will rename, and wait for my OK.
2. Element repositories: folder <REPO FOLDER>, one repository per page.
   - Elements I already captured: rename them to the names in elements.csv following
     skills/avo-assure/references/rename-workflow.md (one at a time, Save every 5, verify).
   - Elements that are missing: create them with Create Manually using the type and identifiers
     in elements.csv, then Save.
3. Data tables: Manage Integrations > Test Data Management, folder <DATA FOLDER>; create each table
   with its columns and rows, Save, re-read.
4. Reusable step groups: build each one in Reusable Library folder <LIBRARY FOLDER>, Save, re-read.
5. Test cases: folder <TEST CASE FOLDER>; create each test case, add the reused groups first, then
   its own step groups and steps (element from repository, then Action, then Input). Link the data
   table on the Get Param row for data-driven cases. Add step details (description, expected, pass,
   fail) on verification steps. Save.
6. E2E flow only if the design has one.
7. Verify: reopen every test case, reusable group and data table by its link and compare with
   avo-design.md. Write qa/SMRTBSKT-<NUMBER>/avo-build-report.md: what was created (with Avo
   paths), what was reused, what was renamed (old > new), and anything that differs or failed.

Do not run Debug; I will do that in the Avo client and send you any failures.
```

After Debug, for each failure:
```
Debug result for <TEST CASE>: step <N> failed with "<ERROR TEXT>". <PASTE LOG OR SCREENSHOT>.
Find the likely cause using the failure table in verified-procedures.md and action-syntax.md
(identifier, wrong action, timing, data). Propose the smallest fix and wait for my OK before changing
anything in Avo. After the change, save and re-read the step, and update avo-design.md to match.
```
