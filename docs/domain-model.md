# Domain Model

## Overview

The SMB Subscription Platform is a multi-tenant SaaS application that enables merchants to create subscription plans, onboard customers, manage recurring subscriptions, and process subscription payments.

The platform is intentionally designed as an MVP focused on subscription lifecycle management, tenant isolation, payment processing, and recurring billing automation.

The domain model defines the core business entities, ownership boundaries, and business rules.

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
* Water Delivery Service

### Responsibilities

* Owns business data
* Creates subscription plans
* Manages customers
* Manages subscriptions
* Monitors payment activity

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

* Access platform functionality
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

The platform does not support Product Catalog Management.

Plans are the primary offerings exposed by a Tenant.

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

Represents an end customer subscribing to a Tenant's plan.

Examples:

* Ravi
* Priya
* Arun

### Responsibilities

* Subscribe to plans
* Make payments
* Maintain subscription relationship

### Business Rules

* A Customer belongs to exactly one Tenant.
* A Customer can have only one Subscription within a Tenant.
* A Customer cannot belong to multiple Tenants.
* A real-world individual may exist as a Customer in multiple Tenants.
* Customers are never shared across Tenants.

### Example

CultFit

* Ravi

UFC Gym

* Ravi

Yoga Center

* Ravi

The above records represent three independent customer records belonging to different Tenants.

### Attributes

* Name
* Email
* Phone Number
* Status

---

## Subscription

### Description

Represents the agreement between a Customer and a Plan.

The Subscription entity is the primary business entity within the platform and acts as the aggregate root for billing and renewal workflows.

### Responsibilities

* Track subscription lifecycle
* Track billing status
* Track renewal schedule
* Maintain subscription state

### Business Rules

* A Subscription belongs to one Customer.
* A Subscription belongs to one Plan.
* A Subscription belongs to one Tenant.
* A Subscription must have a successful payment before activation.
* A Subscription can have multiple Payment attempts.
* A Customer can have only one Subscription within a Tenant.

### Subscription States

* PENDING
* ACTIVE
* PAST_DUE
* CANCELLED
* EXPIRED

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

Represents a payment attempt associated with a Subscription.

### Responsibilities

* Record payment outcomes
* Support auditing
* Support reconciliation
* Drive subscription state changes

### Business Rules

* A Payment belongs to one Subscription.
* A Payment belongs to one Tenant.
* A Payment cannot be modified after completion.
* Every Payment must have a status.

### Payment States

* PENDING
* SUCCESS
* FAILED

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
* subscription.renewed

### Responsibilities

* Ensure idempotent processing
* Support auditing
* Support troubleshooting

### Business Rules

* Every event must be uniquely identifiable.
* An event can only be processed once.
* Events must be persisted before processing.

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
└── Subscriptions

Customer
└── Subscription

Plan
└── Subscription

Subscription
└── Payments

Payment
└── WebhookEvents
```

---

# Aggregate Boundaries

## Tenant Aggregate

Root Entity

* Tenant

Contains

* Users
* Plans
* Customers

Purpose

* Tenant ownership
* Tenant isolation
* Access control

---

## Subscription Aggregate

Root Entity

* Subscription

Contains

* Payments

References

* Customer
* Plan

Purpose

* Subscription lifecycle management
* Billing management
* Renewal processing

---

# MVP Constraints

1. Product Catalog Management is not supported.
2. Customers are tenant-scoped.
3. Customers are never shared across Tenants.
4. A Customer can have only one Subscription within a Tenant.
5. One Subscription maps to exactly one Plan.
6. Subscription activation requires successful payment confirmation.
7. Payment processing is webhook-driven.
8. Tenant isolation is enforced across all business entities.

---

# Architectural Constraints

1. Tenant isolation must be enforced at every layer.
2. Subscription state transitions must be controlled by domain rules.
3. Payment events must be processed idempotently.
4. Subscription activation requires successful payment confirmation.
5. All business entities must be tenant-aware.
6. Webhook events must be persisted before processing.
7. Business operations must remain stateless at the application layer.
