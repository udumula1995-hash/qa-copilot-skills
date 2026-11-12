# Avo Assure keywords (actions) for Web

Source: official docs. This is the commonly needed subset, not all 1400+ actions.
The Action dropdown in the test step is the final authority on spelling and casing.
If an action you need is not here, look it up via `docs-index.md`; do not guess.

## Syntax rules
- Input arguments are separated by `;`. Skip an optional argument by leaving it empty
  but keeping its `;` (example: `textbox;;0;John`).
- Dynamic variable: `{Name}`. Created automatically when used in Output Value.
- Output Value pattern: `<Dynamic Variable>;<Dynamic Variable>`. For "get" actions the first
  holds the value and the second the status. For other actions both hold True or False.
- Constant variables are project-level and cannot be modified after creation.

## @Browser
| Action | Input Value | Notes |
|---|---|---|
| Open Browser | none | Opens the browser chosen at debug/execution time |
| Navigate to URL | `<url>` | |
| Navigate to Page with Authentication | `<URL>;<UserName>;<EncryptedPassword>` | Browser auth popup only; AES-encrypted password |
| Maximize Browser | none | |
| Get Browser Name / Get Current URL / Get Page Title | none | Value goes to Output Value |
| VerifyCurrentURL / VerifyPageTitle / VerifyTextExist | expected value | |
| Navigate Back / Refresh Browser | none | |
| Switch to different Window / Open New Tab | see docs | |
| Send Keystrokes / Save File / Bring Browser to Foreground / Clear the Cache | see docs | |

## Captured elements (action list depends on the element type)
| Element type | Common actions | Input Value |
|---|---|---|
| Textbox | Set Text | text to type |
| Textbox | Set Secure Text | encrypted text |
| Textbox | Delete Text, Get Text field Length, verifyText | none / none / expected text |
| Button | click action, Get Button Name, verifyButtonName | none / none / expected name |
| Link | click action, Get Link Text, verifyLinkText | none / none / expected text |
| Dropdown / List | Select Value by Text | visible text |
| Dropdown / List | Select Value by Index | index |
| Dropdown / List | Get Selected Value from Dropdown, Get List Count, Get all Values from List, verifySelectedValue, verifyValuesExists, verifyAllValues, verifyCount | see docs |
| Checkbox | Select Checkbox, Unselect Checkbox | none |
| Radio Button | Select Radio Button | none |
| Element (any) | Click on Element, Double Click, Right Click, Get Text of Element, verifyElementText, Hover Mouse, Set Focus | see docs |
| Any | verifyExists, verifyDoesNotExists, verifyVisible, Wait for Element to be Visible, Get Tool Tip Text, verifyToolTipText, Get Object Status | see docs |
| Any | Send Keystroke Values, Send Function keys as Inputs, Simulate Tab Keypress, Scroll Vertically, Scroll Horizontally | see docs |
| File input | Upload a File, Drop File On the Object | file path |
| Table | Set Text | `<Row>;<Column>;<ObjectType>;<Index>;<Text>` e.g. `2;2;input;1;System Architect` |
| Table | VerifyText | `<Row>;<Column>;<ObjectType>;<Index>;<Text>` |
| Table | Get row data, Verify Cell Value | see docs |

"click action" = the click keyword shown in the Action list for that element; the exact
label for captured buttons and links is not confirmed here, so pick it from the list.

## @Custom (no captured element; identify by type + index)
Use for dynamic elements or when no element-specific action fits.
**A verifyExists step must run before @Custom actions.**
Input pattern: `<ObjectType>;<VisibleText (optional)>;<Index>;<value>;<abs xpath (optional)>`

| Action | Example Input Value |
|---|---|
| Set Text | `textbox;;0;John@123` or `textbox;Password;0;John@123` |
| verifyText | `textbox;;1;Pass@1999` |
| Select Value by Text | `select;;0;Last Month` |
| Get Text From Object | `button;;0` (Output: `{Text}`) |
| Click on Object | `<ObjectType>;<VisibleText>;<Index>` |
Object types include `textbox`, `button`, `radio`, `checkbox`, `dropdown`/`select`, link,
image and other HTML tags.

## @Generic
| Group | Actions |
|---|---|
| Variable Operations | Create Dynamic Variable, Delete Dynamic Variable, Create Constant Variable (`<Constant Variable>;<Value>`), Modify Variable Value (`{Var};newValue`), Copy Value, Display Variable Value, Get index count, Delete Constant Variable |
| Loops and Conditions | If, Else-If, Else, EndIf, For, End-For, Get Param, Start Loop, End Loop |
| Skip Operations | Jump Steps By (`3` forward, `-6` back) |
| String Operations | String Generation (`char;4`) and others |
| Other groups | Interrupt (waits/pauses), Date, Database, Mathematical, File and Folder, XML and Json, Orchestration |

Conditions: `If` / `Else-If` Input Value = `(<operand1>;<operator>;<operand2>)`
- Operators (case-sensitive): `>`, `<`, `==`, `>=`, `<=`, `!=`, `in`, `not in`; combine with `AND`, `OR`, `NOT`.
- Examples: `({Salary};>=;20000)`, `({Department};in;Technology)`.
- `EndIf` takes no input and is mandatory.

Loops: `For` Input Value = number of iterations (e.g. `5`); Output Value `{i}` holds the
iteration count; close with `End-For`.
Data-driven: `Get Param` (data table path) > `Start Loop` (pipe-referenced columns) > steps > `End Loop`.

## Pattern: assert and branch
| Element | Action | Input Value | Output Value |
|---|---|---|---|
| lbl_Error | Get Text of Element | | {Msg} |
| @Generic | If | ({Msg};in;Invalid password) | |
| @Generic | Display Variable Value | {Msg} | |
| @Generic | EndIf | | |
