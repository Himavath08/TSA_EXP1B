# Ex.No: 1B               CONVERSION OF NON STATIONARY TO STATIONARY DATA
# Date: 26/08/2025
### AIM:
To perform regular differncing,seasonal adjustment and log transformatio on international airline passenger data
### ALGORITHM:
1. Import the required packages like pandas and numpy
2. Read the data using the pandas
3. Perform the data preprocessing if needed and apply regular differncing,seasonal adjustment,log transformation.
4. Plot the data according to need, before and after regular differncing,seasonal adjustment,log transformation.
5. Display the overall results.
### PROGRAM:
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from statsmodels.tsa.seasonal import seasonal_decompose

data = pd.read_csv('cardekho.csv')
ts_data = data.groupby("year")["selling_price"].mean().reset_index()

ts_data["year"] = pd.to_datetime(ts_data["year"], format="%Y")
ts_data.set_index("year", inplace=True)

ts_data["selling_price"] = ts_data["selling_price"]

ts_data["price_diff"] = ts_data["selling_price"] - ts_data["selling_price"].shift(1)

period = 2 if len(ts_data) > 2 else 1
result = seasonal_decompose(ts_data["selling_price"], model="additive", period=period)
ts_data["price_sea_diff"] = result.resid

ts_data["price_log"] = np.log(ts_data["selling_price"])

ts_data["price_log_diff"] = ts_data["price_log"] - ts_data["price_log"].shift(1)

if ts_data["price_log_diff"].dropna().shape[0] > period:
    result_log = seasonal_decompose(ts_data["price_log_diff"].dropna(), model="additive", period=period)
    ts_data["price_log_seasonal_diff"] = result_log.resid
else:
    ts_data["price_log_seasonal_diff"] = np.nan

plt.figure(figsize=(12, 20))

plt.subplot(6, 1, 1)
plt.plot(ts_data["selling_price"], label="Original")
plt.legend(loc="best")
plt.title("Original Data (Average Selling Price by Year)")
plt.xlabel("Year")
plt.ylabel("Price")

plt.tight_layout()
plt.show()
plt.figure(figsize=(12, 20))
plt.subplot(6, 1, 2)
plt.plot(ts_data["price_diff"], label="Regular Difference")
plt.legend(loc="best")
plt.title("Regular Differencing")
plt.xlabel("Year")
plt.ylabel("Differenced Price")
plt.figure(figsize=(12, 20))
plt.subplot(6, 1, 3)
plt.plot(ts_data["price_sea_diff"], label="Seasonal Adjustment")
plt.legend(loc="best")
plt.title("Seasonal Adjustment")
plt.xlabel("Year")
plt.ylabel("Seasonally Adjusted Price")
plt.figure(figsize=(12, 20))
plt.subplot(6, 1, 4)
plt.plot(ts_data["price_log"], label="Log Transformation")
plt.legend(loc="best")
plt.title("Log Transformation")
plt.xlabel("Year")
plt.ylabel("Log(Price)")
plt.figure(figsize=(12, 20))
plt.subplot(6, 1, 5)
plt.plot(ts_data["price_log_diff"], label="Log Transformation and Regular Differencing")
plt.legend(loc="best")
plt.title("Log Transformation and Regular Differencing")
plt.xlabel("Year")
plt.ylabel("RDiff(Log(Price))")
plt.figure(figsize=(12, 20))
plt.subplot(6, 1, 6)
plt.plot(ts_data["price_log_seasonal_diff"], label="Log + Regular + Seasonal Differencing")
plt.legend(loc="best")
plt.title("Log + Regular + Seasonal Differencing")
plt.xlabel("Year")
plt.ylabel("SDiff(RDiff(Log(Price)))")
```

### OUTPUT:
<img width="1251" height="385" alt="image" src="https://github.com/user-attachments/assets/4b7f7884-d424-49b2-a703-4fd845427df7" />


REGULAR DIFFERENCING:
<img width="1237" height="413" alt="image" src="https://github.com/user-attachments/assets/e97fac39-a772-476f-8643-142858877b92" />


SEASONAL ADJUSTMENT:
<img width="1235" height="403" alt="image" src="https://github.com/user-attachments/assets/18908e05-70d3-42ee-9298-3c00fbd7d5c8" />


LOG TRANSFORMATION:
<img width="1241" height="422" alt="image" src="https://github.com/user-attachments/assets/866b9d5c-554c-4884-8d92-92798a02013f" />
<img width="1241" height="421" alt="image" src="https://github.com/user-attachments/assets/c8a9855d-6f6a-4b53-a06f-45c359c22dcd" />
<img width="1247" height="417" alt="image" src="https://github.com/user-attachments/assets/679fdd39-bbd9-4bbc-9208-bb2aead0223c" />



### RESULT:
Thus we have created the python code for the conversion of non stationary to stationary data on international airline passenger
data.
