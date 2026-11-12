---
name: avo-assure
description: Write manual and automated web test cases and build them in Avo Assure Design Studio (folders, test cases, test step groups, test steps, element repositories, manual elements, mapping, renaming, data tables, debug). Use whenever the user mentions Avo Assure, a test step group, an element repository, or asks to write, add, rename, map or automate test cases for the web application under test.
---

# Avo Assure test authoring

Avo Assure is a no-code, keyword-driven test automation platform. This skill covers the
**folder-structure UI (v24.2 and later)** for **Web** applications. If the user's screen shows
Mindmaps (Test Suite > Test Case > Screen nodes), that is the older UI: say so and stop,
because the click paths below will not match.

Do not explore the UI to work out how things are done. Every click path you need is in
`references/ui-procedures.md`. Read only the section you need.

## Facts that save time (do not rediscover these)

1. **New test cases cannot be imported from a file.** Import Testcase accepts only JSON that
   Avo Assure itself exported, unmodified. Do not generate JSON, Excel or CSV for import.
   New test cases are created by clicking through Design Studio.
2. **Test step groups cannot be imported or exported.** To reuse one, use Copy/Paste or the
   Reusable Library.
3. **Capturing elements needs the Avo Assure Client** (desktop app) connected on the user's
   machine. You cannot capture from a browser session alone. If an element is not in the
   repository yet, create it with **Create Manually** (name + type) and tell the user to
   capture and map it later with **Tools > Map Element**.
4. **Mapping only works between the same element type** (button to button, textbox to textbox).
5. **An element or repository in use by any test case cannot be deleted.** Remove it from the
   test steps first.
6. **Renaming differs by object**: test case = `...` > Rename; test step group = single click
   on its name; element = double-click its name; repository or folder = `...` > Rename.
7. **Every If needs EndIf, every For needs End-For, every Start Loop needs End Loop.**
   A missing closer fails the run with a "dangling" error.
8. **Passwords never go in Set Text.** Use Set Secure Text with a value the user encrypted in
   Avo Assure. Never ask for, store or type a plain-text password.
9. **Debug and execution run through the Avo Assure Client**, so the user starts them, or
   confirms the Client is connected before you do.

## Object model

```
Project
└─ Design Studio
   ├─ Element Repository tab:  Folder > Repository (Web) > Elements
   ├─ Test Case tab:           Folder > Test case > Test Step Group > Test Step
   │                           Reusable Library > Folder > Reusable test step group
   ├─ Execution tab:           Execution lists
   └─ Reports tab
Data Tables (per project) feed Get Param / Start Loop / End Loop.
```

A **test step** has: **Element**, **Action** (the keyword), **Input Value**, **Output Value**,
and optional **Step Details** (Test Step description, Expected Result, Pass Result, Fail Result).

Elements are either captured/manual elements from a repository, or built-ins:
`@Browser` (browser actions), `@Generic` (variables, loops, conditions, strings, dates, files,
database), `@Custom` (act on an element by type + index without capturing it).

## Workflow

Follow these steps in order. Do not start clicking until step 4 is agreed.

1. **Collect inputs** (ask once, in one message, only for what is missing):
   requirement or user story, Avo Assure project name, target folder, repository to use,
   test data, and whether the user wants manual cases, automation, or both.
   Read `references/site-notes.md` first; it holds this team's conventions and anything
   learned in earlier sessions.
2. **Write the manual test cases** using the manual format below. Cover positive, negative,
   boundary and validation-message cases. One objective per test case.
3. **Convert each manual case to an Avo Assure design** using the automation format below.
   Group steps into test step groups by screen or business action (Login, Search, Checkout)
   so groups can be reused. Check `references/keywords-web.md` for action names and input
   syntax. Never invent an action name: if it is not listed there, write
   `<pick from Action list>` and choose it from the dropdown when entering the step.
4. **List the elements needed** and mark each one: exists in repository / create manually /
   user must capture. Show the user the design and the element list and get a go-ahead.
5. **Build it in Avo Assure** (only if you have a browser tool and the user is logged in):
   follow `references/ui-procedures.md`. Order: repository and elements first, then folder,
   test case, test step groups, steps, step details. Save after each test step group and
   confirm the save message before moving on.
   If you have no browser tool, output the design as a build sheet the user can enter by hand.
6. **Report**: what was created (with names), which elements still need capturing/mapping,
   and that the user should run **Debug** to verify.

## Manual test case format

| Field | Content |
|---|---|
| ID | `TC_<Module>_<NNN>` |
| Title | Verify that ... |
| Preconditions | State, user role, data needed |
| Test data | Concrete values |
| Steps | Numbered, one action per step |
| Expected result | One observable result per step or per case |
| Priority / Type | High, Medium, Low / Positive, Negative, Boundary |

## Automation design format

Test case: `<name>`  |  Folder: `<folder>`  |  Repository: `<repo>`

Test Step Group: `<name>` (Application Type: Web)

| # | Element | Action | Input Value | Output Value | Expected Result (Step Details) |
|---|---|---|---|---|---|
| 1 | @Browser | Open Browser | | {Status} | Browser opens |
| 2 | @Browser | Navigate to URL | https://app.example.com/login | | Login page loads |
| 3 | txt_Username | Set Text | qa.user01 | | Username is entered |
| 4 | txt_Password | Set Secure Text | `<encrypted value from user>` | | Password is entered |
| 5 | btn_Login | `<click action from Action list>` | | | Dashboard is shown |
| 6 | @Browser | VerifyCurrentURL | https://app.example.com/home | {UrlOk} | URL matches |

Rules for values:
- Separate multiple inputs with `;`. To skip an optional input, leave it empty but keep the `;`.
- Dynamic variables are written `{Name}`. Put one in Output Value to store a result; use it
  later in any Input Value.
- Output Value is usually `<result variable>;<status variable>`; status is True or False.

## Naming (defaults; `references/site-notes.md` overrides these)

- Folder: module or feature name. Test case: `TC_<Module>_<NNN>_<ShortTitle>`.
- Test step group: `<Screen>_<Action>`, for example `Login_ValidUser`.
- Repository: one per application area or page. Element: `<type prefix>_<LogicalName>`:
  `txt_` textbox, `btn_` button, `lnk_` link, `ddl_` dropdown, `chk_` checkbox,
  `rad_` radio, `tbl_` table, `img_` image, `lbl_` text or other element.

## Driving the UI with a browser tool

- The user logs in. Never enter credentials yourself.
- Prefer **Add Manually** (dropdown selection) over **Add from Repository** (drag and drop),
  because drag and drop is unreliable under automation.
- Work top-down and verify each object appears in the left pane before creating its children.
- Do not delete, overwrite or rename existing artifacts unless the user asked for that
  specific change. Never import into an existing repository: import replaces its elements.
- If a label in the UI differs from `references/ui-procedures.md`, use what is on screen,
  finish the task, and tell the user so they can record the difference in
  `references/site-notes.md` (or append it yourself if you can edit files).

## When something is not covered

Look it up once instead of experimenting: `references/docs-index.md` lists the official
documentation page for each task. Fetch only that page (add `.md` to the URL for plain text).
