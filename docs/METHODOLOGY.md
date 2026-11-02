# Methodology summary (v3)

This summarizes the Methodology and Scenario_Control sheets of the Excel model. The workbook is the authoritative version.

## 1. Data sources (all official public data)

- Hospital Provider Cost Report PUF (HCRIS, CMS-2552-10), file years 2022-2024: revenue, expenses, Medicaid revenue/charges, uncompensated care, balance sheet. data.cms.gov
- Hospital General Information (Care Compare, 2026): hospital type, ownership, overall star rating. data.cms.gov/provider-data/dataset/xubh-q36u
- HAC Reduction Program FY2026, Hospital VBP Total Performance Score FY2026 and Hospital Readmissions Reduction Program FY2026 (discharge-weighted excess readmission ratio across 6 conditions): quality/performance risk. data.cms.gov/provider-data
- Medicare Inpatient Hospitals by Provider (2022-2024): Medicare discharges, dual-eligible share. data.cms.gov
- State Medicaid & CHIP Applications, Eligibility Determinations and Enrollment Data (Oct 2026 release, through Jul 2026): enrollment and expansion status. data.medicaid.gov
- State inputs: KFF FMAP FY2027; CMS Medicaid Enrollment, New Adult (VIII) Group, June 2025; MACPAC MACStats Exhibits 19 (FY2022) and 23 (FY2024); KFF provider-tax Appendix Table 1 (rates as of Jul 1, 2025); KFF SDP Appendix Table 1 (state SDP spending, total and federal, Jun 2026); HRSA FORHP rural ZIP file (Sep 2025). Effective federal share: MACPAC Exhibit 16 (FY2024 federal / total Medicaid spending; includes IHS 100% match, which raises AK, SD and AZ).
- Policy calibration: CBO (Aug 2025) coverage and SDP estimates; RAND RR-A4098-1 (2026); KFF (2025-2026) provider-tax and SDP analyses; CMS SDP proposed rule (May 20, 2026) and provider-tax proposed rule (Jul 21, 2026) and their regulatory impact analyses; ASPE SDP payment levels; MedPAC March 2026; CMS RHTP awards (Dec 2025).

## 2. Universe

- 4,556 short-term acute (STH) and critical access hospitals (CAH) in 50 states + DC with a usable cost report (NPR and opex > $1M, >= 180 days). Most recent file year used as baseline (97% from the 2024 file, FY ending Oct 2023-Oct 2025).
- Multiple reports for one CCN in a year are summed; periods shorter than 335 or longer than 395 days are annualized to 365 days. Psychiatric, rehab, LTCH, children's and VA hospitals excluded.
- Join key: CMS Certification Number (CCN). Match rates: General Info 96%, Medicare inpatient 93% of STH, HAC/VBP for IPPS hospitals only (CAHs are exempt from these programs).
## 3. Channel formulas (full effect, $ per year)

