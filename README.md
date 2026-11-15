# FleetFlow 2.0 — Vehicle Rental Management

## Features
- Dashboard with counts, active rentals, overdue bookings and **recorded payment revenue**.
- Customer and vehicle create/read/update/delete; search.
- Rental booking, edit, start, return, cancel and permitted delete.
- Booking-date overlap prevention (locks the vehicle row during booking writes).
- Payments (cash, UPI, card, bank transfer, refund), deletion and balance display.
- Manual location/tracking entries linked to a vehicle and optionally a rental.
- Monthly payment reports, rental status counts and top vehicles.
- Responsive interface retaining the FleetFlow purple/navy styling.

## Requirements
Node.js 18+ and MySQL 8+ (or compatible MySQL). The database account needs CREATE TABLE, SELECT, INSERT, UPDATE and DELETE privileges.

## Setup
1. Create a MySQL database: `CREATE DATABASE fleetflow CHARACTER SET utf8mb4;`
2. Copy `.env.example` to `.env` and set DB_HOST, DB_PORT, DB_USER, DB_PASSWORD, DB_NAME. Do not commit `.env`.
3. Run `npm install` then `npm start`.
4. Visit `http://localhost:3000` and verify `http://localhost:3000/api/db-health`.

On startup, FleetFlow creates `ff_customers`, `ff_vehicles`, `ff_rentals`, `ff_payments`, `ff_tracking` if they do not exist. The dashboard reads these tables instead of the original mismatched legacy schema. **Existing `customer`, `vehicle`, `rental` records are not automatically migrated**: the original tables remain untouched. Export/back up your original DB and migrate records carefully after reviewing column mappings and IDs. Do not delete old tables until verified.

## Usage
Add customers and vehicles first, then create a booking. Start and return rentals with the action buttons. Add payments separately; recorded revenue is not the same as total booked value. Add tracking entries manually; this is **not GPS tracking**.

## Constraints / production considerations
- This is a local/trusted-environment management prototype. **No authentication or user roles are implemented. Do not expose the server publicly or store sensitive customer data in production without adding authentication, authorization, HTTPS, backups, CSRF/security protections and audit logging.**
- Deletion is blocked by foreign keys when records have linked history. Prefer inactive vehicle status instead of deleting historical vehicles.
- Payment records are manually entered, not connected to a payment gateway. Refund is an accounting entry, not a real transfer.
- Returns use today's date. Vehicle availability is based on bookings overlapping today's date and base vehicle status.
- No automated tests against a real MySQL server were run in the build environment. Verify schema creation and workflows with your database before use.
