# Ex.No: 02 LINEAR AND POLYNOMIAL TREND ESTIMATION
Date:
### AIM:
To Implement Linear and Polynomial Trend Estiamtion Using Python.

### ALGORITHM:
Import necessary libraries (NumPy, Matplotlib)

Load the dataset

Calculate the linear trend values using least square method

Calculate the polynomial trend values using least square method

End the program
### PROGRAM:
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

data = pd.read_csv(
    '/content/weather_2013_to_2024.csv',
    parse_dates=['DATE'],
    index_col='DATE'
)

data.head() 
resampled_data = data['temp'].resample('YE').mean().to_frame()

resampled_data.head()

resampled_data.index = resampled_data.index.year

resampled_data.reset_index(inplace=True)

resampled_data.rename(columns={'temp': 'Temperature'}, inplace=True)
resampled_data.rename(columns={'DATE': 'Year'}, inplace=True)

resampled_data.head()

years = resampled_data['Year'].tolist()

temperature = resampled_data['Temperature'].tolist()

```
A - LINEAR TREND ESTIMATION
```python
X = [i - years[len(years) // 2] for i in years]

x2 = [i ** 2 for i in X]

xy = [i * j for i, j in zip(X, temperature)]

n = len(years)

b = (n * sum(xy) - sum(temperature) * sum(X)) / \
    (n * sum(x2) - (sum(X) ** 2))

a = (sum(temperature) - b * sum(X)) / n

linear_trend = [a + b * X[i] for i in range(n)]
```

B- POLYNOMIAL TREND ESTIMATION
```python
x3 = [i ** 3 for i in X]

x4 = [i ** 4 for i in X]

x2y = [i * j for i, j in zip(x2, temperature)]

coeff = [
    [len(X), sum(X), sum(x2)],
    [sum(X), sum(x2), sum(x3)],
    [sum(x2), sum(x3), sum(x4)]
]

Y = [
    sum(temperature),
    sum(xy),
    sum(x2y)
]

A = np.array(coeff)

B = np.array(Y)

solution = np.linalg.solve(A, B)

a_poly, b_poly, c_poly = solution

poly_trend = [
    a_poly + b_poly * X[i] + c_poly * (X[i] ** 2)
    for i in range(n)
]
```
```
# Visualising results

print(f"Linear Trend: y = {a:.2f} + {b:.2f}x")

print(f"\nPolynomial Trend: y = {a_poly:.2f} + "
      f"{b_poly:.2f}x + {c_poly:.2f}x²")

resampled_data['Linear Trend'] = linear_trend

resampled_data['Polynomial Trend'] = poly_trend

resampled_data.set_index('Year', inplace=True)

# Plot Original Data

resampled_data['Temperature'].plot(
    kind='line',
    color='blue',
    marker='o'
)

# Plot Linear Trend

resampled_data['Linear Trend'].plot(
    kind='line',
    color='black',
    linestyle='--'
)

# Plot Polynomial Trend

resampled_data['Polynomial Trend'].plot(
    kind='line',
    color='red',
    marker='o'
)

plt.title('Weather Data Trend Analysis')

plt.xlabel('Year')

plt.ylabel('Temperature')

plt.grid(True)

plt.show()
```

### OUTPUT
LINEAR TREND ESTIMATION AND POLYNOMIAL TREND ESTIMATION
<img width="750" height="523" alt="image" src="https://github.com/user-attachments/assets/67fb80a2-9194-456f-912d-224af59093c6" />


### RESULT:
Thus the python program for linear and Polynomial Trend Estiamtion has been executed successfully.
