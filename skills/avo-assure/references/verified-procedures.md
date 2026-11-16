# Avo Assure UI: verified procedures (read BEFORE any click, drag or typing)
Verified live on Avo Assure v26.10.0 (Ant Design UI, no data-testid). Follow these exact paths. Do not explore. If a step does not behave as written, stop and use the failure table, do not try new routes.

## 0. Ground rules
1. The user logs in. Never type credentials. Work only in the project the user names. Never delete or rename anything you did not create unless the user asked.
2. Deep links survive reload: `/landing/project/<P>`, `/repository/project/<P>[/element-repository/<id>]`, `/design/project/<P>[/testcase/<id>]`, `/design/project/<P>/reusable-library/<id>`, `/execute/project/<P>`, `/reports`, `/ai-analytics`. Prefer the deep link over clicking through menus.
3. After every save, check the toast text and re-read the list (see section 9). A change is not done until saved AND verified.
4. Prefer reading page text / accessibility tree to screenshots. Use one find/read per step, not repeated screenshots.
5. Menus and element refs go stale as soon as a menu closes. Re-find the element right before each click.
6. Ant selects: type to filter where noted, then CLICK the option. Enter does not select in the Actions dropdown or the Element Type dropdown.

## 1. Navigate
Landing `/landing` lists projects. Click project card -> `/landing/project/<P>` -> "Design Studio" button. Top tabs: Element Repository, Test Case, Execution, Reports, Optimizer. The "Project:" combobox top-left switches project.

## 2. Folders (same in all three trees)
- Empty tree: button "Create Folder (Shift+F)"; or the "+" next to the tree heading.
- An inline name box appears prefilled "New Folder": Ctrl+A, type the name, Enter.
- Row "..." menu appears only on the selected or hovered row (click the row first). Items: Create folder (Shift+F), Create repository/test case, Assign, Paste (Shift+V), Copy (Shift+C), Open in new tab, Copy link, Rename, Delete (Shift+D). Rename: Ctrl+A, type, Enter. Copy then Paste on the target folder duplicates.
- Create repository is a HOVER submenu: hover "Create repository" and click "Web" in the same action. Inline box default "New Repository".

## 3. Element repository
Create: folder -> Create Repository -> Web -> name -> Enter. Open repo (deep link has repo id).
Manual elements: "Create Manually" (or Add Elements > Create Manually) -> modal "Create Element". Required: Element Name, Element Type. Optional: URL, Name Attribute, Relative Xpath, Class Name, ID Attribute, CSS Selector, Absolute Xpath, Label, Href, custom1, custom2. Types: Link, Textbox/Textarea, Table, List, Dropdown, Image, Button, Radiobutton, Checkbox, Element. Pick the type by CLICKING its text (typing does not filter; Enter picks Link). "+" in the modal adds another element. Submit closes the modal, toast "Elements has been created successfully."
**MUST click the toolbar "Save" afterwards.** Submit alone is lost on reload (names show blue until saved). Save toast: "Element updated successfully."
Capture (record from the app) needs the Avo Assure Client; it cannot be done from the browser alone.
Rename element: SAVE FIRST. Double-click the name -> inline input, text selected -> type the new name -> Enter -> click toolbar Save -> toast "Element updated successfully." Re-read all row names to confirm (a bug showed duplicate names when renaming with unsaved rows).
Edit properties: hover row, right pencil -> right pane with identifier priority table (CSS Selector, ID, Absolute XPath, Relative XPath, Name, Classname, Href, Label, custom1, custom2; top is used first). "Save Properties" then repo Save.
Repo "..." menu: Copy, Open in new tab, Copy link, Rename, Delete, Send for review. Tools menu: Map Element (needs captured elements), Self Heal.

## 4. Test case and step group
Test Case tab: folder -> "Create Test Case (Shift+T)" -> inline name -> Enter. Test case "..." menu: Record with Avo Genius, Create recovery, Copy (Shift+C), Open in new tab, Copy link, Rename, Delete (Shift+D), Send for review, Copy as path.
Add group: "Create Test Step Group" (or the "+" under the steps) -> "Add new test step group" -> dialog: Test Step Group Name, Application Type (Web) -> Create. A new group is unsaved: Save is enabled, Debug disabled until Save. Save toast: "Test case data saved successfully."

## 5. Steps
Table columns: Sl.No. | Elements | Actions | Input Value | Output Value.
Manual step: "Add manually" (or row hover "Add Step" > "Add step manually"). Row opens in edit mode (only one row at a time).
1. Elements: click the cell, type to filter, click the option. Built-ins: @Browser, @Generic, @Excel, @Custom, @BrowserPopUp, @Object, @Word. Choosing an element sets its default action.
2. Actions: click, type to filter, CLICK the option (Enter does nothing).
3. Input Value: click, type, Enter commits the row. The placeholder shows the syntax of the chosen action (see action syntax reference).
4. Output Value as needed.
**Elements dropdown rule (verified):** it lists the built-ins plus ONLY the repository elements currently used in this test case's steps. If you overwrite or delete the last step using an element, it disappears from the list again. So: add each new repository element by drag from "Add from repository" first; after that you can pick it in other rows.
**Changing a row's Action clears its Input Value.** Always pick Element, then Action, then type Input.
**Row commit:** typing in Input Value and pressing Enter commits. Clicking another row's cell moves edit mode to that row.
Add from repository: opens a side pane (repo > elements). Elements are added ONLY by drag and drop onto a step row. In a test case the element is inserted ABOVE the row you drop on; in the Reusable Library editor it is inserted BELOW it (and several drops on the same row end up in reverse order). Plan the target row: e.g. first add a closing step (verify or Close Browser) manually, then drop elements onto it in order. Real mouse drag with Playwright `browser_drag` is untested here; coordinate drags and synthetic drag events from the Claude browser failed, calling the React handlers worked. If drag fails, add the element name via the Elements dropdown after one successful drag, or ask the user.
Delete a step: hover the row > trash icon > popconfirm "Are you sure you want to delete this test step?" > Yes > toast "Test Step(s) Deleted Successfully!". Delete is saved at once.
Row hover "+" (add_circle) opens "Add step manually / Add from repository". Do NOT rely on it from scripts; use the "+" under the table (adds at the end) or the group header "+" (in the Reusable Library editor this adds at the TOP).
Step details: row "..." button -> right pane: Test Step description, Report Annotations (Expected, Pass, Fail result) -> Save -> toast "Test case info saved."

