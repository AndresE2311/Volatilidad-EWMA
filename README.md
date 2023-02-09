# Volatilidad-EWMA
# Solución tarea
!pip install --quiet yfinance 
import yfinance as yf
import numpy as np
import pandas as pd

p_name = yf.download("TSLA",start="2021-01-01",end="2022-12-31")['Adj Close'].dropna()
r_name = np.log(p_name/p_name.shift(1)).dropna()
# Primero definimos 'k' que es la cantidad de retornos en un periodo dado 
k=len(r_name)
# Determinamos una matriz en donde estan todos los posibles valores enteros que puede tomar 'k-1'
dk = np.linspace(k-1,0,502)
# Determino lamda
lamda = 0.94
# Ahora determino la matriz de todos los posibles alfas teniendo encuenta que los ultimos años son los que tienen mayor alfa y los primeros menor. El alfa es el peso de importancia que le ponemos a cada dato 
# Sin embargo, nos hace falta encontrar el (1-lamda), que lo hacemos al final por estrategia de la ecuación.
alph = lamda**dk
# Entonces, la ecuación final quedaria así:
(r_name**2*alph).sum()*(1-lamda) # Esta seria la varianza 
# Por lo tanto la volatilidad se determina así:
np.sqrt((r_name**2*alph).sum()*(1-lamda))
