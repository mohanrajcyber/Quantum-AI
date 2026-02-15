# Rural AI - Fairness Algorithm Specification

## Overview

Rural AI uses an ethical fairness algorithm to ensure balanced, transparent, and fair recommendations for farmers and rural communities. This algorithm prevents over-exposure of specific sellers and ensures equitable distribution of opportunities.

## NecessityScore Calculation

### Formula
```
NecessityScore = Impact × Urgency × Credibility × (1 - FairnessPenalty)

Where each component ranges from 0 to 1
```

## Component Breakdown

### 1. Impact Score (0 - 1)

Measures how many people are affected by the information.

**Calculation:**
```
Impact = min(1, AffectedPopulation / PopulationThreshold)

Where:
- AffectedPopulation = Number of people impacted
- PopulationThreshold = 10,000 (baseline for rural areas)
```

**Examples:**
- 500 farmers affected → Impact = 500/10000 = 0.05
- 5,000 farmers affected → Impact = 5000/10000 = 0.50
- 15,000 farmers affected → Impact = min(1, 15000/10000) = 1.0

**Categories:**
- Low Impact (0 - 0.3): Individual queries, personal advice
- Medium Impact (0.3 - 0.7): Village-level information
- High Impact (0.7 - 1.0): District/state-level alerts

---

### 2. Urgency Score (0 - 1)

Measures time sensitivity of the information.

**Calculation:**
```
Urgency = 1 - (TimeToImpact / MaxTimeWindow)

Where:
- TimeToImpact = Hours until event occurs
- MaxTimeWindow = 72 hours (3 days baseline)
```

**Examples:**
- Cyclone warning in 2 hours → Urgency = 1 - (2/72) = 0.97
- Mandi price change in 24 hours → Urgency = 1 - (24/72) = 0.67
- Seasonal advice (48 hours) → Urgency = 1 - (48/72) = 0.33

**Categories:**
- Critical (0.8 - 1.0): Immediate action needed (weather alerts)
- High (0.5 - 0.8): Action needed within 24 hours
- Medium (0.3 - 0.5): Action needed within 2-3 days
- Low (0 - 0.3): General information, no time pressure

---

### 3. Credibility Score (0 - 1)

Measures reliability of the information source.

**Calculation:**
```
Credibility = (SourceReliability × 0.5) + (DataQuality × 0.3) + (HistoricalAccuracy × 0.2)

Where each sub-component ranges 0-1
```

**Source Reliability:**
- Government APIs (IMD, Agmarknet): 1.0
- Verified NGOs: 0.8
- Crowdsourced (verified): 0.6
- Unverified sources: 0.3

**Data Quality:**
- Real-time sensor data: 1.0
- Recent official reports (<6 hours): 0.9
- Aggregated data (6-24 hours): 0.7
- Older data (>24 hours): 0.5

**Historical Accuracy:**
- Source accuracy >95%: 1.0
- Source accuracy 85-95%: 0.8
- Source accuracy 70-85%: 0.6
- Source accuracy <70%: 0.4

**Example:**
```
Weather alert from IMD:
- SourceReliability = 1.0
- DataQuality = 1.0 (real-time)
- HistoricalAccuracy = 0.95 (IMD track record)

Credibility = (1.0 × 0.5) + (1.0 × 0.3) + (0.95 × 0.2)
            = 0.5 + 0.3 + 0.19
            = 0.99
```

**Minimum Threshold:** Credibility must be ≥ 0.6 to proceed

---

### 4. Fairness Penalty (0 - 0.5)

Prevents over-exposure of specific sellers/vendors in mandi pricing.

**Calculation:**
```
FairnessPenalty = 0 if SellerExposure ≤ ExposureCap
FairnessPenalty = min(0.5, (SellerExposure - ExposureCap) / ExposureCap)

Where:
- SellerExposure = % of recommendations for this seller in last 30 days
- ExposureCap = 20% (maximum allowed exposure)
```

**Examples:**

**Case 1: Fair Distribution**
```
Seller A exposure = 15% (below 20% cap)
FairnessPenalty = 0
```

**Case 2: Slight Over-Exposure**
```
Seller B exposure = 25% (5% over cap)
FairnessPenalty = (25 - 20) / 20 = 5/20 = 0.25
```

**Case 3: Severe Over-Exposure**
```
Seller C exposure = 40% (20% over cap)
FairnessPenalty = min(0.5, (40 - 20) / 20) = min(0.5, 1.0) = 0.5
```

**Additional Fairness Checks:**

1. **Price Deviation Check:**
```
PriceDeviation = |SellerPrice - MarketAverage| / MarketAverage

Reject if PriceDeviation > 0.15 (15%)
```

2. **Geographic Fairness:**
```
Ensure recommendations distributed across:
- Different mandis
- Different regions
- Different seller sizes (small/medium/large)
```

---

## Complete Examples

### Example 1: Critical Weather Alert

**Scenario:** Cyclone warning for coastal district

