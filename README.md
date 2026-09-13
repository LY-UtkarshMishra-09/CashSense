# CashSense — AI-Powered Affordability Engine

CashSense is a financial decision engine built for HackerRank Orchestrate. It determines whether a user can safely afford a requested purchase while accounting for recurring commitments, minimum balance reserves, multi-currency conversion, and pending financial events over a 90-day trajectory.

---

## 🌟 Key Architecture & Highlights

1. **Deterministic Financial Forecasting Engine**  
   - Employs a zero-hallucination daily cashflow solver rather than relying purely on LLM reasoning for money calculations.
   - Strictly enforces safety reserve constraints:  
     $$\text{Balance}(t) \ge \text{minimum\_balance\_to\_keep} \quad \forall t \in [0, 90]$$

2. **Event Parsing & Deduplication**  
   - Instantly applies pending debits to user balances.
   - Holds pending credits until confirmed settlement dates.
   - Extracts numeric receipt amounts directly from visual records (`images.csv`) when receipt amounts are blank.

3. **Multi-Currency Normalization**  
   - Converts non-native transaction values into the user's base currency using exact-date exchange rates (`exchange_rates.csv`).

4. **Dynamic Decision Routing**  
   - Generates compliant recommendations for `affordable_now`, `affordable_with_plan`, `affordable_later`, or `not_affordable`.
   - Schedules clean installment breakdown dates (`YYYY-MM-DD:amount|YYYY-MM-DD:amount`) and calculates safe full-payment target dates.

---

## 📁 Repository Structure

```text
├── code/
│   ├── main.py                   # Primary pipeline execution entry point
│   └── buy_or_wait/
│       ├── ingestion.py          # Data loaders & exchange rate converters
│       ├── ocr.py                # Visual receipt extraction & fallback logic
│       ├── evidence.py           # Transaction deduplication & state resolution
│       ├── forecast.py           # 90-day forward cash flow simulation
│       ├── decide.py             # Recommendation & payment plan selector
│       └── validate.py           # Schema contracts & output guardrails
├── evaluation/
│   └── usage_report.md           # Model execution & token usage analysis
├── dataset/                      # Corpus & CSV inputs (excluded from submission zip)
├── requirements.txt              # System dependencies
└── README.md                     # Documentation