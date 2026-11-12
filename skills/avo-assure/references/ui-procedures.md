# Avo Assure UI procedures (Design Studio, Web)

Labels in **bold** are the on-screen names from the official documentation.
Every procedure starts from: **Home** page > **Projects** > select the project > **Design Studio**.

## Contents
1. Test Case tab: folder, test case, test step group, steps
2. Step details, debug
3. Rename, copy, delete (test cases)
4. Element Repository tab: folder, repository, capture, create manually
5. Element operations: rename, edit, move, delete, map
6. Reusable Library
7. Data tables (parameterization)
8. Import and export

## 1. Test Case tab

### Create a folder and a test case
1. **Design Studio** > **Test Case**.
2. Select the **Create Folder (+)** icon > **Create main folder**.
3. Type the name in the **New Folder** text box.
4. On the folder, select **See more (...)** > **Create test case**.
5. Type the name in the **New Testcase** text box.

### Add a test step group
1. Select the test case in the **Test Cases** pane.
2. **Create Test Step Group** > **Add new test step group**. The **Test Step Group** dialog opens.
3. **Test Step Group Name**: type the name.
4. **Application Type**: select **Web**.
5. Select **Create**.

Other options under **Create Test Step Group**: record with Avo Genius, paste a copied
group, or add a group from the Reusable Library.

### Add test steps
- **Add Manually** (preferred for automation): select **Add Manually**; a new step row
  appears. Choose the element from the **Elements** list, then the action, then fill
  **Input Value** and **Output Value**. For the next row select the **Add Step (+)** icon,
  then add step manually. Repeat.
- **Add from Repository**: select **Add from Repository** to open the **Element Repository**
  pane, drag elements into the group. A default action is added for each; edit afterwards.
- Select **Save** to save the steps to the test step group.

## 2. Step details and debug

### Add Step Details
1. Hover over the test step and select the **Add Step Details (...)** icon.
2. Fill **Test Step description**, **Expected Result**, **Pass Result**, **Fail Result**.
3. **Save**. Message: "Test case info saved". These appear under Remarks in the report.

### Debug
1. Select the test case, then **Debug** in the Test Case pane. The **Debug** dialog opens.
2. Tick the test step groups to run (or **Select Teststep Group(s)** for all).
3. Select **Debug**. Result status is shown in the **Avo Assure Client**.
Prerequisite: Avo Assure Client connected. Edits made during a debug run apply to the next run.

## 3. Rename, copy, delete (Test Case tab)

- **Rename a test case**: expand the folder, select the test case, **ellipses (...)** > **Rename**.
- **Rename a test step group**: open the test case, single-click the test step group name, type.
- **Rename a folder**: **ellipses (...)** > **Rename** (documented for repository folders;
  expected to be the same here, confirm on screen).
- **Copy/paste** test case folders and test step groups: via the **(...)** menu
  (see docs-index for the dedicated pages).
- **Delete** test case, test step group or step: via the delete option on that object; confirm.

## 4. Element Repository tab

### Create a folder and a Web repository
1. **Design Studio** (opens on **Element Repository**).
2. **Create Folder (+)** icon > **Create main folder**; type the name in **New Folder**.
   (For a parent-level folder, click the blank workspace first.)
3. On the folder: **Ellipsis (...)** > **Create Repository** > **Web**; type the name in
   **New Repository**.

### Capture elements (user does this; needs Avo Assure Client connected)
1. In the repository select **Capture Elements**. The **Select Browser** dialog opens.
2. Select a browser > **Capture**. The **Avo Assure Client - Capture** window opens.
3. **Step 1: Enter URL**: type the application URL > **Navigate**.
4. **Step 2: Capture Element**: choose **Visible** (visible elements), **All** (includes
   hidden), or **Manual** (user clicks elements; set **Delay** for slow pages).
   For Visible/All pick element types in **Capture Type**.
5. **Capture**, then **Add** to save the selected elements to the repository.
   **Locate** highlights an element in the app; **Delete** removes it from the capture list.

### Create an element manually (no capture needed)
1. In the repository select **Create Manually**. The **Create Element** dialog opens.
2. **Element Name** and **Element Type** are mandatory; other attributes optional.
3. **Submit**. The element shows a **Manual** label.
Supported for Web and Desktop only.

## 5. Element operations

- **Rename element**: open the repository, double-click the element name, type the new name.
- **Edit properties**: select the element, hover the empty column next to it, select the
  **Edit** icon; change values in the **Details** pane; **Save Properties**. Properties can
  be dragged to change identifier priority.
- **Move element**: select it > **Cut** icon > open destination repository > **Paste** icon > **Save**.
- **Delete element**: select in the **Element List** > **Delete** icon > confirm. Blocked if
  the element is used in any test case.
- **See where an element is used**: **View Test Cases** icon next to the element.
- **Rename repository or folder**: **Ellipsis (...)** > **Rename**.
- **Copy / paste repository**: **Ellipsis (...)** > **Copy** (Shift+C); on the target folder
  **Ellipsis (...)** > **Paste** (Shift+V).
- **Delete repository**: **Ellipsis (...)** > **Delete** > **Yes**. Blocked if in use.

### Map a manual element to a captured element
1. **Tools** > **Map Element**. The **Replace custom elements** dialog opens.
2. Under **Manually Created Elements**, expand the element type (for example **Link**).
3. From **Captured Elements**, drag the captured element onto the matching manual element.
4. **Submit**. Toast: "Mapped captured element successfully!"
Same element type only. **Show All Elements** lists all captured elements; **Un-Link** removes
a mapping. **Tools** also has **Compare Element** (enter the URL, finds changed elements)
and **Replace Element** (drag a captured element onto an existing one).

## 6. Reusable Library

1. **Design Studio** > **Test Case**; expand **Reusable Library** in the left pane.
2. **Add (+)** next to Reusable Library > **Create main folder**; name it.
3. On the folder: **See more (...)** > **Create reusable test steps groups** > choose **Web**.
4. Name it in **New test step group**; add steps as in section 1; **Save**.
Use it in a test case via **Create Test Step Group** > add from Reusable Library.

## 7. Data tables

1. Create the Data Table in the same project (up to 100 columns).
2. On the data table: **(...)** > **Copy datatable path**.
3. In the test step group add: `@Generic` / **Get Param** with the copied path as Input Value.
4. Add `@Generic` / **Start Loop**; in its Input Value reference the data table column
   names using the pipe operator (names must match the data table's column names).
5. Add the data-driven steps, then `@Generic` / **End Loop**.

## 8. Import and export

- **Export test case**: select test cases > **Export Testcase** icon > **Export**.
  JSON goes to Downloads.
- **Import test case**: **Import Testcase** icon > **Choose file** > select the JSON >
  optional **Import related elements / repositories** > **Import**. Only unmodified JSON
  exported from Avo Assure works.
- **Import element repository**: **Import Element Repository** icon > **Choose file**
  (.json) > **Import**. Importing into an existing repository REPLACES its elements.
