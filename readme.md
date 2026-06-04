# SMBS Subscription Platform

A cloud-native multi-tenant subscription management platform designed for Small and Medium Businesses (SMBs).

This project demonstrates the design and implementation of a SaaS subscription platform capable of onboarding merchants, managing customers, creating subscription plans, processing recurring payments, and handling subscription lifecycle events through a secure and scalable architecture.

The primary objective of this project is to showcase software architecture, domain-driven design, multi-tenancy, payment integration patterns, and cloud-native backend development using modern Java technologies.

---

## Problem Statement

Many small and medium businesses rely on manual processes to manage recurring customer payments.

Examples include:

* Gyms and fitness centers
* Coaching institutes
* Yoga studios
* Milk delivery services
* Water can delivery businesses
* Maintenance service providers

These businesses often manage subscriptions using spreadsheets, manual follow-ups, and ad-hoc payment collection methods.

Enterprise-grade subscription platforms are frequently too expensive or unnecessarily complex for these businesses.

The goal of this platform is to provide a lightweight subscription management solution focused on simplicity, automation, and operational efficiency.

---

## Project Objectives

This project aims to demonstrate the following architectural concepts:

* Multi-Tenant SaaS Architecture
* Tenant Data Isolation
* Subscription Lifecycle Management
* Payment Gateway Integration
* Event-Driven Webhook Processing
* Automated Renewal Workflows
* Stateless Authentication
* Cloud-Native Application Design
* Scalable Backend Architecture
* Domain-Driven Design Principles

---

## Core Capabilities

### Tenant Management

Merchants can:

* Register on the platform
* Authenticate using JWT
* Manage subscription offerings
* Manage customers
* Monitor subscription activity

### Plan Management

Merchants can:

* Create subscription plans
* Configure pricing
* Configure billing intervals
* Activate or deactivate plans

Example:

| Plan                 | Price | Billing Cycle |
| -------------------- | ----- | ------------- |
| Monthly Membership   | ₹999  | Monthly       |
| Quarterly Membership | ₹2499 | Quarterly     |
| Annual Membership    | ₹8999 | Yearly        |

### Customer Management

Merchants can:

* Create customers
* View customer subscriptions
* Track subscription status
* Monitor payment activity

### Subscription Management

The platform supports:

* Subscription Creation
* Subscription Activation
* Subscription Cancellation
* Renewal Processing
* Subscription State Management

### Payment Processing

The platform integrates with payment providers to:

* Initiate subscription payments
* Process payment confirmations
* Receive webhook events
* Update subscription status

### Renewal Engine

Automated renewal workflows support:

* Scheduled renewals
* Payment retries
* Subscription status transitions
* Failure handling

---

## Subscription Lifecycle

The subscription state machine follows the lifecycle below:

PENDING

↓

ACTIVE

↓

PAST_DUE

↓

CANCELLED

State transitions are driven by payment events and renewal outcomes.

---

## Multi-Tenant Architecture

The platform follows a shared database multi-tenant architecture.

Each merchant is represented as a Tenant.

Every business object is associated with a tenant identifier to ensure strict tenant isolation.

Tenant A

* Plans
* Customers
* Subscriptions

Tenant B

* Plans
* Customers
* Subscriptions

Data access is enforced through tenant-aware authorization and query filtering.

---

## Technology Stack

### Backend

* Java 21
* Quarkus
* REST APIs
* JWT Authentication

### Database

* PostgreSQL

### Infrastructure

* Docker
* Docker Compose

### Payment Integration

* Payment Provider Sandbox
* Webhook Processing

---

## Architectural Focus Areas

This project intentionally focuses on demonstrating:

### Domain Modeling

Key entities:

* Tenant
* User
* Plan
* Customer
* Subscription
* Payment
* WebhookEvent

### Security

* JWT Authentication
* Tenant Context Enforcement
* Request Authorization

### Reliability

* Idempotent Webhook Processing
* Transactional Consistency
* Retry Handling

### Scalability

* Stateless Services
* Horizontal Scaling Readiness
* Cloud-Native Deployment Model

---

## MVP Scope

The initial MVP includes:

### Included

* Tenant Registration
* Authentication
* Plan Management
* Customer Management
* Subscription Creation
* Payment Integration
* Webhook Processing
* Automated Renewals
* Subscription Dashboard

### Excluded

The following features are intentionally excluded from the MVP:

* Product Catalog Management
* Coupons and Promotions
* Tax Management
* Accounting Features
* CRM Capabilities
* Inventory Management
* Usage-Based Billing
* Multi-Currency Support
* Refund Management
* Advanced Analytics

---

## Project Structure

/docs

* Architecture Documentation
* Sequence Diagrams
* Domain Model
* Database Design
* API Specifications

/backend

* Quarkus Services
* Domain Layer
* Application Layer
* Infrastructure Layer

/frontend

* Merchant Portal

/docker

* Local Development Environment

---

## Success Criteria

A merchant should be able to:

1. Register as a tenant.
2. Create a subscription plan.
3. Create a customer.
4. Create a subscription.
5. Process a payment.
6. Activate the subscription.
7. Observe automated renewals.
8. Track subscription status changes.

Successful execution of the above workflow validates the core business capabilities and architectural goals of the platform.

---

## Future Enhancements

Potential future extensions include:

* Product Management
* Coupon Engine
* Tax Engine
* Usage-Based Billing
* Event Streaming
* Kafka Integration
* Kubernetes Deployment
* Database Per Tenant Isolation Model
* Reporting and Analytics
* White-Label Support
