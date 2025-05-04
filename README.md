
# 🔄 SQL, Pandas, and PySpark Syntax Mapping Reference

A handy template mapping common operations between **SQL**, **Pandas**, and **PySpark**. Useful for data engineers, analysts, and scientists switching between different tools.

---

## 📌 Table of Contents

- [Select Columns](#select-columns)
- [Filtering Rows](#filtering-rows)
- [Sorting](#sorting)
- [Aggregation](#aggregation)
- [Group By](#group-by)
- [Joins](#joins)
- [Add/Update Columns](#addupdate-columns)
- [Rename Columns](#rename-columns)
- [Drop Columns](#drop-columns)
- [Null Handling](#null-handling)
- [Window Functions](#window-functions)

---

## Select Columns

| Operation | SQL | Pandas | PySpark |
|----------|-----|--------|---------|
| Select all | `SELECT * FROM table` | `df` | `df` |
| Select specific | `SELECT col1, col2 FROM table` | `df[['col1', 'col2']]` | `df.select('col1', 'col2')` |

---

## Filtering Rows

| Operation | SQL | Pandas | PySpark |
|----------|-----|--------|---------|
| Filter | `SELECT * FROM table WHERE age > 30` | `df[df['age'] > 30]` | `df.filter(df.age > 30)` |
| Multiple conditions | `WHERE age > 30 AND gender = 'M'` | `df[(df['age'] > 30) & (df['gender'] == 'M')]` | `df.filter((df.age > 30) & (df.gender == 'M'))` |

---

## Sorting

| Operation | SQL | Pandas | PySpark |
|----------|-----|--------|---------|
| Ascending | `ORDER BY age` | `df.sort_values('age')` | `df.orderBy('age')` |
| Descending | `ORDER BY age DESC` | `df.sort_values('age', ascending=False)` | `df.orderBy(df.age.desc())` |

---

## Aggregation

| Operation | SQL | Pandas | PySpark |
|----------|-----|--------|---------|
| Count | `SELECT COUNT(*) FROM table` | `df.shape[0]` | `df.count()` |
| Mean | `SELECT AVG(age) FROM table` | `df['age'].mean()` | `df.agg({'age': 'avg'})` |
| Sum | `SELECT SUM(salary) FROM table` | `df['salary'].sum()` | `df.agg({'salary': 'sum'})` |

---

## Group By

| Operation | SQL | Pandas | PySpark |
|----------|-----|--------|---------|
| Group and count | `SELECT dept, COUNT(*) FROM table GROUP BY dept` | `df.groupby('dept').size()` | `df.groupBy('dept').count()` |
| Group and aggregate | `SELECT dept, AVG(age) FROM table GROUP BY dept` | `df.groupby('dept')['age'].mean()` | `df.groupBy('dept').agg({'age': 'avg'})` |

---

## Joins

| Operation | SQL | Pandas | PySpark |
|----------|-----|--------|---------|
| Inner Join | `SELECT * FROM A INNER JOIN B ON A.id = B.id` | `pd.merge(A, B, on='id', how='inner')` | `A.join(B, on='id', how='inner')` |
| Left Join | `LEFT JOIN` | `how='left'` | `how='left'` |
| Right Join | `RIGHT JOIN` | `how='right'` | `how='right'` |
| Outer Join | `FULL OUTER JOIN` | `how='outer'` | `how='outer'` |

---

## Add/Update Columns

| Operation | SQL | Pandas | PySpark |
|----------|-----|--------|---------|
| Add new column | `ALTER TABLE ADD COLUMN` | `df['new'] = df['col'] * 2` | `df = df.withColumn('new', df.col * 2)` |

---

## Rename Columns

| Operation | SQL | Pandas | PySpark |
|----------|-----|--------|---------|
| Rename column | `SELECT col AS new_name` | `df.rename(columns={'col': 'new_name'})` | `df = df.withColumnRenamed('col', 'new_name')` |

---

## Drop Columns

| Operation | SQL | Pandas | PySpark |
|----------|-----|--------|---------|
| Drop column | `ALTER TABLE DROP COLUMN` | `df.drop('col', axis=1)` | `df = df.drop('col')` |

---

## Null Handling

| Operation | SQL | Pandas | PySpark |
|----------|-----|--------|---------|
| Filter nulls | `WHERE col IS NOT NULL` | `df[df['col'].notnull()]` | `df.filter(df.col.isNotNull())` |
| Fill nulls | `N/A` | `df.fillna(value)` | `df.fillna(value)` |

---

## Null Handling

## Window Functions

| Operation | SQL | Pandas | PySpark |
|----------|-----|--------|---------|
| Row Number | `ROW_NUMBER() OVER (PARTITION BY dept ORDER BY salary DESC)` | `df.groupby('dept')['salary'].rank(method='first', ascending=False)` | `df.withColumn('row_num', F.row_number().over(Window.partitionBy('dept').orderBy(df.salary.desc())))` |
| Rank | `RANK() OVER (...)` | `df.groupby(...).rank(method='min')` | `df.withColumn('rank', F.rank().over(w))` |
| Dense Rank | `DENSE_RANK() OVER (...)` | `df.groupby(...).rank(method='dense')` | `df.withColumn('dense_rank', F.dense_rank().over(w))` |
| Lag | `LAG(col, 1) OVER (...)` | `df['col'].shift(1)` | `df.withColumn('prev', F.lag('col', 1).over(w))` |
| Lead | `LEAD(col, 1) OVER (...)` | `df['col'].shift(-1)` | `df.withColumn('next', F.lead('col', 1).over(w))` |
| Moving Average | `AVG(col) OVER (ORDER BY date ROWS BETWEEN 2 PRECEDING AND CURRENT ROW)` | `df['col'].rolling(window=3).mean()` | `df.withColumn('mov_avg', F.avg('col').over(Window.orderBy('date').rowsBetween(-2, 0)))` |

---
