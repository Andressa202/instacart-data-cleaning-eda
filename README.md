# 🛒 Instacart Shopping Behavior Analysis | Data Cleaning & EDA

### Exploring Customer Purchasing Patterns, Product Popularity, and Reorder Behavior

**Python • Pandas • Matplotlib • Data Cleaning • Exploratory Data Analysis**

## 📌 Project Overview

Understanding customer purchasing behavior is essential for improving product availability, inventory management, and customer experience in the grocery delivery industry.

This project analyzes historical grocery orders from **Instacart**, an online grocery delivery platform, to identify shopping trends, purchasing frequency, product popularity, and repeat purchase behavior.

The analysis focuses on cleaning and exploring multiple interconnected datasets to generate meaningful insights into customer shopping habits.

**Business Goal:** Understand how customers shop, when they place orders, which products they prefer, and how frequently they reorder products.

## 🎯 Project Objectives

- Clean and prepare multiple datasets for analysis.
- Identify missing values and duplicate records.
- Analyze order distribution by hour and day of the week.
- Investigate the time between consecutive orders.
- Identify the most frequently purchased products.
- Examine customer purchasing frequency.
- Analyze product reorder patterns.
- Identify the products most frequently added to carts first.

## 🛠️ Technologies Used

| Technology | Purpose |
|---|---|
| Python | Data processing and analysis |
| Pandas | Data cleaning, transformation, and aggregation |
| Matplotlib | Data visualization |
| Jupyter Notebook | Analysis and documentation |

## 📂 Dataset Description

The project uses five interconnected datasets.

| Dataset | Description |
|---|---|
| `instacart_orders.csv` | Order information, customer IDs, order dates, and times |
| `products.csv` | Product names and category identifiers |
| `order_products.csv` | Products included in each order and reorder information |
| `aisles.csv` | Product aisle information |
| `departments.csv` | Product department information |

The datasets are connected through identifiers such as `order_id`, `product_id`, `aisle_id`, and `department_id`.

The dataset is an adapted version of Instacart's original grocery-order data.

## 🧹 Data Cleaning and Preprocessing

The preprocessing workflow included:

1. Loading CSV files with the appropriate separator.
2. Inspecting dataset dimensions and column types.
3. Identifying and removing duplicate records.
4. Investigating missing product names.
5. Handling missing cart-order positions.
6. Converting numerical columns to appropriate data types.
7. Validating order times and days of the week.

### Data Quality Findings

**Duplicate orders
