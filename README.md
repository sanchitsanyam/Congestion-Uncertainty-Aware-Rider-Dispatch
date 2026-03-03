# CUARD — Congestion & Uncertainty-Aware Rider Dispatch

Zomathon Hackathon Submission  
65%+ Rider Wait Reduction · ₹90+ Crore Annual Savings  

---

## Problem

Zomato’s delivery ETA depends heavily on Kitchen Prep Time (KPT) prediction.  
KPT models rely on merchant-marked Food Order Ready (FOR) signals.

However:

- 40% Rider-Triggered (marked ready when rider arrives)
- 19.9% Early Marker (marked before food ready)
- Only 40.1% Honest

Approximately 60% of FOR signals are biased.

### Impact of Biased Signals
- Early rider dispatch  
- High idle waiting at restaurants  
- Noisy KPT model training  
- ETA fluctuations and cancellations  
- Increased operational cost  

---

## Solution — CUARD

CUARD enhances dispatch accuracy without retraining base KPT models by improving signal quality and incorporating congestion-aware uncertainty modeling.

### 1. FOR Signal De-biasing
- Outlier clipping (1st–99th percentile)
- Merchant-type correction logic
- Merchant Reliability Score (MRS)

Result: 52% improvement in FOR signal MAE

---

### 2. Extended Kitchen Congestion Index (KCI_extended)

Captures real kitchen rush beyond Zomato orders using:
- Google Maps Popular Times API
- POS transaction velocity
- BLE IoT footfall counter

Result: +65% variance captured vs Zomato-only KCI

---

### 3. Uncertainty-Aware Dispatch
- Sigma inflation via KCI × MRS
- Gradient Boosting dispatch policy
- SLA constraint:  
  P(Late > 3 min) ≤ 10%

---

### 4. IoT Smart Buzzer (Hardware Innovation)
- ₹500 device
- Staff presses when packaging complete
- Direct timestamp sent to server
- Eliminates merchant app bias

Payback period: < 1 month

---

## Results (Time-Split Validation)

| Metric | Baseline | CUARD Final |
|--------|----------|-------------|
| Avg Rider Wait | ~3.4 min | ~1.2 min |
| P90 Wait | ~6.7 min | ~2.8 min |
| High Wait % | ~54% | ~16% |
| Improvement | — | +65% |

Estimated Annual Savings: ₹90+ Crore  

---

## Production Architecture

```
Order Confirmed
    ↓
Kafka Event Bus
    ↓
Flink Processor
    ↓
GBM Model (ONNX)
    ↓
SLA Check
    ↓
Dispatch Signal → Rider App
```

End-to-End Latency: <50ms  
Horizontally Scalable (300K+ merchants, 1M+ daily orders)

---

## Tech Stack

- Python
- PyTorch
- Scikit-learn
- Gradient Boosting (GBM)
- Kafka
- Apache Flink
- Redis
- ONNX Runtime

---

## Dataset

- 45,584 delivery orders
- 80/20 time-split validation
- Future-only evaluation (no leakage)

---

## Key Contributions

- Merchant-type-aware FOR correction
- Reliability-weighted uncertainty modeling
- Extended congestion capture
- SLA-constrained dispatch optimization
- IoT instrumentation proposal

---

## Business Impact

- 65% rider wait reduction
- 52% signal quality improvement
- ₹90+ crore annual savings
- Scalable across 300K+ merchants

CUARD — Building a Smarter Dispatch System for Zomato Riders
