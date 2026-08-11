# edi-databricks-pipeline
End-to-end EDI 850 purchase order processing and analytics using Databricks, PySpark, Delta Lake, and SQL.

# EDI 850 Purchase Order Processing with Databricks

This is a hands-on project I built to combine my previous experience working with EDI with the data engineering and analytics skills I am currently developing.

I worked with EDI transactions in my previous role, but I wanted to get more practical experience with tools such as **Databricks, PySpark, Delta Lake, and SQL**. So I created a sample EDI 850 Purchase Order and built a small end-to-end pipeline around it.

> The EDI data used in this project is fictional and created only for learning and demonstration purposes.

## What is this project about?

An EDI 850 is a Purchase Order transaction used to send purchase order information between businesses.

For this project, I started with a raw EDI 850 file and used PySpark in Databricks to turn the EDI segments into structured data that can be used for analysis.

The basic flow is:

```text
Raw EDI 850 File
       ↓
Databricks Volume
       ↓
PySpark
       ↓
Parse & Clean EDI Segments
       ↓
Validate EDI Data
       ↓
Structured Purchase Order Data
       ↓
Delta Table
       ↓
SQL Analytics
```

## What I worked on

I parsed and worked with several EDI segments, including:

* ISA / IEA
* GS / GE
* ST / SE
* BEG
* REF
* N1
* PO1
* CTT

From these segments, I extracted information such as:

* Purchase order number
* Purchase order date
* Buyer
* Ship-to location
* Department
* Product ID
* Quantity
* Unit of measure
* Unit price
* Line total

For example, a PO1 segment like:

```text
PO1*1*5*EA*50.00**BP*WATCH1001
```

is converted into structured information:

```text
Line Number: 1
Quantity: 5
UOM: EA
Unit Price: 50.00
Product ID: WATCH1001
```

I also calculated the line totals and validated the overall purchase order amount.

## Validation

I added validation checks for the EDI control segments and also checked whether the purchase order total matched the sum of the line totals.

For the sample PO:

```text
WATCH1001  → 5 × $50.00 = $250.00
WALLET1001 → 3 × $30.00 = $90.00
```

Total:

```text
$250.00 + $90.00 = $340.00
```

The validation passed successfully.

## Delta Table

After transforming the data, I saved the final dataset as a Delta table in Databricks:

```text
workspace.default.edi_purchase_orders
```

The final table contains fields such as:

```text
po_number
po_date
buyer_name
ship_to_name
department
line_number
product_id
quantity
uom
unit_price
line_total
```

## SQL Analytics

Once the data was stored in Delta, I used SQL to answer a few simple business questions.

For example:

* What is the total value of the purchase order?
* How many units were ordered for each product?
* Which product has the highest order value?
* What is the total quantity in the purchase order?
* What is the overall purchase order summary?

For the sample data, the total purchase order value is **$340.00** and the total quantity is **8 units**.

## Technologies I Used

* Databricks
* PySpark
* Python
* SQL
* Delta Lake
* EDI X12 / 850

## Project Files

```text
edi-databricks-pipeline/
│
├── 01_Load_EDI_File.ipynb
├── 04_SQL_Analytics.ipynb
├── 850_Sample copy_github.txt
└── README.md
```

### `01_Load_EDI_File.ipynb`

This notebook contains the EDI reading, parsing, transformation, validation, and Delta table creation.

### `04_SQL_Analytics.ipynb`

This notebook contains the SQL queries I used to analyze the processed purchase order data.

## Why I Built This

I wanted to build something that connects my previous EDI experience with the data and analytics skills I am learning now.

Instead of only learning PySpark and Databricks through separate exercises, I wanted to use them on a problem that I already understand from my previous work.

This project is a starting point for me, and I plan to continue improving it by adding more EDI transactions, more transformations, and additional analytics.