- Coverage = (hospital share of state hospital Medicaid revenue) x (state expansion-group enrollment, Jun 2025) x (coverage-loss rate: 20% / 30% / 35%; Base matches CBO's ~6.0M uninsured from work requirements and redeterminations) x (Medicaid $ per expansion adult, MACPAC, capped at $15,000) x (41% hospital share) x (net-loss factor).
- SDP cap: state federal hospital SDP spending above the limits = KFF estimates for 8 states; the rest of KFF's $60B national total (Scenario_Control B29, live) is allocated in proportion to each state's actual federal SDP spending (KFF Appendix Table 1). The 10 jurisdictions with no SDPs get zero. Converted to all funds with each state's actual SDP federal share. Net hospital loss = all-funds excess x (1 - state share x share of state share financed by hospitals) x (1 - reinvestment). Allocated within the state by Medicaid revenue, capped at 30% of NPR. Phase-down follows the CMS proposed rule (default) or the statute (selector on Scenario_Control B37).
- Provider tax: above-threshold hospital-tax revenue by state = NPR x (band midpoint - 3.5%) x (1 - SDP overlap share in states with SDPs). Hospital payments lost = that revenue x share not backfilled x hospital share of tax-funded spending / (1 - effective federal share). Each hospital's net = its share of payments lost (by Medicaid revenue) minus the tax it no longer pays (by NPR). Low-Medicaid hospitals can show a small gain; safety-net hospitals lose most. Non-expansion states are frozen, not reduced, so they show $0.
- Annual impact = sum of each channel x its phase-in share for that year. Projected margin = (baseline net income - impact) / (baseline revenue - impact). Constant baseline dollars; no inflation, volume growth or management response.

## 4. Vulnerability index & risk tiers

- Index (0-100, static, Base 2032) = weighted percentile ranks: Medicaid dependence 15%, projected H.R.1 hit 25%, weak 3-yr average total margin 25%, thin liquidity 10% (down-weighted: facility cash is unreliable for system members), uncompensated care 15%, quality risk 10% (star rating, HAC penalty, VBP TPS, HRRP excess readmission ratio).
- Tiers: 1-Critical = projected margin < -5% AND weak liquidity (5-60 days cash, or under 5 days, which usually means cash is held by the parent system, with a negative 3-yr average margin); 2-High = projected margin < 0 OR index >= 70; 3-Elevated = projected margin < 3% OR index >= 55; 4-Stable = all others. Data-flagged rows are excluded from all hospital counts and tiers. Weights are the analyst's method, not an established credit model.

## 5. Key limitations

- Medicaid revenue now includes supplemental/DSH payments: S-10 line 2 plus line 5 when line 4 = 'N' (net of taxes), extracted from CMS HCRIS raw files (hosp10fy2022-2025, Oct 2026 release) for each hospital's latest report and scaled to its baseline. SDP and provider-tax payments are allocated on this basis; coverage loss is still allocated on line 2 (base claims). Investment income (G-3 line 7) is used for a margin excluding investment income.
- Hospital-specific SDP and provider-tax exposure is estimated by allocation. A hospital's actual exposure depends on its specific state programs. Validate against the state's SDP preprints (CMS) before using it for a specific hospital decision.
- Provider-tax rates are known only to the band KFF reports (3.51-5.5% or 5.51-6.0%); the model uses band midpoints, with Low/High at the band edges. Tax bases vary by state (some exclude Medicare revenue), so applying the rate to net patient revenue is an approximation.
- Channel overlap: provider-tax dollars that fund SDPs are removed from the provider-tax channel (Scenario_Control row 34). Coverage-channel spending includes managed-care payments, some of them SDPs, so a small overlap with the SDP channel remains. Rural = HRSA FORHP rural ZIP or critical access designation (2,328 hospitals).
- Days cash is understated for system-member hospitals whose cash is held at the parent. Cost-report margins are unaudited total margins (they include investment income). Rows with |margin| > 50%, Medicaid share > 80%, non-positive revenue or non-general-acute facilities are flagged and excluded from counts, tiers and rankings, but kept in dollar totals.
- Channel 4 (v3) adds retroactive-coverage limits (Jan 2027), immigrant-eligibility limits (Oct 2026), expansion-adult cost sharing (Oct 2028) and MCO-tax uniformity (CA, MA, MI, NY, IL, OH, WV), each calibrated to CBO's federal savings. Still excluded: emergency Medicaid match change (a state cost, not a payment cut), the proposed extension of Medicare-based limits to all services (2029), ACA Marketplace changes and the end of enhanced premium tax credits. Status: the statutory PAYGO sequester was cancelled by P.L. 119-37 (Nov 2025); Medicaid DSH cuts were delayed to FY2029 and cut to $8B (Feb 3, 2026).
- Rural Health Transformation Program ($10B/yr, FY2026-2030) goes to states for transformation projects. Provider payments are capped at 15% of a state's award, so it is shown for context only and not netted against hospital impacts.

## 6. How to validate after implementation

- As post-2027 cost reports arrive, compare margin and uncompensated-care changes in high- vs. low-exposure hospitals (difference-in-differences, controlling for state, size and ownership) to move from projected to measured impact. Monthly enrollment (Trend sheet) is the earliest leading indicator.

© 2026 Hamza Saleh, MPH. All rights reserved. No reproduction, distribution or derivative works without written permission. Public source data (CMS, KFF, CBO, MACPAC, HRSA) remain the property of their publishers.

## Scenario assumptions

| Assumption | Low | Base | High |
| --- | --- | --- | --- |
| Expansion-group adults losing coverage at full effect | 0.2 | 0.3 | 0.35 |
| Hospital share of Medicaid spending per expansion adult | 0.41 | 0.41 | 0.41 |
| Net margin loss per $1 of lost Medicaid revenue | 0.5 | 0.65 | 0.8 |
| Share of capped SDP dollars states redirect back to hospitals | 0.4 | 0.2 | 0 |
| Hospital tax above 3.5%: states in the 5.51-6.0% band (share of NPR) | 0.02 | 0.0225 | 0.025 |
| Hospital tax above 3.5%: states in the 3.51-5.5% band (share of NPR) | 0.005 | 0.01 | 0.0175 |
| Share of the lost state share not backfilled by the state | 0.4 | 0.6 | 0.8 |
| Share of SDP non-federal share financed by hospitals (provider taxes, IGTs) | 0.75 | 0.5 | 0.25 |
| Share of above-threshold hospital-tax revenue that funds SDPs (overlap; states with SDPs) | 0.6 | 0.4 | 0.2 |
| Share of tax-funded Medicaid spending paid to hospitals | 0.6 | 0.75 | 0.9 |
| Hospital share of retroactive-coverage spending | 0.6 | 0.75 | 0.9 |
| Net margin loss per $1 of cost-sharing reduction | 0.3 | 0.5 | 0.7 |
| Hospital share of MCO-tax-funded spending | 0.15 | 0.3 | 0.45 |

© 2026 Hamza Saleh, MPH. All rights reserved.
