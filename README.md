# Inventory & Warehouse Analytics — Stock, Turnover & ABC

**🌐 Language:** **English** · [Українська](README.uk.md)

![Power BI](https://img.shields.io/badge/Power%20BI-Report-F2C811?logo=powerbi&logoColor=black)
![Power Query](https://img.shields.io/badge/Power%20Query-M-217346?logo=microsoftexcel&logoColor=white)
![DAX](https://img.shields.io/badge/DAX-Measures-336791)

End-to-end **Power BI** project analysing the warehouse inventory of a **paint manufacturer & distributor**: stock value, inventory turnover, slow-moving and dead stock, and an **ABC classification** of items. I built it to answer one question a warehouse or supply manager asks constantly — **where is money tied up in stock, and which items deserve tight control?**

> **Scope:** within inventory (TMC), this project focuses on the **goods-for-resale** category. The same approach generalises to other inventory groups — raw materials, finished goods and spare parts.
>
> 🔗 The **production-cost** side of the same company is analysed in a separate project — [**Manufacturing Cost Analytics**](https://github.com/innaanalysts/manufacturing-cost-analytics).

![Overview dashboard](inventory_warehouse_dashboard1.png)

---

## 🎯 Business questions this project answers

- What is the total **stock value**, and how does it change month to month?
- How fast does inventory turn over (**inventory turnover**, **days of supply**)?
- Which items are **slow-moving** or **dead stock** — where money sits frozen?
- How is stock value distributed by **ABC class** (where the value is concentrated)?
- How is stock split across **warehouses** and **categories**?

---

## 📊 Key results (2024)

| Metric | Value |
|---|---|
| Annual consumption value | **≈ 16.7 M ₴** |
| Year-end stock value | **≈ 2.2 M ₴** |
| Inventory turnover | **12.7× / year** |
| Days of supply | **≈ 29 days** |
| Dead stock | **17.4 K ₴ across 4 SKUs** |
| **ABC split** | **A: 12 · B: 15 · C: 21 · D (dead): 4** |

The ABC analysis confirms a textbook Pareto: **~25% of SKUs drive ~80% of consumption value**. Four items had **zero movement all year** — stock sitting on the shelf with money frozen — so I flagged them as a separate class **D (dead stock)**.

---

## 🛠️ Tech stack

- **Power BI** — data model (star schema), DAX measures, 3-page interactive report
- **Power Query (M)** — data cleaning, merges, and the full **ABC classification** pipeline
- **Excel / CSV** — source data

---

## 🗂️ Data model

A **star schema** with two fact tables (`Stock Movements`, `Inventory Snapshots`) sharing three dimensions (`Products`, `Warehouses`, `Calendar`).

![Data model](inventory_warhouse_data_model.png)

| Table | Role | Description |
|---|---|---|
| `Products` | Dimension | 48 goods with category, cost, price, supplier + **ABC class** |
| `Warehouses` | Dimension | 2 warehouses (Kyiv, Lviv) |
| `Calendar` | Dimension | 2024 date table (month, quarter, year) |
| `Stock Movements` | Fact | Receipts & issues transactions |
| `Inventory Snapshots` | Fact | Month-end stock on hand per item/warehouse |

---

## 🔧 Power Query highlights

The data transformation is the backbone of this project:

- **Merged** `unit_cost` / `category` from `Products` into the fact tables and computed value columns (`movement_value`, `stock_value`).
- Built the **ABC classification entirely in Power Query**: filtered issues → grouped by SKU → sorted by consumption value → running total → cumulative % → conditional class (A ≤ 80%, B ≤ 95%, C otherwise).
- Broke a **circular reference** with an independent cost-lookup query — a real modelling problem, solved cleanly.
- Detected **dead stock**: items with no issues fall out of the ABC and were labelled class **D**, turning a `null` into a business signal.

## 🧮 Key DAX measures

`Total Issues Value` · `Closing Stock Value` · `Avg Stock Value` · **`Inventory Turnover`** · **`Days of Supply`** · **`Dead Stock Value`** · `Cumulative Consumption %` (for the Pareto curve).

---

## 📈 Report pages

**1. Overview** — KPIs, stock-value trend, split by warehouse and category, key insights.
![Overview](inventory_warehouse_dashboard1.png)

**2. ABC Analysis** — Pareto chart (bars + cumulative %), value by ABC class, top products.
![ABC Analysis](inventory_warehouse_dashboard2.png)

**3. Slow-moving & Dead Stock** — dead-stock table (no movement), slow movers by days of supply, action recommendations.
![Slow-moving & Dead Stock](inventory_warehouse_dashboard3.png)

---

## 📁 Repository structure

```
├── data/
│   ├── products.csv
│   ├── warehouses.csv
│   ├── stock_movements.csv
│   └── inventory_snapshots.csv
├── inventory_warehouse.pbix          # Power BI report (model + 3 pages)
├── inventory_warehouse_dashboard1.png
├── inventory_warehouse_dashboard2.png
├── inventory_warehouse_dashboard3.png
└── inventory_warhouse_data_model.png
```

---

> **Note on the data:** figures are synthetically generated for demonstration and don't represent any real company. The assortment, seasonality and inventory behaviour, however, are modelled on how a real building-materials warehouse actually behaves.

---

## 📬 Contact

**Inna Tkachenko**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?logo=linkedin&logoColor=white)](https://www.linkedin.com/in/inna-tkachenko-3aba54b5/)
[![GitHub](https://img.shields.io/badge/GitHub-innaanalysts-181717?logo=github&logoColor=white)](https://github.com/innaanalysts)

If you found this project interesting or have feedback, feel free to reach out — I'm always open to a conversation.
