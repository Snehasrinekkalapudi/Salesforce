# Sprint 5 – Building Complete Business Transactions with SOQL, DML and Apex

## Student Details

- **Name:** NEKKALAPUDI SNEHA SRI
- **Register Number:** 23PA1A05H6
- **Project:** E-Commerce Management System
- **Sprint:** Sprint 5 – Building Complete Business Transactions with SOQL, DML and Apex

---

## Objective

The objective of this sprint was to learn how enterprise Salesforce applications combine SOQL, DML, and Apex to perform complete business transactions. The focus was on retrieving only the required data, validating business rules, preventing duplicate records, creating new records, and updating existing records using clean Service classes.

---

## Tasks Completed

### Sprint 7 – Retrieve Product Information

- Created a `ProductService` class.
- Retrieved only the required Product fields using SOQL.
- Retrieved Product Name, Price, Stock Quantity, and Availability.

### Sprint 8 – Retrieve Customer Order Information

- Created an `OrderService` class.
- Retrieved Customer, Customer Email, and Order Status using SOQL.
- Retrieved only the fields required for business processing.

### Sprint 9 – Prevent Duplicate Orders

- Created a `DuplicateOrderService` class.
- Checked whether an Order already existed with the same Order Number.
- Prevented duplicate Orders before saving records.

### Sprint 10 – Create Order

- Created a `CreateOrderService` class.
- Used DML (`insert`) to create a new Order after all validations were completed.
- Ensured business validation occurred before database changes.

### Sprint 11 – Update Order Status

- Created an `UpdateOrderService` class.
- Updated Order Status using DML (`update`).
- Supported Order Status values:
  - Pending
  - Packed
  - Shipped
  - Delivered
  - Cancelled

### Sprint 12 – Complete Business Transaction

Implemented the complete business transaction flow:

```
Customer Request
        ↓
Retrieve Product
        ↓
Retrieve Customer Information
        ↓
Check Duplicate Order
        ↓
Validate Stock Availability
        ↓
Create Order
        ↓
Save Order
        ↓
Display Confirmation
```

---

## Apex Classes Developed

- ProductService
- OrderService
- DuplicateOrderService
- CreateOrderService
- UpdateOrderService

---

## Concepts Learned

- SOQL Queries
- DML Operations
- Apex Service Classes
- Business Validation
- Duplicate Record Prevention
- Record Creation
- Record Update
- Enterprise Business Transactions
- Clean Code Architecture
- Salesforce Developer Console

---

## Engineering Principles Followed

- Retrieved only the fields required for business decisions.
- Performed business validation before DML operations.
- Used Service classes to separate business responsibilities.
- Avoided unnecessary SOQL queries.
- Kept the code modular, reusable, and maintainable.

---

## Testing Performed

Successfully tested:

- Product Retrieval
- Customer/Order Retrieval
- Duplicate Order Validation
- Order Creation
- Order Status Update

Verified execution through the Salesforce Debug Log.

---

## Learning Outcomes

During Sprint 5, I learned:

- How SOQL retrieves business information efficiently.
- How DML is used to insert and update Salesforce records.
- Why business validation should always occur before database changes.
- How Service classes improve code organization and maintainability.
- How complete business transactions are built using Apex, SOQL, and DML.
- Why retrieving only the required fields improves application performance.

---

## Reflection

Today I learned that enterprise applications first retrieve the required business information using SOQL, validate all business rules, and only then perform DML operations. Separating business logic into Service classes makes the application more organized, reusable, and easier to maintain.
