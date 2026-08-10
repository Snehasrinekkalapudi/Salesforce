# E-Commerce Automation System

## Architecture

The E-Commerce Automation System is developed using Salesforce Lightning Web Components (LWC), Apex, SOQL, and Salesforce custom objects.

The application follows a component-based architecture where the main ecommerce component manages the product listing, product details, shopping cart, and checkout flow.

---

## Component Architecture

The component hierarchy of the application is:

```text
ecommerceHome
│
└── productList
    │
    ├── productCard
    │
    ├── Product Details
    │
    ├── Shopping Cart
    │
    └── checkout
```

### Component Responsibilities

| Component | Responsibility |
|---|---|
| `ecommerceHome` | Main entry point of the ecommerce application |
| `productList` | Retrieves products and manages product, details, cart, and checkout views |
| `productCard` | Displays individual product information and provides View Details and Add to Cart actions |
| `checkout` | Collects customer information and displays the order summary |
| `ProductController` | Apex controller responsible for retrieving product records from Salesforce |

---

## Data Flow

The application follows this data flow:

```text
Salesforce Product__c
        │
        ▼
ProductController.cls
        │
        │ @wire
        ▼
productList
        │
        ▼
productCard
        │
        ├──────────────────┐
        │                  │
        ▼                  ▼
View Details          Add to Cart
        │                  │
        ▼                  ▼
Product Details      Shopping Cart
                           │
                           ▼
                  Proceed to Checkout
                           │
                           ▼
                       checkout
                           │
                           ▼
                     Customer Details
                           │
                           ▼
                     Order Summary
                           │
                           ▼
                      Place Order
```

---

## Application Flow

```text
Product Catalog
      │
      ▼
View Product Details
      │
      ▼
Add Product to Cart
      │
      ▼
Shopping Cart
      │
      ├── Increase Quantity
      │
      ├── Decrease Quantity
      │
      └── Remove Product
      │
      ▼
Cart Summary
      │
      ▼
Proceed to Checkout
      │
      ▼
Customer Information
      │
      ▼
Order Summary
      │
      ▼
Place Order
```

---

## Salesforce Architecture

```text
                    Salesforce Platform
                           │
                           ▼
                     Product__c
                           │
                           ▼
                ProductController.cls
                           │
                           │ Apex / SOQL
                           ▼
                    ecommerceHome
                           │
                           ▼
                     productList
                           │
             ┌─────────────┼─────────────┐
             │             │             │
             ▼             ▼             ▼
       productCard    Product Details   Cart
             │                           │
             │                           ▼
             │                       checkout
             │                           │
             └───────────────────────────┘
                                         │
                                         ▼
                                   Place Order
```

---
