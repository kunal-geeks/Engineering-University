# Exercise Solutions — Lesson 29

## E-commerce aggregates

- **Order** (root): line items, status — invariant: total matches sum of lines
- **Product** (catalog context): separate aggregate — reference by ID only
- **Payment**: separate bounded context — integrate via events

## Context map integration styles

- Catalog → Checkout: customer/supplier (API)
- Checkout → Payment: conformist or ACL
- Checkout → Analytics: published language (events)
