# E-Commerce Automation System

## Project Overview

A Salesforce-based ecommerce application built using Lightning Web Components (LWC), Apex, SOQL, and Salesforce Custom Objects.

Users can browse products, view details, add products to cart, manage quantities, and proceed to checkout.

## Component Tree

- `ecommerceHome`
  - `productList`
    - `productCard`
    - `checkout`

## Component Communication

### Parent → Child
`productList` passes product data to `productCard` and cart data to `checkout`.

### Child → Parent
`productCard` and `checkout` use Custom Events to send actions and data back to `productList`.

## Data Strategy

- **LDS:** Used for simple Salesforce record operations where possible.
- **Wire:** Used for reactive product retrieval through Apex.
- **Imperative Apex:** Used for user-triggered operations such as order creation and stock updates.

## Validation Strategy

### Client Validation
Checkout validates required customer information before submission.

### Server Validation
Apex validates important business rules such as stock availability and order data.

## Reusability

- **productCard:** Reusable for displaying different products.
- **checkout:** Separate reusable component for customer and order information.

## Debugging

One issue was that Add to Cart worked in the console but the cart did not appear on screen. We checked the event, cart data, view condition, component HTML, and deployment. Redeploying the LWC and refreshing Salesforce fixed the issue.

## Architectural Decision

We separated `productList`, `productCard`, and `checkout` into different components instead of creating one large component. This improved reusability, maintainability, and debugging.

## What I Learned

- Salesforce data modeling and Custom Objects.
- Lightning Web Components.
- Parent-Child communication.
- Custom Events.
- LDS, Wire, and Imperative Apex.
- Apex and SOQL.
- Client and server validation.
- Reusable components.
- Shopping cart management.
- Debugging LWC applications.
- Salesforce Governor Limits.
- Real-world ecommerce application design.
