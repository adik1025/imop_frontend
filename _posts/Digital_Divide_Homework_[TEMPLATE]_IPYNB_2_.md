---
layout: notebook
---

```python
import pandas as pd

data = pd.read_csv("internet_users.csv").drop(columns=['Notes', 'Year.2', 'Users (CIA)', 'Rate (ITU)', 'Year.1']) # Drop extra columns: we will not be using these

data_cleaned = data.dropna() # Drop rows with NaN (aka blank) values

print(data_cleaned.head()) # Display the first few rows of the cleaned data

# print(len(data)) # Check num of rows before removing blank rows
# print(len(data_cleaned)) # Check num of rows after removing blank rows
```

          Location  Rate (WB)    Year
    0        World       67.4  2023.0
    1  Afghanistan       18.4  2020.0
    2      Albania       83.1  2023.0
    3      Algeria       71.2  2022.0
    5      Andorra       94.5  2022.0



```python
y = data_cleaned['Rate (WB)'] # Take Percentage of the population using the internet from World Bank data in dataset
name = data_cleaned['Location'] # take contry name from WB data in dataset

# [INSERT YOUR CODE HERE]
```

# Submission Instructions

Link your notebook on your personal repository **with all the code cells executed** on [this google form](https://docs.google.com/forms/d/e/1FAIpQLSfoeD9YkYSNtQzHxfSILKQU_RIfXgftufUGT86oz5rmcPnnlg/viewform).

Also write a 3 sentence summary of what you added and HOW it works.
