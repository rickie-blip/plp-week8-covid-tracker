# plp-week8-covid-tracker
# Step 1: Import Libraries
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Optional: better plot styles
sns.set(style='whitegrid')
plt.rcParams['figure.figsize'] = (14, 7)

# Step 2: Load Dataset
df = pd.read_csv("owid-covid-data.csv")

# Step 3: Basic Exploration
print(df.columns)
print(df[['location', 'date', 'total_cases', 'total_deaths']].head())

# Step 4: Convert 'date' to datetime
df['date'] = pd.to_datetime(df['date'])

# Step 5: Filter Countries of Interest
countries = ['Kenya', 'United States', 'India']
df_countries = df[df['location'].isin(countries)]

# Step 6: Handle Missing Values
df_countries = df_countries[['location', 'date', 'total_cases', 'total_deaths', 'new_cases', 'new_deaths', 'total_vaccinations', 'population']]
df_countries.fillna(0, inplace=True)

# Step 7: Plot Total Cases Over Time
plt.figure()
for country in countries:
    country_df = df_countries[df_countries['location'] == country]
    plt.plot(country_df['date'], country_df['total_cases'], label=country)
plt.title('Total COVID-19 Cases Over Time')
plt.xlabel('Date')
plt.ylabel('Total Cases')
plt.legend()
plt.tight_layout()
plt.show()

# Step 8: Plot Total Deaths Over Time
plt.figure()
for country in countries:
    country_df = df_countries[df_countries['location'] == country]
    plt.plot(country_df['date'], country_df['total_deaths'], label=country)
plt.title('Total COVID-19 Deaths Over Time')
plt.xlabel('Date')
plt.ylabel('Total Deaths')
plt.legend()
plt.tight_layout()
plt.show()

# Step 9: Calculate Death Rate = total_deaths / total_cases
df_countries['death_rate'] = df_countries['total_deaths'] / df_countries['total_cases'].replace(0, 1)

# Step 10: Plot Vaccination Progress
plt.figure()
for country in countries:
    country_df = df_countries[df_countries['location'] == country]
    plt.plot(country_df['date'], country_df['total_vaccinations'], label=country)
plt.title('COVID-19 Vaccinations Over Time')
plt.xlabel('Date')
plt.ylabel('Total Vaccinations')
plt.legend()
plt.tight_layout()
plt.show()

# Step 11: Summary Insights
latest = df_countries[df_countries['date'] == df_countries['date'].max()]
print("🔍 Key Metrics on Latest Date:\n")
print(latest[['location', 'total_cases', 'total_deaths', 'death_rate', 'total_vaccinations']])
