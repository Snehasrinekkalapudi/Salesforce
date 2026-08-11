# E-Commerce Automation System

## 3. Asynchronous and Synchronous Processing

### 3.1 Asynchronous — Shipment Sync (Queueable)

Used when the user does **not need to wait** for the external system's response before continuing their work.

```text
Order Confirmed (Trigger)
        │
        ▼
Service class
Business logic decides "sync this"
        │
        ▼
Queueable Apex — ShipmentSyncQueueable
        │
        ▼
Idempotency guard
Skip if Integration_Status__c == 'Sent'
        │
        ▼
HttpRequest
Named Credential — Courier_API
        │
        ▼
Http.send()
        │
        ▼
HttpResponse
        │
   ┌────┴───────────────────────────┐
   ▼                                ▼
200 Success                    4xx/5xx/Timeout
   │                                │
   ▼                                ▼
Integration_Status__c          Integration_Status__c
= 'Sent'                       = 'Failed' or
   │                           'Retry Required'
   ▼                                │
Update Shipment__c                  ▼
                              Integration_Error__c
                              populated
                                   │
                                   ▼
                         ShipmentRetryBatch
                         Batch Apex

###3.2 Synchronous — Coupon Validation (Imperative Apex Call)

Used when the user is actively waiting on the screen for a yes/no answer.
```text
LWC — couponValidator
        │
        │ User enters coupon
        │ and clicks Apply
        ▼
Apex — CouponValidationService.validateCoupon()
        │
        │ @AuraEnabled
        ▼
HttpRequest
        │
        ▼
Named Credential
        │
        ▼
Http.send()
        │
        ▼
External Coupon API Response
        │
        ▼
Response returned directly to LWC
        │
        ▼
LWC displays result
        │
   ┌────┼──────────────┐
   ▼    ▼              ▼
 VALID  INVALID        ERROR

###3.3 Scheduled Batch — Nightly Stock Sync

Used for high-volume, time-based processing where no user is waiting.
```text
Scheduled Apex
NightlyStockSyncScheduler
        │
        │ Cron:
        │ 0 0 2 * * ?
        │
        ▼
2:00 AM Daily
        │
        ▼
Database.executeBatch(
    NightlyStockSyncBatch,
    batchSize = 5
)
        │
        ▼
start()
Query all active Product__c records
        │
        ▼
execute()
Process each chunk of 5 products
        │
        ▼
For each Product__c
        │
        ▼
Call Warehouse Stock API
        │
        ▼
Update Stock_Quantity__c
        │
        ▼
Failures are skipped
        │
        ▼
Retried during the next nightly run
        │
        ▼
finish()
Log completion
