
# 📄 Functional Requirements Document  
## Module: Order Tracking – eCommerce Order Lifecycle  
**Version**: 1.0  
**Date**: July 26, 2025  

---

## 🎯 Purpose
To define the complete end-to-end order tracking process for both users and internal systems—from order placement through shipment and delivery, including notifications, logistics integration, and exception handling.

---

## 🔍 Scope

**Includes:**
- Tracking customer orders in real-time
- Handling split shipments and returns
- Integration with logistics partners
- Support for internal dashboards and user notifications
- Tracking for both standard and express delivery
- Edge cases like failed deliveries, returns, reroutes

**Excludes:**
- Order placement and payment flows
- Logistics partner onboarding

---

## 👤 Actors

| Actor | Role |
|-------|------|
| Customer | Track orders, receive notifications, raise delivery concerns |
| Customer Support | Assist users in tracking and resolving issues |
| Warehouse Staff | Update status like “packed” or “ready to ship” |
| Delivery Agent | Updates last-mile delivery statuses |
| System (OMS + Courier API) | Maintains order status timeline |
| Admin | Manages exceptions, edits statuses |

---

## 🧩 Functional Requirements

### 4.1 Order Summary View
- List all user orders with filters (All, Delivered, Cancelled, etc.)
- Sorting options (e.g., Newest First, Price High to Low)
- Each order card shows:
  - Product thumbnails
  - Status badge (e.g., "Out for Delivery")
  - “Track Order” button
  - Delivery ETA
  - Last known tracking event

---

### 4.2 Tracking Page
- Displays:
  - Progress bar with key stages
  - Shipment products
  - Delivery partner name
  - Tracking ID (link)
  - Live location (if supported)
  - Delivery agent contact
  - Expected time slot
- Timeline:
  - Scrollable if more than 5 events
  - Each entry shows status, timestamp, source
- Retry logic for tracking API (max 3 times/30 minutes)

---

### 4.3 Order Lifecycle Stages

| Stage | Description |
|-------|-------------|
| Order Placed | Order successfully created |
| Confirmed | Payment verified |
| Packed | Items packed |
| Shipped | Handed over to courier |
| In Transit | On the way to delivery city |
| Out for Delivery | Last-mile delivery started |
| Delivered | Order completed |
| Failed Delivery | Delivery failed |
| Reattempt Scheduled | New ETA set |
| Returned | Picked for return |
| Refunded | Amount returned |
| Cancelled | Order/user cancellation |

Each stage:
- Creates a log entry
- May trigger notification

---

### 4.4 Courier Integration
- Partner APIs via REST/GraphQL/XML
- Support webhook + polling fallback
- Unified internal tracking format:
```json
{
  "shipmentId": "XYZ123",
  "status": "in_transit",
  "location": "Mumbai Facility",
  "timestamp": "2025-07-25T14:22:00Z"
}
```
- Store raw + parsed responses
- Retry if rate-limited

---

### 4.5 Split Shipments
- Order split due to:
  - Inventory
  - Warehouse/supplier separation
- Each shipment has:
  - Unique tracking ID
  - Timeline + ETA
- Multiple shipment cards per order

---

### 4.6 Notifications
- Triggered on:
  - Status change
  - Delay > 12 hours
  - Failure or refund
- Channels:
  - Push notification
  - SMS
  - Email
- Format:
  - Status, order ID, ETA, link to track

---

### 4.7 Exceptions & Failures
- Show failure reasons (e.g., "Address Not Found")
- Allow address update or support contact
- Status: "Reattempt Scheduled" shows next ETA
- Admin override tool + audit log

---

### 4.8 Returns & Refunds Tracking
- Return pickup tracking shown
- Refund timeline:
  - Pickup → QC Passed → Refund Initiated → Refund Complete

---

### 4.9 Admin & Internal Tools
- Internal UI shows:
  - Raw API data
  - Parsed status
- Role-based update permissions
- Override reasons logged
- Alerts if:
  - Status unchanged > 24h
  - SLA missed

---

## 📊 Analytics & Reporting
- Avg time/stage
- SLA delivery % by courier
- Daily tracking errors
- Return/refund success metrics

---

## 🔐 Security & Compliance
- Encrypted API keys
- Role-based access control (RBAC)
- GDPR/CCPA compliant
- Data retention: 12–24 months

---

## ⚙️ Non-Functional Requirements

| Requirement | Details |
|-------------|---------|
| Performance | < 1.5s response for tracking |
| Scalability | 10K concurrent users |
| Availability | 99.95% uptime |
| Observability | Logs & metrics for courier APIs |
| Resilience | Retry + fallback support |

---

## ✅ Acceptance Criteria
- Accurate real-time order tracking
- Proper timeline stages shown
- External courier integration works
- Notifications on all key events
- Split/return shipments supported
- Exceptions logged and visible
