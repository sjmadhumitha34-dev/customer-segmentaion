# customer-segmentaionimport os
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.cluster import KMeans
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import silhouette_score

# Ensure data directory exists to prevent FileNotFoundError
os.makedirs("data", exist_ok=True)

# Load data
df = pd.read_csv("data/customer_data.csv")
print("Shape:", df.shape)
print(df.head())
print("\nMissing values:\n", df.isnull().sum())
print("\nDuplicates:", df.duplicated().sum())

# Clean Data (Fixed SettingWithCopy Warning)
target_cols = ["Age", "AnnualIncome", "SpendingScore", "Purchases"]
df[target_cols] = df[target_cols].fillna(df[target_cols].median())
df = df.drop_duplicates().reset_index(drop=True)

# Feature engineering
df["CustomerValue"] = (df["AnnualIncome"] * df["SpendingScore"]) / 100

# Scaling
features = ["AnnualIncome", "SpendingScore", "Purchases"]
scaler = StandardScaler()
X_scaled = scaler.fit_transform(df[features])

# Evaluate best K using both Elbow (Inertia) & Silhouette Scores
inertia = []
silhouette_scores = []
k_range = range(2, 11)

for k in k_range:
    model = KMeans(n_clusters=k, random_state=42, n_init=10)
    cluster_labels = model.fit_predict(X_scaled)
    inertia.append(model.inertia_)
    silhouette_scores.append(silhouette_score(X_scaled, cluster_labels))

# Plotting metrics side-by-side to choose optimal K
fig, ax1 = plt.subplots(figsize=(8, 5))
ax1.plot(k_range, inertia, marker="o", color="b", label="Inertia (Elbow)")
ax1.set_xlabel("Number of Clusters (k)")
ax1.set_ylabel("Inertia", color="b")
ax1.tick_params(axis="y", labelcolor="b")

ax2 = ax1.twinx()
ax2.plot(k_range, silhouette_scores, marker="s", color="r", label="Silhouette Score")
ax2.set_ylabel("Silhouette Score", color="r")
ax2.tick_params(axis="y", labelcolor="r")

plt.title("Elbow Method & Silhouette Score Evaluation")
plt.tight_layout()
plt.show()

# --- Run K-Means with the chosen cluster size ---
# TIP: Look at the plot generated above. Pick the k where Silhouette peaks or Inertia bends.
optimal_k = 4  
kmeans = KMeans(n_clusters=optimal_k, random_state=42, n_init=10)
df["Cluster"] = kmeans.fit_predict(X_scaled)

print(f"Final Silhouette Score (k={optimal_k}):", round(silhouette_score(X_scaled, df["Cluster"]), 3))

# Segmentation chart
plt.figure(figsize=(10, 7))
sns.scatterplot(data=df, x="AnnualIncome", y="SpendingScore", hue="Cluster", palette="Set2", s=90)
plt.title(f"Customer Segmentation using K-Means (k={optimal_k})")
plt.show()

# Summary
cluster_summary = df.groupby("Cluster").agg(
    Customers=("CustomerID", "count"),
    AvgAge=("Age", "mean"),
    AvgIncome=("AnnualIncome", "mean"),
    AvgSpending=("SpendingScore", "mean"),
    AvgPurchases=("Purchases", "mean"),
    AvgCustomerValue=("CustomerValue", "mean")
).round(2)

print("\n--- Cluster Summary Table ---")
print(cluster_summary)

# Save results
df.to_csv("data/customer_segmented.csv", index=False)
cluster_summary.to_csv("data/cluster_summary.csv")
