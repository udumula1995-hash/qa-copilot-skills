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

---
# Part 2: remaining element types (from the app's catalogue, standard keywords only)
Display name [keyword] | input | output. Output is `<Dynamic Variable>(Optional);<Dynamic Variable>(Optional)` unless shown.

## Element (generic web element)
Click on Element [clickElement] | Double Click [doubleClick] | Right Click [rightClick] | Hover [mouseHover] | Set Focus | Tab | Press
Get Text of Element [getElementText] out `<Dynamic Variable>` | Get Text From Object [getText] | Set Text `<input value>` | Delete Text [clearText] | Send Keystroke Values [sendValue] `<input value>` | Send Keystrokes Securely / Set Secure Text `<encrypted input>`
Get Object Attribute Value `<AttributeName>;<css>(Optional)` out `<AttributeValue>;<Status>` | Get Count of Child Elements `<tag name>(Optional)` | Get Child Elements `<tag>` | Get Tool Tip Text `<ObjectType>;<VisibleText(optional)>;<Index>` | Get Tooltip Based on Coordinates `Coordinates`
Combo: cmbGetCount, cmbGetAllValues, cmbSelectValueByIndex `<index>`, cmbSelectValueByText `<text>`
Upload a File `<FilePath>;<FileName>;<Wait sec>` | Drop File `<FilePath>;<FileName>` | Drag / Drop `<time in sec>(optional)` | Wait for Element to be Visible
verifyElementText `<input value>` | verifyText `<input value>` | verifyAttribute `<AttributeName>;<AttributeValue(Optional)>;<css>(Optional)` | verifyStyle `<Style attribute>;<Style Value>` | verifyToolTipText `<Input Value>;<Coordinates>(Optional)` | verifyVisible / verifyHidden `<ObjectType>;<VisibleText(optional)>;<Index>` (inputs optional for a mapped element) | verifyExists, verifyEnabled, verifyDisabled, verifyDoesNotExists | getElementCount `<Text>` | dateSelect `<Date format>`
Note: for a label (lbl_) use **verifyElementText**; Verified in the reference project.

## Image
Click on Image [click] | doubleClick | mouseHover | Get Image similarity Percentage `<image path>` | verifyWebImages `<image path>` | uploadFile, dropFile, drag/drop | getAttributeValue | getToolTipText | verifyExists/Visible/Hidden/Enabled/Disabled/DoesNotExists | verifyToolTipText | verifyStyle | waitForElementVisible.

## Table (inputs use Row;Column;ObjectType;Index, 1-based)
Click on Table Cell [cellClick] `<Row>;<Column>;<ObjectType>;<Index>` | Double Click Table Cell same | rightCellClick same | Simulate Mouse Click / Hover `<Row>;<col>`
Set Text `<Row>;<Col>;<Object Type>;<Index>;<Text>` | Set Secure Text …;`<Encrypted Text>` | Clear Text From Cell `<Row>;<Col>;<Object Type>;<Index>` | Send Keystroke Values …;`<Text>`
Get Cell Value `<row>;<col>` | Get Column Number by Text `<text>` | Get Row Number by Text `<text>` | Get Total Row Count | Get Total Column Count | Get Cell Tool Tip `<row>;<col>` | Get Row Data `<Row Value>` or `index;<Row Index>` | Get Column Data `<Column Header Value>` or `index;<Column Index>` | Get xpath of Inner Table `<Row>;<Column>(Optional)` | Get Selected / Get Status `<Row>;<Col>;<Object Type>;<index>`
Select Value by Text `<Row>;<Col>;<Object Type>;<index>;<input value>` | by Index …;`<index>` | Select By Value …;`<value>` | Select Based on Absolute Value `<row>;<col>;<dropdown>;<elementIndex>;<exact text>`
Sort Table `<Column Header Value>` or `<Column Index>` | Verify Table Sort `<Column Header Value>;<Sort Order>` | Verify Value in Table `<value>` | Verify Column Header `<header>` | verifyCellValue `<Row>;<Column>;<value>` | verifyCellToolTip `<row>;<col>;<value>` | verifyTextExists `<text>` | verifyText `<Row>;<Col>;<Object Type>;<Index>;<Text>` | verifyTextboxLength …;`<Value>` | verifyAttribute `<row>(Opt);<col>(Opt);<objectType>(Opt);<index>(Opt);<AttributeName>;<AttributeValue(Opt)>;<css>(Opt)` | verifyStyle `<row>;<col>;<objectType>(Opt);<index>(Opt);<AttributeName>;<AttributeValue>`

## List (multi-select list)
Select all Values | Deselect all | Select Value by Index `<index>` / by Text `<text>` | Select Multiple Values by Indexes `<i1>;<i2>;…` | Select Values By Text `<t1>;<t2>;…` | Get Value by Index `index` | Get Multiple Values by Indexes `<i1>;<i2>;…` | Get Selected | Get List Count | verifyAllValues / verifyValuesExists / verifySelectedValues `<t1>;<t2>;…` | verifyCount `<n>` | plus the common verify/get actions.

## @BrowserPopUp (JS alert/confirm)
Accept Popup | Dismiss Popup | Get Popup Text (out required) | verifyPopUpText `<text>`.

## @Object (a table captured into a variable)
Same as Table but first input is `<variable name>`, e.g. Get Cell Value `<variable name>;<row>;<col>`.

