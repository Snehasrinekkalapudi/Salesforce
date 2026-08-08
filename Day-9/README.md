# E-Commerce Product Portal

## Business Problem

The goal of this project is to provide users with a simple e-commerce experience inside Salesforce.

Users can:

- View available products
- View product details
- Add products to a cart
- Increase or decrease product quantity

The application also handles loading, empty, and error states so users receive proper feedback when data is being retrieved or is unavailable.

---

## Engineering Decisions

### 1. Product data is retrieved through Apex

Product data is retrieved from Salesforce using an Apex controller and SOQL.

This keeps the Salesforce data-access logic in Apex instead of placing it directly inside the UI.

### 2. ProductCard is a child component

ProductCard was separated from ProductList because it represents an independent UI responsibility.

ProductList manages the product collection, while ProductCard displays and handles actions for one product.

### 3. Custom Events are used for communication

ProductCard uses custom events to communicate user actions back to ProductList.

For example:

- `productselect` for View Details
- `addtocart` for Add to Cart

This keeps communication between the child and parent components clear.

### 4. Duplicate products increase quantity

When a user clicks Add to Cart for a product that is already in the cart, the product is not added again.

Instead, its quantity is increased.

For example:

```text
Samsung S24
Quantity: 1

        ↓ Add to Cart

Samsung S24
Quantity: 2
```

This provides more realistic shopping-cart behavior.

---

## Challenges Faced

One debugging problem occurred while implementing the shopping cart.

Initially, clicking **Add to Cart** multiple times added the same product multiple times.

For example:

```text
Samsung S24
Samsung S24
Samsung S24
```

The problem was solved by checking the product Id before adding it to the cart.

If the product already exists, its quantity is increased instead.

The final result is:

```text
Samsung S24
Quantity: 3
```

This helped me understand the importance of managing application state correctly.

---

## Learning

This project changed the way I think about user-interface development.

I learned that building a UI is not only about displaying data.

The UI also needs to handle:

- Loading
- Empty data
- Errors
- User interactions
- Application state
- Communication between components

I also learned how data flows from Salesforce through Apex and SOQL into an LWC, and how child components can communicate user actions back to their parent using custom events.

The biggest learning was that good UI development requires thinking about what happens in different situations, not just what the screen looks like when everything works correctly.
