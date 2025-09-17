# Day 1 – SQL & ELT Basics  
📅 2025-09-13  

---

## 📝 Session Notes

### Foundations of Data Engineering
- DE = combines skills from SWE, DevOps, DBA, and Analysts.  
- Why DE matters: higher digital maturity → more data → higher DE demand.  
- Not just pipelines, but **trust, scalability, usability**.  

### Hands-On w/ Data Pipelines
- Tools introduced: **dlt**, **dbt**, **ClickHouse**, **DBeaver**, **Metabase**.  
- Local + remote setups (personal laptop + shared AWS server).  
- Dataset: **Auto MPG**.  

### First Steps in SQL
- The “Big 6”: SELECT, FROM, WHERE, GROUP BY, HAVING, ORDER BY.  
- OLTP vs OLAP → row vs column storage.  
- Ran first SQL queries in ClickHouse.  

### Data Quality & Cleaning
- Raw data = messy → must be cleaned before use.  
- Stages: **Raw (bronze) → Clean (silver) → Mart (gold)**.  
- Cleaning: NULL handling, type casting, standardization.  
- dbt used to enforce transformations + tests.  

### Visualization
- Used **Metabase** to explore data.  
- Built scatter plot: **Horsepower vs MPG**.  
- Visualization = bridge between queries & insights.  

---

## 🎯 Key Takeaways
- **Pipeline mindset**: raw → clean → mart.  
- SQL is the foundation of DE.  
- Cleaning is not optional — it’s part of the pipeline.  
- Visualization helps tell a story with data.  

---

## ✨ Reflection
- Easiest: Writing simple SQL queries (SELECT, ORDER BY).  
- Hardest: Understanding pipeline architecture (raw-clean-mart separation).  
- Surprised by: How much LLMs can support learning (but require reflection).  
- TIL words: OLAP, ClickHouse, dbt, dlt.
  
---

## 📌 Next Steps
- Practice SQL queries with Auto MPG dataset.  
- Build a clean layer in dbt.  
- Try more visualizations in Metabase.  
