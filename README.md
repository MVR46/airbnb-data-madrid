# Airbnb Data

This repository contains Airbnb listings data in CSV format.

## Files

- `listings.csv` - Main dataset containing Airbnb listing information

## Usage

### Accessing from Google Colab

You can directly load this data into a Google Colab notebook using pandas:

```python
import pandas as pd

# Load the data directly from the raw GitHub URL
url = 'https://raw.githubusercontent.com/MVR46/airbnb-data-madrid/main/listings.csv'
df = pd.read_csv(url)

# Display basic information
print(f"Dataset shape: {df.shape}")
df.head()
```

## Data Source

Airbnb listings dataset.

