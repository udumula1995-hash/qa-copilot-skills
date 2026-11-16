# Reference project: SauceDemo_Reference (built in the Avo beta instance)
A small example that follows the conventions. Site under test: https://www.saucedemo.com (public demo).

## Element Repository
SauceDemo/ (folder)
- Login_Page: txt_Username, txt_Password, btn_Login, lbl_ErrorMessage
- Products_Page (12): lbl_PageTitle, ddl_SortProducts, lnk_ProductName_Backpack, img_Product_Backpack, lbl_FirstProductName, lbl_FirstProductPrice, btn_AddToCart_Backpack, btn_Remove_Backpack, lnk_ShoppingCart, lbl_CartBadge, btn_Menu, lnk_Logout
- Cart_Page (6) incl. lbl_PageTitle, lbl_CartItemName, lbl_CartItemPrice
- Checkout_Page (10)

## Reusable Library
Common/Login_StandardUser: @Browser Open Browser | @Browser Navigate to URL https://www.saucedemo.com/ | txt_Username Set Text standard_user | txt_Password Set Text secret_sauce | btn_Login Perform Click | @Browser verifyCurrentURL https://www.saucedemo.com/inventory.html
(On a real system the password step would be Set Secure Text with an encrypted value.)

## Test cases
- Login/TC_Login_001_ValidLogin: one group = reused Login_StandardUser.
- Login/TC_Login_002_DataDrivenLogin: group Login_DataDriven
  1 @Generic Get Param `avoassure/Login/DT_Login_Users` (picked with the data table icon)
  2 @Generic Start Loop
  3 @Browser Open Browser
  4 @Browser Navigate to URL https://www.saucedemo.com/
  5 txt_Username Set Text `|Username|`
  6 txt_Password Set Text `|Password|`
  7 btn_Login Perform Click
  8 @Browser verifyCurrentURL `|ExpectedURL|`
  9 @Browser Close Browser
  10 @Generic End Loop
  Data table Login/DT_Login_Users (Manage Integrations > Test Data Management): columns Username | Password | ExpectedURL, one row standard_user | secret_sauce | https://www.saucedemo.com/inventory.html. Not executed (needs the client).
- Cart/TC_Cart_001_AddItemToCart: group 1 = reused Login_StandardUser; group 2 Cart_AddBackpackAndVerify:
  btn_AddToCart_Backpack Perform Click | lbl_CartBadge verifyElementText `1` | lnk_ShoppingCart click | lbl_CartItemName verifyElementText `Sauce Labs Backpack` | @Browser verifyCurrentURL https://www.saucedemo.com/cart.html

None of these were run with Debug (needs the Avo client).

## E2E flow
Purchase/E2E_Login_AddToCart: TC_Login_001_ValidLogin, then TC_Cart_001_AddItemToCart. Saved, not run.