**Input:**
- AffectedPopulation = 50,000 farmers
- TimeToImpact = 6 hours
- Source = IMD (Government)
- SellerExposure = N/A (not applicable for weather)

**Calculation:**
```
Impact = min(1, 50000/10000) = 1.0

Urgency = 1 - (6/72) = 0.92

Credibility:
- SourceReliability = 1.0 (IMD)
- DataQuality = 1.0 (real-time)
- HistoricalAccuracy = 0.95
- Credibility = (1.0 × 0.5) + (1.0 × 0.3) + (0.95 × 0.2) = 0.99

FairnessPenalty = 0 (not applicable)

NecessityScore = 1.0 × 0.92 × 0.99 × (1 - 0) = 0.91
```

**Result:** HIGH PRIORITY - Requires human approval (>0.8)

---

### Example 2: Mandi Price Recommendation

**Scenario:** Tomato price alert for local mandi

**Input:**
- AffectedPopulation = 200 farmers
- TimeToImpact = 12 hours (market opens tomorrow)
- Source = Agmarknet (verified)
- Seller = Vendor A
- Vendor A exposure in last 30 days = 18%

**Calculation:**
```
Impact = 200/10000 = 0.02

Urgency = 1 - (12/72) = 0.83

Credibility:
- SourceReliability = 1.0 (Agmarknet)
- DataQuality = 0.9 (6 hours old)
- HistoricalAccuracy = 0.85
- Credibility = (1.0 × 0.5) + (0.9 × 0.3) + (0.85 × 0.2) = 0.84

FairnessPenalty = 0 (18% < 20% cap)

NecessityScore = 0.02 × 0.83 × 0.84 × (1 - 0) = 0.014
```

**Result:** LOW PRIORITY - Auto-approve (no human needed)

---

### Example 3: Over-Exposed Seller

**Scenario:** Wheat price from frequently recommended seller

**Input:**
- AffectedPopulation = 1,000 farmers
- TimeToImpact = 24 hours
- Source = Verified aggregator
- Seller = Vendor B
- Vendor B exposure = 28% (over cap)

**Calculation:**
```
Impact = 1000/10000 = 0.10

Urgency = 1 - (24/72) = 0.67

Credibility:
- SourceReliability = 0.8 (verified aggregator)
- DataQuality = 0.7 (12 hours old)
- HistoricalAccuracy = 0.80
- Credibility = (0.8 × 0.5) + (0.7 × 0.3) + (0.80 × 0.2) = 0.77

FairnessPenalty = (28 - 20) / 20 = 0.40

NecessityScore = 0.10 × 0.67 × 0.77 × (1 - 0.40)
               = 0.10 × 0.67 × 0.77 × 0.60
               = 0.031
```

**Result:** PENALIZED - Score reduced by 40% due to over-exposure

---

## Decision Matrix

| NecessityScore | Action | Human Approval |
|----------------|--------|----------------|
| 0.8 - 1.0 | HIGH PRIORITY | Required |
| 0.5 - 0.8 | MEDIUM PRIORITY | Optional (flagged) |
| 0.3 - 0.5 | LOW PRIORITY | Not required |
| 0 - 0.3 | INFORMATIONAL | Auto-approve |

## Rejection Criteria

Automatically reject if ANY of these conditions:

1. **Credibility < 0.6**
   - Source not reliable enough

2. **SellerExposure > 30%**
   - Severe fairness violation

3. **PriceDeviation > 15%**
   - Price manipulation suspected

4. **Impact = 0**
   - No one affected

## Edge Cases

### Case 1: Zero Urgency
```
If TimeToImpact > MaxTimeWindow:
  Urgency = 0
  NecessityScore = 0
  Action: Reject (not time-sensitive)
```

### Case 2: Multiple Sellers
```
For mandi recommendations with multiple sellers:
  Calculate NecessityScore for each
  Sort by score
  Apply fairness rotation
  Select top N with exposure < 20%
```

### Case 3: Conflicting Sources
```
If multiple sources provide different data:
  Use weighted average based on Credibility
  Flag for human review if variance > 20%
```

## Monitoring Metrics

Track these metrics for Rural AI fairness algorithm performance:

1. **Fairness Index:**
   - Standard deviation of seller exposure
   - Target: <5% deviation

2. **Accuracy Rate:**
   - % of predictions that were correct
   - Target: >85%

3. **User Satisfaction:**
   - Feedback on recommendations
   - Target: >4.0/5.0

4. **Human Approval Rate:**
   - % of high-priority items approved
   - Target: >90% (indicates good scoring)

## Tuning Parameters

These can be adjusted based on real-world performance:

| Parameter | Current Value | Adjustable Range |
|-----------|---------------|------------------|
| PopulationThreshold | 10,000 | 5,000 - 50,000 |
| MaxTimeWindow | 72 hours | 24 - 168 hours |
| ExposureCap | 20% | 15% - 30% |
| PriceDeviationLimit | 15% | 10% - 20% |
| CredibilityThreshold | 0.6 | 0.5 - 0.8 |
| HighPriorityThreshold | 0.8 | 0.7 - 0.9 |
