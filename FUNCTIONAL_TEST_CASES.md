# TableTap - Functional Test Cases

## Test Case 1: Diner Session & Menu Access
**Test ID**: TC_001  
**Tested By**: Nikesh Pyakurel 
**Description**: Verify a diner can scan a QR code/access the link and view the menu  
**Preconditions**: Restaurant and Table IDs are valid and active in the database  

| Step | Test Step | Test Data | Expected Result | Actual Result | Status | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Scan QR Code / Access URL | URL with `?restaurant=ID&table=ID` | System checks for existing session. If none, prompts for Phone Verification. | | | |
| 2 | Enter Phone Number | Valid Mobile Number | OTP sent (simulation), user verified. Session created. | | | |
| 3 | View Menu Page | - | Menu loads with Categories and Products specific to the restaurant. | | | |
| 4 | Access with Invalid ID | Invalid Restaurant ID | Error message: "Restaurant not found/Invalid QR" | | | |

**Postcondition**: User is authenticated for that specific table session.

---

## Test Case 2: Cart & Order Placement
**Test ID**: TC_002  
**Tested By**: Nikesh Pyakurel 
**Description**: Verify diner can add items to cart and place an order  
**Preconditions**: User has an active session (TC_001 passed)  

| Step | Test Step | Test Data | Expected Result | Actual Result | Status | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Add Item to Cart | Product: "Burger", Qty: 2 | Cart counter increments. Item added to local state. | | | |
| 2 | View Cart | - | Cart page shows "Burger" x2 with correct total price. | | | |
| 3 | Place Order | - | API call sent. User redirected to "Order Success" page. Order ID generated. | | | |
| 4 | WebSocket Notification | - | Admin dashboard receives real-time alert for new order. | | | |

**Postcondition**: Order status is set to "Pending" in the database.

---

## Test Case 3: Admin Login & Dashboard
**Test ID**: TC_003  
**Tested By**: Nikesh Pyakurel 
**Description**: Verify admin/staff can log in and access the dashboard  
**Preconditions**: Admin account exists  

| Step | Test Step | Test Data | Expected Result | Actual Result | Status | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Navigate to Login | - | Login form displays (Email/Password). | | | |
| 2 | Enter Valid Credentials | Admin Email, Password | Redirect to Dashboard Home. Access to all modules. | | | |
| 3 | Enter Invalid Credentials | Wrong Password | Error: "Invalid credentials". Login denied. | | | |
| 4 | Staff Login | Staff Email, Password | Redirect to Dashboard with limited permissions (if configured). | | | |

**Postcondition**: User is authenticated and token is stored (localStorage/sessionStorage).

---

## Test Case 4: Product Management
**Test ID**: TC_004  
**Tested By**: Nikesh Pyakurel 
**Description**: Verify admin can add and manage products  
**Preconditions**: Admin is logged in  

| Step | Test Step | Test Data | Expected Result | Actual Result | Status | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Navigate to Products | - | List of existing products is displayed. | | | |
| 2 | Add New Product | Name: "Pizza", Price: $15, Category: "Main" | Product added successfully. Appears in the list. Visible on Client Menu. | | | |
| 3 | Edit Product | Change Price to $18 | Product details updated instantly. | | | |
| 4 | Delete/Archive Product | - | Product removed from active list/menu. | | | |

**Postcondition**: Inventory and Menu reflect the changes.

---

## Test Case 5: Order Lifecycle Management
**Test ID**: TC_005  
**Tested By**: Nikesh Pyakurel 
**Description**: Verify staff can process an order from Pending to Paid  
**Preconditions**: Pending order exists (from TC_002)  

| Step | Test Step | Test Data | Expected Result | Actual Result | Status | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | View Orders | - | New order displayed in "Orders" list with status "Pending". | | | |
| 2 | Accept Order | Click "Accept/Confirm" | Status changes to "Confirmed". Kitchen notified. | | | |
| 3 | Mark as Ready/Served | Click "Serve" | Status changes to "Served". | | | |
| 4 | Generate Bill | Click "Bill/Payment" | Bill generated. Status changes to "Payment Pending". | | | |
| 5 | Complete Payment | Select Payment Method | Status changes to "Completed/Paid". Order moves to History. | | | |

**Postcondition**: Order is closed and stored in Order History.

---

## Test Case 6: Quick Order (POS)
**Test ID**: TC_006  
**Tested By**: Nikesh Pyakurel 
**Description**: Verify valid takeaway/quick order creation by staff  
**Preconditions**: Admin/Staff logged in  

| Step | Test Step | Test Data | Expected Result | Actual Result | Status | Notes |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | Navigate to Quick Order | - | POS interface loads with menu items. | | | |
| 2 | Select Items | Various Products | Cart updates on the right/bottom panel. | | | |
| 3 | Checkout | Customer Name: "Walk-in" | Order placed immediately without table session. | | | |
| 4 | Print Bill | - | Receipt generated for printing. | | | |

**Postcondition**: Takeaway order recorded in system.
