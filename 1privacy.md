# Privacy Policy

**Effective Date:** <<EFFECTIVE_DATE>>

This Privacy Policy describes how <<SHOP_LEGAL_NAME>> ("we", "us", "our")
collects, uses, stores, and shares information when you (the merchant)
use our autobody-shop CRM application (the "Service"), including its
optional integration with QuickBooks Online ("QuickBooks").

If you have questions about this policy, contact us at
**<<CONTACT_EMAIL>>**.

## 1. Who is the data controller

<<SHOP_LEGAL_NAME>> is the data controller for information you and your
end customers submit to the Service. You can reach us at
<<CONTACT_EMAIL>>.

## 2. What QuickBooks data we read

When you connect your QuickBooks Online company to the Service via
Intuit's OAuth 2.0 flow, you grant us access only under the single
`com.intuit.quickbooks.accounting` scope. Within that scope we read:

- **Your QuickBooks company id (realm id)** — provided by Intuit on
  the OAuth callback so we know which company to write into. We do not
  call any company-info read endpoint.
- **Chart of accounts** — your active Accounts (e.g. Income, Other
  Income, Other Current Liability), used to populate the
  account-mapping picker and to find a default Income account when we
  need to create a default Service item.
- **Items list** — your active Product/Service Items, used so you can
  map our invoice line types (Subtotal, Discount, Tax, CC Fee, Tip) to
  the correct Items in your books.
- **Customer list** — used only to look up an existing QuickBooks
  customer by display name before we create a new one, so we don't
  duplicate customers.

We do **not** read transactions, payroll, employees, vendors, bank
feeds, attachments, journal entries, or any other QuickBooks data
outside the accounts, items, and customer-name lookups described above.

## 3. What we write back to QuickBooks

When automatic sync is enabled, we write the following to your
QuickBooks Online company on your behalf:

- **Customers** — created when a matching display name does not already
  exist in your QuickBooks customer list.
- **Invoices** — one Invoice per settled Service transaction, with
  SalesItemLineDetail lines for Subtotal, Discount (as a discount line
  or negative item line, depending on your mapping), Tax, Credit-Card
  Fee, and Tip, using the Items you mapped in the QuickBooks settings
  panel.
- **Payments** — one Payment record per settled invoice, marked paid in
  full.
- **Reversals for voids and refunds** — when a previously-synced
  transaction is **voided** in the CRM, we delete the corresponding
  Invoice and Payment from your QuickBooks company. When a
  previously-synced transaction is **refunded**, we create a QuickBooks
  Refund Receipt for the same customer and amount (we do not delete the
  original Invoice or Payment, so the refund appears as a separate,
  audit-friendly record).
- **A default Service Item** — if you haven't mapped one and your
  realm doesn't already have a suitable item, we may create a single
  Service item named "Autobody CRM Services" linked to your realm's
  first Income account, so invoice lines have somewhere to land.

## 4. What CRM data we send to QuickBooks

For each synced transaction we send only what's needed to write the
invoice and payment described above:

- Customer display name (business name, personal name, or — for
  unattached payments — the payer name you typed at checkout). No
  other contact details (email, phone, address) are sent to QuickBooks
  today.
- Invoice line amounts: subtotal, discount, sales tax, credit-card fee,
  and tip, in cents.
- Payment amount, payment date, and a reference to the corresponding
  invoice.
- Tip recipient name (as a description on the tip line), when present.

## 5. What we explicitly do NOT send to QuickBooks

The following categories of data are **never** sent to QuickBooks
under any circumstance, regardless of how they are stored inside the
CRM:

- **Full credit-card numbers, CVVs, expiry dates, and
  magnetic-stripe data.** Card processing is performed end-to-end by
  Dejavoo iPos. The Service itself never receives or stores full card
  numbers — only a tokenized response (last-4, card brand, approval
  code).
- **Driver's license numbers.** Captured optionally at point of sale
  and stored in the CRM for the merchant's own fraud records, but
  never transmitted to QuickBooks.
- **Vehicle photos and other attachments** stored in the CRM's secure
  object storage.
- **The bodies of SMS or email messages exchanged with end customers**,
  along with their attachments.
- **Internal notes, parts-tracking detail, and estimate /
  work-order line-item descriptions.** QuickBooks invoices receive
  only the aggregated money lines (subtotal, discount, tax, CC fee,
  tip), not the individual line-item descriptions from the underlying
  estimate or work order.

## 6. How we store QuickBooks OAuth tokens

Your QuickBooks access token and refresh token are encrypted at rest
before they touch the database, using per-shop envelope encryption:
each shop has a randomly generated Data Encryption Key (DEK) wrapped by
a master Key Encryption Key (KEK) derived from our application secret
via HKDF-SHA256. Tokens are stored as ciphertext only; the database
never sees plaintext tokens. Every credential change is recorded in an
append-only audit log (actor, IP, user agent, and which fields
changed — never the secret value itself).

## 7. Subprocessors

To deliver the Service we share data with the following subprocessors,
each acting under our instructions and their own published terms:

- **PostgreSQL hosting provider** — primary database storage.
- **Google Cloud Storage** — secure object storage for photo uploads
  and email/MMS attachments.
- **Resend** — outbound email delivery.
- **Telnyx** — outbound and inbound SMS / MMS delivery.
- **Postmark** — inbound email webhook delivery.
- **Dejavoo iPos** — credit-card processing gateway.
- **Intuit / QuickBooks Online** — the integration this policy is about.
- **Cloudflare** — TLS termination and custom-domain routing.

## 8. Data retention

- **QuickBooks OAuth tokens** are deleted from our database immediately
  when you disconnect QuickBooks (either via the on/off switch in the
  QuickBooks settings panel or by clicking Disconnect). On disconnect we
  also make a best-effort revoke call to Intuit so the tokens are
  invalidated upstream.
- **QuickBooks sync logs** (per-transaction success/failure rows used
  for troubleshooting) are retained for troubleshooting purposes and
  are deleted on written request to <<CONTACT_EMAIL>>.
- **CRM transactional data** (estimates, work orders, transactions,
  customers, vehicles) is retained for as long as your account is active
  and as required by tax and accounting law. You may request export or
  deletion at any time as described below.

## 9. Your rights

You may at any time:

- **Disconnect QuickBooks** by clicking Disconnect on the QuickBooks
  settings panel. This removes our stored tokens immediately.
- **Pause sync without disconnecting** by toggling the automatic-sync
  switch off. Tokens stay in place; no new data flows to QuickBooks
  until you toggle it back on.
- **Request a data export** of your CRM data by emailing
  <<CONTACT_EMAIL>>.
- **Request data deletion** by emailing <<CONTACT_EMAIL>>. We will
  delete your data within a reasonable time, subject to legal retention
  obligations.

## 10. Sale of data and advertising

We do **not** sell your data, your end customers' data, or your
QuickBooks data to anyone. We do not share QuickBooks data with any
third party for that third party's own purposes. We do not use
QuickBooks data to target advertising.

## 11. Children's privacy

The Service is intended for use by automotive repair businesses and is
not directed to children. We do not knowingly collect personal
information from anyone under 13 (or the equivalent minimum age in your
jurisdiction).

## 12. Changes to this policy

We may update this Privacy Policy from time to time. Material changes
will be communicated by email to the address on file for your account
and by updating the **Effective Date** above. Continued use of the
Service after the new Effective Date constitutes acceptance of the
revised policy.

## 13. Contact

Questions, requests, or complaints about this policy or your data
should be sent to **<<CONTACT_EMAIL>>**.
