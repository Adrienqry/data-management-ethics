{% docs gross_revenue %}
**Gross revenue (EUR)** — total amount paid by the customer for an order, VAT included, shipping included, before refunds.
Owner: Head of Sales · Steward: Karim Benali · Status: approved (2026-11-10)
{% enddocs %}

{# TODO (Exercise 3): add the terms net_revenue, active_customer, churned_customer, order, customer #}

{% docs active_customer %}
**Active customer** — A customer account with at least one non-cancelled order during the 12 calendar months ending on the reporting date.

**Calculation** — For each customer_id, require at least one fct_orders record with a known status other than cancelled and order_date between reporting_date minus 12 months and reporting_date, inclusive.

**Example** — On 2026-11-25, a customer's shipped order dated 2026-03-10 qualifies. A cancelled order alone does not qualify.

**Governance** — Owner CRM Manager. Steward CRM Data Steward. Approved (2026-11-25).

**Usage notes** — Approved in the lab role-play. Sales keeps a separately named Recently active customer (6 months) metric, using the same eligibility rule with a 6-month window. dim_customers.last_order_date includes all statuses and is insufficient on its own.
{% enddocs %}

{% docs net_revenue %}
**Net revenue** — Order revenue in euros after recorded refunds and excluding VAT, with shipping fees included.

**Calculation** — Per order, ROUND((gross_revenue_eur - refunded_eur) / 1.20, 2) at the 20% VAT rate used in the lab. Sum the per-order values for a report.

**Example** — A EUR 120.00 order including shipping and VAT, with a EUR 24.00 refund, yields EUR 80.00 net revenue.

**Governance** — Owner Paul Girard (Head of Finance). Steward Finance Data Steward. Approved (2026-11-25).

**Usage notes** — Approved in the lab role-play. Sales can track a separate amount after refunds including VAT for commissions. The current model uses recorded order totals.
{% enddocs %}

{% docs churned_customer %}
**Churned customer** — A previously purchasing customer account with no non-cancelled order during the 12 calendar months ending on the reporting date.

**Calculation** — Require at least one dated non-cancelled order before the 12-month window and no dated non-cancelled order during that window, including the reporting date.

**Example** — On 2026-11-25, an account whose last non-cancelled order was on 2025-08-15 meets the proposed rule.

**Governance** — Owner CRM Manager. Steward CRM Data Steward. Draft (2026-11-25).

**Usage notes** — Draft definition recorded on the lab date for owner review. Excludes accounts with no qualifying purchase history. The churn model owner should confirm the window.
{% enddocs %}

{% docs customer %}
**Customer** — A registered ShopFast account identified by a distinct customer_id.

**Calculation** — Count distinct customer_id values from stg_customers. Each account is counted once.

**Example** — Two accounts sharing an email address count as two customer accounts.

**Governance** — Owner CRM Manager. Steward CRM Data Steward. Draft (2026-11-25).

**Usage notes** — Draft definition recorded on the lab date. The model tracks account identity and can associate multiple accounts with the same person.
{% enddocs %}

{% docs order %}
**Order** — A ShopFast purchase record identified by one order_id that can contain multiple product lines and payment events.

**Calculation** — Count distinct order_id values from stg_orders. Apply status filters separately for the metric being calculated.

**Example** — An order containing three product lines counts as one order.

**Governance** — Owner Sara Martin (Head of Sales). Steward Sales Data Steward. Draft (2026-11-25).

**Usage notes** — Draft definition recorded on the lab date. Cancelled orders remain records and are excluded from the active-customer calculation.
{% enddocs %}
