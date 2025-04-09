---
layout: notebook
title: Dataset testing
permalink: /posts/panda-test
comments: True
---

## Evaluation of Pavement Condition Assessments Data
[Pavement Condition Assessments](https://data.sandiego.gov/datasets/streets-overall-condition-index/)


```python
import pandas as pd

def clean_and_analyze_pavement_data(file_path):
    # Load data
    data = pd.read_csv(file_path)
    
    # Drop rows with missing values
    data_cleaned = data.dropna().copy()
    
    # Convert PCI to numeric for analysis
    data_cleaned.loc[:, 'pci'] = pd.to_numeric(data_cleaned['pci'], errors='coerce')
    
    # Display more data
    print(data_cleaned.head(25))
    
    return data_cleaned

def calculate_average_pci(data_cleaned):
    return data_cleaned['pci'].mean()

def display_best_and_worst_conditions(data_cleaned):
    # Sort the data by PCI (ascending for worst, descending for best)
    best_conditions = data_cleaned.nlargest(10, 'pci')
    worst_conditions = data_cleaned.nsmallest(10, 'pci')
    
    print("\n5 Best Road Conditions:")
    print(best_conditions[['seg_id', 'pci']])
    
    print("\n5 Worst Road Conditions:")
    print(worst_conditions[['seg_id', 'pci']])

# Example usage
file_path = "pavement_condition_assessment_2023_datasd.csv"
data_cleaned = clean_and_analyze_pavement_data(file_path)
print(f"Average PCI: {calculate_average_pci(data_cleaned):.2f}")
display_best_and_worst_conditions(data_cleaned)
```

               seg_id    pci      pci_desc
    1   SS-000002-PV1  56.59          Fair
    2   SS-000003-PV1  80.00  Satisfactory
    3   SS-000004-PV1  86.40          Good
    4   SS-000005-PV1  67.86          Fair
    5   SS-000006-PV1  95.53          Good
    6   SS-000007-PV1  87.44          Good
    7   SS-000008-PV1  67.61          Fair
    8   SS-000009-PV1  89.71          Good
    9   SS-000010-PV1  70.12  Satisfactory
    10  SS-000011-PV1  86.65          Good
    11  SS-000012-PV1  91.41          Good
    12  SS-000013-PV1  51.16          Poor
    13  SS-000014-PV1  91.47          Good
    14  SS-000015-PV1  91.31          Good
    15  SS-000016-PV1  60.48          Fair
    16  SS-000018-PV1  57.95          Fair
    17  SS-000019-PV1  19.88       Serious
    18  SS-000020-PV1  43.24          Poor
    19  SS-000021-PV1  50.49          Poor
    20  SS-000022-PV1  47.70          Poor
    21  SS-000023-PV1  45.90          Poor
    22  SS-000024-PV1  92.34          Good
    23  SS-000025-PV1  88.20          Good
    24  SS-000026-PV1  12.92       Serious
    25  SS-000027-PV1  63.64          Fair
    Average PCI: 66.23
    
    5 Best Road Conditions:
                seg_id    pci
    89   SS-000093-PV1  100.0
    137  SS-000141-PV1  100.0
    138  SS-000142-PV1  100.0
    144  SS-000149-PV1  100.0
    182  SS-000187-PV1  100.0
    185  SS-000193-PV1  100.0
    186  SS-000194-PV1  100.0
    222  SS-000230-PV1  100.0
    237  SS-000245-PV1  100.0
    271  SS-000285-PV1  100.0
    
    5 Worst Road Conditions:
                  seg_id   pci
    1973   SS-002156-PV1  0.00
    3697   SS-004009-PV1  0.00
    9295   SS-010155-PV1  0.00
    14310  SS-015639-PV1  0.00
    19911  SS-021718-PV1  0.00
    23384  SS-025527-PV1  0.00
    25204  SS-027542-PV1  0.00
    10069  SS-011007-PV1  0.06
    7419   SS-008141-PV1  0.28
    2632   SS-002855-PV1  0.33


### Column Descriptions for the Pavement Condition Dataset

- **seg_id**: 
  - *Definition*: Unique identifier for a specific road segment.
  - *Meaning*: This column helps in distinguishing between different sections of the road network. Each segment will have a unique `seg_id` to reference its condition and associated repair data.
  
- **pci**: 
  - *Definition*: Pavement Condition Index (PCI), a numeric scale (0-100) representing the condition of the pavement.
  - *Meaning*: This index is crucial for understanding the health of the road. A higher PCI value indicates better road quality, whereas a lower PCI value reflects significant deterioration. This metric helps in prioritizing roads for maintenance or repair. The PCI is essential for deciding which roads require urgent attention.

- **pci_desc**: 
  - *Definition*: Descriptive rating of the road condition based on PCI.
  - *Meaning*: This column provides a verbal description of the pavement's condition, such as "Excellent," "Good," "Fair," or "Poor." It helps contextualize the numeric PCI score by giving a more intuitive understanding of the road's condition.

---

### Evaluation of the Dataset for Our Project

**Why This Dataset Is Good for Our Project:**

1. **Predictive Modeling**:
   - The `pci` column allows us to forecast the urgency of maintenance needs based on the pavement condition of various road segments. Using historical data on road conditions, we can apply predictive models to estimate when a segment will need repairs or maintenance.

2. **Prioritization**:
   - The PCI scores are essential for prioritizing road segments. By identifying areas with the lowest PCI values (i.e., worst road conditions), we can generate a ranked list of assets that need immediate attention. This helps allocate resources effectively for repairs.

3. **Spatial Analysis**:
   - If geographical data (e.g., coordinates) is available, the `seg_id` and `pci` can be linked to spatial analysis. This will help identify clusters of poorly conditioned roads in specific areas, enabling more targeted interventions.

4. **Resource Allocation Recommendations**:
   - Based on the PCI values, we can suggest which road segments should be prioritized in terms of maintenance crews and budget allocation, which aligns with our project's goal of efficient resource management.

---

**Why This Dataset Could Be Limiting for Our Project:**

1. **Limited Contextual Data**:
   - While the dataset provides critical information on road conditions, it lacks contextual factors like traffic volume, weather patterns, and urban zoning, which are crucial for understanding broader maintenance demands. Without this supplementary data, predictions and prioritizations may miss critical environmental influences that affect road degradation.
  
2. **Lack of Real-Time Data**:
   - The dataset does not include real-time updates, such as current repair projects or citizen-reported pothole issues, which could impact the immediate need for repairs. Including more dynamic data sources would provide a more accurate and responsive system for maintenance planning.

3. **Potential Data Gaps**:
   - If certain road segments are missing from the dataset or have incomplete condition assessments, the analysis could be skewed. Having a more comprehensive and continuously updated dataset would increase the reliability of predictive modeling and prioritization.
