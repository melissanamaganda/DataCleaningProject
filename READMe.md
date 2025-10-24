# Nashville Housing Data Cleaning Project using SQL

## Executive Summary
This project focuses on **cleaning and transforming raw Nashville housing data** to create a structured, analysis-ready dataset.  
By standardizing date formats, splitting address fields, handling missing values, and removing duplicates, the dataset is prepared for **insight generation and visualization**.  

**Highlights**
- Standardized **sale dates** for consistency.  
- Split **property and owner addresses** into separate columns (Address, City, State).  
- Cleaned categorical values (e.g., `SoldAsVacant` Y/N → Yes/No).  
- Removed **duplicate records** and unnecessary columns.  

**Next Steps:** Use the cleaned data for pricing analysis, trend visualization, or predictive modeling in tools like Power BI or Python.

---

## Business Problem
Raw housing data often contains **inconsistent formats, missing fields, duplicates, and unstructured address data**, making it difficult to analyze property trends or sales performance.  
This project addresses these challenges to provide a **clean, accurate, and standardized dataset** for real estate analysis and decision-making.

---

## Methodology
The analysis was performed using **SQL Server** to clean and transform the `NashvilleHousing` table.  

**Process Overview**
1. **Data Cleaning:**  
   - Standardized `SaleDate` to DATE format.  
   - Converted `SoldAsVacant` values from 'Y/N' to 'Yes/No'.  
2. **Data Integration & Transformation:**  
   - Populated missing `PropertyAddress` fields using matching `ParcelID` records.  
   - Split `PropertyAddress` and `OwnerAddress` into separate columns (Address, City, State) using `SUBSTRING` and `PARSENAME`.  
3. **Deduplication:**  
   - Used **CTEs** with `ROW_NUMBER()` to identify and remove duplicate rows.  
4. **Optimization:**  
   - Dropped unused columns like `OwnerAddress`, `TaxDistrict`, and the original `PropertyAddress` to simplify the dataset.  

Example snippet:
```sql
-- Standardize SaleDate
UPDATE NashvilleHousing
SET SaleDate = CONVERT(DATE, SaleDate);

ALTER TABLE NashvilleHousing
ALTER COLUMN SaleDate DATE;

-- Split PropertyAddress into Address and City
ALTER TABLE NashvilleHousing
ADD PropertySplitAddress NVARCHAR(255), PropertySplitCity NVARCHAR(255);

UPDATE NashvilleHousing
SET PropertySplitAddress = SUBSTRING(PropertyAddress, 1, CHARINDEX(',', PropertyAddress) - 1),
    PropertySplitCity = SUBSTRING(PropertyAddress, CHARINDEX(',', PropertyAddress) + 1, LEN(PropertyAddress));
