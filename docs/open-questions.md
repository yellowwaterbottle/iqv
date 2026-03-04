# Open Questions & Remaining Work

## Remaining Schedule Builds

### Round 2 — Working Capital (remaining)
| Item | Driver | Notes |
|------|--------|-------|
| Income taxes receivable | % revenue (placeholder) | Replace with tax schedule output once built |
| Deposits and other assets | % revenue (placeholder) | Replace with lease schedule output once built |
| Income taxes payable | % revenue (placeholder) | Replace with tax schedule output once built |
| Other current liabilities | % COGS | Pull footnote first — likely accrued comp, benefits, vendor accruals |
| Other liabilities | % revenue | Pull footnote first — may contain pension, LT deferred revenue |

### Round 3 — Complex Schedules
| Schedule | Key Footnote Data Needed |
|----------|--------------------------|
| PP&E & Depreciation | Gross PP&E, accumulated depreciation, average useful life by asset class |
| Intangibles & Amortization | Useful life by category (customer relationships, technology, trade names), vintage breakdown |
| Operating Lease | Weighted avg discount rate, weighted avg remaining lease term, future minimum lease payments schedule |
| Debt & Interest | All tranches with maturity, coupon rate, amortization schedule |
| Tax (Levered & Unlevered) | NOL carryforward by jurisdiction + expiry, interest carryforward (163j) balance, effective rate reconciliation by year |

### Round 4
| Schedule | Inputs Needed |
|----------|---------------|
| Acquisition Schedule | Historical PPA footnotes (goodwill %, intangibles % of purchase price per deal), going-forward M&A spend assumption |

---

## Deferred Decisions

- **Income taxes receivable/payable**: Currently % revenue placeholder. Should be replaced with tax schedule outputs once that schedule is built. Correct formula: cash taxes = accrued tax expense − Δ taxes payable + Δ taxes receivable.
- **Deposits and other assets**: Currently % revenue placeholder. Should link to operating lease ROU asset balance once lease schedule is built (lease security deposits typically correlate with lease portfolio size).
- **Other current liabilities / Other liabilities**: Need footnote breakdown before setting final drivers. May split into sub-components if material enough.
- **DTA/DTL BS projection**: Use NOL and interest carryforward schedule outputs × 21% for those specific DTA lines; use historical gross/net ratios to back into BS DTA asset and DTL liability split.

---

## Open Methodology Questions

- **Revolver in SCF**: Model as net (single line: proceeds − repayments) or gross (two separate lines)? Currently keeping two separate lines to match reported format.
- **FX effect in projections**: Zero out or model with a small assumption? Most models zero it — recommend zero for base case.
- **Acquisition spend in projections**: Management has been spending $700M–$1.7B/year on M&A. What is the base case assumption going forward? Need to set this before the acquisition schedule can be projected.
- **NCI in projections**: $127M NCI balance appeared in 2025. Keep static or grow with NCI net income? Very small — likely just keep static.
- **AOCI in projections**: Zero change assumption or model a small FX movement? Recommend zero for simplicity.

---

## Revenue Model Open Items

### R&DS
- [ ] Pull historical backlog data by year from 10-Ks and earnings releases
- [ ] Pull book-to-bill by year from earnings releases
- [ ] Pull passthrough revenue % by year (disclosed in segment footnotes)
- [ ] Set cancellation rate assumption
- [ ] Model patent cliff pull-forward for Keytruda, Eliquis, etc.
- [ ] Model IRA impact toggle
- [ ] Model BIOSECURE Act scenario

### TAS
- [ ] Pull segment revenue breakdown (technology platforms vs. RWE vs. analytics vs. information offerings) — may need to estimate from earnings call commentary
- [ ] Pull Veeva 10-K for SaaS benchmark metrics (NRR, churn rate, ACV growth)
- [ ] Set customer cohort splits (large/mid/EBP) — assumption-driven, benchmark from industry
- [ ] Build ARR waterfall for technology platforms
- [ ] Build study volume model for RWE
- [ ] Model FDA/IRA real world evidence mandate as uplift toggle
- [ ] Build information offerings subscription model by geography

### CSMS
- [ ] Simple: % growth assumption or headcount × revenue per FTE

---

## Checks to Run Before Projecting

- [ ] All schedule EoP rows tie to historical BS for 2021–2025 (green check rows)
- [ ] SE schedule: APIC + RE + Treasury + AOCI + NCI = Total equity per BS each year
- [ ] Investments schedule: FX/other plugs are small and consistent (large plugs = something is wrong)
- [ ] DTA schedule: Net DTA (DTA asset − DTL liability from BS) ties to footnote net position each year
- [ ] AR schedule: Gross AR − allowance = net AR per BS each year
- [ ] AP schedule: AP + total accrued expenses = AP&AE per BS each year
