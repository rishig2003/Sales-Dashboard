# Sales-Dashboard

An interactive Tableau dashboard that turns raw order, customer, product and location data into a year-over-year view of sales performance and customer behaviour.

**File:** `SaleDash.twb` (Tableau workbook) · **Dashboards:** Sales Dashboard, Customer Dashboard · **Years covered:** 2020 – 2023 (selectable)

---

## 1. Problem Statement

Sales data usually lives in several flat files (orders, customers, products, locations). On their own, these files cannot answer the questions a business actually asks:

- Are we growing or shrinking compared with last year?
- Are we growing **profitably**, or only buying revenue?
- Which product sub-categories make money, and which lose it?
- When during the year does the business peak or slump?
- Do we depend on a few big customers, or is demand spread out? Do customers come back?

**This project solves that by consolidating four CSV sources (`Orders`, `Customers`, `Products`, `Location`) into one data model and presenting the answers in two dashboards:**

| Dashboard | Question it answers | Key views |
|---|---|---|
| **Sales Dashboard** | How are sales and profit performing vs. the previous year, and where do they come from? | KPI cards for Sales, Profit and Quantity (current year vs. prior year, with sparklines and min/max markers); Sales & Profit by Sub-Category (profit vs. loss highlighted); Weekly Sales & Profit Trends (weeks flagged above/below average) |
| **Customer Dashboard** | Who are our customers, and how loyal and valuable are they? | KPI cards for Customers, Sales per Customer and Orders (vs. prior year); Customer Distribution by Number of Orders; Top 10 Customers (latest order date, sales, profit, order count) |

**How it works**

- A **Select Year** parameter (2020–2023, default 2023) drives every metric. Current year (CY) is automatically compared against the previous year (PY), so each KPI shows a % change.
- Filters for **Category, Sub-Category, Region, State and City** apply across the dashboard.
- Charts are linked: clicking a sub-category, a week, or an order-frequency bucket filters the rest of the dashboard.

**Data model:** `Orders` joins to `Customers` (on Customer ID), `Location` (on Postal Code) and `Products` (on Product ID). The CSVs are semicolon-delimited and order dates are in `dd/mm/yyyy` format.

---

## 2. Why This Problem

- **Revenue alone is misleading.** A business can post record sales and still lose money through discounting or unprofitable product lines. Showing Sales, Profit and Quantity side by side, each against last year, exposes that gap straight away.
- **Year-over-year is the question leadership asks first.** Building the CY vs. PY comparison into every KPI removes the manual work of exporting data and building the same comparison each reporting cycle.
- **Averages hide the real story.** Sub-category and weekly views show *where* and *when* performance is strong or weak, which is where action can be taken (pricing, promotion, stock, staffing).
- **Customer concentration is a business risk.** Knowing how many customers order once versus repeatedly, and how much the top 10 contribute, shows whether growth is built on a loyal base or on one-off purchases.
- **A single interactive view replaces many static reports.** Self-service filtering by region, state, city and category lets non-technical stakeholders explore the data without writing queries.
- **It is a realistic end-to-end analytics exercise.** It covers calculated fields (CY/PY logic, LOD expressions, window calculations), parameter-driven design, and dashboard interactivity.

---

## 3. Insights and Recommendations

