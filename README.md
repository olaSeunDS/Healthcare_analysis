
Jasper Logo

# Healthcare Dashboard

### Dashboard Link : https://app.powerbi.com/links/CEq80VpJNQ?ctid=8c7a9d64-4f17-4464-86bd-a4c3e1f2960b&pbi_source=linkShare

## Problem Statement

This dashboard enables a hospital to analyze patient health data collected from three cities over one year. By examining various demographics, it provides insights into the populations that are healthiest or at the highest risk. With this information, the hospital can identify vulnerable groups and prioritize interventions for those most at risk.

### Data Cleaning Steps

Here's a step-by-step report detailing the data cleaning process after uploading the data into Power Query.

1. **City Standardization**
Replaced inconsistent city names with standardized versions:

     - "Albuque" → "Albuquerque"
     - "ALBUQUERQUE" → "Albuquerque"
     - "Albuqerque" → "Albuquerque"
     - "Atl" → "Atlanta"
     - "Balti" → "Baltimore"
All other city names remained unchanged

2. **Salary Field Cleaning**
- Extracted text before delimiter "(" in Salary field
- Converted the extracted value to absolute number (removed negative values)

3. **Data Type Conversion**
Set proper data types for all columns:

     - ID: Text
     - Name: Text
     - Age: Integer
     - Gender: Text (though later removed)
     - Blood Type: Text
     - Education: Text
     - Employment Status: Text
     - Credit Score: Integer
     - Date of Admission: Date
     - City: Text
     - Salary: Integer
     - Health Condition: Text

4. **Name Field Standardization**
Applied proper case formatting to Name field (capitalized each word)

5. **Column Removal**
Removed the "Gender" column entirely from the dataset

6. **Text Field Cleaning (Using Cleaning Function)**
Applied a custom cleaning function to multiple text fields:
- Cleaned **`Name`** field using reference cleaning table
- Cleaned **`Education`** field using reference cleaning table
- Cleaned **`Employment Status`** field using reference cleaning table
     
     ### **Custom Data Cleaning Function (`CleaningFunction`)**  

The custom function, `CleaningFunction`, is designed to standardize text data by replacing inconsistent or incorrect values with clean ones based on a reference table (`CleaningTable`). 

![Image](https://github.com/user-attachments/assets/e8c1f7f8-9d38-4af6-bdab-4ca9ba5b591f)
Cleaning Table

#### **Function Parameters**

![Image](https://github.com/user-attachments/assets/0b659704-1790-437b-bb4f-9de6242d2f0b)

The function operates using the following parameters:

- **`OriginalTable`** – The table containing the column to be cleaned.  
- **`OTColumnToBeCleaned`** – The name of the column (as text) within `OriginalTable` that requires cleaning.  
- **`CleaningTable`** – A reference table containing two key columns:  
- **`CTOldValue`** – Holds the dirty or inconsistent values to be replaced.  
- **`CTNewValue`** – Contains the corresponding standardized replacement values.  

#### **Function Logic**

![Image](https://github.com/user-attachments/assets/9a4aa6f7-c43c-45e9-850c-0fe2c5523ce0)

i. **Determine Replacement Count (`maxcount`)**  
The function first calculates the number of replacements required by counting the rows in `CleaningTable`.  

ii. **Iterative Replacement Process (`iteration`)**  
Utilizes `List.Generate` to iterate through each row in `CleaningTable`:  
- Starts with `counter = 0` and applies the first replacement.  
- For each iteration, replaces occurrences of `CTOldValue` with `CTNewValue` in the specified column.  
- Continues until all replacements (`maxcount`) have been applied.  

iii. **Output**  
- Returns the final cleaned table after all specified replacements have been made.  

The `CleaningFunction` was applied to the following columns during the data cleaning process:  
- **`Name`**  
- **`Education`**  
- **`Employment Status`**  

This ensured the standardization of values, correcting inconsistencies such as:  
- `"Bachleor"` → `"Bachelor"`  
- `"Unemployd"` → `"Unemployed"`

8. **Error Handling**
Removed rows containing errors in critical numeric fields:
- `Age`
- `Salary`
- `Credit Score`

9. **Data Quality Filtering**
Filtered out rows with missing or empty values in key fields:
- `Health Condition`
- `Blood Type`
- `Education`

10. **Date Table**

![Image](https://github.com/user-attachments/assets/25cf72c1-1033-4e8b-b72a-4e162e3f3b48)

Added a Date table for time intelligence analysis

### Final Output

![Image](https://github.com/user-attachments/assets/cf63cae2-6092-40cb-b1c8-b06f91713137)

![Image](https://github.com/user-attachments/assets/d7073eb2-a207-4651-a68b-a68458ae711b)

The resulting dataset contains clean, standardized data with:
- Consistent city names
- Properly formatted names
- Cleaned categorical fields (Education, Employment Status)
- Valid numeric values
- No missing values in critical fields
- Removed unnecessary columns

### Data Model

After cleaning and shaping the date in Power Query, three tables were generated and one function. Only two were needed for analysis and so were imported into Power BI.

![Image](https://github.com/user-attachments/assets/ccac02a6-d0c8-473d-a33e-9820f89106d9)

1. **The Healthcare Data** – the main table with all the patient records, now free of inconsistencies.  
2. **A Date Table** – A calendar table for analyzing trends over time.  

### **How They Connect**  
Relationships between both table was created using the date fields—specifically:  
- The **Date Table** acts as the "source of truth" for dates (with one unique entry per day).  
- The **Healthcare Table** connects to it through the *"Date of Admission"* field (since many patients can be admitted on the same day).  

![Image](https://github.com/user-attachments/assets/260605ca-1a32-4554-b242-b7919a5bde34)

This setup lets us easily:  
Slice and dice the data by time (like seeing monthly admission trends).  
Build reports with dynamic date filters (year → quarter → month).  
Keep everything consistent when analyzing patient data over time.  

![Image](https://github.com/user-attachments/assets/cd807c34-1cb7-4bf1-abb1-8280b71a9223)
