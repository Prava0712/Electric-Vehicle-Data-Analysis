# Project Title :  Electric-Vehicle-Data-Analysis
# Analyze a dataset related to electric vehicles (EVs). The dataset contains various features such as electric range, energy consumption, price, and other relevant attributes. Your goal is to conduct a thorough analysis to uncover meaningful insights, conduct hypothesis testing and provide actionable recommendations based on the data
# Task 1: A customer has a budget of 350,000 PLN and wants an EV with a minimum range of 400 km.
import pandas as pd
EVdata = pd.read_csv(r"C:\Users\Pravallika\Downloads\EVdata.csv")
EVdata.head()
# a) Your task is to filter out EVs that meet these criteria
filtered_evs = EVdata[(EVdata["Minimal price (gross) [PLN]"] <= 350000) & (EVdata["Range (WLTP) [km]"] >= 400)]
print("EVs within budget and required range:")
print(filtered_evs[["Make", "Model", "Minimal price (gross) [PLN]",
"Range (WLTP) [km]", "Battery capacity [kWh]"]])

# b) Group them by the manufacturer (Make)
grouped = filtered_evs.groupby("Make")
print("\nNumber of EVs by Manufacturer:")
print(grouped.size())

# c) Calculate the average battery capacity for each manufacturer.
grouped = filtered_evs.groupby("Make")["Battery capacity
[kWh]"].mean().reset_index()
grouped.rename(columns={"Battery capacity [kWh]":
"Avg_Battery_Capacity_kWh"}, inplace=True)
print("\nAverage Battery Capacity per Manufacturer:")
print(grouped)

# Task 2: You suspect some EVs have unusually high or low energy consumption.
# Find the outliers in the mean - Energy consumption [kWh/100 km] column.
import pandas as pd
import numpy as np
from scipy import stats
EVdata = pd.read_csv(r"C:\Users\Pravallika\Downloads\EVdata.csv")
consumption = EVdata["mean - Energy consumption [kWh/100 km]"]
z_scores = stats.zscore(consumption)
outliers = EVdata[np.abs(z_scores) > 3]
print("Outliers in Energy Consumption:")
print(outliers[["Make", "Model", "mean - Energy consumption [kWh/100 km]"]])

# Task 3: Your manager wants to know if there's a strong relationship
between battery capacity and range.
# a) Create a suitable plot to visualize
# b) Highlight any insights.
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
EVdata = pd.read_csv(r"C:\Users\Pravallika\Downloads\EVdata.csv")
plt.figure(figsize=(8,6))
sns.scatterplot(
 data=EVdata,
 x="Battery capacity [kWh]",
 y="Range (WLTP) [km]",
 hue="Make",
 palette="tab10"
)
plt.title("Battery Capacity vs Range of EVs")
plt.xlabel("Battery Capacity (kWh)")
plt.ylabel("Range (km)")
plt.legend(title="Make", bbox_to_anchor=(1.05, 1), loc="upper left")
plt.grid(True, linestyle="--", alpha=0.6)
plt.tight_layout()
plt.show()

# Task 4: Build an EV recommendation class. The class should allow
users to input their
# budget, desired range, and battery capacity. The class should then
return the top three EVs
# matching their criteria.
import pandas as pd
class EVRecommender:
 def __init__(self, data_file):
 self.df = pd.read_csv(data_file)
 def recommend(self, budget, min_range, min_battery):
 Recommend top 3 EVs based on user input.
 """
 filtered = self.df[
 (self.df["Minimal price (gross) [PLN]"] <= budget) &
 (self.df["Range (WLTP) [km]"] >= min_range) &
 (self.df["Battery capacity [kWh]"] >= min_battery)
 ]
 filtered = filtered.sort_values(by="Range (WLTP) [km]",
ascending=False)
 return filtered.head(3)[["Make", "Model", "Minimal price
(gross) [PLN]", "Range (WLTP) [km]", "Battery capacity [kWh]"]]
if __name__ == "__main__":
 recommender = EVRecommender(r"C:\Users\Pravallika\Downloads\
EVdata.csv")
 budget = 350000
 desired_range = 400
 desired_battery = 70
 recommendations = recommender.recommend(budget, desired_range,
desired_battery)
 print("Top 3 Recommended EVs:")
 print(recommendations)

 # Task 5: Inferential Statistics – Hypothesis Testing: Test whether there is a significant
# difference in the average Engine power [KM] of vehicles manufactured by two leading
# manufacturers i.e. Tesla and Audi. What insights can you draw from the test results?
# Recommendations and Conclusion: Provide actionable insights based on your analysis.
# (Conduct a two sample t-test using ttest_ind from scipy.stats module)

import pandas as pd
from scipy.stats import ttest_ind
df = pd.read_csv(r"C:\Users\Pravallika\Downloads\EVdata.csv")
tesla_power = df[df["Make"] == "Tesla"]["Engine power [KM]"].dropna()
audi_power = df[df["Make"] == "Audi"]["Engine power [KM]"].dropna()
t_stat, p_val = ttest_ind(tesla_power, audi_power, equal_var=False)
print("T-statistic:", round(t_stat, 3))
print("P-value:", round(p_val, 4))
