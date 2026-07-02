# Project_4_Data_Visualization
# Orders Performance Dashboard - Power BI

A three-page Power BI dashboard built on 1,200 retail orders spanning January 2023 to June 2025. The goal was to move beyond surface-level reporting and tell a clear business story: where revenue is going, where it is being lost, and what customer behavior actually looks like underneath the numbers.

---

## The Story This Dashboard Tells

Most dashboards stop at "here is your revenue." This one goes further.

**Page 1 - Overview** sets the baseline: $1.26M in total revenue, 1,200 orders, $1,054 average order value across seven product categories. Revenue holds steady month to month with no clear growth trend, which by itself is worth surfacing to leadership.

**Page 2 - Cancellations and Returns** is where the real finding lives. 41.1% of total revenue, roughly $520K, never actually reaches the customer. It gets cancelled or returned before completion. That is not a product quality problem, the losses are spread almost evenly across all seven products and all five payment methods. That pattern points to something in the process, checkout friction, fulfillment gaps, or post-purchase experience, rather than a single weak SKU.

**Page 3 - Customer Behavior** closes the story. Of 1,189 unique customers, only 11 came back for a second order. That is a 0.9% repeat purchase rate. Meanwhile all five acquisition channels perform within 17% of each other in revenue terms, and coupons shift average order value by just $34. The data says the business has an acquisition-heavy model that is not converting first-time buyers into loyal customers.

---

## Dashboard Pages

### Page 1 - Overview
- KPI cards: Total Revenue, Total Orders, Avg Order Value, Lost Revenue (flagged red)
- Monthly revenue trend line, Jan 2023 to Jun 2025
- Revenue by product, horizontal bar chart
- Orders by status, column chart with status-coded colors

### Page 2 - Cancellations and Returns
- KPI cards: Lost Revenue ($520K), Revenue lost to cancellations (41.1%), Cancelled + Returned order count (497)
- Stacked bar showing completed vs lost revenue side by side
- Lost revenue breakdown by product
- Cancelled vs returned counts per product, grouped columns

### Page 3 - Customer Behavior
- KPI cards: Unique Customers (1,189), Customers who came back (11), Repeat Purchase Rate (0.9%, flagged red)
- Revenue by referral source, horizontal bar
- Revenue by payment method, horizontal bar
- Average order value by coupon code, column chart

---

## Key Findings

- Revenue is flat across the 30-month period, no growth, no decline
- $519,674 lost to cancellations and returns, spread evenly across all products and channels
- Chair has the highest cancellation count (45 orders); Tablet has the highest return count (43 orders)
- Repeat purchase rate is 0.9%, meaning almost every customer buys exactly once
- Coupons do not meaningfully change basket size, the difference between using one and not is just $34

---

## DAX Measures Written

```dax
Total Revenue = SUM(Orders[TotalPrice])

Total Orders = COUNTROWS(Orders)

Avg Order Value = DIVIDE([Total Revenue], [Total Orders])

Lost Revenue =
CALCULATE(
    [Total Revenue],
    Orders[OrderStatus] IN {"Cancelled", "Returned"}
)

Lost Revenue % = DIVIDE([Lost Revenue], [Total Revenue])

Completed Revenue = [Total Revenue] - [Lost Revenue]

Cancelled Count = CALCULATE([Total Orders], Orders[OrderStatus] = "Cancelled")

Returned Count = CALCULATE([Total Orders], Orders[OrderStatus] = "Returned")

Unique Customers = DISTINCTCOUNT(Orders[CustomerID])

Repeat Customers =
SUMX(
    FILTER(
        VALUES(Orders[CustomerID]),
        CALCULATE(COUNTROWS(Orders)) > 1
    ),
    1
)

Repeat Customer % = DIVIDE([Repeat Customers], [Unique Customers])
```

---

## Data Model

- Single fact table: Orders (1,200 rows, 12 columns after cleaning)
- Separate DateTable built with DAX CALENDAR function, Jan 2023 to Jun 2025
- One-to-many relationship: DateTable[Date] to Orders[Date]
- Removed columns: ShippingAddress (all generic values, no analytical use), TrackingNumber (no analytical value)

---

## Tools Used

- Power BI Desktop
- Power Query for cleaning and column removal
- DAX for all measures and the DateTable

---

## Dataset

1,200 retail orders across 7 product categories: Laptop, Monitor, Tablet, Phone, Printer, Chair, Desk.

Covers January 2023 to June 2025. Order statuses: Delivered, Shipped, Pending, Cancelled, Returned. Acquisition channels: Instagram, Email, Google, Facebook, Referral. Payment methods: Credit Card, Debit Card, Cash, Gift Card, Online.

---

## Screenshots

*(Upload your three dashboard page screenshots here)*

---

## Project Context

Built as the capstone visualization project for the DecodeLabs Data Analytics internship, Batch 2026. The brief required boardroom-ready insights using the SCR storytelling framework (Situation, Complication, Resolution) rather than generic chart production.
```
