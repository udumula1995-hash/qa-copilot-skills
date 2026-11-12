# Web testing checklists

Use only the sections that match the pages in scope. Each line is a test condition prompt,
not a finished test case.

## Forms and input
- Required fields: empty submit shows a message per field; message text is clear.
- Each field: valid, invalid format, min/max length, boundary values, whitespace only.
- Trimming of leading/trailing spaces; case sensitivity where relevant.
- Field types: email, phone, date (picker and typed), number (decimals, negative), currency.
- Dependent fields: enabling, disabling, clearing when the parent changes.
- Default values, placeholder vs real value, tab order, Enter key submits.
- Data kept after a validation error; data cleared after success or cancel.
- Double submit creates one record. Cancel and back do not save.
- Success message, redirect target, and the new data visible where expected.

## Login, session, access
- Valid login; wrong password; unknown user; empty fields; locked or inactive user.
- Error message does not reveal which of user or password was wrong.
- Password masked; show/hide toggle; no password in the URL.
- Logout ends the session; back button after logout does not show protected pages.
- Direct URL to a protected page without login redirects to login.
- Session timeout behaviour; remember me; concurrent sessions if a rule exists.
- Each role sees only its menus, pages, buttons and data.
- Forgot password flow, if in scope.

## Lists, tables, search, filters
- Empty state, one row, many rows; pagination first/last/next/previous, page size.
- Sort each sortable column both directions; sort is stable with equal values.
- Search: exact, partial, case, no result, special characters, leading/trailing spaces.
- Filters alone and combined; clear filters; filter + sort + page together.
- Counts and totals match the rows shown. Export matches the filtered list.

## Create, edit, delete
- Created record appears with every field as entered.
- Edit loads current values; saving unchanged data is safe; cancel discards.
- Delete asks for confirmation; cancel keeps the record; deleted record is gone everywhere.
- Unique-field duplicates are rejected with a clear message.
- Two users editing the same record: defined behaviour, no silent overwrite.

## Navigation and UI
- Every link and button goes where its label says; no dead links.
- Breadcrumbs, back button, refresh keep a sensible state.
- Page title, headings, labels, spelling, consistent terms and date/number formats.
- Layout at common widths (desktop, tablet, phone if supported); nothing cut off.
- Loading indicators for slow actions; friendly error page for failures.

## File upload and download
- Allowed type and size; disallowed type; over the size limit; zero-byte file.
- File name with spaces, long name, special characters; same file twice.
- Uploaded file can be opened and matches the original. Download has the right name and content.

## Input-handling probes (harmless strings only)
- Text shows back exactly as entered and is not interpreted: `<b>test</b>`, `O'Brien`,
  `"quoted"`, `a & b`, `100%`.
- If a probe changes page layout, runs as markup, or causes a server error, report it as a
  defect with the exact input and stop probing that field.

## Accessibility basics
- Every control reachable and operable by keyboard; visible focus; logical tab order.
- Inputs have labels; images have alternative text; errors are tied to their field.
- Text contrast is readable; page works at 200% zoom.

## Compatibility
- Browsers and versions named in the test basis; note differences only.

## Non-functional notes (observe, do not measure formally)
- Pages or actions that feel slow (over about 3 seconds): record page, action, rough time.
