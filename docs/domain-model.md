# Domain Model

## Overview

The SMB Subscription Platform is a multi-tenant SaaS application that enables merchants to create subscription plans, onboard customers, manage recurring subscriptions, and process subscription payments.

The domain model defines the core business entities and their relationships.

---

# Core Domain

## Tenant

### Description

A Tenant represents a merchant or business using the platform.

Examples:

* Gym
* Yoga Center
* Coaching Institute
* Milk Delivery Service

### Responsibilities

* Owns business data
* Creates subscription plans
* Manages customers
* Manages subscriptions
* Views payment activity

### Business Rules

* A Tenant can have multiple Users.
* A Tenant can have multiple Plans.
* A Tenant can have multiple Customers.
* A Tenant can have multiple Subscriptions.
* A Tenant can only access its own data.

---

## User

### Description

Represents an authenticated merchant user.

Examples:

* Business Owner
* Manager
* Staff Member

### Responsibilities

* Access platform features
* Manage customers
* Manage plans
* Manage subscriptions

### Business Rules

* A User belongs to exactly one Tenant.
* A User cannot access another Tenant's data.

---

## Plan

### Description

Represents a subscription offering created by a Tenant.

Examples:

* Monthly Membership
* Quarterly Membership
* Annual Membership

### Responsibilities

* Define subscription pricing
* Define billing frequency
* Act as a template for subscriptions

### Business Rules

* A Plan belongs to exactly one Tenant.
* A Plan may be subscribed to by multiple Customers.
* A Plan can be activated or deactivated.
* Deactivated plans cannot accept new subscriptions.

### Attributes

* Name
* Description
* Price
* Billing Frequency
* Status

---

## Customer

### Description

Represents an end customer who subscribes to a Tenant's plans.

Examples:

* Ravi
* Priya
* Arun

### Responsibilities

* Subscribe to plans
* Make payments
* Maintain active subscriptions

### Business Rules

* A Customer belongs to exactly one Tenant.
* A Customer can have multiple Subscriptions.
* A Customer cannot belong to multiple Tenants.

### Attributes

* Name
* Email
* Phone Number
* Status

---

## Subscription

### Description

Represents an agreement between a Customer and a Plan.

The Subscription entity is the core business entity within the platform.

### Responsibilities

* Track subscription lifecycle
* Track renewal schedules
* Track billing status
* Maintain subscription state

### Business Rules

* A Subscription belongs to one Customer.
* A Subscription belongs to one Plan.
* A Subscription belongs to one Tenant.
* A Subscription must have a successful payment before activation.
* A Subscription can have multiple Payment attempts.

### Subscription States

PENDING

ACTIVE

PAST_DUE

CANCELLED

EXPIRED

### State Transitions

PENDING
→ ACTIVE

ACTIVE
→ PAST_DUE

PAST_DUE
→ ACTIVE

PAST_DUE
→ CANCELLED

ACTIVE
→ EXPIRED

### Attributes

* Start Date
* End Date
* Next Billing Date
* Status
* Retry Count

---

## Payment

### Description

Represents a payment attempt associated with a subscription.

### Responsibilities

* Record payment outcomes
* Support auditing
* Support reconciliation
* Drive subscription state changes

### Business Rules

* A Payment belongs to one Subscription.
* A Payment belongs to one Tenant.
* A Payment cannot be modified after completion.
* Every payment must have a status.

### Payment States

PENDING

SUCCESS

FAILED

REFUNDED

### Attributes

* Amount
* Currency
* Status
* Payment Reference
* Payment Timestamp

---

## WebhookEvent

### Description

Represents an event received from a payment provider.

Examples:

* payment.success
* payment.failed
* payment.refunded

### Responsibilities

* Ensure idempotent processing
* Support auditing
* Support troubleshooting

### Business Rules

* Every event must be uniquely identifiable.
* An event can only be processed once.
* Events must be stored before processing.

### Attributes

* Event Type
* Provider Event Id
* Payload
* Processing Status
* Received Timestamp

---

# Domain Relationships
```
Tenant
├── Users
├── Plans
├── Customers
├── Subscriptions
└── Payments

Customer
└── Subscriptions

Plan
└── Subscriptions

Subscription
└── Payments

Payment
└── Webhook Events
```

---

# Aggregate Boundaries

The following aggregates are identified:

## Tenant Aggregate

Root Entity:

* Tenant

Contains:

* Users
* Plans
* Customers

Purpose:

* Tenant ownership and isolation.

---

## Subscription Aggregate

Root Entity:

* Subscription

Contains:

* Payments

Purpose:

* Subscription lifecycle management.
* Billing management.
* Renewal processing.

---

# Architectural Constraints

1. Tenant isolation must be enforced at every layer.
2. Subscription state transitions must be controlled by domain rules.
3. Payment events must be processed idempotently.
4. Subscription activation requires successful payment confirmation.
5. All business entities must be tenant-aware.
