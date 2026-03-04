# Model Notes — IQVIA DCF

Running log of methodology decisions, design choices, and implementation details.

---

## BS Construction

**Superset approach**: Union of all line items across every 10-K year. Backfill zeros for years where a line item didn't exist. "Longest" filing ≠ superset since companies sometimes consolidate items in later filings.

**Disambiguation**: "Investments in debt, equity and other securities" appears twice on the BS face (current and non-current). Added (current)/(non-current) suffixes.

**Common stock**: BS combines $3M par value + APIC into one line ("Common stock and additional paid-in capital"). SE schedule carries the $3M consistently in the BoP so EoP ties exactly.

**2020 anchor**: 2020 BS included as a hidden/reference column to enable 2021 delta calculations in the SCF and working capital schedule.

---

## SCF Philosophy

**Historical SCF**: Hardcoded blue from reported 10-Ks. Not formula-driven. This is intentional — BS deltas don't match reported SCF due to:
1. Acquired working capital (target company WC lands on BS via investing, not operating cash flows)
2. FX translation (BS uses period-end rates; SCF uses average rates)
3. Non-cash debt assumed in acquisitions

**Projected SCF**: Entirely formula-driven from projected BS deltas and schedule outputs. No hardcodes in projection years.

**Why not plug the historical SCF?**: Operating section plugs ranged up to ~20% of net cash from operations in 2025 ($526M). Too large to be defensible.

---

## SCF Line Items Decision

Kept exact reported line items from 10-K (no consolidation). This gives:
- Clean historical columns matching reported statements verbatim
- Same row labels in projection columns — seamless extension
- Auditability: every line traceable to 10-K

One-time/non-recurring items (gain on disposals, NCI acquisition, contingent consideration) kept as rows — just zeroed in projections.

---

## Supporting Schedule Design

**General principle**: Every BS line in projection years is driven by either a schedule output or a simple % assumption. The SCF is then derived from BS deltas — never linked directly to schedules.

**Roll-forward structure**: Each schedule has historical columns (hardcoded blue, verify vs. BS) and projection columns (formula-driven, assumption-driven inputs in blue from assumptions tab).

**Check rows**: Every schedule has a green check row at the bottom comparing EoP to the historical BS tab. Must show zero difference for all historical years before projecting.

---

## Schedule-Specific Notes

### Goodwill
- Roll by segment (TRS, R&DS, CSMS) using footnote data
- FX/other = plug to tie to BS (fair value movements + FX translation)
- Projection: acquisition spend from acquisition schedule; FX = zero

### Shareholders' Equity
- Format: one roll per component (not columnar 10-K format)
- APIC: includes $3M common stock par value in every BoP; footnote strips it out but we add it back so EoP = BS exactly
- NCI: Quest NCI buyout in 2021 took NCI from $279M (2020) to $0; new NCI of $127M appeared in 2025 from acquisition
- Treasury stock: small discrepancies vs. SCF repurchase line due to IRA excise tax on buybacks (post-2022), which hits treasury stock but is classified differently on SCF
- AOCI projection: single "OCI assumption" line (zero or small FX); don't try to project derivatives, pension, FX separately

### Investments
- Signs: SCF shows purchases as negative (cash outflow); BS roll stores as positive (balance increases). When linking projected SCF to BS, negate the delta.
- FX/other = derived plug (= EoP BS − BoP − cash flows). Not sourced from a footnote.
- Affiliates equity earnings: sign convention in schedule is positive (increases carrying value); SCF shows as negative (non-cash income reversed out). Flip when linking.
- Footnote disclosure very limited — no full roll available. Simple BoP + cash flows + FX plug = EoP.

### AR Schedule
- Split: billed AR (DBO = billed/revenue × 365), unbilled (DUO = unbilled/revenue × 365), allowance % gross AR
- 2021 filing used label "Billed" vs. later years using "Trade accounts receivable" — same item
- Contract assets (29% of unbilled) = milestone-based; unbilled receivables (71%) = time-based. Both in unbilled services line.
- Allowance historically ~1.3–1.5% of gross AR; immaterial bad debt expense per footnote

### Unearned Income
- % revenue is correct driver. Footnote confirms "majority of BoP unearned income recognized during year" — high turnover, ~1x annually.
- RPO ($34.2B for 2025) ≠ backlog because RPO excludes contracts with unilateral cancellation rights. RPO understates true pipeline.
- No formal contract liability rollforward disclosed; % revenue is ceiling of precision.

### AP & Accrued Expenses
- Broken into components using accrued expenses footnote
- Accounts payable = total AP&AE − total accrued expenses (derived)
- Interest accrued → will link from debt schedule in projections
- Contingent consideration → will link from acquisition schedule in projections
- Compensation driver: % of total labor cost (COGS + SG&A) rather than % revenue

### Prepaid Expenses
- Days prepaid / COGS = ~5–6 days (too low, COGS denominator is wrong for prepaid)
- Balance is very stable ($141–162M over 5 years despite significant revenue growth)
- Best driver: % revenue. Balance is not volume-driven — it's insurance contracts and software licenses renewing annually at similar amounts.

### Deferred Tax Assets
- Superset: line items changed across years (lease liability/ROU asset lines appeared 2021–2023, disappeared 2024–2025; NOL label changed)
- Net DTA per footnote = BS DTA (asset) − BS DTL (liability) ✓ (within rounding)
- Gross DTA ($1,135M) does not directly reconcile to BS DTA line ($357M) — jurisdictional netting in BS presentation. Footnote → BS reconciliation only works in reverse.
- 163(j) interest carryforward: "U.S. interest expense limitation" DTA line. Implied gross carryforward: $310M (2025), $443M (2024) based on 21% tax rate.
- Projection approach: NOL and interest carryforward lines driven by respective schedules × tax rate; other lines driven by % assumptions linked to relevant BS/IS items; valuation allowance as % of gross DTA.

---

## Revenue Model Notes

### R&DS
- Backlog-driven: BoP backlog + net new bookings − revenue recognized = EoP backlog
- Revenue = burn rate × BoP backlog
- Four cohorts: EBP (VC-lagged), Small/Mid (R&D budget), Large Pharma (MSA/patent cliff), Gov/Academic (NIH/BARDA)
- Passthrough revenue modeled separately at 0% margin
- Patent cliff inputs: Keytruda, Eliquis, Ozempic-adjacent, Humira biosimilar tail
- IRA and BIOSECURE Act as toggle scenarios

### TAS
- Sub-segments: technology platforms (ARR/SaaS), real world solutions (study volume), analytics & consulting (engagement volume), information offerings (subscription count × ACV)
- Technology platforms: full SaaS unit economics waterfall — beginning ARR + new logo + expansion − churn = ending ARR; NRR, NDR, LTV:CAC
- Three customer cohorts per sub-segment: large pharma / mid pharma / EBP
- Veeva (VEE) is primary comp for SaaS benchmarks (NRR, churn, ACV growth)
- Alt data: ClinicalTrials.gov, FDA approvals, IQVIA Institute reports, LinkedIn job postings, World Bank API

---

## Data Sources

- 10-Ks: BAMSEC (2020–2025 filings)
- Macro: World Bank API (GDP), OECD health expenditure, EvaluatePharma (pharma R&D spend)
- Clinical trials: ClinicalTrials.gov
- SaaS benchmarks: Veeva Systems 10-K filings
- Industry: IQVIA Institute for Human Data Science (free annual reports)
- Alt data: PitchBook/NVCA (VC funding), LinkedIn job postings, G2/Gartner Peer Insights
