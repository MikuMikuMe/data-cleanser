# data-cleanser

Certainly! I'll provide you with a complete Python program for a basic data cleansing library called `DataCleanser`. This library will include functionality for handling missing values, removing duplicates, encoding categorical variables, and normalizing numerical features. This should serve as a good starting point for implementing automated data cleaning and preprocessing in machine learning workflows.

Here's a basic implementation:

```python
import pandas as pd
from sklearn.preprocessing import LabelEncoder, StandardScaler

class DataCleanser:
    def __init__(self, dataframe):
        """
        Initialize the DataCleanser with a pandas DataFrame.
        
        Args:
            dataframe (pd.DataFrame): The dataset to be cleaned.
        """
        if not isinstance(dataframe, pd.DataFrame):
            raise ValueError("Input is not a valid pandas DataFrame")
        self.df = dataframe

    def handle_missing_values(self, strategy='mean', fill_value=None):
        """
        Handle missing values in the DataFrame.
        
        Args:
            strategy (str): The strategy to fill missing values ('mean', 'median', 'most_frequent', or 'constant').
            fill_value (any): Value to use with the 'constant' strategy.
        """
        if strategy not in ['mean', 'median', 'most_frequent', 'constant']:
            raise ValueError("Strategy not recognized. Use 'mean', 'median', 'most_frequent', or 'constant'.")

        if strategy == 'mean':
            self.df.fillna(self.df.mean(), inplace=True)
        elif strategy == 'median':
            self.df.fillna(self.df.median(), inplace=True)
        elif strategy == 'most_frequent':
            self.df.fillna(self.df.mode().iloc[0], inplace=True)
        elif strategy == 'constant':
            if fill_value is None:
                raise ValueError("fill_value must be provided when using 'constant' strategy")
            self.df.fillna(fill_value, inplace=True)

    def remove_duplicates(self):
        """
        Remove duplicate entries from the DataFrame.
        """
        self.df.drop_duplicates(inplace=True)

    def encode_categorical(self):
        """
        Encode categorical features using label encoding.
        """
        encoder = LabelEncoder()

        for column in self.df.select_dtypes(include=['object']).columns:
            try:
                self.df[column] = encoder.fit_transform(self.df[column])
            except Exception as e:
                print(f"Error encoding column {column}: {e}")

    def normalize_numerical(self):
        """
        Normalize numerical features using z-score normalization.
        """
        scaler = StandardScaler()

        try:
            numerical_columns = self.df.select_dtypes(include=['float64', 'int64']).columns
            self.df[numerical_columns] = scaler.fit_transform(self.df[numerical_columns])
        except Exception as e:
            print(f"Error normalizing data: {e}")

    def get_clean_data(self):
        """
        Return the cleaned DataFrame.
        
        Returns:
            pd.DataFrame: The cleaned dataset.
        """
        return self.df.copy()

# Usage Example
if __name__ == '__main__':
    try:
        # Sample data
        data = {
            'age': [25, 30, None, 29, None],
            'salary': [50000, None, 60000, 58000, 54000],
            'gender': ['Male', 'Female', 'Female', None, 'Male']
        }
        df = pd.DataFrame(data)
        
        # Instantiate DataCleanser
        cleaner = DataCleanser(df)
        
        # Handle missing values, remove duplicates, and encode categorical variables
        cleaner.handle_missing_values(strategy='mean')
        cleaner.remove_duplicates()
        cleaner.encode_categorical()
        cleaner.normalize_numerical()
        
        # Retrieve the cleaned data
        clean_df = cleaner.get_clean_data()
        print(clean_df)
    except Exception as e:
        print(f"An error occurred: {e}")
```

### Key Features
- **Handles Missing Values**: Choose a strategy like mean, median, most frequent, or a constant to fill missing values.
- **Removes Duplicates**: Removes duplicate rows from the DataFrame.
- **Encodes Categorical Variables**: Converts categorical text data into numerical form using label encoding.
- **Normalizes Numerical Variables**: Standardizes numerical data to have a mean of zero and a standard deviation of one.

### Error Handling
The program includes basic error handling to ensure the functions handle unexpected inputs gracefully. It prints an informative error message if something goes wrong in any process.