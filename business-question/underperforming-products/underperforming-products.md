# Identifying Underperforming Products by Revenue

## Business Scenario

A beverage distribution company sells several products across different regions. Management wants to identify which products generate the most revenue and which products may need additional attention.

You receive a dataset containing:
- Product
- Region
- Units_Sold
- Unit_Price
- Unit_Cost

Using Python and pandas:
1. Calculate total revenue for each transaction.
2. Calculate total profit for each transaction.
3. Group the data by product.
4. Calculate total revenue, total profit, and units sold for each product.
5. Identify the product generating the lowest total profit.

Sample Data:

```python
import pandas as pd

data = {
    "Product": ["Energy Drink", "Water", "Juice",
                "Energy Drink", "Water", "Juice"],
    "Region": ["West", "West", "East",
               "East", "South", "South"],
    "Units_Sold": [100, 200, 150, 80, 250, 100],
    "Unit_Price": [3, 1, 2, 3, 1, 2],
    "Unit_Cost": [1.5, 0.4, 1.2, 1.5, 0.4, 1.2]
}

df = pd.DataFrame(data)
```

## Solution

```python
# Calculate revenue
df["Revenue"] = df["Units_Sold"] * df["Unit_Price"]

# Calculate profit
df["Profit"] = (
    df["Units_Sold"] * (df["Unit_Price"] - df["Unit_Cost"])
)

# Aggregate by product
product_performance = df.groupby("Product").agg(
    Total_Revenue=("Revenue", "sum"),
    Total_Profit=("Profit", "sum"),
    Total_Units_Sold=("Units_Sold", "sum")
)

# Sort by profit
product_performance = product_performance.sort_values(
    "Total_Profit",
    ascending=True
)

print(product_performance)
```

## interpretation

Expected output:
| Product |	Revenue |	Profit |	Units Sold |
| --- | --- | --- | --- |
| Juice |	$500 |	$200 |	250 |
| Water |	$450 |	$270 |	450  |
| Energy | Drink |	$540 |	$270 |	180 |

Juice generates the lowest total profit at $200.
