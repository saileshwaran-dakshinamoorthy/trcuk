# Truckwise mock application

Run `npm run dev` in `ui`. Build with `npm run build`; run domain checks with `npm test` (Node 22.6+).

The header has demo company and role selectors. No login or backend is connected. Data and receipt attachments persist in localStorage on the current browser. Super Admin can reset them from Global settings. All identities and records are fictional. The sample reference date is 12 November 2026.

## Roles

| Role | Capabilities |
| --- | --- |
| Super Admin | Dedicated navigation for Company onboarding, Subscription plans, Support and Global settings. Create company workspaces with owner contact and initial subscription; manage access, plans and tickets. |
| Admin | Manage the selected company, approve or reject support expenses, manage drivers and trips, see cross-company driver history. |
| Support | Manage the selected company's trucks, services and trips, add drivers, submit expenses and receipts for approval, rate completed trips. Global driver ratings and history are reserved for admins. |

Company switching is an intentional demo control. These client permissions are illustrative and do not provide security. Production must enforce role and tenant isolation on the server.

## Try the flows

1. Select Support and Sri Balaji Transport. Open Expenses, add a Fuel or Toll expense, and optionally attach a JPG, PNG or PDF under 750 KB. Switch to Admin to approve or reject it. Dashboard and expense totals update; rejected amounts are excluded.
2. Search Drivers for `9000001001` or `TN0120200001001` as Admin. Open Suresh Rajan's history to see trips and feedback from both Sri Balaji Transport and Deccan Logistics.
3. Plan a trip using the available TN 10 KL 5544 and Karthik Selvam, select full time or part time, and use 12 November 2026 as departure. Open Trips to start it, then Live tracking to run simulated GPS. Complete it in Trips and submit a rating, issue and comment.
4. Inspect truck RC details, insurance dates and service history. Add a service record to update the next service date. Service records do not create ledger entries automatically, preventing duplicate costs with separately uploaded invoices.
5. Select Super Admin and open Company onboarding to create or suspend a workspace. Onboarding records the company, owner contact, plan and trial end date together. Switch to Admin or Support to see that workspace. Suspended workspaces are read only.
6. In Subscription plans, edit the catalogue or manage a company's plan, status and renewal date. Plans cannot be downgraded below existing fleet usage, and assigned/default plans cannot be archived. Truck creation respects the assigned plan's fleet limit. Support seat capacity is descriptive; employee provisioning is not implemented.
7. In Support, create a mock company ticket, filter requests, reply and change status or priority. Replies stay in this browser and send no email.
8. In Global settings, change the platform name, support email, default onboarding plan, trial duration, onboarding availability and announcement. Maintenance mode makes company operations read only while Super Admin remains available. Defaults affect future onboarding, not existing subscriptions.

In Company onboarding, **View company** opens a wide, read-only detail view with Overview, Trucks, Drivers, Trips, Expenses and Support sections. Truck details include RC, insurance, odometer and expandable service history. Drivers are associated through the company's trips, including past and scheduled work; their company-specific trip history and feedback are expandable. Unassigned drivers in the global directory are not treated as company employees. Trip details include assigned truck/driver, employment type, revenue, spend, feedback and simulated location for in-transit trips. Expense receipts can be downloaded. All operational records and support conversations in this view belong to the selected company.

Assignments reserve a truck and driver while a trip is scheduled or in transit. Cross-company driver conflicts are blocked. Expired licences and insurance block departure. Trip completion releases assignments and increments the truck odometer. Expenses support filters by category, date, truck, text and approval status. The dashboard includes pending expenses; revenue is shown for completed trips only. Cost-per-km uses planned distances, not measured GPS distances.

## Client business flow

- Admin and Support have a Clients menu for company-specific contact details, billing address, optional GSTIN, payment terms, route defaults, goods, quantities, load weight, handling instructions and notes.
- Use Plan a trip to select an existing client or choose Create a new client. New clients and their first trip save together. Pick a client deal to prefill cargo, route and agreed freight, or create a direct trip agreement. Client defaults can be overridden per trip. Weight in kg/tonnes is calculated from quantity; boxes/items require a separate total weight. Loads exceeding the truck's capacity are blocked.
- View client includes Profile, Deals, Trips and Transactions. Quotes do not add to balances; linked trip freight creates a booking entry. Booked totals include scheduled trips and are separate from the dashboard's completed-trip revenue.
- Record payment from View client to log an advance or received payment against a trip, with date, method, reference and notes. Overpayments, duplicate company references and cross-company payments are blocked. No money is transferred. Transactions show chronological bookings, receipts and a running balance.
- Completed trips mark linked deals Completed. Client edits do not change past trip cargo snapshots. Trips with recorded payments cannot change clients/deals or reduce freight below received payments.
- Super Admin can inspect client profiles, deals, trips and transaction histories in View company → Clients. Existing browser data is migrated without deleting existing records; unknown legacy trips remain unlinked until edited.

## Integration boundaries

Real authentication, server validation, durable multi-user storage, uploaded RC/insurance documents, receipt object storage, notifications and mobile GPS ingestion remain backend/mobile work. The route view is a schematic with explicitly simulated positions, not a map service. Historical records are mock inputs rather than verified driver credentials.

Subscription billing and onboarding invitations are also simulated: no payment gateway, invoice delivery or authentication invitation is connected. Monthly recurring revenue counts only subscriptions marked Active. Trial, Past due and Cancelled records do not automatically change company access; manage access separately in Company onboarding. Existing browser data is retained when platform records are introduced.
