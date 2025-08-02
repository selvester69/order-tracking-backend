
# 📦 Entities for Order Tracking & Return/Refund Flow (eCommerce)

## 🛒 1. Order
| Field | Type | Description |
|-------|------|-------------|
| `orderId` | String | Unique identifier |
| `userId` | String | Customer who placed the order |
| `orderDate` | DateTime | Time of placement |
| `totalAmount` | Number | Total paid |
| `status` | Enum | `Placed`, `Shipped`, `Delivered`, `Returned`, `Cancelled`, etc. |
| `estimatedDelivery` | DateTime | ETA shown to user |
| `deliveryAddressId` | String | Shipping location |
| `isSplitShipment` | Boolean | True if multiple shipments |
| `trackingSummary` | Object | Overall status overview |

## 📦 2. Shipment
| Field | Type | Description |
|-------|------|-------------|
| `shipmentId` | String | Unique shipment ID |
| `orderId` | String | Reference to order |
| `trackingId` | String | Provided by courier |
| `courierPartnerId` | String | Courier ID |
| `status` | Enum | `Packed`, `In Transit`, `Delivered`, etc. |
| `eta` | DateTime | Shipment ETA |
| `warehouseId` | String | Origin |
| `lastUpdated` | DateTime | Last event timestamp |
| `items` | Array<ProductItem> | Included products |

## 🚚 3. TrackingEvent
| Field | Type | Description |
|-------|------|-------------|
| `eventId` | String | Event unique ID |
| `shipmentId` | String | Related shipment |
| `status` | String | e.g., `Shipped`, `Delivered` |
| `timestamp` | DateTime | Event time |
| `location` | String | Where it happened |
| `source` | Enum | `INTERNAL`, `COURIER_API` |
| `description` | String | Optional details |

## 🧾 4. ProductItem
| Field | Type | Description |
|-------|------|-------------|
| `productId` | String | Catalog reference |
| `productName` | String | Title shown to user |
| `quantity` | Integer | Ordered units |
| `shipmentId` | String | Shipment it belongs to |

## 🧑 5. Customer
| Field | Type | Description |
|-------|------|-------------|
| `userId` | String | Unique customer ID |
| `name` | String | Full name |
| `email` | String | Email address |
| `phone` | String | Mobile number |
| `preferredNotificationMethod` | Enum | `SMS`, `Email`, `Push` |

## 🏠 6. Address
| Field | Type | Description |
|-------|------|-------------|
| `addressId` | String | Unique address ID |
| `userId` | String | Belongs to customer |
| `line1`, `line2`, `city`, `state`, `pincode`, `country` | String | Address fields |

## 🏭 7. CourierPartner
| Field | Type | Description |
|-------|------|-------------|
| `courierPartnerId` | String | Unique ID |
| `name` | String | Company name |
| `apiEndpoint` | String | For live tracking |
| `trackingUrlFormat` | String | Redirect template |
| `supportsWebhook` | Boolean | For real-time status |
| `contactSupport` | String | Escalation contact |

## 🛠️ 8. AdminActivityLog
| Field | Type | Description |
|-------|------|-------------|
| `logId` | String | Log entry ID |
| `shipmentId` | String | Affected shipment |
| `previousStatus` | String | Before update |
| `newStatus` | String | After update |
| `updatedBy` | String | Admin ID |
| `reason` | String | Why the change happened |
| `timestamp` | DateTime | Change time |

## 🔔 9. Notification
| Field | Type | Description |
|-------|------|-------------|
| `notificationId` | String | Unique ID |
| `userId` | String | Recipient |
| `orderId` | String | Related order |
| `shipmentId` | String | Optional shipment |
| `message` | String | Notification content |
| `type` | Enum | `SMS`, `Email`, `Push` |
| `sentAt` | DateTime | Time sent |

---

# 🌀 Return & Refund Entities

## 🔁 10. ReturnRequest
| Field | Type | Description |
|-------|------|-------------|
| `returnRequestId` | String | Unique ID |
| `orderId` | String | Parent order |
| `shipmentId` | String | If return is partial |
| `userId` | String | Customer |
| `reasonCode` | Enum | `Damaged`, `Wrong Item`, `Unsatisfied`, etc. |
| `comments` | String | Optional user notes |
| `pickupScheduledDate` | Date | Pickup date |
| `pickupStatus` | Enum | `Scheduled`, `In Progress`, `Completed`, `Failed` |
| `status` | Enum | `Initiated`, `Approved`, `Picked Up`, `Rejected`, `Completed` |
| `createdAt` | DateTime | Request date |
| `updatedAt` | DateTime | Last update time |

## 💸 11. Refund
| Field | Type | Description |
|-------|------|-------------|
| `refundId` | String | Unique refund ID |
| `returnRequestId` | String | Related return ID |
| `userId` | String | Customer receiving refund |
| `orderId` | String | Related order |
| `amount` | Number | Amount to refund |
| `method` | Enum | `UPI`, `Card`, `Wallet`, etc. |
| `status` | Enum | `Initiated`, `In Progress`, `Success`, `Failed` |
| `initiatedAt` | DateTime | Start of refund |
| `refundedAt` | DateTime | Completion timestamp |
| `transactionReference` | String | Payment gateway transaction ID |

---

## 📋 Summary Table

| Entity | Purpose |
|--------|---------|
| Order | Primary transaction and grouping of items |
| Shipment | Each physical parcel under an order |
| TrackingEvent | Status changes for a shipment |
| ProductItem | Items in the order/shipment |
| Customer | Buyer info |
| Address | Where to deliver |
| CourierPartner | Delivery service |
| Notification | Updates to user |
| AdminActivityLog | Manual actions by internal team |
| ReturnRequest | Customer-initiated return |
| Refund | Payment refund lifecycle |
