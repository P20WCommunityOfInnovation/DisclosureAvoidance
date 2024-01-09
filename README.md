# Data Anonymizer Script
## Overview
The Data Anonymizer script is designed to anonymize sensitive data in a DataFrame. This script is especially useful in scenarios where data privacy and confidentiality are paramount. It includes various methods for redacting sensitive information based on criteria such as frequency, minimum thresholds, and user-requested records.

## Requirements
Python Version:
* Python 3.10

Packages: 
* Pandas
* itertools

You can install the required packages using pip:

```bash
pip install pandas
```
(Note: itertools is part of the Python standard library and does not need to be installed separately.)

## Modules and Functions
### Import Statements
The script uses:

Pandas for data manipulation (pd)

### DataAnonymizer Class
#### __init__(df, parent_organization=None, child_organization=None, sensitive_columns=None, frequency=None, redact_column=None, minimum_threshold=10)
`df`: Initializes the DataAnonymizer object with a data frame df.

`parent_organization`: Parent organization column name.

`child_organization`: Child organization column name.

`sensitive_columns`: Columns that contain sensitive data.

`frequency`: Column indicating frequency/count of data.

`redact_column`: Column to apply redaction.

`minimum_threshold`: The minimum threshold for redaction.

#### create_log()

Generates a log of data groups based on sensitive columns and thresholds. The create log method aggregates data, applies minimum threshold checks, and prepares a detailed log for further redaction steps.
#### redact_user_requested_records()

Specifically redacts records as per user requests. If a record is marked for redaction in the 'UserRedact' column, the redact user requested records method will update its status to reflect user-requested redaction.
#### less_than_threshold_not_zero()

Applies primary suppression to records where the frequency is less than the specified minimum threshold but not zero. The less than threshold not zero updates these records to indicate they have been redacted due to failing to meet the minimum threshold criteria.

#### redact_threshold()

Identifies and redacts data that overlaps with other redacted values. The redact threshold method is useful for maintaining data integrity and ensuring that sensitive data is not indirectly exposed through overlapping data points.
#### sum_redact()

Redacts records based on the sum of their frequencies. If the total sum of a group of records is less than the minimum threshold, those records are redacted for additional privacy protection.

#### one_count_redacted()

Targets and redacts individual records in a group if they are the only ones left after other redactions. The one count redacted method is a form of secondary suppression to prevent singling out individual data points.
#### one_redact_zero()

Specifically focuses on if one single data point is redacted then redact the zero. The one redact zero method ensures that these unique cases are handled appropriately to maintain data anonymity.

#### cross_suppression()

Implements a more complex form of redaction where records are suppressed based on cross-referencing between parent and child organizations. The cross suppression method is designed to handle cases where sensitive data might be indirectly exposed through relationships between different data groups.
#### apply_log()

Apply log method finalizes the redaction process by merging the redaction log with the original data. The apply log method ensures that all redaction rules are applied consistently across the dataset.
#### apply_anonymization()

The central method that orchestrates the entire data anonymization process. Apply anonymization sequentially calls other methods in the class to apply a comprehensive anonymization strategy, resulting in a fully redacted and anonymized dataset.

#### get_log()

Outputs log in a dataframe for the user to access at any point. The main point is to be able to retrieve the log if an error occurs while running another method.

### Example Usage
Here is a quick example:

```python
import pandas as pd
from data_anonymizer import DataAnonymizer

# Sample DataFrame
data = {
    'Parent_Organization': ['Parent1', 'Parent1', 'Parent2', 'Parent2', 'Parent2', 'Parent3'],
    'Organization': ['Org1', 'Org2', 'Org3', 'Org4', 'Org5', 'Org6'],
    'Gender': ['F', 'M', 'F', 'M', 'F', 'M'],
    'Count': [20, 0, 13, 5, 25, 9]
}

df = pd.DataFrame(data)

# Instantiate the anonymizer
anonymizer = DataAnonymizer(df, parent_organization='Parent_Organization', child_organization='Organization', sensitive_columns='Gender', frequency='Count')

# Apply anonymization
df_anonymized = anonymizer.apply_anonymization()

# Output to CSV
df_anonymized.to_csv('AnonymizedData.csv', index=False)
```

## Known Issues
### Performance with Large Datasets
The script may experience performance degradation when processing extremely large datasets, leading to longer execution times. We plan to utilize Spark in the future to better handle large datasets. 

### Handling of Null Values
Current implementation might not optimally handle null or missing values in the dataset, which could affect the accuracy of the anonymization process.

### Limited Support for Non-Numeric Data
The script may have limited functionality when dealing with non-numeric or categorical data, potentially leading to less effective anonymization for such data types.

### Inconsistent Redaction Across Similar Values
There might be instances where similar values are not consistently redacted, leading to potential risks of data re-identification.

### Complex Nested Data Structure
The current version may struggle with complex nested data structures, like JSON objects or XML trees, potentially leading to incomplete anonymization.

### Overlapping Data Sensitivity
In cases where data sensitivity overlaps between parent and child organizations, the script might not optimally redact the overlapping sensitive data.

### Balance Between Anonymization and Data Utility
Striking the right balance between data anonymization and retaining the utility of the data for analysis purposes can be challenging.

### Unicode and Special Characters Handling
The script might face issues in correctly processing text fields containing Unicode or special characters.

### Limited Customization in Redaction Methods
As of now, the customization options for different redaction methods are limited, which may not suit all types of datasets.

### Dependency on External Libraries
The script's reliance on external libraries like Pandas may pose compatibility issues with different versions or in environments where these libraries are not available.

## Future Enhancements
### Dynamic Threshold Adjustment
Implement an algorithm that dynamically adjusts the minimum threshold based on the dataset's size and diversity. Dynamic Threshold Adjustment would ensure more efficient and context-sensitive anonymization.

### Enhanced Pattern Recognition
Develop functionality to identify and redact indirect identifiers or patterns that could lead to data re-identification, such as unique combinations of non-sensitive attributes.

### User Interface for Parameter Configuration
Create a graphical user interface (GUI) that allows users to easily configure anonymization parameters without editing the code directly.

### Automated Data Quality Checks
Integrate automated checks to ensure data quality and consistency post-anonymization, helping to maintain the integrity and usability of the data.

### Support for More Data Types
Extend the class to handle a wider range of data types, such as geospatial data, timestamps, and complex data structures, for more comprehensive anonymization.

### Parallel Processing for Large Datasets
Implement parallel processing capabilities to handle large datasets more efficiently, reducing the time required for anonymization.

### Customizable Redaction Techniques
Allow users to define custom redaction techniques, such as hashing or encryption, for specific columns or data types.

### Machine Learning Integration
Leverage machine learning algorithms to identify and protect potential indirect identifiers, enhancing the effectiveness of the anonymization process.

### API and Integration Support
Develop an API for the DataAnonymizer class, allowing it to be easily integrated with other software applications or data processing pipelines.

### Continuous Learning and Improvement
Implement a feedback mechanism where the tool learns from its anonymization decisions and improves over time, especially in handling complex or ambiguous cases.

### Enhanced Security Features
Add features to ensure the security of the data during the anonymization process, such as secure memory handling and protection against data leaks.


## Authors
Drew Bennett-Stein

Nathan Clinton

Garett Amstutz

Seth Taylor