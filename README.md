# KYC / AML Compliance Operations Simulation (Excel)

A formula-driven Excel workbook that simulates the daily work of a **Client Data / KYC Quality Analyst**: reviewing client records for errors, monitoring transactions for AML red flags, tracking CTR obligations, reconciling two data sources, escalating with written risk summaries, and resolving client issues within SLA.

> **Note:** All clients, transactions and amounts are **synthetic**. Thresholds, country scores and monitoring rules are **illustrative** and not legal advice. This is a self-built simulation to demonstrate method and skills, not real work history.

**No code, no macros.** Everything is built with standard Excel formulas (`SUMIFS`, `COUNTIFS`, `INDEX/MATCH`, `IFERROR`, `EDATE`, `NETWORKDAYS`), conditional formatting and drop-down validation, so every result can be traced and explained cell by cell.

![Dashboard](docs/dashboard.png)

## Skills demonstrated

| Skill | Where to look |
|---|---|
| KYC record review and quality checks | `Client_Master`: document completeness, risk score vs assigned rating, review due dates, duplicates, date logic, blank fields |
| AML transaction monitoring | `Alert_Monitoring`: six scenario rules (structuring, cash vs profile, high-risk jurisdiction, PEP, pass-through, sanctions hit) |
| CTR reporting | `CTR_Log`: same-day cash aggregation per client and direction, threshold test, 15-day filing deadline, timeliness tracking |
| Data reconciliation | `Recon_Client` (CRM vs core banking) and `Recon_Txn` (ledger vs bank statement) with a control-total proof showing zero unexplained difference |
| Escalation procedures | `Escalation_Matrix` (levels, owners, SLAs) and `Escalation_Log` (10 cases with analyst risk summaries and SLA tracking) |
| Customer issue resolution | `Customer_Issues`: 14 tickets with priority-based SLAs and resolution notes |
| Reporting | `Dashboard`: KPIs and charts for every process |

## How it works

1. **Assumptions** holds every threshold (CTR limit, structuring band, SLAs, risk weights). Change one and the whole workbook recalculates.
2. **Client_Master** scores each client: `Country + Industry + Entity type + PEP weight` gives a calculated rating, which is compared with the assigned rating. Review due dates come from the rating's review cycle.
3. **Transactions** adds helper columns (cash flag, near-threshold flag, daily cash total, country score) that feed `CTR_Log` and `Alert_Monitoring`.
4. **Alert_Monitoring** applies rules R1 to R6 and returns **ESCALATE**, **Analyst Review** or **No Action**.
5. Escalated items are documented in **Escalation_Log** with a risk summary, level, SLA and outcome.

## Planted scenarios (what a reviewer should find)

- **CTR aggregation:** two same-day deposits ($6,200 + $5,100) for one client combine to exceed $10,000; one CTR from a jeweller is past its filing deadline.
- **Structuring:** four cash deposits of $9,200 to $9,800 within five days, each below the threshold.
- **High-risk jurisdiction:** $105,000 wired to a score-3 country by a client rated lower than the model suggests.
- **PEP and pass-through:** a PEP with $240,000 of wires; $250,000 in and $246,500 out within three days.
- **Sanctions and onboarding:** an unresolved potential match; a wire received before KYC was complete.
- **Data breaks:** name, country and rating mismatches; a client missing from core banking; an active account with no KYC record; a duplicate client; a date-logic error; 3 amount breaks, 2 missing items and 1 duplicate in the bank statement.

## Key results (as of 26-Oct-2026)

| Area | Result |
|---|---|
| KYC files complete | 35 of 40 (88%); 6 periodic reviews overdue; 4 rating mismatches |
| CTRs | 5 client-days above threshold; 3 filed on time, 1 pending, 1 late |
| Monitoring | 4 ESCALATE, 2 Analyst Review, 34 No Action |
| Escalations | 10 cases, 8 open, 1 SLA breach |
| Client recon | 35 reconciled, 4 field breaks, 2 missing records, 6 active accounts with incomplete or no KYC |
| Transaction recon | 107 matched, 7 breaks, **0.00 unexplained difference** |
| Customer issues | 14 logged, 9 open, 79% within SLA |

## How to use

1. Download `KYC_AML_Compliance_Operations_Project.xlsx` and open it in Excel (or Google Sheets / LibreOffice).
2. Start at `README` and `Dashboard`, then follow any exception to its sheet.
3. Try changing the CTR threshold in `Assumptions`, or add a transaction, and watch the flags update.

Colour legend: navy header = input data, grey header = formula column (don't overwrite), red = exception, amber = attention, green = OK.

## Limitations

- Synthetic data and simplified rules; real programmes use more scenarios, customer segmentation and system-based screening.
- Country and industry scores are illustrative, not an official FATF or regulatory list.
- The CTR logic follows the US $10,000 / 15-day convention; adjust `Assumptions` for other regimes.
