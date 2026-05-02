 🎵 Music Store SQL Analysis

> End-to-end SQL project analyzing a music store database to answer real business questions — from identifying top customers to finding the best city for a music festival.

## 📌 Project Overview

This project analyzes a music store database using **PostgreSQL**. The goal is to answer business questions across three difficulty levels using SQL — covering basic aggregations, subqueries, CTEs, and advanced window functions.

**Database:** 11 tables | **Tool:** PostgreSQL | **Concepts:** JOINs, CTEs, Window Functions, Subqueries

---

## 🗂️ Database Schema

![Music Store Schema](schema/MusicDatabaseSchema.png)

**Key tables:** Artist → Album → Track → InvoiceLine → Invoice → Customer → Employee

---

## 📊 Questions & Solutions

### 🟢 Set 1 — Easy

| # | Question | SQL Concept |
|---|----------|-------------|
| 1 | Senior most employee by job title | ORDER BY, LIMIT |
| 2 | Countries with most invoices | GROUP BY, COUNT |
| 3 | Top 3 invoice totals | ORDER BY DESC, LIMIT |
| 4 | Best city for Music Festival | SUM, GROUP BY |
| 5 | Best customer by total spend | JOIN, SUM, GROUP BY |

### 🟡 Set 2 — Moderate

| # | Question | SQL Concept |
|---|----------|-------------|
| 1 | Rock music listeners (email, name) | Subquery, DISTINCT |
| 2 | Top 10 rock artists by track count | Multi-table JOIN, COUNT |
| 3 | Tracks longer than average duration | Scalar Subquery, AVG |

### 🔴 Set 3 — Advanced

| # | Question | SQL Concept |
|---|----------|-------------|
| 1 | Customer spend per artist | CTE, multi-table JOIN |
| 2 | Most popular genre per country | ROW_NUMBER(), PARTITION BY |
| 3 | Top customer per country | Window Function, CTE |

---

## 🔍 Key SQL Patterns Used

**CTE (Common Table Expression)**
```sql
WITH best_selling_artist AS (
  SELECT artist_id, name, SUM(unit_price * quantity) AS total_sales
  FROM invoice_line
  JOIN track USING (track_id)
  JOIN album USING (album_id)
  JOIN artist USING (artist_id)
  GROUP BY 1
  ORDER BY 3 DESC
  LIMIT 1
)
SELECT c.first_name, c.last_name, bsa.artist_name, SUM(il.unit_price * il.quantity)
FROM invoice i
JOIN customer c USING (customer_id)
JOIN invoice_line il USING (invoice_id)
JOIN track t USING (track_id)
JOIN album alb USING (album_id)
JOIN best_selling_artist bsa USING (artist_id)
GROUP BY 1, 2, 3
ORDER BY 4 DESC;
```

**Window Function**
```sql
ROW_NUMBER() OVER(PARTITION BY customer.country ORDER BY COUNT(invoice_line.quantity) DESC)
```

---

## 🛠️ How to Run

1. Install [PostgreSQL](https://www.postgresql.org/download/)
2. Create a new database: `CREATE DATABASE music_store;`
3. Load the schema + data: `psql -d music_store -f sql/music_store_database.sql`
4. Run the queries: `psql -d music_store -f sql/music_store_queries.sql`

---

## 📁 Repo Structure

```
music-store-sql-analysis/
├── README.md
├── schema/
│   └── MusicDatabaseSchema.png
├── sql/
│   ├── music_store_database.sql
│   └── music_store_queries.sql
└── questions/
    └── Music_Store_Analysis_Questions.pdf
```

---

## 💡 What I Learned

- How to break complex business questions into SQL logic step by step
- When to use subqueries vs CTEs for readability
- How window functions (ROW_NUMBER + PARTITION BY) solve "top N per group" problems
- Joining across 5–6 tables without losing track of relationships

---

*Project inspired by a real-world SQL analytics exercise. Dataset from a sample music store.*
