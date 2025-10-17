# The Loan Officer Challenge

## Assignment Overview

Welcome to your role as a **Loan Officer at Data Bank**! Your task is to build predictive models to make profitable lending decisions. This isn't just about building accurate models—it's about making **smart business decisions** that maximize profit.

**The Challenge:** You have 1,000 new loan applications. For each one, you must decide:

1. **Should we approve or deny this loan?**
2. **If approved, what interest rate should we offer?**

The catch? You don't know the actual outcomes (whether borrowers accept your rates or default on loans) until I merge your predictions with the actual outcomes.

**Requirements:** You must try **at least 4 different supervised learning methods** and compare their performance. Your grade depends on both methodology and results.

---

## What You're Given

### Training Data (`loan_training_data.csv`)

Historical loan data with **5,000 loans** containing:

**Borrower Information:**

- Demographics (age, income, employment length, home ownership)
- Credit history (credit score, number of credit lines, delinquencies, credit utilization)
- Loan details (loan amount, purpose)

**Historical Outcomes:**

- `offered_rate`: The interest rate our bank offered in the past
- `offer_accepted`: Whether the borrower accepted our offer (1 = yes, 0 = no)
- `default`: Whether the loan defaulted (**only known for accepted loans**)

### Test Data (`loan_test_data.csv`)

**1,000 new loan applications** with the same borrower features, but:

- No offered rates
- No acceptance information
- No default information

**Your job:** Make decisions for these 1,000 loans!

---

## How Profit is Calculated

All loans are **3-year term loans**. Your profit for each loan depends on your decision and the outcome:

### If You DENY the Loan

**Profit = $0**

No revenue, no costs, no risk.

### If You APPROVE the Loan

Your profit depends on whether the borrower accepts your rate and whether they default.

**Case A: Borrower accepts AND pays back**

```
Profit = (Loan Amount × Interest Rate x 3 years) - $200
```

- You earn interest revenue for 3 years
- You pay $200 in operating costs (underwriting, servicing)

**Example:** $10,000 loan at 12% → ($10,000 × 0.12 x 3) - $200 = **$3,400 profit**

**Case B: Borrower accepts BUT defaults**

```
Loss = -(Loan Amount + $700)
```

- You lose the entire principal
- You pay $200 operating cost + $500 collection cost = $700 total costs

**Example:** $10,000 loan → -($10,000 + $700) = **-$10,700 loss**

**Case C: Borrower rejects your rate**

**Profit = $0**

The borrower goes elsewhere. You miss the opportunity but incur no costs.

### Your Objective

Maximize total profit across all 1,000 loans by making smart decisions about:

1. Which loans to approve vs deny
2. What interest rate to offer on approved loans

**Key insight:** You must predict both default probability AND acceptance probability to optimize your strategy.

---

## What You Need to Submit

### Final Submission

**You must submit TWO files:**

#### 1. Predictions CSV File (`GroupName_predictions.csv`)

**Format:**
```
loan_id,decision,interest_rate
1,approve,0.10
2,deny,0.00
3,approve,0.12
...
```

**Requirements:**

- Must include all 1,000 loans from test data
- `decision`: Either "approve" or "deny"
- `interest_rate`: Your offered rate (set to 0 for denied loans)

#### 2. Final Report (`GroupName_report.md`)

**Required Sections:**

1. **Executive Summary** 
   - Your approach and expected performance

2. **Exploratory Data Analysis**
   - Key findings with visualizations
   - Analysis of acceptance patterns
   - Analysis of default patterns

3. **Feature Engineering**
   - New features created and rationale

4. **Model Development**
   - Description of models you tried (must use at least 4 different methods)
   - Performance metrics with cross-validation
   - Model selection justification

5. **Business Strategy**
   - How you set interest rates
   - How you make approve/deny decisions
   - Risk management approach

6. **Model Interpretation**
   - Feature importance
   - Key predictors and business insights

7. **Conclusion**

---


## Understanding the Challenge

### The Selection Bias Problem

In the training data, you only know default outcomes for **accepted** loans. Rejected offers never got funded, so you don't know if those borrowers would have defaulted.

### The Rate Sensitivity Problem

Different borrowers accept different rates. You need to figure out what rates different borrowers will accept based on the historical data.

### The Optimization Problem

For each loan, you're trying to maximize:

$$
\begin{aligned}
\text{Expected Profit} = &\bigg[\underbrace{(1 - P(\text{Default})) \times r \times L \times 3}_{\text{Expected Interest Revenue}} &- \underbrace{P(\text{Default}) \times (L + 700)}_{\text{Expected Default Loss}} - \underbrace{200}_{\text{Operating Cost}}\bigg] \times P(\text{Accept})
\end{aligned}
$$

Where:

- $r$ = Interest Rate
- $L$ = Loan Amount  
- $P(\text{Accept})$ = Probability borrower accepts your offered rate
- $P(\text{Default})$ = Probability loan defaults

This requires predicting BOTH acceptance probability AND default probability.

---

## Key Questions to Consider

**Understanding the Data:**

- What patterns do you see in offer acceptance?
- How does offered rate affect acceptance?
- What predicts default among accepted loans?
- Do different borrower types have different rate sensitivity?

**Building Models:**

- What data should you use to train a default prediction model?
- What data should you use to train an acceptance prediction model?
- Which features are most important?
- How will you validate your models?

**Setting Rates:**

- Should you charge everyone the same rate?
- How do you balance profit per loan with acceptance probability?
- When should you deny a loan entirely?

---

## Grading Rubric (100 points total)

| Component | Points | What We're Looking For |
|:---------:|:------:|:----------------------:|
| **Profit Ranking** | 20 | Competitive performance on test set |
| **Model Variety** | 15 | Used 4+ different methods, properly implemented |
| **Cross-Validation** | 15 | Proper k-fold CV methodology |
| **Feature Engineering** | 10 | Created meaningful features with clear rationale |
| **Visualizations** | 10 | Clear, informative plots |
| **Business Reasoning** | 15 | Strong justification of strategy |
| **Code Quality** | 10 | Clean, commented, reproducible |
| **Report Clarity** | 5 | Well-written and organized |

### Profit Ranking Formula

Your Profit Ranking score (out of 20 points) will be calculated as:

**Score = 10 + 10 × (Your Profit - Minimum Profit) / (Maximum Profit - Minimum Profit)**

Where:
- **Maximum Profit** = highest profit achieved by any team
- **Minimum Profit** = lowest profit achieved by any team
- **Your Profit** = your team's total profit on the test set

This means:
- Top-performing team receives **20 points**
- Lowest-performing team receives **10 points** (if valid predictions submitted)
- All other teams are scaled linearly between 10-20 based on relative performance
- Invalid submissions (wrong format, missing loans) receive **0 points**

**Extra Credit:** 

- Top team from each section presents on final day: +5 points

---

## Final Presentations

On the last day of class, we will:

1. Reveal the actual outcomes and calculate each team's profit
2. Announce the leaderboard
3. Have the **top-performing team from each section** present their findings and strategy (10 minutes each)

Teams selected to present will receive extra credit points.

---


**Good luck! See you on presentation day for the big reveal!**







