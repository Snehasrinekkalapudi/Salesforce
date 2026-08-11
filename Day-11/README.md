# Sprint 11 – Crossing the Salesforce Boundary

## External Integration: Shipment Sync, Coupon Validation & Nightly Stock Sync

This sprint extends the Salesforce E-Commerce Automation System by integrating Salesforce with external systems.

The main objective of this sprint was to understand how Salesforce can securely communicate with external services while handling asynchronous processing, synchronous responses, failures, retries, and scheduled high-volume processing.

Three integration scenarios were implemented:

1. Shipment synchronization with an external courier system
2. Real-time coupon validation
3. Nightly product stock synchronization

---

## 1. Business Problem

The E-Commerce application manages orders, products, payments, and shipments inside Salesforce. However, some business operations depend on external systems.

The system needs to:

- Notify an external courier when a shipment is created.
- Handle courier API failures without affecting the main Salesforce transaction.
- Validate coupon codes using an external service while the customer is checking out.
- Synchronize product stock with an external warehouse system every night.
- Handle external API errors safely.
- Prevent duplicate shipment notifications.
- Retry temporary integration failures.

To address these requirements, three Salesforce integration patterns were implemented.

| Requirement | Salesforce Pattern |
|---|---|
| Shipment synchronization | Queueable Apex + HTTP Callout |
| Coupon validation | Synchronous Apex Callout |
| Nightly stock synchronization | Scheduled Apex + Batch Apex |

---

## 2. External System

For this sprint, a mock external API using `https://httpbin.org` was used because a real courier or warehouse API was not available.

The mock API was used to demonstrate:

- HTTP request creation
- HTTP response handling
- Successful responses
- Server errors
- Timeout handling
- Error classification
- Retry processing
- External system communication

The integration follows the same basic approach that would be used with a real courier or warehouse API. In a production environment, the mock endpoint can be replaced with the actual external service.

---

## 3. Authentication and Secure Configuration

A Salesforce **Named Credential** named `Courier_API` was configured for the external integration.

### Configuration

- **Named Credential:** `Courier_API`
- **Endpoint:** `https://httpbin.org`
- **Identity Type:** Anonymous
- **Authentication:** No Authentication

The Apex classes do not contain hardcoded external URLs, usernames, passwords, tokens, or API keys.

Instead, the callouts use the Named Credential reference:

`callout:Courier_API`

This keeps the integration configuration separate from the business logic.

If a real courier API requires authentication in the future, the authentication configuration can be updated in Salesforce without changing the core callout implementation.

---

## 4. Shipment Synchronization

### 4.1 Requirement

When a shipment is created, Salesforce needs to notify the external courier system.

The customer should not have to wait for the courier API response because courier synchronization is a secondary process after the Salesforce transaction.

### 4.2 Implementation

Shipment synchronization was implemented using:

- Queueable Apex
- HTTP Callout
- Named Credential
- Idempotency check
- Integration status tracking
- Error handling
- Retry processing

The main class is:

`ShipmentSyncQueueable.cls`

The queueable job performs the external callout in the background.

### 4.3 Idempotency

A status check was implemented to prevent the same shipment from being sent to the external system more than once.

The integration checks:

`Integration_Status__c`

If the status is already:

`Sent`

the shipment synchronization is skipped.

This prevents duplicate external shipment notifications when the same queueable job is accidentally executed again or when a retry process encounters an already successful shipment.

### 4.4 Integration Status

The shipment integration maintains its own synchronization status.

Possible states include:

- `Sent`
- `Failed`
- `Retry Required`

This separates the Salesforce shipment status from the external integration status.

For example:

A shipment can successfully exist in Salesforce while its courier notification is temporarily unsuccessful.

These are two different business facts and therefore need to be tracked separately.

---

## 5. Error Handling

The integration does not treat every HTTP failure in the same way.

The response status is classified before updating the shipment.

