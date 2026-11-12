# Data dictionary

All files are synthetic and were generated for this hackathon. They contain no real customers, accounts or transactions. The country codes `XA` to `XD` are fictional.

| File | Rows | One row per |
|---|---:|---|
| [`transactions.csv`](transactions.csv) | 3,079 | Transaction record, October 2026 |
| [`accounts.csv`](accounts.csv) | 150 | Account |
| [`high_risk_countries.csv`](high_risk_countries.csv) | 4 | High-risk country |

Every file is a UTF-8, comma-separated CSV with a header row.

## transactions.csv

| Column | Type | Example | Notes |
|---|---|---|---|
| `transaction_id` | Text | `TXN-002741` | Identifies the transaction. See [Things to know](#things-to-know). |
| `timestamp` | ISO 8601 date and time with offset | `2026-10-28T14:53:56+03:00` | Kuwait time (UTC+03:00, no daylight saving time), with a few exceptions. See [Things to know](#things-to-know). |
| `account_id` | Text | `ACC-100055` | The bank's own account. Joins to `accounts.csv`. |
| `direction` | `DEBIT` or `CREDIT` | `DEBIT` | `DEBIT` is money out of the account; `CREDIT` is money in. |
| `transaction_type` | Text | `TRANSFER_INTERNATIONAL` | `CARD_PAYMENT`, `CASH_DEPOSIT`, `CASH_WITHDRAWAL`, `TRANSFER_DOMESTIC` or `TRANSFER_INTERNATIONAL` |
| `channel` | Text | `ONLINE` | `ATM`, `BRANCH`, `MOBILE`, `ONLINE` or `POS` |
| `amount` | Decimal | `2540.72` | The amount in `currency`. |
| `currency` | ISO 4217 code | `USD` | Always `KWD` for cash, card and domestic transactions. |
| `amount_kwd` | Decimal, 3 places | `780.001` | The amount in Kuwaiti dinars, at a fixed rate for the month. Use this column for `LARGE_AMOUNT`. |
| `counterparty_id` | Text | `CP-801533` | `MER-…` for card merchants and `CASH` for cash transactions. |
| `counterparty_country` | ISO 3166-1 alpha-2 code | `AE` | The beneficiary's country for debits and the sender's country for credits. Always `KW` for cash. |

## accounts.csv

| Column | Type | Example | Notes |
|---|---|---|---|
| `account_id` | Text | `ACC-100001` | Unique key. |
| `customer_id` | Text | `CUS-74044` | The account holder. |
| `customer_segment` | Text | `SME` | `RETAIL`, `SME` or `CORPORATE`. Useful for the per-segment thresholds stretch goal. |
| `home_branch` | Text | `Jahra` | The branch that manages the account. |
| `opened_on` | Date | `2020-10-24` | The date the account was opened. |

## high_risk_countries.csv

| Column | Type | Example | Notes |
|---|---|---|---|
| `country_code` | ISO 3166-1 alpha-2 code | `XB` | Compare with `counterparty_country`. |
| `country_name` | Text | `Belvaria` | Fictional. |
| `reason` | Text | `Fictional: under increased monitoring` | Why the country is on the list. |

In a real bank, this list comes from the compliance team, for example based on FATF publications.

## Things to know

Real transaction feeds are messy, so this one is too. The transactions file deliberately contains a small number of:

- **Duplicate records:** the same `transaction_id` appears more than once.
- **Missing values:** some rows have no `counterparty_country`, and one row has no amount.
- **Inconsistent country codes:** a few codes use a different case or have extra spaces.
- **Late-arriving records:** the last few rows of the file are out of time order.
- **Mixed time zones:** a few records from the international payments feed are in UTC (ending in `Z`) instead of Kuwait time.

These issues can change which transactions get flagged. Decide how your app handles each one, record the decision in your plan, and test it.

KWD has 3 decimal places (1 KWD = 1,000 fils). If your stack has a decimal type for money, use it.
