# 🌷 Day 2 – Dimensional Modeling with Chinook
📅 2025-09-27

---

## 1. Ingestion (DLT → RAW layer)
We ingest tables from the Chinook database into ClickHouse.

**Example Pipeline Snippet:**

```
@dlt.resource(write_disposition="append", name="artist_lou_jenner")
def artist():
    """Extract all artists from the Chinook sample DB."""
    conn = get_connection()
    cur = conn.cursor(cursor_factory=RealDictCursor)
    cur.execute("SELECT * FROM artist;")
    for row in cur.fetchall():
        yield dict(row)
    conn.close()

```

**Run Command**
```
docker compose --profile jobs run --rm \
  dlt python extract-loads/02-dlt-chinook-pipeline.py

```
✅ Output: Data is loaded into raw schema, e.g.:
- raw.chinook_lou_jenner___artist_lou_jenner
- raw.chinook_lou_jenner___album_lou_jenner
- raw.chinook_lou_jenner___track_lou_jenner

![raw schema](../assets/meme2.jpg)

## 2. Transformation (dbt → CLEAN layer)
We create staging views to standardize, clean, and type-cast raw data.

**Checklist of Cleaned Tables**
- [ ] Albums
- [ ] Artists
- [ ] Tracks
- [ ] Genres
- [ ] Invoices
- [ ] Invoice Lines
- [ ] Customers
- [ ] Employees

**Example (Albums):**
```
{{ config(materialized="view", schema="clean", tags=["staging", "chinook"]) }}

SELECT
    MIN(toInt64(album_id)) AS album_id,
    COALESCE(
        initcap(replaceRegexpAll(trim(title), '\\s+', ' ')),
        'Unknown Album'
    ) AS album_title,
    toInt64(artist_id) AS artist_id,
    now() AS cleaned_at
FROM {{ source('raw', 'chinook_lou_jenner___albums_lou_jenner') }}
GROUP BY
    initcap(replaceRegexpAll(trim(title), '\\s+', ' ')),
    artist_id

```

✅ What staging achieves: 
- Deduplication
- Standardized casing (initcap)
- Consistent typing (int, decimal, string)
- Dropping DLT metadata

## 3. Dimensional Model (MART layer → Star Schema)
We design fact and dimension tables.

## Fact Table:
**fact_invoice_line** 
* Grain: one row per invoice line 
* Columns: invoice_line_id, invoice_id, track_id, quantity, price_usd, line_amount

## Dimension Tables:
* dim_track → joins album, artist, genre into one track view
* dim_album
* dim_artist
* dim_genre
* dim_customer (with support_rep_id → employee)
* dim_employee (sales reps, with hierarchy reports_to)
* dim_date (from invoice_date: date_key, year, month, quarter, etc.)

## Relationships:
* Album → Artist
* Track → Album, Genre
* InvoiceLine → Track, Invoice
* Invoice → Customer
* Customer → Employee (support_rep_id)

## 4.Visualization (Metabase)
* Connect to the mart schema.
* Build dashboards: revenue trends, customer segments, employee performance.
* Validate queries match the dimensional model.

## 5. Assignment Queries (Metabase)

**1. Top revenue by genre per country**
```
query  here
```

**2. Customer segmentation by spending tier**
```
query  here
```

**3. Monthly sales trend** 
```
query  here
```

**4. Employee sales performance**
```
query  here
```

**5.Popular tracks by quantity sold**

```
query  here
```

**6.Regional Pricing Insights**

```
query  here
```
**CHALLENGE: TOP ARTIST PER QUARTER**

```
query  here
```
## TEAM PROCESS 
![team process](../assets/meme2.jpg)