| Response | Meaning | Action |
|---|---|---|
| 200 | Successful request | Mark as `Sent` |
| 401 / 403 | Authentication or authorization issue | Mark as `Failed` |
| 500+ | External server failure | Mark as `Retry Required` |
| Timeout | External service did not respond in time | Mark as `Retry Required` |
| Other status | Unexpected response | Mark as `Failed` |

For failures, the error information is stored in:

`Integration_Error__c`

This makes the failure visible for debugging and monitoring.

---

## 6. Retry Processing

Temporary integration failures should not require manual correction every time.

A separate Batch Apex class was implemented:

`ShipmentRetryBatch.cls`

The batch process:

1. Finds shipments where `Integration_Status__c = 'Retry Required'`.
2. Processes the failed shipments.
3. Re-submits them using `ShipmentSyncQueueable`.
4. Allows temporary external failures to be recovered automatically.

The retry process uses the same shipment synchronization logic instead of implementing a second version of the callout.

This keeps the integration behavior consistent between the original attempt and the retry attempt.

In a production environment, the retry batch can be scheduled to run periodically using Scheduled Apex.

---

## 7. Coupon Validation

### 7.1 Requirement

Coupon validation is different from shipment synchronization.

When a customer enters a coupon code during checkout, the customer expects an immediate answer.

Therefore, this integration was implemented synchronously.

### 7.2 Implementation

The following components were used:

- Lightning Web Component
- Imperative Apex
- `@AuraEnabled` method
- HTTP Callout
- Named Credential
- External coupon validation service

The Apex service is:

`CouponValidationService.cls`

The LWC is:

`couponValidator`

### 7.3 User Experience

The process is:

1. Customer enters a coupon code.
2. Customer clicks **Apply**.
3. LWC calls the Apex validation method.
4. Apex sends the request to the external service.
5. The response is returned to the LWC.
6. The LWC displays the result.

The result can be:

- `VALID`
- `INVALID`
- `ERROR`

Synchronous processing was selected because the customer is actively waiting for the validation result before continuing checkout.

---

## 8. Nightly Stock Synchronization

### 8.1 Requirement

Product stock needs to be synchronized with an external warehouse system.

This process may involve many products and does not require user interaction.

Therefore, Scheduled Apex and Batch Apex were used.

### 8.2 Components

The implementation contains:

`NightlyStockSyncScheduler.cls`

and

`NightlyStockSyncBatch.cls`

### 8.3 Scheduled Processing

The scheduler runs the stock synchronization automatically at 2:00 AM every day.

Cron expression:

`0 0 2 * * ?`

The scheduler starts:

`NightlyStockSyncBatch`

with a batch size of 5.

### 8.4 Batch Processing

The batch process:

1. Retrieves active `Product__c` records.
2. Processes products in manageable chunks.
3. Calls the external warehouse API.
4. Receives the latest stock information.
5. Updates `Stock_Quantity__c`.
6. Continues processing even if an individual product synchronization fails.
7. Logs completion when processing finishes.

Batch Apex was selected because it allows large numbers of records to be processed safely within Salesforce governor limits.

---

## 9. Synchronous vs Asynchronous Processing

The integration approach was selected based on the business requirement.

| Integration | Processing | Reason |
|---|---|---|
| Shipment synchronization | Asynchronous | Customer does not need to wait for courier notification |
| Coupon validation | Synchronous | Customer needs an immediate validation result |
| Nightly stock synchronization | Scheduled + Batch | Large number of products must be processed automatically |

This helped avoid using the same integration pattern for every requirement.

---

## 10. Integration Components

| Component | Type | Responsibility |
|---|---|---|
| `ShipmentSyncQueueable.cls` | Queueable Apex | Performs asynchronous shipment synchronization |
| `ShipmentRetryBatch.cls` | Batch Apex | Processes shipments requiring retry |
| `CouponValidationService.cls` | Apex Service | Performs synchronous coupon validation |
| `couponValidator` | LWC | Provides the coupon validation interface |
| `NightlyStockSyncBatch.cls` | Batch Apex | Synchronizes product stock |
| `NightlyStockSyncScheduler.cls` | Scheduled Apex | Starts nightly stock synchronization |
| `Courier_API` | Named Credential | Provides secure external endpoint configuration |

