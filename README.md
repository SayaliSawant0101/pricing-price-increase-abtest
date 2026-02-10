# Price Increase A/B Test (Target Product)

This project analyzes an A/B test where the **target product price** was increased:

- **Control price:** $19.99  
- **Treatment price:** $24.99  
- **Experiment:** `exp_price_graphic_tee_2026Q1`  
- **Population:** users who **viewed the target product at least once**

## Goal
Evaluate whether the price increase improves **Revenue per User (RPU)** while keeping key conversion and experience guardrails within acceptable limits.

---

## Metrics

### Primary metric
- **RPU (Revenue per User)** = total revenue per user (including any purchases)

### Secondary metrics (diagnostics)
- **Target CVR**: % users who purchased the target product  
- **ATC rate (Target)**: % users who added target product to cart  
- **Cart abandonment (Target)**: % users who ATC’d but didn’t purchase target  
- **Substitution rate**: % users who purchased a substitute product

### Guardrails
- **Overall purchase rate**: % users who purchased anything  
- **Return rate**: % users who returned an item

---

## Repo structure (Phases)

- `notebooks/01_phase1_setup_srm_price_audit.ipynb`  
  ✅ Data checks, population definition, SRM, and price audit  
- `notebooks/02_phase1_primary_secondary_metrics.ipynb`  
  ✅ KPI table + bootstrap CI for RPU + proportion tests for conversion metrics  
- `notebooks/03_phase2_funnel_substitution_break_even.ipynb`  
  ✅ Funnel analysis, substitution & recovery, segments, break-even CVR, rollout rules  

Outputs (tables saved from notebooks):
- `outputs/tables/`

---

## Key results (Overall)

### A) Primary outcome — RPU
- **Control RPU:** 15.9567  
- **Treatment RPU:** 18.1255  
- **ΔRPU:** +2.1688 (**+13.59% lift**)  
- **95% bootstrap CI (ΔRPU):** **[+1.0062, +3.3267]**

✅ Interpretation: revenue per user increased meaningfully.

### B) Conversion + funnel impact (Target Product)
- **Target CVR:** 54.553% → 47.373% (**-7.18 pp**, p≈0.000001)  
- **ATC rate (Target):** 70.578% → 64.186% (**-6.39 pp**, p≈0.000004)  
- **ATC → Purchase rate:** 77.30% → 73.81% (**-3.49 pp**)  
✅ Interpretation: most loss comes from **fewer users adding to cart**, plus a smaller drop from ATC→Purchase.

### C) Substitution behavior
- **Substitution rate:** 3.327% → 13.923% (**+10.60 pp**, p<0.000001)  
✅ Interpretation: higher price pushes many users to buy substitutes.

### D) Guardrails
- **Overall purchase rate:** 56.305% → 56.349% (flat, p≈0.976)  
- **Return rate:** 4.947% → 4.991% (flat, p≈0.946)

---

## Break-even analysis (Decision support)

We solved for the minimum target conversion rate needed in treatment to match control RPU:

- **Break-even treatment target CVR:** **40.74%**  
- **Actual treatment target CVR:** **47.37%**  
- **Margin vs break-even:** **+6.64 pp**

✅ Interpretation: even with lower target conversion, treatment still clears break-even due to higher price + non-target revenue.

---
![Funnel](outputs/figures/funnel.png)


## Rollout recommendation
**Recommendation:** **Conditional rollout**  
- Primary metric improved strongly (RPU ↑) ✅  
- Guardrails stable ✅  
- Target conversion fell significantly ⚠️  
- Substitution increased sharply ⚠️  

**Suggested rollout approach:**
- Roll out gradually (e.g., 10% → 25% → 50% → 100%)  
- Monitor substitution mix, long-term retention, and brand/CS impact  
- Consider messaging/positioning tests (value framing, bundles, loyalty offsets)

---

## How to run
```bash
pip install -r requirements.txt
