# Avo Assure Web: action syntax reference
Source: the app's own keyword catalogue (Avo Assure v26.10.0), read from the instance, plus docs. Syntax rules apply in the step table columns Elements | Actions | Input Value | Output Value.

## Rules
- Separate multiple input parts with `;`. Parts marked `(optional)` can be left out.
- Output Value takes a dynamic variable name, e.g. `{pageTitle}` style names as you define them. Most actions allow two outputs: `<Dynamic Variable>(Optional);<Dynamic Variable>(Optional)`. Get/verify actions that return data require the first one.
- Elements dropdown: the element name from the repository (it only appears after the element was added to the test case once, by drag from the "Add from repository" pane) or a built-in: @Browser, @Generic, @Excel, @Custom, @BrowserPopUp, @Object, @Word.
- Actions dropdown: type to filter, then CLICK the option. Enter does not apply it.
- Input cell: type, press Enter to commit. Save the test case afterwards (toast: "Test case data saved successfully.").

## @Browser
| Action | Input | Notes |
|---|---|---|
| openBrowser | none | first step of a flow |
| navigateToURL | `<url>` | |
| navigateWithAuthenticate | `<URL>;<UserName>;<EncryptedPassword>` | basic-auth pages |
| closeBrowser, openNewTab, refresh, navigateBack, maximizeBrowser, clearCache | none | |
| switchToWindow | `<window number>` | |
| closeSubWindows | `<all>` or `<n1;n2>` | optional |
| sendKeys | `<FunctionKey>;<Number(optional)>` | |
| sendSecureKeys | `<encrypted value>` | |
| setBrowserZoomLevel | `<Zoom Level>` | |
| saveFile | `<FilePath>;<FileName.extn>;<Wait sec>(optional)` | downloads |
| getCurrentURL, getPageTitle, getBrowserName | none, Output required for get | |
| verifyCurrentURL | `<url>` | |
| verifyPageTitle | `<page title>` | |
| verifyTextExists | `<text>` | text anywhere on page |

## Textbox / Textarea (input)
setText `<input value>` | setSecureText `<encrypted input>` (passwords) | sendValue / sendSecureValue (same inputs, key-by-key) | clearText | click, doubleClick, rightClick, mouseHover, setFocus, tab, press | sendFunctionKeys `<FunctionKey>;<Number(optional)>` | getText (Output required) | getTextboxLength | getToolTipText | getAttributeValue `<AttributeName>;<css>(Optional)` output `<AttributeValue>;<Status>` | verifyText `<input value>` | verifyTextboxLength `<n>` | verifyToolTipText `<text>` | verifyAttribute `<AttributeName>;<AttributeValue(Optional)>;<css>(Optional)` | verifyStyle `<Style attribute>;<Style Value>` | verifyExists, verifyDoesNotExists, verifyVisible, verifyHidden, verifyEnabled, verifyDisabled, verifyReadOnly (no input) | waitForElementVisible | dropFile `<FilePath>;<FileName>` | dateSelect `<Date format>`.

## Button
click (default), doubleClick, press, mouseHover, setFocus, tab | getButtonName, getToolTipText (Output required) | verifyButtonName `<text>` | verifyToolTipText `<text>` | uploadFile `<FilePath>;<FileName>;<Wait sec>` | clickonnextbutton `<nextCount>` | verifyExists / DoesNotExists / Visible / Hidden / Enabled / Disabled | waitForElementVisible | verifyAttribute, verifyStyle, getAttributeValue as above.

## Link (a)
click (default), jsclick (use when a normal click is intercepted), doubleClick, rightClick, mouseHover | getLinkText (Output) | verifyLinkText `<text>` | verifyToolTipText | uploadFile | verifyExists etc | waitForElementVisible.

## Dropdown (select)
selectValueByText `<text>` (default) | selectValueByIndex `<index>` | selectByValue `<value>` | selectByAbsoluteValue `<text>` | getAllValues, getCount, getSelected (Output) | getValueByIndex `<index>` | verifySelectedValue `<text>` | verifyAllValues / verifyValuesExists `<text1>;<text2>;...` | verifyCount `<n>` | getElementCount `<Text>` | mouseleftclickwithcontrol / mouserightclickwithcontrol | verifyExists etc.

## Checkbox
selectCheckbox, unselectCheckbox, click, clickinput | getStatus (Output) | verifyExists/Enabled/Disabled/Visible/Hidden/ReadOnly | verifyToolTipText.

## Radio button
selectRadioButton | getStatus (Output) | verifyExists/Enabled/Disabled/Visible/Hidden/ReadOnly | verifyToolTipText.

## Generic patterns for all web element types
getAttributeValue, getElementTagValue, getToolTipText, verifyAttribute, verifyStyle, verifyToolTipText, verifyExists, verifyDoesNotExists, verifyVisible, verifyHidden, mouseHover, setFocus, tab, drag, drop `<time in sec>(optional)`, waitForElementVisible exist on nearly every type.

## Other elements and keywords seen in the app
- @Custom pattern `<ObjectType>;<VisibleText(optional)>;<Index>;<value>;<abs(optional)>`.
- Table Set Text `<Row>;<Col>;<Object Type>;<Index>;<Text>`.
- @Generic: Get Param `<file path>;<sheetname in case of excel>(Optional);<row count OR start row-end row>(Optional)`; If `(<operand1>;<operator>;<operand2>)`; For `<number>`; Create Dynamic Variable `<variable name>;<value to be stored>`; Create Constant Variable `<Constant Variable>;<Value>`.
- Table, image, list, element, grid and other types exist; use the Actions dropdown for the exact list and read its placeholder text, which shows the syntax.
- The instance also holds team-made custom keywords. They show a pencil icon in the Actions list and are specific to your instance; read them there.

## Defaults per type (what Avo preselects)
textbox Set Text; button Perform Click; link click; element Click on Element; image Click on Image; checkbox Select Checkbox; radio Select Radio Button; dropdown Select Value by Text; @Browser Open Browser.