## 6. Reusable steps
1. Reusable Library panel (bottom-left): click its chevron to expand, "+" creates folder "New Reusable Library" (rename via "...").
2. Convert an existing group: group header "Reuse" button -> popover "Select Folder" -> choose folder -> Confirmation "move ... to reuse step groups?" -> Yes. The group moves; header shows "Reused | Detach".
3. Use in another test case: "+" under steps -> Add new test step group -> leave name empty -> "Add test step group from Reusable Library" dropdown -> click the group -> Create -> Save.
4. Edit once in the library page (`/reusable-library/<id>`). Save shows a warning "This reusable Test Step Group is used in some of the Test Cases ... continue?" > Yes > toast "Reused data saved successfully." Every test using it updates (verified).
   The library editor is a different table: first click on a cell enters edit mode, a second click focuses the input. Changing the Element there resets the Action to the first action (not the default), so set the Action after the Element.
5. In a test case, the reused group header shows "Reused | Detach | delete". Detach makes a normal editable copy. The delete icon asks "remove ... from reuse step groups?" > Yes > toast "Reused TSG removed Successfully!" and removes the group from that test case only (the library group stays). Saved at once.
6. Rename a library folder or group: row "..." > Rename > Ctrl+A > type > Enter (no toast). Name groups by flow, e.g. Common/Login_StandardUser.

## 6b. Data driven tests (verified build, not run)
1. Step 1: @Generic Get Param. Input `<file path>;<sheet name>` (e.g. `C:\AvoData\App\Login.xlsx;Login`); optional third part `<row count OR start row-end row>`.
2. Step 2: @Generic Start Loop. Then the steps that use data, with column names in pipes as input: `|Username|`.
3. Last: @Generic End Loop. Missing Start/End Loop = "dangling node" error at run time.
4. After a save, each Input cell shows a small table icon = "Select dataTable from the folder" (pick a Data Table or one of its columns). In this instance the picker was EMPTY: no Data Tables exist and no screen to create them was found (Test Case, Execution, Utilities, landing, user menu). Ask the team admin where Data Tables are created, or use Excel files.
5. The Excel file path is on the machine running the Avo client; the header row must match the |names|.

## 6c. Test Case tree "+" menu
Create main folder / Create sub folder / Creator (Beta) / Create new test case (the last two need a folder selected). Creating anything while the open test case has unsaved changes fails with "there are unsaved testcase data please save it before creating the folder." Save first. Shortcuts with a folder selected: Shift+F folder, Shift+T test case.

## 6d. Utilities (left rail on the Home page)
Encryption (AES: plain text > Encrypt > Copy to Clipboard; use for every Secure action input), Authorization (generate an API auth token with expiry), Migration To Avo.

## 7. Execution and E2E
Execution tab: "Execution List" tree, Create folder (Shift+F), "Add Test Case(s) Shift+>" at the bottom. E2E Flows panel (Test Case tab, bottom-left): "+" > Create main folder; sub folder and flow are enabled after a folder exists. Not exercised beyond the menus.

## 8. Debug
Debug needs the Avo Assure Client and is done by the user. A test case must be saved first.

## 9. Verification after every write
- Repo: re-read Element count and names; Save button disabled and names black means saved.
- Test case: reload the deep link and re-read the rows.
- Never report done without the toast text and the re-read.

## 10. Failure table
| Symptom | Cause | Fix |
|---|---|---|
| Elements gone after reload | Submit without Save | Click repo Save, re-create |
| Several rows show the same name | Rename with unsaved rows | Save, rename each, verify |
| Element Type became Link | Pressed Enter in type select | Click the option text |
| Action not applied | Pressed Enter in Actions | Click the option |
| Element missing in step Elements list | Never added from repository | Drag once via Add from repository |
| "Create repository" does nothing | Hover submenu closed | Hover and click Web in one action |
| Row "..." menu missing | Row not selected | Click the row, then the dots |
| Debug disabled | Unsaved changes | Save |
| Menu ref not found | Stale ref | Re-find, never reuse |
| Drag does nothing | Synthetic drag blocked | Try real browser_drag; else ask the user |
| Element not in Elements list after edit | Its last step was overwritten | Drag it in again from the repository |
| Input emptied | Action changed after input | Set Action first, then Input |
| "unsaved testcase data" error | Creating while unsaved | Save, then create |
| Steps added in wrong order | Drop inserts above (test case) / below (library) | Delete wrong rows, re-drop onto the right target |
| Delete repo prompt "Safe to Delete" | Elements unused | Yes; if it warns they are used, stop and ask |
