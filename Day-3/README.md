# Salesforce Flow Automation Assignment

# Order Processing Automation with Salesforce Flow

## Project Overview

This project demonstrates how Salesforce's declarative automation features can streamline order management without requiring Apex code. A **Record-Triggered Flow** was configured to automate order-related actions, while **Validation Rules** were implemented to ensure accurate and consistent data entry.

---

# Project Objectives

* Automate order creation activities using Salesforce Flow Builder.
* Enforce business rules through Validation Rules.
* Understand the appropriate use cases for Flow, Validation Rules, and Apex.
* Improve data accuracy while reducing manual effort.

---

# Technology Stack

* Flow Builder
* Record-Triggered Flow
* Validation Rules
* Email Action

---

# Flow Configuration

## Flow Type

**Record-Triggered Flow**

## Target Object

**Order**

## Trigger Condition

Runs automatically whenever a new Order record is created.

## Automation Process

The flow performs the following actions:

1. Assigns the current date to the **Order Date** field.
2. Saves the updated Order record.
3. Sends an email notification to the system administrator confirming the Order creation.

---

# Flow Logic

```
Start
   ↓
Assign Order Date
   ↓
Update Order Record
   ↓
Send Email Notification
   ↓
End
```

# Screenshots

The repository contains screenshots demonstrating the completed implementation, including:

* Flow Canvas
* Assignment Element
* Send Email Action
* Email Notification
* Successful Flow Execution

---

# Assignment Responses

## 1. Which requirements were implemented using Flow?

The following automation requirements were completed using a Record-Triggered Flow:

* Automatically populate the Order Date during record creation.
* Update the Order record with the assigned value.
* Send an email notification after a new Order is successfully created.

---

## 2. Which requirements were handled using Validation Rules?

Validation Rules were created to enforce business constraints and prevent invalid records from being saved.

Implemented validations include:

* Total Amount must be greater than zero.
* Order Date cannot be empty.
* A Customer must be selected before saving the Order.

---

## 3. Which requirements would require Apex?

Apex was not necessary for this implementation because the required functionality was fully supported by Salesforce's declarative tools.

Typical situations where Apex would be more appropriate include:

* Complex business processes involving multiple objects
* Integration with external systems through APIs
* Advanced calculations and custom processing
* Large-scale bulk operations requiring custom logic

---

## 4. Why were these solutions selected?

**Flow** was selected because it provides an efficient no-code approach for automating record updates and notifications.

**Validation Rules** were implemented to enforce business requirements at the point of data entry, helping maintain high-quality and reliable records.

**Apex** was intentionally not used, as the project requirements were straightforward and could be achieved using Salesforce's declarative capabilities, making the solution easier to maintain.

---

# Learning Outcomes

Through this project, I gained practical experience in:

* Designing and configuring Record-Triggered Flows.
* Using Assignment and Update Records elements within Flow Builder.
* Sending automated email notifications through Flow.
* Creating Validation Rules to enforce business requirements.
* Selecting the appropriate Salesforce automation tool based on the complexity of the requirement.
