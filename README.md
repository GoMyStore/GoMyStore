
# GO My Store

## Overview

GO My Store is a multi-vendor marketplace where:
- Vendors can create customizable stores, list products/services, and interact with customers.
- Customers can search across all stores, purchase using credits, and communicate with vendors.
- Staff moderates content, verifies vendors/products, and handles disputes.

The platform supports physical products, digital goods, and services, with a unified product database and credit-based payment system.

## Resources

- [Clickup](https://app.clickup.com/9015365228/v/b/s/90153887936)
- [Eraser](https://app.eraser.io/workspace/1WcBh4v2AxsLI7UIX2mG)

## Vendors
### Store Management

- Choose from pre-built templates (minimalist, modern, boutique, etc.)
- Drag-and-drop UI builder (headers, product grids, banners, testimonials)
- Custom CSS/JS injection for advanced branding
- Logo, cover image, color scheme customization
### Product Management
- Add products from the global database (with custom pricing, descriptions, variants)
- Request new products (fills a form for staff approval)
- Bulk import/export (CSV/Excel)
- Inventory & stock alerts
- Digital product delivery (auto-send download links after purchase)
- Service bookings (calendar integration for appointments)

### Marketing and Sales
- Create discounts, flash sales, coupon codes
- Affiliate/referral program (optional)
- Email/SMS notifications for restocks, price drops

### Communication & Support
- Live chat with customers, staff, and other vendors
- Complaint & dispute tickets (track resolution status)
- FAQ & automated responses for common queries

### Finance & Payouts
- Deposit/withdraw credits (converted to real money)
- Revenue dashboard (sales analytics, pending payouts)
- Withdrawal methods (bank transfer, PayPal, crypto)

## Customers

### Search and Discovery

- Global product search (filters: price, ratings, location, availability)
- Vendor search (filter by verified status, category, reviews)
- AI-powered recommendations ("Customers also bought...")

### Wishlist & Alerts

- Save favorites in wishlists (public/private)
- Watchlist (get alerts for price drops/restocks)
- "Notify me when available" for out-of-stock items

### Checkout & Payments

- Buy credits (bank transfer, card, PayPal, crypto)
- Order history & tracking
- Split payments (partial credit + card)
- One-click checkout (saved payment methods)

### Community & Support

- DM vendors (for product inquiries)
- Public Q&A section on product pages
- Leave reviews & ratings (with photo/video uploads)
- Report fake listings/scams

## Moderators
### Vendor & Product Approval

- Verify vendors (ID check, business proof)
- Approve/reject new products (ensure compliance)
- Remove fake/scam listings

###  Dispute Resolution

- Handle complaints (refunds, bans, warnings)
- Mediate vendor-customer conflicts
- Fraud detection (AI flags suspicious transactions)

### Platform Analytics

- Sales & traffic reports
- Vendor performance metrics
- Customer behavior insights

### Credit & Payout Management

- Monitor credit flow (prevent fraud)
- Process withdrawals (manual/auto approval)
- Set credit conversion rates

## Architecture
### Services

#### User Service
* **Database:** PostgreSQL (Users)
* **Responsibilities:** Auth, profiles, roles (customer/vendor/staff)
* **Interaction Pattern:** REST API → Auth events (NATS)

#### Storefront Service
* **Database:** MongoDB (Stores)
* **Responsibilities:** Vendor store configs, themes, components
* **Interaction Pattern:** REST API + SSE (real-time updates)

#### Product Catalog
* **Database:** MongoDB (Products)
* **Responsibilities:** Global product DB + vendor customizations
* **Interaction Pattern:** GraphQL API → Catalog events (NATS)

#### Inventory Service
* **Database:** PostgreSQL (Inventory)
* **Responsibilities:** Stock levels, reservations, alerts
* **Interaction Pattern:** Event-driven (NATS)

#### Search Service
* **Database:** mellisearch
* **Responsibilities:** Indexed products/vendors for fast queries
* **Interaction Pattern:** Async indexing (NATS)

#### Order Service
* **Database:** PostgreSQL (Orders)
* **Responsibilities:** Checkout, order lifecycle, credit deduction
* **Interaction Pattern:** CQRS + Saga Pattern (NATS)

#### Credit Ledger
* **Database:** PostgreSQL (Ledger)
* **Responsibilities:** Double-entry credit tracking, withdrawals
* **Interaction Pattern:** Transactional API (gRPC)

#### Chat Service
* **Database:** Redis (Messages)
* **Responsibilities:** Real-time DMs, support tickets
* **Interaction Pattern:** WebSockets + Redis Pub/Sub

#### Notification Service
* **Database:** MongoDB (Logs)
* **Responsibilities:** Email/SMS/push alerts
* **Interaction Pattern:** Subscribes to NATS events

#### Reporting Service
* **Database:** ClickHouse
* **Responsibilities:** Analytics, dashboards
* **Interaction Pattern:** Subscribes to NATS + DB CDC
