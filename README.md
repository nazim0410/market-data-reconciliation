# Financial Trade Reconciliation & Reporting Pipeline
> 🚧 In Progress

A personal project I'm building to simulate the kind of trade reconciliation and financial reporting work I did professionally — ingesting multi-source market data, applying SQL-based reconciliation logic to detect position breaks and P&L discrepancies, and surfacing results through a Tableau dashboard.

The goal is a front-to-back pipeline that mirrors real capital markets reporting environments, backed by proper BA documentation (data mapping, business rules) the way it would be done on an actual Finance team.

---

## What it does

- Pulls trade and pricing data from Yahoo Finance API and SEC EDGAR into AWS S3
- Applies reconciliation logic to detect daily P&L breaks and position mismatches by instrument
- Documents data mapping rules and business logic in BA-style functional specs
- Surfaces P&L trends, break reports, and position summaries in a Tableau Public dashboard

---

## Stack
`Python` `SQL` `AWS S3` `PostgreSQL` `Tableau` `Apache Airflow`

---

## Progress

- [x] Architecture design and BA documentation
- [ ] Data ingestion (Yahoo Finance / SEC EDGAR → S3)
- [ ] Reconciliation engine and break detection
- [ ] P&L calculation by instrument and asset class
- [ ] Tableau dashboard
- [ ] Automated scheduling

---

## Docs
- [Data Mapping](docs/data_mapping.md)
- [Business Rules](docs/business_rules.md)

---

**Mohammad Nazim Rehman** · [github.com/nazim0410](https://github.com/nazim0410)
