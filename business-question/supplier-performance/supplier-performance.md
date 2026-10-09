# Supplier Performance

Business Scenario:

A company works with multiple suppliers. Management wants to know which suppliers are consistently delivering late.

You have a dataset containing:
- Supplier
- Order_ID
- Promised_Date
- Actual_Delivery_Date

Interview Question:

Using pandas, calculate the numver of late orders per supplier and identify the supplier with the hiestest late-delivery rate.

```python
df["Promised_Date"] = pd.to_datetime(df["Promised_Date"])
df["Actual_Delivery_Date"] = pd.to_datetime(df["Actual_Delivery_Date"])

df["Late"] = df["Actual_Delivery_Date"] > df["Promised_Date"]

supplier_performance = (
    df.groupby("Supplier")
      .agg(
          Total_Orders=("Order_ID", "count"),
          Late_Orders=("Late", "sum")
      )
)

supplier_performance["Late_Rate"] = (
    supplier_performance["Late_Orders"] /
    supplier_performance["Total_Orders"]
)

supplier_performance = supplier_performance.sort_values(
    "Late_Rate",
    ascending=False
)

print(supplier_performance)
```

Interpretation:

Suppose you get:
| Supplier | Total Orders | Late Orders | Late Rate |
|---|---:|---:|---:|
| Supplier A | 100 | 8 | 8% |
| Supplier B | 80 | 20 | **25%** |
| Supplier C | 120 | 12 | 10% |

Supplier B has the worst performance at 25% late deliveries.
