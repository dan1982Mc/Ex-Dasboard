# Household finances dashboard

A local-first household finance analysis app for Revolut CSV exports. It is separate from the existing Energy use dashboard.

## Start

1. Download or clone this repository to your computer.
2. Open index.html in a modern browser (Chrome, Edge or Firefox).
3. Select all available CSV exports under Dan's account and Anca's account.
4. Import the files. You can import later statements at any time; exact re-imported rows are skipped.
5. Use the Overview, Comparisons, Projects, Rules, Transactions and Savings forecast sections.

No bank login, API key, server, external JavaScript library or internet connection is required after the files are on your computer.

For a detailed explanation of CSV parsing, classification priority, treatment, incidentals, merchant grouping, formulas, savings reconstruction and current assumptions, read [DATA_FLOW_AND_CALCULATIONS.txt](DATA_FLOW_AND_CALCULATIONS.txt).

For a detailed explanation of CSV parsing, classification priority, treatment, incidentals, merchant grouping, formulas, savings reconstruction and current assumptions, read [DATA_FLOW_AND_CALCULATIONS.txt](DATA_FLOW_AND_CALCULATIONS.txt).

## Local data and privacy

- Transactions are stored in the browser's IndexedDB database on that device.
- Classification rules and forecast settings are stored in local browser storage.
- The app makes no network requests and does not fetch CSVs from GitHub.
- Use Backup to export a JSON file containing transactions, rules, overrides and forecast settings. Keep that backup private.
- Use Restore backup to restore or merge a backup.
- Clearing browser site data can remove the local database; export a backup regularly.

This repository contains the dashboard application code. Bank CSV files are selected locally by the user and are not uploaded by the app.

## Import format

The importer expects the standard Revolut statement columns:
Type, Product, Started Date, Completed Date, Description, Amount, Fee, Currency, State, Balance.

Completed Date is used when present; Started Date is used when Completed Date is blank. Non-completed rows remain visible but are excluded from totals. The application calculates cash effect as Amount minus Fee.

## Classification model

- Card payments are treated as expenses and get a conservative merchant-category suggestion.
- Card refunds reduce expenses in the merchant's category.
- Known employer payments, SVB benefits and Belastingdienst refunds are recognised as income.
- Known Dan↔Anca and named Revolut pocket movements are treated as internal transfers.
- Recognised investment destinations are tracked separately from household consumption.
- Unrecognised transfers and incoming top-ups are sent to Needs review, not guessed as income or expenses.
- Custom rules can match account, description text, transaction type, date range and amount range. They can set a category, project, treatment or expense nature.
- Merchant categories can be managed centrally and apply to the merchant's historical transactions and future imports.
- Individual transaction overrides take precedence over custom and built-in rules.

Review the initial classifications before relying on totals. In particular, confirm ambiguous transfer descriptions and any new merchants.

## Version 1 scope and limitations

Included:
- Multi-file Dan and Anca CSV import and exact duplicate detection.
- Local IndexedDB storage, JSON backup/restore and filtered CSV export.
- Monthly income, expenses, investments and net cash-flow overview.
- Same-month previous-year and available previous-years average comparisons by category.
- Same-day cutoff for the latest incomplete imported month.
- Custom merchant / date / account / type / amount rules and project labels.
- Manual transaction classification overrides.
- Searchable, sortable merchant management with persistent merchant-level category assignments.
- One-time / limited-expense candidates based on merchant frequency and configurable thresholds.
- Historical 26th–25th salary-cycle reconstruction from imported EUR running balances, with savings potential, actual cash balance movement, recognised investment contributions and annual comparisons.\n- Pre-salary savings estimate using balances entered by the user, days to salary, recent 90-day spending, known payments and a minimum reserve.

Not yet included:
- Automatic bank balance retrieval or Revolut API integration.
- Automatic pairing of every possible internal transfer; unknown transfers remain in review.
- Fully automatic recurring-payment calendar with due-date prediction.
- Multi-condition rule groups such as nested AND/OR logic, rule testing and bulk undo.
- A natural-language question box or an embedded AI assistant. Use the comparison filters and exports, then ask ChatGPT to investigate more complex questions.
- Budget targets, category caps, multi-scenario savings plans and asset / net-worth tracking.
- Automatic transaction matching between refunds and original purchases.
- Cloud sync between browsers or devices.

## Important

This is a decision-support tool, not accounting or tax software. Imported data can be incomplete, bank descriptions can be ambiguous, and automatic classifications are suggestions. Confirm review items and check the date coverage before acting on a result.


## Stage 3 · historical savings reconstruction\n\n- Salary cycles run from the 26th through the 25th and are grouped into the year in which the cycle ends.\n- Opening balances use the latest imported EUR Balance snapshot before the 26th; closing balances use the latest snapshot on or before the 25th. Snapshots are grouped by account and Revolut Product.\n- The first salary cycle can be partial when imported history starts after the 26th; that partial history window is labelled and excluded from annual comparisons. Gaps inside otherwise imported date ranges cannot be detected automatically, so check that imported CSVs cover the full period. A cycle is marked complete only when every EUR balance stream active by the cycle end has both an opening and a closing snapshot. Incomplete cycles still show classified savings potential, but are excluded from cash-retention totals. The current cycle appears as in progress with classified totals to date, not as a completed month.\n- Savings potential is classified income less net expenses. Actual cash retained is the change in reconstructed household EUR balances. Other movement reconciles the cash-balance change against classified potential savings and recognised investment transfers. Cash + invested adds recognised net investment transfers; it is a contribution measure, not a valuation of investment holdings or returns.\n- Non-EUR balances are excluded because the export does not provide a consistent conversion basis in this dashboard.\n- The local app does not upload account balances or transaction data.\n\n## Recent interface improvements

- The overview places automatic insights first, followed by household filters, financial overview, data import and the application introduction.
- Comparison columns can be sorted by category or value. Change versus last year shows the euro difference and percentage separately.
- Merchant management groups spend by normalised merchant, supports search and category/period filtering, and applies merchant category choices to past and future transactions.


Recent refinements: AAG Beleggingen is now classified as Housing & mortgage (not Investment); TAROM is Travel; Dunea, Budget Energie, Greenchoice, Engie Nederland and Innova Energie are Utilities & telecom; CJIB, Stichting de Haagse Scholen, Voogd Voogd Verzekering and ABN AMRO Schadev NV have updated categories. Period selection is shared across Overview, Comparisons, Merchant management and Transactions, with This month, Previous month, Last 6 months, YTD and Custom presets.
