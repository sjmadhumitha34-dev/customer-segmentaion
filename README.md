# 📊 RFM Customer Segment Calculator

An interactive **Streamlit app** that calculates **RFM (Recency, Frequency, Monetary)** scores and assigns customer segments based on transaction metrics.  
This tool helps businesses analyze customer behavior and design targeted engagement strategies.

---

## 🚀 Features
- **Interactive Inputs**: Enter customer metrics via sidebar controls.
- **Automated Scoring**: Calculates R, F, and M scores on a 1–5 scale.
- **Segment Classification**: Maps customers into segments (Champions, Loyal, At-Risk, etc.).
- **Visual Insights**:
  - Metrics dashboard
  - RFM code display
  - Radar chart visualization
  - Strategy recommendations

---

## 🧮 RFM Scoring Logic
- **Recency (R)**: Days since last purchase  
  - Lower days → Higher score
- **Frequency (F)**: Number of orders  
  - More orders → Higher score
- **Monetary (M)**: Total spend ($)  
  - Higher spend → Higher score

Each metric is scored from **1 (lowest)** to **5 (highest)**.

---

## 📂 Customer Segments
| Segment | Icon | Strategy |
|---------|------|----------|
| Champions / VIPs | 🏆 | Reward loyalty, exclusive access, VIP support |
| Loyal Customers | 💙 | Upsell, subscriptions, encourage reviews |
| New / Promising | 🌱 | Onboarding emails, discounts, guides |
| At-Risk / Need Attention | ⚠️ | Re-engagement offers, win-back campaigns |
| Hibernating / Lost | 💤 | Aggressive discounts or reduce spend |
| Potential Loyalists | 📈 | Membership programs, complementary items |

---

## 📊 Example Output
- **Recency Score**: 4 / 5  
- **Frequency Score**: 3 / 5  
- **Monetary Score**: 5 / 5  
- **Combined RFM Code**: 435  
- **Segment**: 💙 Loyal Customers  
- **Recommended Strategy**: Upsell higher-value products, recommend subscriptions, and encourage reviews.

---