> All figures below are for **2023 vs. 2022** (the dashboard's default year) and were computed from the four source CSVs: 9,994 order lines, 793 customers, orders from 3 Jan 2020 to 30 Dec 2023. Change the **Select Year** parameter to see other years.

### Headline numbers

| KPI | 2023 | 2022 | Change |
|---|---|---|---|
| Sales | $733,215 | $609,206 | **+20.4%** |
| Profit | $93,439 | $81,795 | **+14.2%** |
| Profit margin | 12.7% | 13.4% | **-0.7 pts** |
| Quantity | 12,476 | 9,837 | **+26.8%** |
| Orders | 1,687 | 1,315 | **+28.3%** |
| Customers | 693 | 638 | **+8.6%** |
| Sales per customer | $1,058 | $955 | **+10.8%** |

### Key insights

**1. Growth is strong, but profit is lagging sales (Sales Dashboard, KPI cards)**
Sales grew 20.4% and quantity 26.8%, but profit grew only 14.2%, so the margin slipped from 13.4% to 12.7%. Orders grew faster than sales (+28.3% vs +20.4%), so the average order value fell from about $463 to $435: customers are buying more often but spending less per order. The business has grown every year since 2021 (sales of $484K in 2020, $471K in 2021, $609K in 2022, $733K in 2023), so this is a margin problem, not a demand problem.

**2. Four sub-categories lose money (Sales and Profit by Subcategory)**
| Sub-Category | 2023 Sales | 2023 Profit | Margin | Note |
|---|---|---|---|---|
| Tables | $60.9K | -$8.1K | -13.4% | Average discount 26.5%; 56% of lines discounted above 20% |
| Machines | $43.5K | -$2.9K | -6.6% | Was **+$2.9K** in 2022; average discount 30%; sales fell 22% |
| Supplies | $16.0K | -$1.0K | -6.0% | Loses money despite a low average discount (7%), so this is a cost/price issue |
| Bookcases | $30.0K | -$0.6K | -1.9% | Average discount 21.5% |

The best performers are **Copiers** ($25.0K profit, 39.8% margin), **Accessories** ($15.7K, 26.1%), **Phones** ($12.8K on the largest sales of $105.3K) and **Paper** ($12.0K, 43.5% margin). By category, Furniture earned only **$3.0K on $215K of sales (1.4% margin)**, versus 16.1% for Office Supplies and 18.7% for Technology.

**3. Sales are heavily seasonal, and the peak is the least profitable period (Weekly Trends)**
September–December produced **50.2%** of 2023 sales, and November alone produced $118.4K. January–February produced just 8.8%. Sales were above the weekly average in 22 of 52 weeks, with the strongest weeks being 48, 46 and 44, and the weakest being weeks 21, 8 and 15. Profit does not follow sales: November's margin was **8.2%** versus **25.1%** in March, and April made $933 profit on $36.5K of sales (2.6%). Five weeks in 2023 had a net loss.

**4. Customers are returning and buying more often (Customer Dashboard)**
Orders per customer rose from 2.06 to 2.43. The share of customers with only one order fell from **40.9% to 28.9%** (200 customers), and 52 customers (7.5%) ordered five or more times. Of the 638 customers from 2022, 558 (87.5%) came back; 80 did not. 135 customers were new in 2023.

**5. Profit is concentrated in a few customers, and some are unprofitable (Top 10 Customers)**
The top 10 customers by profit generated **$30.1K, which is 32.2% of total profit from only 10.4% of sales**. They are led by Raymond Buch ($6.8K), Hunter Lopez ($5.0K) and Tom Ashbrook ($4.6K). Three of the ten last ordered more than 90 days before the end of the data: Raymond Buch (25 Sep), Helen Wasserman (4 Sep) and Steven Roelle (17 Jun). At the other end, 146 customers were unprofitable in 2023, losing **$37.5K** in total.

**6. West and East carry the business; Central and South are barely profitable (Region / State filters)**
| Region | Sales | Profit | Margin | Sales YoY | Profit YoY |
|---|---|---|---|---|---|
| West | $250.1K | $43.8K | 17.5% | +33.4% | +82.1% |
| East | $212.2K | $33.0K | 15.5% | +19.4% | +70.3% |
| South | $123.7K | $9.1K | 7.4% | +28.1% | **-50.7%** |
| Central | $147.1K | $7.6K | 5.1% | -0.2% | **-62.1%** |

Nine of 46 states lost money in 2023. The largest losses were Texas (-$8.8K), Illinois (-$6.7K), Pennsylvania (-$5.1K), North Carolina (-$5.1K) and Colorado (-$4.4K). California, New York and Washington together account for 41.7% of sales.

**7. Same Day shipping and the Corporate segment have the thinnest margins**
Same Day shipping earned only a 6.9% margin versus 14.6% for First Class and 12.4% for Standard Class. The Corporate segment's margin was 11.1% versus 13.7% for Consumer and 13.2% for Home Office.

### Recommendations

**1. Cap discounts at 20% and require approval above it:** Lines above 20% lost $47.6K in 2023; this single control is the biggest profit lever in the data. 

**2. Reprice or restructure Tables and Machines:** Both combine heavy discounting with losses. For Supplies, which loses money without discounting, renegotiate supplier cost or raise prices. Review whether Bookcases justify their discount levels. 

**3. Fix Furniture economics:** A 1.4% margin on $215K of sales means the category's volume is not paying for itself. Shift marketing toward Copiers, Accessories, Phones and Paper, where margins are 12–44%. 

**4. Protect margin during the Q4 peak and lift the slow months:** Stock and staff ahead of September–December, but avoid deep discounts in November (8.2% margin). Use targeted promotions in January–February and the weakest weeks (8, 15, 21) to smooth demand.

**5. Convert one-time buyers and win back lapsed customers:** Target the 200 one-order customers and 135 new customers with follow-up offers, and run a win-back campaign for the 80 customers from 2022 who did not return.

**6. Re-engage the top customers who have gone quiet:** Contact Raymond Buch, Helen Wasserman, and Steven Roelle, and give the top 10 dedicated 
account attention, as they produce a third of profit. Review discount levels given to the 146 unprofitable customers.

**7. Run a profitability turnaround in Central and South:** Investigate Texas, Illinois, Pennsylvania, North Carolina, and Colorado (discounts, product mix, shipping cost) and replicate what works in the West.

**8. Add a surcharge or minimum order for same-day shipping:** and review pricing for Corporate accounts.




