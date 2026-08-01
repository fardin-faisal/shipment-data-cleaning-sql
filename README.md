# Data Cleaning Pipeline: Logistics & Shipments Dataset (MySQL)

A comprehensive SQL-based data cleaning pipeline designed to process, standardize, and sanitize raw shipment datasets in MySQL. This workflow handles duplicate records, leading/trailing whitespace, text standardization, null string handling, invalid numeric values, and inconsistent date formatting.

---

## 🛠️ Data Cleaning Pipeline Steps

### **STEP 1: Create a Clean Working Table**
* **Step 1.1:** Create a target table schema based on the raw dataset structure and insert records separately.
* **Step 1.2:** Alternatively, copy structure and data simultaneously (`CREATE TABLE ... AS SELECT`).
* **Purpose:** Ensures raw source data (`raw_shipments`) remains untouched throughout the transformation process.

### **STEP 2: Data Cleaning Operations**

#### **STEP 2.1: Remove Duplicates**
* **Primary Key Check:** Verify uniqueness across `shipment_id`.
* **Logical Duplicate Check:** Identify identical shipments partitioned across key business attributes (`origin_warehouse`, `destination_city`, `carrier`, `ship_date`, `weight_kg`, `freight_cost`).
* **Action:** Retain only the first record using window functions (`ROW_NUMBER()`) and purge redundant rows.

#### **STEP 2.2: Standardizing the Data**
* **Trim Whitespace:** Strip leading/trailing spaces across text fields (`origin_warehouse`, `destination_city`, `carrier`, `destination_state`).
* **Text Formatting & Casing:**
  * Capitalize first and last letter formatting for warehouse IDs.
  * Standardize city names into Title Case (handling single-word and multi-word cities).
  * Convert state codes to uppercase standard formatting (`UPPER`).
  * Normalize carrier names into proper brand capitalization (e.g., `FastFreight`, `QuickShip`, `SpeedyHaul`).
  * Standardize status flags and reporting fields (`shipment_status`, `damage_reported`).

#### **STEP 2.3: Handle Null / Blanks**
* **Data Sanitization:** Detect string literals like `'NULL'`, empty strings `''`, or unassigned values across critical attributes (`destination_city`, `ship_date`, `delivery_date`, `freight_cost`).
* **Action:** Convert invalid text values to explicit SQL `NULL`.

#### **STEP 2.4: Fix Negative and Suspicious Numeric Values**
* **Logic Correction:** Convert negative package weights (`weight_kg < 0`) to absolute positive values using `ABS()`.
* **Zero Value Handling:** Set invalid non-positive values (`weight_kg = 0`) to `NULL`.

#### **STEP 2.5: Date Formatting & Normalization**
* **Format Unification:** Parse heterogeneous date strings (e.g., `MM/DD/YYYY`, `YYYY-MM-DD`, `YYYY/MM/DD`, `Mon DD YYYY`, `Month DD YYYY`).
* **Conversion:** Standardize all heterogeneous entries into proper MySQL `DATE` formats using conditional parsing (`CASE` with `STR_TO_DATE`).

#### **STEP 2.6: Advanced Data Cleaning (Outlier Detection)**
* **will learn soon *

---
