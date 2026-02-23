# 🏭 Manufacturing Database Design & Data Migration System  
> A complete end-to-end **Manufacturing Operations Database & Automation System** that transforms unstructured Excel-based plant data into a fully normalized, constraint-driven, trigger-enabled relational database using SQL Server.

---

## 🚀 Overview  

This project covers the **full database engineering lifecycle**:

- **Database Design** – Normalized relational schema (3NF)
- **Data Cleaning & Standardization** – Handling inconsistencies, duplicates, embedded data
- **Data Migration** – Excel (CSV) to SQL Server (SSMS)
- **Constraint Implementation** – Check constraints, foreign keys, unique rules
- **Business Logic Automation** – SQL Triggers for real-time rule enforcement
- **Operational Monitoring** – Inventory alerts & schedule conflict prevention

The system eliminates spreadsheet dependency and enables:

✔ Data integrity  
✔ Production traceability  
✔ Machine scheduling control  
✔ Inventory accuracy  
✔ Quality-driven workflow automation  

---

## 📂 Dataset  

- Source: Internal Manufacturing Operational Data  
- Records: 10,000 rows  
- File Imported: `ABC_10000.csv`  
- Data Includes:
  - Suppliers
  - Raw Material Batches
  - Machine Details
  - Production Orders
  - Material Consumption
  - Quality Inspections
  - Employees

---

## 🏗 Database Architecture  

Raw CSV (ABC_10000)  
        ↓  
Staging Table (All NVARCHAR)  
        ↓  
Data Cleaning & Standardization  
        ↓  
Normalized Tables (3NF)  
        ↓  
Triggers + Constraints  
        ↓  
Automated Manufacturing Control System  

---

## 🛠 Tech Stack  

- **Database**: Microsoft SQL Server  
- **Query Tool**: SQL Server Management Studio (SSMS)  
- **Language**: T-SQL  
- **Data Source**: CSV (Excel-based Manufacturing Logs)  
- **Techniques Used**:
  - Data Normalization (1NF → 3NF)
  - Identity Keys
  - Foreign Key Relationships
  - Check Constraints
  - Triggers
  - TRY_CONVERT for safe migration
  - Data Standardization using CASE logic

---

## 🧱 Database Schema (Core Tables)

### 1️⃣ Suppliers
- SupplierID (PK, Identity)
- SupplierCode (Unique)
- SupplierName

### 2️⃣ Customers
- CustomerID (PK)
- CustomerCode
- CustomerName

### 3️⃣ RawMaterials
- MaterialID (PK)
- MaterialName
- MaterialGrade  
- Unique(MaterialName, MaterialGrade)

### 4️⃣ Machines
- MachineID (PK)
- MachineName
- MachineType
- PlantID
- LastMaintenanceDate

### 5️⃣ ProductionOrders
- ProductionID (PK)
- ProductionCode (Unique)
- CustomerID (FK)
- AssignedMachineID (FK)
- ComponentToProduce
- QuantityToProduce
- OrderDate
- ScheduledStartDate
- ScheduledEndDate
- Status (Check Constraint)
- PlantID

### 6️⃣ MaterialInventory
- BatchID (PK)
- OriginalBatchID
- MaterialID (FK)
- SupplierID (FK)
- ReceivedDate
- InitialQuantity
- RemainingQuantity
- Unit (Standardized with constraint)

### 7️⃣ ProductionMaterialUsage
- UsageID (PK)
- ProductionOrderID (FK)
- BatchID (FK)
- MaterialID (FK)
- QuantityUsed

### 8️⃣ Employees
- EmployeeID (PK)
- FullName
- Role

### 9️⃣ QualityChecks
- QualityCheckID (PK)
- ProductionOrderID (FK)
- InspectorID (FK)
- CheckTimestamp
- Results

---

## 🧹 Data Cleaning & Standardization  

### ✔ Supplier Name Standardization
Handled inconsistent supplier naming:
- "PlasticPro Pvt" → "PlasticPro Inc."
- "Steel Corp Ltd" → "SteelCorp"

Used CASE statements for normalization.

### ✔ Unit Standardization

Converted:
- kg, KG, kilograms → KG  
- meters → M  
- pieces → PCS  
- square feet → SQ_FT  

Enforced via CHECK constraint.

### ✔ Date Conversion Handling

Used:
TRY_CONVERT(DATE, column, format_code)

Handled mixed formats (dd-mm-yy, mm-dd-yy).

### ✔ Quantity Cleaning

Removed:
- Currency symbols ($)
- Commas
- Spaces
- Non-numeric values  

Converted safely to DECIMAL(10,2)

---

## ⚙ Business Logic Automation (Triggers)

### 🔴 Scenario 1: Failed Quality Check → Auto Rework

If a Quality Check result = 'Failed'

Automatically updates Production Status → 'Rework Required'

Trigger:
AFTER INSERT ON QualityChecks

✔ Ensures quality-driven workflow automation  
✔ No manual intervention needed  

---

### 🟡 Scenario 2: Material Usage Updates Inventory

When:
- Material usage is inserted
- Material usage is updated

System:
- Deducts RemainingQuantity
- Prevents usage if insufficient stock
- Rolls back transaction on violation

✔ Prevents negative inventory  
✔ Ensures stock accuracy  

---

### 🔵 Scenario 3: Prevent Machine Double Booking

Before allowing new production schedule:

System checks:
- Same machine
- Overlapping time window
- Status not Cancelled/Completed

If conflict → Raises error and rolls back

✔ Prevents scheduling conflicts  
✔ Avoids plant downtime  

---

### 🟣 Scenario 4: Low Stock Alert System

If RemainingQuantity < 500

Automatically logs alert in:

MaterialLowStockLog Table

Includes:
- Material Name
- Grade
- Quantity Remaining
- Suggested Order Quantity

✔ Enables proactive procurement  

---

## 📊 Problems Solved

| Problem | Solution |
|---------|----------|
| No unique identifiers | Identity PKs |
| Duplicate data | Unique constraints |
| Invalid entries | CHECK constraints |
| Machine double booking | Overlap trigger |
| Inventory mismatch | Auto-deduction trigger |
| Manual rework updates | Quality trigger |
| No stock alert | Low stock logging trigger |
| Flat Excel structure | 3NF normalized schema |

---

## 📈 Business Impact

- Improved production traceability
- Eliminated duplicate & inconsistent records
- Enforced real-time data validation
- Prevented scheduling conflicts
- Automated quality escalation
- Created scalable enterprise-ready database structure

---

## 🔄 Data Migration Strategy

1️⃣ Imported CSV into staging table (all NVARCHAR)  
2️⃣ Cleaned & standardized data  
3️⃣ Used TRY_CONVERT for safe casting  
4️⃣ Joined staging table with normalized tables  
5️⃣ Inserted validated records  
6️⃣ Reseeded identity values  
7️⃣ Implemented constraints after cleaning  

---

## 📌 Key Learnings

- Importance of staging tables in data migration  
- Handling identity reseeding (DBCC CHECKIDENT)  
- Writing multi-event triggers (INSERT, UPDATE)  
- Preventing logical conflicts via SQL  
- Designing real-world enterprise schema  

---

## 🔹 Author  

👤 Pranav Kapoor  
Data Analyst | SQL Developer | Database Designer  
