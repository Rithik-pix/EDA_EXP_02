# Exp - 2 Netflix Shows & Movies

### Name : Rithik Ram S
### Reg No : 212224230229

## Aim

To analyze Netflix dataset and compare movies vs TV shows, top producing countries, and release year trends.

## Procedure / Algorithm

```
1) Load dataset (netflix_titles.csv).
2) Count movies vs TV shows. 
3) Group by country → top contributors.
4) Create pivot table (release year vs type).
5) Visualize with bar & line charts.

```
## Program

```py
import pandas as pd
```

### 1. Load dataset directly from GitHub
```PY
url = "https://raw.githubusercontent.com/allenkong221/netflix-titles-dataset/main/netflix_titles.csv"
df = pd.read_csv(url)

# Basic inspection
print("Shape:", df.shape)
print("Columns:", df.columns, "\n")
print("Type counts:\n", df['type'].value_counts(), "\n")
```

### 2. Clean 'date_added' and extract year/month
```py
df['date_added'] = df['date_added'].str.strip() # remove extra spaces
df['date_added'] = pd.to_datetime(df['date_added'], errors='coerce') # invalid -> NaT
df['year_added'] = df['date_added'].dt.year
df['month_added'] = df['date_added'].dt.month_name()
```

### 3. Movies vs TV Shows
```py
count_by_type = df.groupby('type')['title'].count()
print("Count by Type:\n", count_by_type, "\n")

```

### 4. Country vs Type Pivot Table
```py
pivot_country_type = df.pivot_table(
index='country',
columns='type',
values='title',
aggfunc='count',
fill_value=0
)

# Add "Total" column and find max country
pivot_country_type['Total'] = pivot_country_type.sum(axis=1)
max_country = pivot_country_type['Total'].idxmax() # country with most titles
max_count = pivot_country_type['Total'].max() # number of titles
print("Pivot Table (Country vs Type):\n", pivot_country_type.head(), "\n")
print(f"Largest Overall: {max_country} with {max_count} titles\n")
```

### 5. Top 5 Directors
```py
top_directors = df['director'].value_counts().head(5)
print("Top 5 Directors:\n", top_directors, "\n")
```

### 6. Yearly Trend of Additions (Movies vs TV Shows)
```py
trend = df.groupby(['year_added', 'type']).size().unstack(fill_value=0)
print("Yearly Trend by Type:\n", trend.head(), "\n")
```

### 7. Expand Genres
```py
df_genre = (
df[['show_id','listed_in']]
.dropna()
.assign(listed_in=df['listed_in'].str.split(', '))
.explode('listed_in')
)

# Merge expanded genres back
df_expanded = df.merge(df_genre, on='show_id', how='left')

# Check merged columns
print("Columns after merge:\n", df_expanded.columns)

# Show sample of expanded genres
print("\nExpanded Genre Sample:\n",
df_expanded[['title','listed_in_y']].head())

# Top 5 Genres (extra useful for insights)
top_genres = df_expanded['listed_in_y'].value_counts().head(5)
print("\nTop 5 Genres:\n", top_genres, "\n")

```

## Ouptut

### Basic inspection 

<img width="1478" height="66" alt="image" src="https://github.com/user-attachments/assets/42db52cb-4110-43cc-a7ef-c0e926b7fba1" />


<img width="1315" height="128" alt="image" src="https://github.com/user-attachments/assets/f63381be-30d6-4a18-b844-b770965ec1a0" />


<img width="1279" height="190" alt="image" src="https://github.com/user-attachments/assets/b5625a99-690d-42c4-8523-5128c052cb67" />


### Movies vs TV Shows


<img width="954" height="201" alt="image" src="https://github.com/user-attachments/assets/8f9ef64c-cf12-492d-bdfc-af5f58951b79" />



### Add "Total" column and find max country

<img width="1267" height="391" alt="image" src="https://github.com/user-attachments/assets/8b6ff80b-6489-461e-9983-5e386c705a9d" />

### Yearly Trend of Additions (Movies vs TV Shows)

<img width="913" height="324" alt="image" src="https://github.com/user-attachments/assets/240d144c-f08c-4588-82b1-882b6bfbfce1" />


### Top 5 Directors

<img width="750" height="235" alt="image" src="https://github.com/user-attachments/assets/1027b236-72d9-4d19-b564-9a4b8e5b6826" />


### Check merged columns

<img width="1066" height="143" alt="image" src="https://github.com/user-attachments/assets/9168e879-603e-4ec0-8bc0-6464393ac765" />


### Show sample of expanded genres

<img width="976" height="200" alt="image" src="https://github.com/user-attachments/assets/476e497c-f75f-4f0b-97b6-396ad9056616" />


### Top 5 Genres (extra useful for insights)

<img width="1200" height="233" alt="image" src="https://github.com/user-attachments/assets/cbd8cd8d-3f85-4433-a804-adbf5b839a3f" />







## Result
Helps Netflix in content planning & investments.
