# Test data management for a large Avo Assure repository
Note: this is general QA practice applied to Avo, not Avo documentation. Data Table screens were NOT found in this version's web UI (see unverified list), so the file based approach below is the safe assumption.

## Principles
1. Separate data from steps. Steps never hold real values except stable constants (URLs, labels). Data lives in one place per data set.
2. One source of truth per data set; steps read it, never copy it.
3. Prefix everything created: `QA_<area>_<yyyymmdd>_<seq>` so test data is findable and cleanable. Never touch rows without the prefix.
4. Tests create their own data when they can, and clean up after. Shared read-only reference data (for example standard users) is allowed but never modified by tests.
5. Never store real customer data or real passwords. Use test accounts; passwords through Avo's encrypted input (setSecureText), not plain text.
6. One test = one outcome. A test that needs 10 inputs gets one data row, not 10 steps.

## Layout for a big repo
- Folders by feature, then by flow: `Login/`, `Cart/`, `Checkout/`.
- Test case names: `TC_<Area>_<NNN>_<Behavior>` (e.g. TC_Login_001_ValidLogin).
- Reusable Library holds only steps shared by 3 or more tests (login, open browser plus navigate, logout). Name groups `<Area>_<Action>` (Login_VerifyProductsPage). Edit them in one place; every test that reuses them updates.
- Element repositories: one per page or screen, element names `<prefix>_<Purpose>` (txt_Username, btn_Login, lnk_Cart, ddl_Sort, chk_Terms). Prefix by type: txt, btn, lnk, ddl, chk, rdo, img, tbl, lst, el.
- Data files (Excel/CSV) in a shared, version-controlled folder, one file per feature, one sheet per scenario group, one row per test iteration. Columns named like the variables they feed.

## Data row design
Columns: TestId | Description | inputs... | expected... | Run (Y/N). Keep expected results in data, not in the step text, so a data change never needs a step edit.
Categories: valid, boundary, invalid, empty, special characters, long input, locked/blocked account.

## Environments
Keep per-environment values (URL, accounts) in a separate small file or variables, selected at run time, so the same test runs against dev, beta and stage without edits. Never point tests at production.

## Maintenance rules
- Review stale data monthly: remove unused rows and files.
- Change log: who changed which data file and why (in the repo's commit message).
- When the app changes, fix the element in the repository once, not in each test.
- Tag each test (smoke, regression, flaky) and keep flaky tests out of the gate until fixed.
- Avoid chaining tests through shared state; each test must run alone and in any order.