## @Excel (file actions; set the path first)
Set Path for File `<file path>;<sheetname>(Optional)` | Get Total Row Count `<column number>(optional)` | Get Total Column Count `<row number>(optional)` | Read Cell From File `<Row number>;<Column number>` | Write to Cell in File `<Row>;<Column>;<Value>;<Type>` | Clear Cell `<rowNum>;<ColNum>` | Delete Row `<RowNumber>;…` | Copy Excel WorkBook `<FilePath1>;<FilePath2>;<Option>(Opt);<text/special>(Opt)` | Get Row Data `<index/value>;<row index>/<cell value>` | Clear Excel File Path.

## @Word / PDF
Read Word Doc `<filename>;<parano>` | Read PDF `<filename>;<pageno>` | Get all Tables From File `<filename>` | Read JSON Value `<filename>;<jsonpara>` | Read XML Value `<filename>;<xmlpara>` | Overwrite Word File `<filename>;<filetext>;<operation>;<file_encoding>`. PDF list: Get Text From File Dynamically `<PDFPath>;<index>(Opt)`, Verify Input Text Dynamically `<PDFPath>;<InputText>;<index>(Opt)`.

## @Generic (flow control, variables, data, files, DB)
**Data / loops:** Get Param `<file path>;<sheetname in case of excel>(Optional);<row count OR start row-end row>(Optional)` | Save Param `<Variable name>` | Start Loop | End Loop | Write To Data Table `<datatable_path>;<row>;<col>;<value>`
**Conditions:** If `(<operand1>;<operator>;<operand2>)` | Else-If same | Else | EndIf | For `<number>` | End-For | Jump Steps By `<count>` | Jump To 'TestSteps' `<test case name>` | Stop Execution `<type>(Optional)` | Pause Execution | Wait `<Time in seconds>`
**Variables:** Create Dynamic Variable `<variable name>;<value>` | Create Constant Variable `<Constant Variable>;<Value>` | Delete Dynamic/Constant Variable `<variable>` | Display Variable Value `<variable>` | Copy Value `<target variable>;<source variable>` | Modify Variable Value `<variable>;<new value>` | Null Check `<variable>` | Type Cast `<Variable>;<type>` | Append Dynamic Variable `<dynamic variable>` | Get Index Count `<list variable>`
**Compare / evaluate:** Compare Values `<value1>;<value2>;<lowercase(True/true)>(optional)` | Evaluate `<operand1> <operator> <operand2>`
**Strings:** Get String Length `<string>` | Get Sub String `<string>;<index OR range>` | To Lower/Upper Case `<string>` | Left/Right `<string>;<position>` | Mid `<string>` | Concatenate `<s1>;<s2>;…` | Find `<string>;<to find>;<wildcard>(opt)` | Replace `<string>;<old>;<new>` | Split `<string>;<char>` | Trim `<string>` | String Generation `<type>;<length>` | Currency Number Formatter `<value>`
**Dates:** Get Current Date `<format>` | Get Current Time `<format>` | Get Current Date,Time `<format>` | Get Current Day | Get Current Day,Date,Time | Date Addition `<date>;<days>;<format>` | Date Difference `<date>;<days OR date>;<format>` | Month/Year Addition `<date>;<n>;<format>` | Compare Date `<date1>;<date2>;<date1 format>` | Change Date Format `<date>;<current>;<required>`
**Files / folders:** Create/Verify/Rename/Delete Folder, Create/Verify/Rename/Delete File (`<path>;<name>`…), Move/Copy File/Folder `<Source>;<Destination>;<1 overwrite/0>(Opt);<ext>(Opt)`, Find File Path `<file name>;<folder path>`, Get File Content, Write to File `<File path>;<sheet>(Opt);<content>;<Newline>`, Clear File Content, Verify Content, Compare Content, Compare Files, Compare Inputs, Cell by Cell Compare, Selective Cell by Cell Compare, Compare PDFs, Find Image in PDF, Get PDF Page Count, Normalize PDF, Save File `<FilePath>;<FileName.extn>;<URL>(opt)`, Execute File `<FilePath>`, Capture Screenshot `<path>(Opt);<filename>(Opt)`
**XML / JSON:** Get Block Count, Get Block Value, Get Tag Value `<xmlInput>;<XMLPath>;@<Attr>(Opt)`, Get Key Value `<JsonInput>;<KeyPath>`, Set Key Value, Compare JSON Content, Get XML Block Data, Compare XML File-XML Block, Beautify `<Filepath/Input>;<xml/json>`
**Database:** Get Data / Verify Data / Export Data / Run Query and their Secure versions: `<DB host or file>;<port>;<user>;<password or encrypted password>;<DB name>;<Query>;<DB Number>;<querytype/warehouse>(Opt);<schema>(Opt)`. Use the Secure versions with an encrypted password (Utilities > Encryption).
**Other:** Save To Clipboard, Get from Clipboard, Send Function Keys `<key>;<number>(Opt)`, Send Secure Function Keys, Get Key Status, Generate TOTP `<Encrypted Secret Key>`, Verify Objects.

## Encrypted inputs
Every "Secure" action (setSecureText, sendSecureValue, secure DB actions, Generate TOTP) needs an AES-encrypted value. Make it in **Utilities (left rail) > Encryption**: Encryption Method AES, paste plain text, Encrypt, Copy to Clipboard. Never put plain passwords in steps for real systems.
