
# Analyzing-industry-carbon-emissions

---

![Factories creating emissions](pollution.jpg)

*Photo by Maxim Tolchinskiy on Unsplash*

> When factoring heat generation required for the manufacturing and transportation of products, **Greenhouse gas emissions attributable to products, from food to sneakers to appliances, make up more than 75% of global emissions.**  
> *(Source: The Carbon Catalogue — The dataset referenced originally in Nature Scientific Data).*  
> `Source: The Carbon Catalogue / Nature Scientific Data — https://www.nature.com/articles/s41597-022-01178-9`

---

## 📌 Project Overview

This project analyzes product carbon footprints (PCFs) across industries using PCF data originally made available via the Nature dataset. We examine where product emissions are generated (upstream / operations / downstream), which industries and companies contribute the most absolute emissions, and relative stage-shares to identify opportunities for mitigation.

The raw data is stored in a **PostgreSQL** table named `product_emissions`. Key columns:

| Field                           | Description |
|---------------------------------|-------------|
| `id`                            | product identifier |
| `year`                          | reporting year |
| `product_name`                  | product name |
| `company`                       | company name |
| `country`                       | country of reporting |
| `industry_group`                | industry group |
| `weight_kg`                     | product weight (kg) |
| `carbon_footprint_pcf`          | product carbon footprint (CO₂e) |
| `upstream_percent_total_pcf`    | upstream share (string, e.g. "45%") |
| `operations_percent_total_pcf`  | operations share (string) |
| `downstream_percent_total_pcf`  | downstream share (string) |

---

## 🎯 Business Objective

Decision-makers and sustainability teams can use this analysis to:

- Identify **high-emissions industries** and companies.
- Understand **which production stages** (upstream / operations / downstream) contribute most in each industry.
- Prioritize mitigation efforts (e.g., supplier decarbonization vs factory efficiency vs logistics).
- Feed results to dashboards for stakeholder communication.

---

## 🔍 SQL: Industry-level summary (use in Pandas)

Run this query in your Pandas environment to build a summary view for the **latest reporting year**. 

```sql
-- Industry-level summary for the latest year (Pandas)
select industry_group, count( distinct company) num_companies, round(sum(carbon_footprint_pcf),1) total_industry_footprint
from product_emissions
where year in(select max(year) from product_emissions)
group by industry_group
order by total_industry_footprint desc;
