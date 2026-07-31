# Salesforce Interview Readiness Bootcamp – Day 4

## Lightning Web Components (LWC) – E-Commerce Automation System

---

# Project Title

**E-Commerce Automation System – Lightning Web Components (LWC)**

---

# Project Overview

This project demonstrates the fundamentals of **Lightning Web Components (LWC)** by building the user interface for an **E-Commerce Automation System**. The objective is to understand how Salesforce applications are developed from the presentation layer to the database layer.

In previous activities, the backend was developed using custom objects, relationships, SOQL, Apex, Validation Rules, and Record-Triggered Flows. This project focuses on building the front-end user interface using Lightning Web Components.

The application includes multiple hands-on activities such as displaying customer information, handling button click events, implementing data binding, and creating a simple dashboard using hard-coded data. No Apex or database integration has been used in this stage.

---

# Technologies Used

* Salesforce Platform
* Lightning Web Components (LWC)
* HTML
* JavaScript
* XML Metadata
* Salesforce Lightning App Builder
* Visual Studio Code
* Salesforce CLI (SFDX)

---

# Project Structure

```
force-app
└── main
    └── default
        └── lwc
            └── ecommerceHome
                ├── ecommerceHome.html
                ├── ecommerceHome.js
                └── ecommerceHome.js-meta.xml
```

---

# Hands-on Activities Completed

## Activity 1 – Create Your First Lightning Web Component

* Created a Lightning Web Component named **ecommerceHome**.
* Displayed a welcome message inside a Lightning Card.
* Deployed the component to Salesforce.
* Added the component to a Lightning App Page using Lightning App Builder.

---

## Activity 2 – Variables and Data Display

Created JavaScript variables to store:

* Customer Name
* Customer Email
* Order Number

Displayed the values on the screen using data binding.

Example:

* Customer: Sneha Sri
* Email: [sneha@gmail.com](mailto:sneha@gmail.com)
* Order: ORD1001

---

## Activity 3 – Button Click Event

Created a button labeled **Show Welcome Message**.

When clicked, the application displays:

> Welcome to Salesforce E-Commerce Automation

This activity demonstrates event handling in Lightning Web Components.

---

## Activity 4 – Status Update

Initially, the application displays:

> Status: Pending

After clicking the **Confirm Order** button, the status changes to:

> Status: Confirmed

This demonstrates updating JavaScript properties and automatically refreshing the user interface.

---

## Mini Project – E-Commerce Dashboard

Created a simple dashboard displaying:

* Current Date
* Total Products
* Total Orders
* Total Payments
* Total Shipments

All values are currently hard-coded and will later be retrieved from Salesforce using Apex and SOQL.

---

## Data Binding Demonstration

Displayed:

```
Hello {customerName}
```

Initially:

```
Hello Sneha Sri
```

After updating the variable:

```
Hello Rahul
```

This demonstrates reactive one-way data binding in Lightning Web Components.

---

# Learning Outcomes

During this project, I learned:

* The architecture of Lightning Web Components.
* The role of HTML, JavaScript, and Meta XML files.
* Creating and deploying Lightning Web Components.
* Using JavaScript variables in LWC.
* Displaying dynamic values through data binding.
* Handling button click events.
* Updating the user interface dynamically.
* Deploying components using Salesforce CLI.
* Adding components to Lightning App Builder.
* Understanding how LWC will communicate with Apex in future development.

---

# Interview Questions

## 1. What is Lightning Web Components (LWC)?

Lightning Web Components (LWC) is Salesforce's modern user interface framework built using web standards such as HTML, JavaScript, and CSS. It enables developers to create reusable, lightweight, fast, and maintainable components that interact with Salesforce data.

---

## 2. What did you build?

I built the user interface of an **E-Commerce Automation System** using Lightning Web Components.

The project includes:

* Welcome page
* Customer details section
* Order status section
* Welcome message button
* Data binding example
* E-Commerce dashboard displaying products, orders, payments, and shipments

The current implementation uses hard-coded values. Future enhancements will retrieve real data from Salesforce through Apex classes and SOQL queries.

---

## 3. Which file contains HTML?

The HTML layout is written inside:

```
ecommerceHome.html
```

This file defines the structure of the user interface, including Lightning Cards, buttons, labels, and displayed data.

---

## 4. Which file contains JavaScript?

The application logic is written inside:

```
ecommerceHome.js
```

This file contains:

* Variables
* Event handlers
* Button click methods
* Business logic
* Data binding properties

---

## 5. What did you learn today?

Today I learned how to build user interfaces using Lightning Web Components. I understood the relationship between HTML, JavaScript, and Meta XML files, implemented data binding and event handling, deployed components using Salesforce CLI, and added them to Lightning App Builder. I also learned how LWC serves as the presentation layer and communicates with Apex to retrieve Salesforce data in enterprise applications.

---

# Conclusion

This project successfully demonstrates the fundamentals of Lightning Web Components by creating an interactive user interface for an E-Commerce Automation System. It establishes the presentation layer of the application and prepares the project for future integration with Apex classes, SOQL queries, and Salesforce database records, completing the end-to-end Salesforce application architecture.
