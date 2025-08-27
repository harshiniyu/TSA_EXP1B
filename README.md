# Ex.No: 1B                     CONVERSION OF NON STATIONARY TO STATIONARY DATA
# Date: 

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
from statsmodels.tsa.stattools import adfuller


data = pd.read_csv("/content/TSLA.csv")

data['Date'] = pd.to_datetime(data['Date'])
data.set_index('Date', inplace=True)

series = data['Adj Close']

data['diff'] = series - series.shift(1)

data['log'] = np.log(series)

data['log_diff'] = data['log'] - data['log'].shift(1)

result = seasonal_decompose(data['log'].dropna(), model='additive', period=5)

data['log_seasonal_diff'] = data['log_diff'] - data['log_diff'].shift(5)


def adf_test(series, title=''):
    series = series.dropna()
    result = adfuller(series, autolag='AIC')
    print(f"ADF Test for {title}")
    print(f"ADF Statistic: {result[0]}")
    print(f"p-value: {result[1]}")
    print("Stationary" if result[1] <= 0.05 else "Non-stationary")
    print("-"*50)

adf_test(series, "Original Series")
adf_test(data['diff'], "Regular Differenced")
adf_test(data['log_diff'], "Log Differenced")
adf_test(data['log_seasonal_diff'], "Log + Seasonal Differenced")

plt.figure(figsize=(16, 16))

plt.subplot(6, 1, 1)
plt.plot(series, label='Original')
plt.legend(loc='best')
plt.title('Original Data (TSLA Adj Close)')

plt.subplot(6, 1, 2)
plt.plot(data['diff'], label='Regular Difference')
plt.legend(loc='best')
plt.title('Regular Differencing')

plt.subplot(6, 1, 3)
plt.plot(result.trend, label='Trend (Decomposition)')
plt.legend(loc='best')
plt.title('Trend (from Seasonal Decomposition)')

plt.subplot(6, 1, 4)
plt.plot(data['log'], label='Log Transformation')
plt.legend(loc='best')
plt.title('Log Transformation')

plt.subplot(6, 1, 5)
plt.plot(data['log_diff'], label='Log Differencing')
plt.legend(loc='best')
plt.title('Log Transformation + Differencing')

plt.subplot(6, 1, 6)
plt.plot(data['log_seasonal_diff'], label='Log + Regular + Seasonal Differencing')
plt.legend(loc='best')
plt.title('Log + Regular + Seasonal Differencing')

plt.tight_layout()
plt.show()

result.plot()
plt.show()
```

### OUTPUT:

<img width="1596" height="267" alt="image" src="https://github.com/user-attachments/assets/e5bf4011-b118-4704-989f-d43a003e3637" />

REGULAR DIFFERENCING:
<img width="1563" height="256" alt="image" src="https://github.com/user-attachments/assets/0151d916-fd36-46aa-9fe4-1091d536b281" />


SEASONAL ADJUSTMENT:
<img width="1570" height="245" alt="image" src="https://github.com/user-attachments/assets/a31a61b8-f886-4b70-99bc-f252525b2636" />


LOG TRANSFORMATION:

<img width="1570" height="517" alt="image" src="https://github.com/user-attachments/assets/8bcecbbc-39d8-46af-b78f-11b16da56076" />

<img width="1558" height="241" alt="image" src="https://github.com/user-attachments/assets/45ae71ec-d84c-4c21-b759-4f3e84fadb6f" />
<img width="635" height="466" alt="image" src="https://github.com/user-attachments/assets/5890e85e-2833-4d02-b0ed-e6ed956b509a" />

### RESULT:
Thus we have created the python code for the conversion of non stationary to stationary data on international airline passenger
data.
