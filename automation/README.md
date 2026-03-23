
<a href="https://github.com/DoubleZero00a19b16">
  <img src="https://github.com/DoubleZero00a19b16.png" width="60px" style="border-radius:50%;" alt="DoubleZero00a19b16's Profile"/>
</a>

**Contributor:** [DoubleZero00a19b16](https://github.com/DoubleZero00a19b16)

- **Development Team**: Epsilon Automation Engineer
- **Role**: Automation Engineer

---

# n8n Automation: Smart Product Intelligence Engine

## 🤖 What is This n8n Workflow?

The n8n automation system is the **brain behind Epsilon's product decisions**. It runs automatically in the background to:

1. **Validate product ratings** - Identifies real feedback vs spam
2. **Build user trust scores** - Learns which customers give reliable feedback
3. **Generate smart recommendations** - Decides what OBA should do with each product (stock more, discount, promote, etc.)

Think of it as a **24/7 analyst** that processes customer feedback and tells OBA exactly what action to take.

---

## 🔄 How It Works (Simple Version)

### The Workflow Has 3 Main Stages:

```
Stage 1: Rating Validation
   ↓
   Checks if ratings are real or spam
   
Stage 2: User Trust Scoring  
   ↓
   Learns which customers are reliable
   
Stage 3: Business Recommendations
   ↓
   Generates smart actions for products
```

---

## 1️⃣ Rating Validation (Quality Control)

### What It Does
When a customer submits a rating, the system checks if it's **genuine feedback or spam**.

### How It Detects Problems
- **Extreme ratings** - When a rating is way too different from others
- **Inconsistent behavior** - When a user keeps changing their ratings wildly
- **Too fast submissions** - When ratings come in suspiciously quick (bot-like)
- **Missing details** - When comments are empty (less trustworthy)

### Result
Each rating gets a **reliability score** (0-100%) showing how much we should trust it.

---

## 2️⃣ User Trust Scoring (Building Reputation)

### What It Does
Learns about each customer based on their rating history.

### How It Works
- **If you give consistent, realistic ratings** → Your trust score goes UP ⬆️
- **If your ratings seem spam-like** → Your trust score goes DOWN ⬇️
- **Over time** → The system learns to weight your feedback more heavily

### Real-World Impact
- **Trusted users** → Their feedback influences recommendations more
- **Spam users** → Their feedback is automatically downweighted
- **New users** → Start with neutral trust, build reputation over time

---

## 3️⃣ Smart Recommendations (Business Decisions)

### What It Does
Takes product performance data and generates **actionable recommendations**.

### Types of Decisions Made

#### Inventory (Stock Management)
- **START_RESTOCK** - Product is popular & rated well → Buy more
- **STOP_RESTOCK** - Product isn't selling & poorly rated → Phase it out
- **TRANSFER_STOCK** - Move inventory between markets based on demand

#### Pricing
- **INCREASE_PRICE** - High demand + great ratings → Customers will pay more
- **DECREASE_PRICE** - Slow sales but decent ratings → Lower price to boost volume
- **DISCOUNT_PRODUCT** - Inventory buildup → Clear stock with discounts

#### Marketing
- **PROMOTE_PRODUCT** - Strong performer → Feature on app & in stores

#### Based On
- Sales volume (how many units sold)
- Rating score (customer satisfaction)
- Customer reviews (what they actually say)
- Local vs global performance (market-specific data)

---

## 🔍 Why This Matters

### Before (Without Automation)
- ❌ Decisions made based on gut feeling
- ❌ Spam reviews mislead inventory planning
- ❌ Responses to customer feedback are slow
- ❌ No way to compare customer reliability

### After (With n8n)
- ✅ Decisions backed by verified customer data
- ✅ Spam automatically filtered out
- ✅ Recommendations generated in real-time
- ✅ Trustworthy customers weighted more heavily
- ✅ Same action never created twice (deduplication)

---

## ⚡ Key Features

### Automated & Scheduled
- Runs automatically at set intervals
- No manual intervention needed
- Works 24/7

### Smart Spam Detection
- Uses statistical analysis to spot fake ratings
- Learns user behavior patterns
- Adapts over time

### Business Rule Engine
- Applies company policies automatically
- Generates structured, API-ready recommendations
- Prevents duplicate recommendations

### Integrated with Epsilon
- Connects directly to our database
- Sends updates via API
- Marks processed items to avoid reprocessing

---

## 📊 Data Flow Overview

```
Customer Submits Rating
        ↓
Reliability Check (Spam Detection)
        ↓
Update Rating Trust Score
        ↓
Update User Trust Score
        ↓
Gather Product Performance Data
        ↓
Apply Business Rules
        ↓
Generate Recommendations
        ↓
Check if Already Exists
        ↓
Create New Recommendation (if needed)
```

---

## 💡 Real-World Example

### Scenario: New Product X

**Week 1:**
- 100 customers buy Product X
- Average rating: 4.8 stars ⭐⭐⭐⭐⭐
- Comments are detailed and positive

**n8n Action:**
1. ✅ Ratings pass validation (not spam)
2. ✅ Users are trusted → reliability 95%+
3. ✅ Recommendation generated: **"START_RESTOCK"**
- Reason: High sales, high ratings, strong demand

**Result:** OBA stocks more of Product X because the data is solid.

---

### Counter Example: Suspicious Rating

**User Y submits:**
- Rating: 1 star (completely different from their history)
- No comment
- Submitted 0.5 seconds after previous rating

**n8n Detection:**
1. ❌ User's pattern doesn't match their history
2. ❌ Too fast (likely bot behavior)
3. ❌ No comment (less credible)
- Rating reliability: 30%
- User trust score: Down

**Result:** This rating is included but weighted very low.

---

## 🎯 Business Outcomes

| Outcome | Impact |
|---------|--------|
| **Better Inventory** | Stock the right products, avoid wasted inventory |
| **Smarter Pricing** | Maximize revenue by pricing based on demand |
| **Faster Decisions** | Recommendations generate automatically, not manually |
| **Data Quality** | Spam filtered = better decision-making |
| **Customer Trust** | Reliable customers influence business more |

---

## 🔒 Safety Features

### No Action Duplicates
- System checks if a recommendation already exists
- Never creates the same recommendation twice
- Prevents database bloat

### Status Tracking
- Each processed rating marked as "done"
- Prevents reprocessing the same data
- Maintains system integrity

### Error Handling
- Validates all data before processing
- Graceful failure handling
- System logs all actions

---

## 📈 Metrics the System Tracks

- **Rating Reliability** - How trustworthy is each rating? (0-100%)
- **User Reliability** - How trustworthy is each customer? (0-100%)
- **Suspicion Score** - How likely is a rating to be spam?
- **Variance** - How inconsistent is user behavior?
- **Product Performance** - Sales, ratings, reviews per market
- **Recommendation Count** - How many actions generated per cycle

---

## 🚀 When Does It Run?

The workflow is **scheduled** to run automatically at regular intervals:
- Processing happens behind the scenes
- No manual trigger needed
- Consistent, predictable operation

---

## 🔗 How It Connects to Epsilon

The n8n system is the **automation backbone** that:

1. **Feeds from:** Customer ratings and reviews from the Epsilon app
2. **Processes:** Validates and analyzes the feedback
3. **Outputs to:** Product recommendations visible in the OBA dashboard
4. **Updates:** User trust scores in the database

---

## 💬 Summary

**n8n = Automated Brain**

Instead of humans reading through thousands of ratings and guessing what to do, n8n:
- ✅ Reads all feedback automatically
- ✅ Spots fake/spam ratings
- ✅ Learns which customers are trustworthy
- ✅ Generates business recommendations
- ✅ Updates OBA systems in real-time

All **24 hours a day, 7 days a week**, without making mistakes.

---

## 📚 Want More Details?

For technical documentation, see:
- `reports/n8n/rating_spam_detection.md` - How spam detection works
- `reports/n8n/main_recommendation_decision_node.md` - Business rules explained
- `reports/n8n/n8n_system_documentation.md` - Complete technical documentation

---

**Status:** ✅ Active & Running  
**Purpose:** Automated Quality & Business Intelligence  
**Part of:** Epsilon Platform  
**Last Updated:** March 2026