---

## 11. Shipment Integration Fields

The following fields are used to track the external integration:

- `Integration_Status__c`
- `External_Shipment_Id__c`
- `Last_Sync_Attempt__c`
- `Integration_Error__c`

These fields help distinguish the Salesforce shipment state from the state of communication with the external system.

---

## 12. Testing

### Manual Shipment Sync

A shipment can be tested by enqueueing the Queueable Apex job with a valid shipment ID.

The expected result is:

- External request is sent.
- Integration status is updated.
- Successful requests are marked `Sent`.
- Temporary failures are marked `Retry Required`.
- Permanent failures are marked `Failed`.

### Retry Testing

The retry batch can be executed manually to verify that shipments marked as `Retry Required` are processed again.

### Coupon Validation Testing

The `couponValidator` component can be added to a Salesforce Lightning page.

Testing steps:

1. Open the page containing the component.
2. Enter a coupon code.
3. Click **Apply**.
4. Verify the response displayed by the component.
5. Test valid, invalid, and error scenarios.

### Nightly Stock Sync Testing

The batch can be executed manually during development to verify:

- Product records are retrieved.
- External callouts are performed.
- Stock quantities are updated.
- Individual failures do not stop the entire batch.

The scheduler can also be configured to run automatically at the required time.

---

## 13. Point-to-Point Integration

This sprint uses a **point-to-point integration** approach.

Salesforce communicates directly with the external API through HTTP callouts.

This approach is suitable for the current project because the integration requirements are limited.

If the application grows to include multiple external systems such as:

- Courier services
- Payment gateways
- Warehouse systems
- SMS services
- Email services

a middleware platform such as MuleSoft could be considered.

Middleware would help centralize:

- Data transformation
- Routing
- Monitoring
- Error handling
- Integration management

---

## 14. Key Technical Decisions

### Queueable Apex for Shipment Sync

Queueable Apex was selected because shipment synchronization does not need to block the user's Salesforce transaction.

### Imperative Apex for Coupon Validation

Imperative Apex was selected because the user needs an immediate response while applying a coupon.

### Batch Apex for Stock Synchronization

Batch Apex was selected because multiple product records need to be processed safely and efficiently.

### Named Credential for External Communication

Named Credentials were used to keep endpoint and authentication configuration separate from Apex code.

### Idempotency for Shipment Sync

An idempotency check was implemented to prevent duplicate shipment notifications.

### Retry Mechanism

Temporary failures such as server errors and timeouts are classified as `Retry Required` so they can be processed again.

---

## 15. What I Learned

Through this sprint, I learned how Salesforce can communicate with systems outside the Salesforce platform.

I learned how to:

- Build HTTP callouts from Apex.
- Use Named Credentials for external API communication.
- Choose between synchronous and asynchronous processing.
- Implement Queueable Apex for background integration.
- Implement Batch Apex for large-volume processing.
- Schedule recurring Salesforce jobs using Scheduled Apex.
- Call Apex imperatively from an LWC.
- Handle HTTP success and failure responses.
- Distinguish temporary failures from permanent failures.
- Implement retry processing for failed integrations.
- Prevent duplicate external requests using idempotency.
- Track external integration status separately from Salesforce business status.
- Design integrations that can be extended to real external services in the future.

---

## 16. Sprint Outcome

By completing Sprint 11, the E-Commerce Automation System moved beyond a Salesforce-only application and gained the ability to interact with external systems.

The sprint demonstrates three important integration patterns:

- **Queueable Apex** for asynchronous shipment synchronization
- **Imperative Apex** for real-time coupon validation
- **Scheduled Apex + Batch Apex** for nightly stock synchronization

The implementation also demonstrates secure endpoint configuration, error classification, retry handling, idempotency, and scalable background processing.

These patterns provide a foundation for integrating the application with real courier, warehouse, payment, and other external services in the future.
