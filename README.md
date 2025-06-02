# Day-1-task
Clean and prepare a raw dataset (with nulls, duplicates, inconsistent formats).
import pandas as pd

df = pd.read_csv('/content/sample_data/netflix_titles.csv')  # Replace with correct path if needed
print(df.shape)
print(df.info())
print(df.head())
print(df.isnull().sum())
df = df.drop_duplicates()
# Check where nulls exist
print(df.isnull().sum())

# Common strategy: Drop rows where 'title' or 'type' is null (important fields)
df = df.dropna(subset=['title', 'type'])

# For less critical ones like 'director' or 'cast', fill with "Unknown"
df['director'] = df['director'].fillna('Unknown')
df['cast'] = df['cast'].fillna('Unknown')
df['country'] = df['country'].fillna('Unknown')
df['date_added'] = df['date_added'].fillna('Unknown')
df['rating'] = df['rating'].fillna('Unknown')
df['duration'] = df['duration'].fillna('Unknown')
# Strip whitespace and capitalize key fields
df['type'] = df['type'].str.strip().str.title()
df['country'] = df['country'].str.strip()
df['rating'] = df['rating'].str.strip().str.upper()
# Fix 'date_added' column to datetime
df['date_added'] = df['date_added'].apply(lambda x: pd.to_datetime(x, errors='coerce'))
df.columns = df.columns.str.strip().str.lower().str.replace(' ', '_')
# For reference
print(df.dtypes)
df['year_added'] = df['date_added'].dt.year
df['month_added'] = df['date_added'].dt.month
df.to_csv('netflix_titles_cleaned.csv', index=False)
