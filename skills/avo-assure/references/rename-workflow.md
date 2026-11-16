# Workflow: user captures elements, Playwright MCP renames them
Roles: the USER captures elements with the Avo client and saves them in the repository, signs in, and runs Debug. Copilot (Playwright MCP) renames and verifies.

## Before starting (ask the user once)
1. Project name and repository path (folder > repository).
2. Naming convention (default `<prefix>_<Purpose>`: txt, btn, lnk, ddl, chk, rdo, img, tbl, lst, el) and a page prefix if wanted.
3. Whether the user will sign in now. Never enter credentials.

## Procedure
1. Open the repository deep link `/repository/project/<P>/element-repository/<id>` (or navigate the tree per the verified procedures).
2. Read the element list as text: Name | Type. Also read each element's properties if a name is unclear (hover row > right pencil). Do not guess a purpose from a generated name alone.
3. To understand the page, open the product page the user names in a SEPARATE tab and match each element by its properties (id, name, label, text, xpath). Do not rely on position.
4. Build a mapping table: old name | type | new name | evidence. Show it to the user and wait for approval when more than 10 elements or any element is ambiguous.
5. Rename ONE element at a time: double-click name > Ctrl+A > type > Enter. After every 5 renames click Save and check the toast "Element updated successfully."
6. Final: Save, reload the deep link, re-read all names and compare to the approved table. Report mismatches.
7. Do not delete, reorder, or edit identifier properties. Do not touch repositories you were not given.

## Naming rules
Unique within the repository, no spaces, no special characters, type prefix matches the element Type column (Textbox txt, Button btn, Link lnk, Dropdown ddl, Checkbox chk, Radio rdo, Image img, Table tbl, List lst, Element el).

## Failure handling
If a rename shows duplicates or a name does not stick, stop, Save, reload, re-read, and redo only the failed rows. Never loop more than twice; ask the user.
