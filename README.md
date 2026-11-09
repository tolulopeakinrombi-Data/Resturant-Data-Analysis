# Restaurant Sales Analysis (Power BI)

An end-to-end Power BI project that cleans, models and analyzes **500 restaurant orders** to answer practical business questions: when do customers order, which cuisines and zones perform best, how do customers pay, and how good is the delivery experience?

![Dashboard](dashboard.png)

## Key Numbers

| Metric | Value |
|---|---|
| Total Sales | 299,071 |
| Total Orders | 500 |
| Items Sold | 2,326 |
| Average Order Value | 598.14 |
| Average Delivery Time | 30.54 mins |
| Average Food Rating | 3.36 / 5 |
| Average Delivery Rating | 2.99 / 5 |
| Customers | 21 |

## Dashboard Overview

The one-page dashboard contains:

| Visual | What it shows |
|---|---|
| KPI cards | Total sales, items sold, total orders and number of customers |
| Gauges | Average delivery rating and average food rating (scale 1 to 5) |
| Donut chart | Total sales by payment type (Card vs Cash) |
| Bar chart | Total orders by restaurant |
| Bar chart | Total orders by customer |
| Treemap | Total orders by time of day |
| Bar chart | Total orders by cuisine |
| Treemap | Total orders by zone |
| Slicer | Filter the whole page by time of day (Morning, Afternoon, Evening, Night) |

## Tools Used

- **Power BI Desktop** for modelling and the dashboard
- **Power Query** for data cleaning and transformation
- **DAX** for measures

## Dataset

Two Excel files, linked by restaurant ID:

| File | Rows | Columns |
|---|---|---|
| `Orders.xlsx` | 500 | Order ID, Customer Name, Restaurant ID, Order Date, Quantity of Items, Order Amount, Payment Mode, Delivery Time Taken (mins), Customer Rating-Food, Customer Rating-Delivery |
| `Restaurants.xlsx` | 20 | RestaurantID, RestaurantName, Cuisine, Zone, Category |

**Data quality:** no blank values, no duplicate order IDs, and every order matches a restaurant. All orders are from a single day (1 January 2022).

## Process

1. **Load** both Excel files into Power Query and set data types.
2. **Clean:** rename the rating columns to `Food Rating` and `Delivery Rating`, and rename `RestaurantID` to `Restaurant ID`.
3. **Transform:**
   - Add an `Hour` column from `Order Date`.
   - Create `Time of Day` (Morning before 12:00, Afternoon 12:00 to 16:59, Evening 17:00 to 18:59, Night from 19:00).
   - Create `Payment Type` (Cash on Delivery = Cash, everything else = Card).
4. **Model:** relate the tables as a star schema, `Restaurant Details[Restaurant ID]` (one) to `Orders Data[Restaurant ID]` (many), instead of merging into one flat table.
5. **Measures:** build the DAX measures below.
6. **Visualize:** build a one-page dashboard with KPI cards, gauges, bar charts, treemaps, a donut chart and a time-of-day slicer.

## DAX Measures

```DAX
Total Sales         = SUM('Orders Data'[Order Amount])
Total Orders        = COUNTROWS('Orders Data')
Items Sold          = SUM('Orders Data'[Quantity of Items])
Avg Delivery Time   = AVERAGE('Orders Data'[Delivery Time Taken (mins)])
Avg Food Rating     = AVERAGE('Orders Data'[Food Rating])
Avg Order Value     = DIVIDE([Total Sales], [Total Orders])
Avg Delivery Rating = AVERAGE('Orders Data'[Delivery Rating])
```

`COUNTROWS` is used for orders (500) and `SUM` for money (299,071), so the two are never mixed up. `DIVIDE` is used instead of `/` to avoid errors when the denominator is zero.

## Key Findings

- **Orders peak at lunch.** Afternoon accounts for 217 of 500 orders (43.4%), and 2 PM alone has 99 orders. There are no orders at 4 PM.
- **Chinese and North Indian cuisines lead** with 81 orders each. North Indian also has the best food rating (3.59).
- **Zone D has the most orders (213) but also 9 of the 20 restaurants.** Per restaurant, Zone C performs best (28.7 orders vs 23.7 in Zone D).
- **Delivery is the weak spot.** The delivery rating (2.99) trails the food rating (3.36). Belgian restaurants are lowest at 2.64.
- **Card payments dominate:** 66.2% of orders and 67.2% of sales.
- **A small, active customer base:** 21 customers placed all 500 orders.

## Recommendations

1. Add kitchen and delivery capacity from 12:00 to 15:00, with extra cover at 2 PM.
2. Test a mid-afternoon promotion to fill the 4 PM gap.
3. Improve delivery quality, starting with the lowest-rated restaurants.
4. Choose locations by orders per restaurant, not total orders.
5. Feature Chinese and North Indian menus.
6. Introduce loyalty rewards and acquisition campaigns to protect and grow the customer base.

## Limitations

- The data covers **one day**, so it shows within-day patterns and not weekly or monthly trends.
- Order amounts have no stated currency.
- There are only 21 customers, so customer-level patterns come from a small group.
- 16 orders have amounts below 50 and may be worth verifying. They were kept in the analysis.

## Repository Contents

```
.
├── README.md
├── dashboard.png                                      # Dashboard screenshot
├── Restaurant_Sales_Dashboard.pbix                    # Power BI file
├── data/
│   ├── Orders.xlsx
│   └── Restaurants.xlsx
└── reports/
    ├── Restaurant_Sales_Analysis_Report.pdf           # Full project report
    └── Restaurant_Sales_Key_Findings_and_Recommendations.docx
```

## How to Use

1. Download the repository.
2. Open `Restaurant_Sales_Dashboard.pbix` in Power BI Desktop.
3. If Power BI asks, point the data source to the files in the `data` folder (Home, Transform data, Data source settings).

## Author

**Tolulope Akinrombi**
LinkedIn: _add your profile link here_# Resturant-Data-Analysis
Power BI dashboard analyzing 500 restaurant orders: cleaned with Power Query, modelled with DAX, and used to find lunch-peak patterns, top cuisines, zone performance and delivery gaps.
