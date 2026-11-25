# CM 2 lab — Tue 24 Nov, 3:10 – 3:50

The former 1-hour CTP 1 is now the hands-on lab at the end of CM 2.

1. **Get READY (15 min):** `docker compose up -d` in `shopfast-kit`, activate the venv, start OpenRefine, run `python check_setup.py`.
2. **ShopFast data inventory (25 min, pairs):** fill `data_inventory_template.csv` for the 7 datasets (description, owner/steward/custodian, personal data, sensitivity, row count, 2+ issues). Use `explore_shopfast.sql` to query the database.
3. Push it to your project repo as `data_inventory_<group>.csv` — reused in CTP 3 and CTP 4.

## Part 2 — Data inventory completed on 24 November 2026

The deliverable covers all seven datasets. For each dataset, it records a description, the business domain, proposed roles, the presence of personal data, the sensitivity level, the row count, and at least two findings. [Download the inventory CSV](data_inventory_Adrien_Queroy.csv).

### Details to confirm

- Confirm the proposed data owner, steward, and custodian roles for ShopFast. The owner sets business rules, the steward oversees data quality and definitions, and the custodian operates the systems.
- The sources reviewed do not specify refresh frequencies, so all seven datasets are marked `To be confirmed`.
- The proposed classification for the full `products` file is `Internal` because it includes suppliers and active status. Public product names and prices alone could be classified as `Public` under the course rubric.

### Dataset summary

The row counts come from the CSV files in `shopfast-kit/data/` and include duplicate records.

| Dataset | Rows | Personal data | Sensitivity | Main findings |
|---|---:|:---:|---|---|
| `customers` | 1241 | Y | Restricted | Inconsistent country codes (`FR`/`fr`), mixed birth date formats, and an email address containing `@@`. |
| `products` | 52 | N | Internal | A duplicate SKU, inconsistent category labels, one missing price, and one negative price. |
| `orders` | 5207 | Y | Confidential | Seven duplicate IDs affecting 14 rows, 14 unmatched customer references, 11 missing dates, and nonstandard statuses. |
| `order_items` | 9193 | Y | Confidential | Eight quantities of zero or less and four unmatched product references. |
| `payments` | 4902 | Y | Confidential | Six payment amounts differ from the linked order totals, and four payment dates precede the linked order dates. |
| `support_tickets` | 422 | Y | Restricted | 53 tickets with an unmasked IBAN pattern, 62 notes with a birth date, and an observed mismatch between a ticket subject and its message. |
| `marketing_campaign_export` | 600 | Y | Confidential | The export lacks consent records, supporting evidence, and an extraction timestamp. |

### Method and limitations

- The full CSV files were checked for missing values, duplicates, category inconsistencies, amounts, dates, and matching identifiers across tables. The first five rows of each table were also inspected in PostgreSQL.
- Row counts for all seven datasets were verified against the CSV files. The setup check also confirmed 1,241 customer records in PostgreSQL. The lab's `explore_shopfast.sql` script includes a query to count rows in all seven database tables.
- Date checks used **24 November 2026** as the reference date. Five orders were dated after that cutoff.
- The six amount discrepancies and four date discrepancies between `payments` and `orders` only cover orders with unique IDs. Investigate both tables to determine the source of each discrepancy. Check payment amounts against business rules for scenarios such as partial payments.
- Whether `card_last4` is required depends on the payment method and the applicable business rules.
- The 53 IBAN matches were identified by their format. Their check digits were not validated. Under the course rubric, an IBAN in a message warrants a `Restricted` classification for the dataset. Birth dates in agent notes are another example of personal data recorded in free text.
- The combination of birth dates and addresses warrants a `Restricted` classification for `customers` under the course rubric.
- Records in `orders`, `order_items`, and `payments` can be linked to customers through identifiers, making them personal data.
- All **600 marketing contacts have `marketing_consent=true` in the current source data**. Checks found no duplicate customer IDs or normalized email addresses. Verifying consent as of the export date requires a timestamp and consent evidence from the source.
- The ShopFast data is fictional, as stated in the kit README. Sensitivity classifications follow the rubric on pages 25–26 of the course slides.

### Final checks and next steps

The CSV has 12 columns and seven data rows, with each field populated. Row counts match the source files, and each dataset has at least two findings. Entries that still need confirmation are marked explicitly.

Part 1 was also completed. PostgreSQL started, the Python 3.12 virtual environment was activated, the setup check reported `READY`, and OpenRefine opened successfully.

The inventory will serve as a reference for CTP 3 and CTP 4. Later exercises will address the issues recorded here.

