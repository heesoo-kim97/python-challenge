# Analyze Warehouse Order Fulfillment Performance

Business Scenario: A beverage distribution company operates three warehouses.
Management wants to understand which warehouse is fulfilling customer orders most efficiently.

You are given a dataset containing:
- Warehouse
- Order_ID
- Order_Date
- Ship_Date
- Expected_Ship_Date

Using Python and pandas:
1. Calculate the number of days each order took to ship.
2. Identify whether each order was shipped late.
3. Calculate the average shipping time and late shipment rate for each warehouse.
4. Determine which warehouse has the worst fulfillment performance.

Sample Data
```python

import pandas as pd

data = {
    "Warehouse": ["LA", "LA", "NY", "NY", "TX", "TX"],
    "Order_ID": [101, 102, 103, 104, 105, 106],
    "Order_Date": [
        "2026-10-01", "2026-10-02",
        "2026-10-01", "2026-10-03",
        "2026-10-02", "2026-10-04"
    ],
    "Ship_Date": [
        "2026-10-02", "2026-10-05",
        "2026-10-04", "2026-10-04",
        "2026-10-03", "2026-10-07"
    ],
    "Expected_Ship_Date": [
        "2026-10-03", "2026-10-03",
        "2026-10-03", "2026-10-05",
        "2026-10-04", "2026-10-05"
    ]
}

df = pd.DataFrame(data)

```

Solution
```python

# Convert columns to datetime
date_columns = ["Order_Date", "Ship_Date", "Expected_Ship_Date"]

for col in date_columns:
    df[col] = pd.to_datetime(df[col])

# Calculate shipping duration
df["Shipping_Days"] = (
    df["Ship_Date"] - df["Order_Date"]
).dt.days

# Identify late shipments
df["Late"] = df["Ship_Date"] > df["Expected_Ship_Date"]

# Aggregate warehouse performance
performance = df.groupby("Warehouse").agg(
    Average_Shipping_Days=("Shipping_Days", "mean"),
    Total_Orders=("Order_ID", "count"),
    Late_Orders=("Late", "sum")
)

# Calculate late shipment rate
performance["Late_Rate"] = (
    performance["Late_Orders"] / performance["Total_Orders"] * 100
)

print(performance.sort_values("Late_Rate", ascending=False))

```

