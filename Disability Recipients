from google.colab import drive
drive.mount('/content/drive')
import os

os.chdir('/content/drive/MyDrive')

print(os.getcwd())





# =====================================================
# VA Disability Compensation Analysis
# =====================================================

import pandas as pd
import matplotlib.pyplot as plt

# -----------------------------------------------------
# Load Data
# -----------------------------------------------------

file_path = "export.csv"

df = pd.read_csv(file_path)

# -----------------------------------------------------
# Convert Numeric Columns
# -----------------------------------------------------

for col in df.columns[3:]:
    df[col] = pd.to_numeric(
        df[col].astype(str).str.replace(",", ""),
        errors="coerce"
    )

# -----------------------------------------------------
# Overall Dataset Metrics
# -----------------------------------------------------

total_recipients = df["Total: Disability Compensation Recipients"].sum()

average_recipients = df[
    "Total: Disability Compensation Recipients"
].mean()

largest_county = df.loc[
    df["Total: Disability Compensation Recipients"].idxmax(),
    [
        "State",
        "County Name",
        "Total: Disability Compensation Recipients"
    ]
]

male_total = df["Male"].sum()
female_total = df["Female"].sum()

male_pct = (
    male_total /
    (male_total + female_total)
) * 100

female_pct = (
    female_total /
    (male_total + female_total)
) * 100

# -----------------------------------------------------
# Print Overall Metrics
# -----------------------------------------------------

print("=" * 60)
print("OVERALL METRICS")
print("=" * 60)

print(f"Total Recipients: {total_recipients:,.0f}")
print(f"Average Recipients per County: {average_recipients:,.0f}")

print("\nLargest County:")
print(largest_county)

print(f"\nMale Recipients: {male_pct:.2f}%")
print(f"Female Recipients: {female_pct:.2f}%")

# -----------------------------------------------------
# Top States
# -----------------------------------------------------

top_states = (
    df.groupby("State")
      ["Total: Disability Compensation Recipients"]
      .sum()
      .sort_values(ascending=False)
      .head(10)
)

print("\n")
print("=" * 60)
print("TOP 10 STATES")
print("=" * 60)

print(top_states)

# -----------------------------------------------------
# SCD Rating Analysis
# -----------------------------------------------------

rating_cols = [
    "SCD rating: 0% to 20%",
    "SCD rating: 30% to 40%",
    "SCD rating: 50% to 60%",
    "SCD rating: 70% to 90%",
    "SCD rating: 100%"
]

rating_totals = (
    df[rating_cols]
    .sum()
    .sort_values(ascending=False)
)

rating_pct = (
    rating_totals /
    rating_totals.sum()
) * 100

rating_results = pd.DataFrame({
    "Recipients": rating_totals,
    "Percent": rating_pct.round(2)
})

print("\n")
print("=" * 60)
print("SCD RATING DISTRIBUTION")
print("=" * 60)

print(rating_results)

# -----------------------------------------------------
# High Disability Levels (70%+)
# -----------------------------------------------------

high_disability = (
    df["SCD rating: 70% to 90%"].sum()
    + df["SCD rating: 100%"].sum()
)

high_disability_pct = (
    high_disability /
    total_recipients
) * 100

print("\n")
print("=" * 60)
print("HIGH DISABILITY (70%+)")
print("=" * 60)

print(f"Recipients with Rating 70%+: {high_disability:,.0f}")
print(f"Percent of All Recipients: {high_disability_pct:.2f}%")

# -----------------------------------------------------
# Visualization
# -----------------------------------------------------

plt.figure(figsize=(12, 6))

colors = [
    "#1f77b4",
    "#2ca02c",
    "#ff7f0e",
    "#d62728",
    "#9467bd"
]

bars = plt.bar(
    rating_results.index,
    rating_results["Recipients"],
    color=colors
)

plt.title(
    "Disability Compensation Recipients by SCD Rating",
    fontsize=15,
    fontweight="bold"
)

plt.ylabel("Recipients")
plt.xlabel("SCD Rating")

plt.xticks(rotation=20)

for bar in bars:
    plt.text(
        bar.get_x() + bar.get_width()/2,
        bar.get_height(),
        f"{int(bar.get_height()):,}",
        ha="center",
        va="bottom",
        fontsize=9
    )

plt.tight_layout()
plt.show()
