1. Keep `amount` in euros with two decimal places. Values sent in cents would be read as amounts 100 times too large in `stg_payments`.
2. Inflated refunds would reduce `fct_orders.net_revenue_eur`, distorting the Sales dashboard, the Finance report and lifetime revenue in `dim_customers`, which can affect churn predictions.
3. Notify Sara Martin for Sales, Paul Girard for Finance, the CRM Manager for `dim_customers` and the Data Science Lead for the churn model.
4. Add an ingestion test that rejects a batch when its median payment amount falls outside the proposed range of EUR 50 to EUR 500.
5. The Finance Manager, as payment data owner, approves any unit change and versions the data contract after affected consumers sign off.
