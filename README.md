 <div id="Portada" style="border-style: double; color:#4472C4;">
    <h1 style="text-align: center;font-size: 35px">Estadística aplicada a la minería</h1>
    <p style="text-align: center; border-bottom: 2px solid #4472C4; border-spacing: 5px; margin-bottom: 100px"></p>
    <img src="escudo.png">
    <h3 style="margin: auto; font-size: 16px; margin-left:10px">Nombre:</h3>
    <p style="color:black; margin:auto; font-size: 14px; margin-left:10px">Terán Miranda Luis Ángel</p>
    
</div>

 <div id="Indice" style="border-style: double; color:#4472C4;">
    <p style="margin: auto; font-size: 26px; margin-left:10px; text-align: center; margin-top: 25px;margin-bottom:20px">Índice</p>
    <ol>
        <li><a href=#AE>Análisis exploratorio</a></li>
        <ol>
            <li><a href=#AE>Análisis exploratorio</a></li>
            <li><a href=#Formato>Formato de los datos</a></li>
            <li><a href=#Limpieza>Limpieza de datos</a></li>
            <li><a href=#Histogramas>Histogramas</a></li>
            <li><a href=#valores>Valores más altos</a></li>
            <li><a href=#Boxplots>Boxplots</a></li>
        </ol>
        <li><a href=#AM>Análisis multivariable</a></li>
        <ol>
            <li><a href=#Scatter>Scatter plot de elementos</a></li>
            <li><a href=#Mcov>Matriz de covarianza</a></li>
            <li><a href=#Mcorr>Matriz de correlación</a></li>
        </ol>
        <li><a href=#AES>Análisis espacial</a></li>
        <ol>
            <li><a href=#Semivariograma>Semivariograma</a></li>
            <li><a href=#CP>Componentes Principales</a></li>
            <li><a href=#clust>Clustering</a></li>           
        </ol>
        <li><a href=#conc>Conclusiones finales</a></li>
    </ol>
    <p style="margin-bottom:50px"></p>
</div>


En el presente trabajo se realiza una clasificación litólogica de unidades de acuerdo a su composición química, obteniendo como resultado un mapa de la distribución espacial de los datos


```python
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
import math as m
import matplotlib.gridspec as gridspec
import seaborn as sns
```


```python
#Se lee el archivo de datos
datos = pd.read_csv('Geochem_dataset_P2.csv')
```

<h1 style="text-align: center; font-size: 35px">Análisis exploratorio</h1><a name='AE'/>

# Formato de los datos <a name='Formato'/>


```python
#Se muestra la tabla de datos original
datos.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
      <th>Unnamed: 1</th>
      <th>Recvd Wt.</th>
      <th>Au</th>
      <th>Ag</th>
      <th>Al</th>
      <th>As</th>
      <th>B</th>
      <th>Ba</th>
      <th>Be</th>
      <th>...</th>
      <th>Sc</th>
      <th>Sr</th>
      <th>Th</th>
      <th>Ti</th>
      <th>Tl</th>
      <th>U</th>
      <th>V</th>
      <th>W</th>
      <th>Zn</th>
      <th>Unnamed: 39</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>NORTE</td>
      <td>ESTE</td>
      <td>kg</td>
      <td>ppb</td>
      <td>ppm</td>
      <td>%</td>
      <td>ppm</td>
      <td>ppm</td>
      <td>ppm</td>
      <td>ppm</td>
      <td>...</td>
      <td>ppm</td>
      <td>ppm</td>
      <td>ppm</td>
      <td>%</td>
      <td>ppm</td>
      <td>ppm</td>
      <td>ppm</td>
      <td>ppm</td>
      <td>ppm</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>1</td>
      <td>4627001</td>
      <td>549050</td>
      <td>2.78</td>
      <td>19</td>
      <td>1.6</td>
      <td>3.71</td>
      <td>4</td>
      <td>&lt;10</td>
      <td>60</td>
      <td>&lt;0.5</td>
      <td>...</td>
      <td>12</td>
      <td>11</td>
      <td>&lt;20</td>
      <td>0.01</td>
      <td>&lt;10</td>
      <td>&lt;10</td>
      <td>155</td>
      <td>&lt;10</td>
      <td>139</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>2</td>
      <td>4627049</td>
      <td>549003</td>
      <td>3.32</td>
      <td>5</td>
      <td>0.3</td>
      <td>5.13</td>
      <td>&lt;2</td>
      <td>&lt;10</td>
      <td>40</td>
      <td>&lt;0.5</td>
      <td>...</td>
      <td>28</td>
      <td>5</td>
      <td>&lt;20</td>
      <td>0.01</td>
      <td>&lt;10</td>
      <td>&lt;10</td>
      <td>240</td>
      <td>&lt;10</td>
      <td>280</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>3</td>
      <td>4627073</td>
      <td>548963</td>
      <td>2.76</td>
      <td>17</td>
      <td>0.6</td>
      <td>1.39</td>
      <td>10</td>
      <td>10</td>
      <td>2240</td>
      <td>&lt;0.5</td>
      <td>...</td>
      <td>8</td>
      <td>67</td>
      <td>&lt;20</td>
      <td>0.01</td>
      <td>&lt;10</td>
      <td>&lt;10</td>
      <td>105</td>
      <td>&lt;10</td>
      <td>44</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>4</td>
      <td>4627050</td>
      <td>548964</td>
      <td>3.2</td>
      <td>8</td>
      <td>0.8</td>
      <td>1.74</td>
      <td>4</td>
      <td>&lt;10</td>
      <td>100</td>
      <td>&lt;0.5</td>
      <td>...</td>
      <td>11</td>
      <td>16</td>
      <td>&lt;20</td>
      <td>&lt;0.01</td>
      <td>&lt;10</td>
      <td>&lt;10</td>
      <td>165</td>
      <td>&lt;10</td>
      <td>48</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 40 columns</p>
</div>



<p style="text-align: center;font-size: 10px;font-style: oblique;">Tabla 1. Tabla de datos originales leídos</p>

<p style="margin-top:25px">Previo a realizar cálculos con los datos resulta necesario hacer correcciones al set de datos entregado, de tal manera que puedan ser manipulados posteriormente
</p>
<p>Las modificaciones realizadas son:
</p>
<ol>
  <li>Renombrar las columnas de coordenadas Este y Oeste debido a que permanecen sin nombrar</li>
  <li>Eliminar la segunda fila de datos de forma que únicamente queden datos numéricos</li>
  <li>Eliminar columnas de datos vacías (última columna)</li>
  <li>Eliminar las últimas 4 filas de datos debido a que se encuentran desplazadas de su posición respectiva y la información se encuentra incompleta</li>
  <li>Reiniciar el índice de valores tomando en cuenta los datos eliminados</li>
</ol>


```python
#---------------------------------------------------FORMATO DE LOS DATOS---------------------------------------------------------
#datos.columns=datos.columns+" ["+datos.loc[0,]+"]"
#Se guarda el tipo de concentración (%, ppm, ppb) para poder distinguirlos posteriormente
agregar=datos.loc[0,]
#Renombramos columnas 
datos.rename(columns={datos.columns[0]:str.lower(datos.loc[0,][0]),datos.columns[1]:str.lower(datos.loc[0,][1])},inplace = True)
#Eliminamos la segunda fila que no contiene datos
datos.drop(0, axis=0, inplace=True)
#Eliminamos la última columna de valores vacíos
datos.drop(datos.columns[-1], axis=1, inplace=True)
#Eliminamos las últimas 4 filas de datos por falta de parámetros
datos = datos[:-4]
#Se reinicia el índice de valores
datos.reset_index(drop=True, inplace=True)
```


```python
#Se muestran los datos con formato
datos.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>norte</th>
      <th>este</th>
      <th>Recvd Wt.</th>
      <th>Au</th>
      <th>Ag</th>
      <th>Al</th>
      <th>As</th>
      <th>B</th>
      <th>Ba</th>
      <th>Be</th>
      <th>...</th>
      <th>Sb</th>
      <th>Sc</th>
      <th>Sr</th>
      <th>Th</th>
      <th>Ti</th>
      <th>Tl</th>
      <th>U</th>
      <th>V</th>
      <th>W</th>
      <th>Zn</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>4627001</td>
      <td>549050</td>
      <td>2.78</td>
      <td>19</td>
      <td>1.6</td>
      <td>3.71</td>
      <td>4</td>
      <td>&lt;10</td>
      <td>60</td>
      <td>&lt;0.5</td>
      <td>...</td>
      <td>&lt;2</td>
      <td>12</td>
      <td>11</td>
      <td>&lt;20</td>
      <td>0.01</td>
      <td>&lt;10</td>
      <td>&lt;10</td>
      <td>155</td>
      <td>&lt;10</td>
      <td>139</td>
    </tr>
    <tr>
      <td>1</td>
      <td>4627049</td>
      <td>549003</td>
      <td>3.32</td>
      <td>5</td>
      <td>0.3</td>
      <td>5.13</td>
      <td>&lt;2</td>
      <td>&lt;10</td>
      <td>40</td>
      <td>&lt;0.5</td>
      <td>...</td>
      <td>&lt;2</td>
      <td>28</td>
      <td>5</td>
      <td>&lt;20</td>
      <td>0.01</td>
      <td>&lt;10</td>
      <td>&lt;10</td>
      <td>240</td>
      <td>&lt;10</td>
      <td>280</td>
    </tr>
    <tr>
      <td>2</td>
      <td>4627073</td>
      <td>548963</td>
      <td>2.76</td>
      <td>17</td>
      <td>0.6</td>
      <td>1.39</td>
      <td>10</td>
      <td>10</td>
      <td>2240</td>
      <td>&lt;0.5</td>
      <td>...</td>
      <td>&lt;2</td>
      <td>8</td>
      <td>67</td>
      <td>&lt;20</td>
      <td>0.01</td>
      <td>&lt;10</td>
      <td>&lt;10</td>
      <td>105</td>
      <td>&lt;10</td>
      <td>44</td>
    </tr>
    <tr>
      <td>3</td>
      <td>4627050</td>
      <td>548964</td>
      <td>3.2</td>
      <td>8</td>
      <td>0.8</td>
      <td>1.74</td>
      <td>4</td>
      <td>&lt;10</td>
      <td>100</td>
      <td>&lt;0.5</td>
      <td>...</td>
      <td>&lt;2</td>
      <td>11</td>
      <td>16</td>
      <td>&lt;20</td>
      <td>&lt;0.01</td>
      <td>&lt;10</td>
      <td>&lt;10</td>
      <td>165</td>
      <td>&lt;10</td>
      <td>48</td>
    </tr>
    <tr>
      <td>4</td>
      <td>4627090</td>
      <td>548870</td>
      <td>3.42</td>
      <td>70</td>
      <td>4.8</td>
      <td>0.23</td>
      <td>47</td>
      <td>&lt;10</td>
      <td>60</td>
      <td>&lt;0.5</td>
      <td>...</td>
      <td>&lt;2</td>
      <td>12</td>
      <td>46</td>
      <td>&lt;20</td>
      <td>&lt;0.01</td>
      <td>&lt;10</td>
      <td>&lt;10</td>
      <td>26</td>
      <td>&lt;10</td>
      <td>58</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 39 columns</p>
</div>



<p style="text-align: center;font-size: 10px;font-style: oblique;">Tabla 2. Tabla de datos con formato</p>

# Limpieza de datos <a name='Limpieza'/>

<p style="margin-top:25px">Posteriormente, se realiza la limpieza de datos, la cual consiste en distinguir entre aquellos datos que resultan de utilidad para el análisis y aquellos que no ofrecen valores certeros en suficiente cantidad debido al rango de medición del instrumento.
</p>
<p>Para ello, se estableció un porcentaje de referencia del 80%. Dicho valor establece el porcentaje mínimo necesario de datos en una columna con valores dentro del rango de medición del instrumento (valores numéricos). En caso de no superar el porcentaje de referencia, se determina que la columna de datos no es útil para el análisis, por lo que se elimina. 
</p>
<p>Por otro lado, incluso cuando la columna de datos supera el porcentaje de referencia, es posible que contenga valores fuera del rango de medición. En esos casos, se optó por asignar el valor más cercano dentro del rango de medición.
</p>
<p>El procedimiento realizado consistió en lo siguiente:
</p>
<ol>
  <li>Establecer un porcentaje de referencia</li>
  <li>Elegir una columna de datos</li>
  <li>Contar el número de datos fuera del rango de medición del instrumento</li>
  <li>Convertir el conteo a porcentaje</li>
  <li>Comparar el porcentaje de conteo con el porcentaje de referencia</li>
  <li>En caso de resultar menor el porcentaje de conteo, se elimina la columna. De lo contrario, se reemplazan los valores fuera del rango por su valor más cercano dentro del mismo</li>
  <li>Repetir el procedimiento para la columna siguiente</li>
</ol>


```python
#------------------------------------------------LIMPIEZA DE LOS DATOS----------------------------------------------------------
#Se establece la referencia del porcentaje de datos útiles para ver qué columnas se eliminan y cuáles no
porc=80
temp2=[]
#Ciclo en el cual se revisan todas las columnas
for i in range (len(datos.columns)):
    #Se hace el conteo de cuántos datos contienen los símbolos ">" o "<", y se calcula en porcentaje
    temp=datos[datos.columns[i]].str.contains('<', '>')
    vm=np.sum(temp)*100/len(datos[datos.columns[i]])    
    #Se compara si el número de datos no útiles es mayor al establecido como referencia
    if (vm>=porc):
        #En caso de serlo se guarda en un vector temporal
        temp2.append(i)
    else:
        #En caso de que la mayor parte de la información sea útil, se sustituyen los "</> número" por "número"
        datos[datos.columns[i]] = datos[datos.columns[i]].str.replace('<','')
        datos[datos.columns[i]] = datos[datos.columns[i]].str.replace('>','')
        #Se convierten los datos de tipo cadena a tipo flotante
        datos[datos.columns[i]] = datos[datos.columns[i]].astype(float)
#Se eliminan los tipos de concentración de las columnas a eliminar
agregar.drop(labels=datos.columns[temp2], inplace=True)
#Se eliminan las columnas que contienen más del 80% de datos no útiles
datos.drop(datos.columns[temp2], axis=1, inplace=True)
#Se guardan los datos "limpios" en un nuevo documento csv
datos.to_csv('datosLimpios.csv')
```


```python
#Se muestra el set de datos una vez realizada la limpieza
display(datos)
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>norte</th>
      <th>este</th>
      <th>Recvd Wt.</th>
      <th>Au</th>
      <th>Ag</th>
      <th>Al</th>
      <th>As</th>
      <th>B</th>
      <th>Ba</th>
      <th>Bi</th>
      <th>...</th>
      <th>Na</th>
      <th>Ni</th>
      <th>P</th>
      <th>Pb</th>
      <th>S</th>
      <th>Sc</th>
      <th>Sr</th>
      <th>Ti</th>
      <th>V</th>
      <th>Zn</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>4627001.0</td>
      <td>549050.0</td>
      <td>2.78</td>
      <td>19.0</td>
      <td>1.6</td>
      <td>3.71</td>
      <td>4.0</td>
      <td>10.0</td>
      <td>60.0</td>
      <td>2.0</td>
      <td>...</td>
      <td>0.08</td>
      <td>8.0</td>
      <td>220.0</td>
      <td>11.0</td>
      <td>0.03</td>
      <td>12.0</td>
      <td>11.0</td>
      <td>0.01</td>
      <td>155.0</td>
      <td>139.0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>4627049.0</td>
      <td>549003.0</td>
      <td>3.32</td>
      <td>5.0</td>
      <td>0.3</td>
      <td>5.13</td>
      <td>2.0</td>
      <td>10.0</td>
      <td>40.0</td>
      <td>2.0</td>
      <td>...</td>
      <td>0.07</td>
      <td>20.0</td>
      <td>160.0</td>
      <td>3.0</td>
      <td>0.01</td>
      <td>28.0</td>
      <td>5.0</td>
      <td>0.01</td>
      <td>240.0</td>
      <td>280.0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>4627073.0</td>
      <td>548963.0</td>
      <td>2.76</td>
      <td>17.0</td>
      <td>0.6</td>
      <td>1.39</td>
      <td>10.0</td>
      <td>10.0</td>
      <td>2240.0</td>
      <td>4.0</td>
      <td>...</td>
      <td>0.08</td>
      <td>2.0</td>
      <td>230.0</td>
      <td>3.0</td>
      <td>0.07</td>
      <td>8.0</td>
      <td>67.0</td>
      <td>0.01</td>
      <td>105.0</td>
      <td>44.0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>4627050.0</td>
      <td>548964.0</td>
      <td>3.20</td>
      <td>8.0</td>
      <td>0.8</td>
      <td>1.74</td>
      <td>4.0</td>
      <td>10.0</td>
      <td>100.0</td>
      <td>2.0</td>
      <td>...</td>
      <td>0.06</td>
      <td>5.0</td>
      <td>70.0</td>
      <td>2.0</td>
      <td>0.07</td>
      <td>11.0</td>
      <td>16.0</td>
      <td>0.01</td>
      <td>165.0</td>
      <td>48.0</td>
    </tr>
    <tr>
      <td>4</td>
      <td>4627090.0</td>
      <td>548870.0</td>
      <td>3.42</td>
      <td>70.0</td>
      <td>4.8</td>
      <td>0.23</td>
      <td>47.0</td>
      <td>10.0</td>
      <td>60.0</td>
      <td>20.0</td>
      <td>...</td>
      <td>0.05</td>
      <td>2.0</td>
      <td>360.0</td>
      <td>16.0</td>
      <td>0.04</td>
      <td>12.0</td>
      <td>46.0</td>
      <td>0.01</td>
      <td>26.0</td>
      <td>58.0</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>77</td>
      <td>4626815.0</td>
      <td>548942.0</td>
      <td>4.78</td>
      <td>5.0</td>
      <td>0.2</td>
      <td>3.37</td>
      <td>4.0</td>
      <td>10.0</td>
      <td>40.0</td>
      <td>2.0</td>
      <td>...</td>
      <td>0.08</td>
      <td>28.0</td>
      <td>170.0</td>
      <td>2.0</td>
      <td>0.02</td>
      <td>27.0</td>
      <td>21.0</td>
      <td>0.23</td>
      <td>217.0</td>
      <td>208.0</td>
    </tr>
    <tr>
      <td>78</td>
      <td>4626837.0</td>
      <td>548969.0</td>
      <td>3.80</td>
      <td>5.0</td>
      <td>0.4</td>
      <td>3.69</td>
      <td>6.0</td>
      <td>10.0</td>
      <td>20.0</td>
      <td>2.0</td>
      <td>...</td>
      <td>0.05</td>
      <td>13.0</td>
      <td>130.0</td>
      <td>2.0</td>
      <td>0.05</td>
      <td>19.0</td>
      <td>4.0</td>
      <td>0.01</td>
      <td>168.0</td>
      <td>210.0</td>
    </tr>
    <tr>
      <td>79</td>
      <td>4626956.0</td>
      <td>548842.0</td>
      <td>4.24</td>
      <td>6.0</td>
      <td>0.2</td>
      <td>0.45</td>
      <td>10.0</td>
      <td>10.0</td>
      <td>100.0</td>
      <td>2.0</td>
      <td>...</td>
      <td>0.08</td>
      <td>1.0</td>
      <td>580.0</td>
      <td>2.0</td>
      <td>0.08</td>
      <td>4.0</td>
      <td>12.0</td>
      <td>0.01</td>
      <td>11.0</td>
      <td>76.0</td>
    </tr>
    <tr>
      <td>80</td>
      <td>4626966.0</td>
      <td>548891.0</td>
      <td>3.36</td>
      <td>11.0</td>
      <td>0.2</td>
      <td>0.98</td>
      <td>8.0</td>
      <td>10.0</td>
      <td>200.0</td>
      <td>2.0</td>
      <td>...</td>
      <td>0.05</td>
      <td>4.0</td>
      <td>330.0</td>
      <td>2.0</td>
      <td>0.07</td>
      <td>14.0</td>
      <td>16.0</td>
      <td>0.01</td>
      <td>237.0</td>
      <td>59.0</td>
    </tr>
    <tr>
      <td>81</td>
      <td>4626808.0</td>
      <td>549001.0</td>
      <td>3.26</td>
      <td>5.0</td>
      <td>0.2</td>
      <td>2.63</td>
      <td>4.0</td>
      <td>10.0</td>
      <td>40.0</td>
      <td>2.0</td>
      <td>...</td>
      <td>0.07</td>
      <td>4.0</td>
      <td>560.0</td>
      <td>3.0</td>
      <td>1.70</td>
      <td>17.0</td>
      <td>14.0</td>
      <td>0.19</td>
      <td>180.0</td>
      <td>184.0</td>
    </tr>
  </tbody>
</table>
<p>82 rows × 31 columns</p>
</div>


<p style="text-align: center;font-size: 10px;font-style: oblique;">Tabla 3. Tabla de datos con limpieza terminada</p>


```python
#-----------------------------------------------VISUALIZACIÓN DE LOS DATOS-----------------------------------------------------
#Se realiza una visualización rápida de los datos muestreados y su ubicación
plt.figure(figsize=(15,13)) 
plt.scatter(datos.este,datos.norte)
plt.title('Figura 1. Mapa de coordenadas del muestreo realizado', fontsize=15)
plt.xlabel('[mE]')
plt.ylabel('[mN]')
plt.grid()
plt.show()
```


![png](outputImages/output_18_0.png)


<p style="text-align: center;font-size: 10px;font-style: oblique;">Figura 1. Mapa de coordenadas del muestreo realizado</p>

# Histogramas <a name='Histogramas'/>

<p style="margin-top:25px">A continuación, se presenta el mosaico de histogramas de cada uno de los elementos contenidos en el set de datos. Se decidió dividir la información en 20 clases, de tal manera que se dividiera el rango de valores de concentración, y así cada una de clases creada representaría el 5% del rango.
</p>
<p>El procedimiento realizado consistió en lo siguiente:
</p>
<ol>
  <li>Establecer las dimensiones del mosaico</li>
  <li>Crear el mosaico</li>
  <li>Definir el tamaño del gráfico</li>
  <li>Definir el espaciamiento entre los histogramas</li>
  <li>Seleccionar una columna</li>
  <li>Calcular el histograma y otorgarle el formato correcto</li>
  <li>Repetir el procedimiento para la columna siguiente</li>
</ol>


```python
#-------------------------------------------------------HISTOGRAMAS-------------------------------------------------------------
#Se establecen las dimensiones del mosaico de histogramas
#Los valores pueden ser mayores al contenido de histogramas pero no menor
dimy=6
dimx=5
temp=0
#Se crea una clase que permite detener la graficación de histogramas, en caso de haber terminado con las columnas de datos
class BreakAllTheLoops(BaseException): pass
#Se crea la variable del mosaico ("gridspec")
AX = gridspec.GridSpec(dimy,dimx)
#Se definen los tamaños de cada figura 
plt.figure(figsize=(30,30))
#Se definen las separaciones entre cada figura 
plt.subplots_adjust(top = 1, right=1)
#Se define el manejo la excepción (try/catch)
try:
    #Se definen dos bucles para cambiar la posicón en el mosaico
    for i in range(dimy):
        for j in range(dimx):  
            #Se usa la excepción para terminar el ciclo en caso de haber terminado de graficar todos los elementos
            if (temp==len(datos.columns)-2):
                raise BreakAllTheLoops()  
            #Se grafica el histograma en la posición del mosaico indicada
            plt.subplot(AX[i,j])
            plt.hist(datos[datos.columns[temp+2]], facecolor='#4472C4',alpha=0.6,label=datos.columns[temp+2], bins=20)
            plt.xlabel('Conc ['+agregar[temp+2]+']'); plt.ylabel('Frecuencia [No. de datos]'); 
            plt.title('Histograma de '+datos.columns[temp+2])
            plt.legend(loc='upper center',prop={'size': 20})
            plt.grid()
            #Se aumenta el contador que controla el número de histogramas a graficar
            temp+=1;
except BreakAllTheLoops:
    pass
#Se guarda una imagen del mosaico de histogramas
plt.savefig('Histogramas.png')
#Se muestra el mosaico
#Nota: En caso de querer visualizar mejor, dar doble click en la imagen
#Por si acaso el programa crea una imagen del mosaico que se guarda en la carpeta donde es ejecutado el programa
plt.show()
```


![png](outputImages/output_22_0.png)


<p style="text-align: center;font-size: 10px;font-style: oblique;">Figura 2. Mosaico de histogramas de elementos contenidos en el set de datos</p>

<p>Dentro del mosaico anterior, podemos definir dos tipos de comportamientos característicos:</p>
<ol>
    <li>Distribución mayoritaria de frecuencias en un valor específico del rango.</li>
    <li>Distribución de frecuencias a lo largo del rango.</li>
</ol>   

<p>En el primer comportamiento, como su nombre lo indica, existe una valor de concentración principal que se tiende a repetir de manera destacable. Existen valores en otras frecuencias dentro del rango, pero presentan frecuencias menores en comparación con la frecuencia principal.Dentro de este comportamiento se encuentran la mayoría de los elementos, los cuales son: Au, Ag, As, B, Ba, Bi, Ca, Cd, Cr, Cu,Ga, Mo, Na, Pb, S y Ti.
</p>
<p>En el segundo comportamiento, la distribución de frecuencias no se encuentra tan concentrada, sino que los valores tienen una mayor variabilidad. Los elementos que presentan este comportamiento son: Al, Co, Fe, K , Mg, Mn, Ni, P, Sc, Sr, Vy Zn.
</p>
<p>Sin embargo, existen elementos que a pesar de presentar un comportamiento en el que la distribución de frecuencias está dado a lo largo del rango, aún existe la presencia de un valor principal, por lo que respresentan un comportamiento intermedio. Como es el caso del Al, Mn y Ni.
</p>
<p>Por otra parte, algunos elementos presentan su distribución de frecuencias dentro de un rango muy pequeño, tales como: Bi, Pb y Na. Mientras que otros, como B y Ga, contienen el total de datos en una misma clase.
</p>

<p>Debido a la distribución observada en los datos, distinta a la distribución gaussiana y focalizada en un solo punto, se optó por eliminar las siguientes columnas de datos:
Au, Ag, As, B, Ba, Bi, Cd, Pb, S, Ti, Na
</p>


```python
#['Au','Ag', 'As', 'B', 'Ba', 'Bi', 'Cd', 'Pb', 'S', 'Ti', 'Na']
#['Au','Ag', 'As', 'B', 'Ba', 'Bi', 'Cd', 'Pb']
```


```python
#Eliminación de columnas con distribución
datos.drop(['Au','Ag', 'As', 'B', 'Ba', 'Bi', 'Cd', 'Pb','Mo', 'Na', 'Ti'], axis=1, inplace=True)
#Se eliminan los tipos de concentración de las columnas a eliminar
agregar.drop(labels=['Au','Ag', 'As', 'B', 'Ba', 'Bi', 'Cd', 'Pb', 'Mo', 'Na', 'Ti'], inplace=True)
```


```python
#Dataframe después de la limpieza completa de los datos
datos.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>norte</th>
      <th>este</th>
      <th>Recvd Wt.</th>
      <th>Al</th>
      <th>Ca</th>
      <th>Co</th>
      <th>Cr</th>
      <th>Cu</th>
      <th>Fe</th>
      <th>Ga</th>
      <th>K</th>
      <th>Mg</th>
      <th>Mn</th>
      <th>Ni</th>
      <th>P</th>
      <th>S</th>
      <th>Sc</th>
      <th>Sr</th>
      <th>V</th>
      <th>Zn</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>4627001.0</td>
      <td>549050.0</td>
      <td>2.78</td>
      <td>3.71</td>
      <td>0.09</td>
      <td>13.0</td>
      <td>26.0</td>
      <td>6040.0</td>
      <td>10.20</td>
      <td>10.0</td>
      <td>0.07</td>
      <td>1.65</td>
      <td>1700.0</td>
      <td>8.0</td>
      <td>220.0</td>
      <td>0.03</td>
      <td>12.0</td>
      <td>11.0</td>
      <td>155.0</td>
      <td>139.0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>4627049.0</td>
      <td>549003.0</td>
      <td>3.32</td>
      <td>5.13</td>
      <td>0.02</td>
      <td>32.0</td>
      <td>77.0</td>
      <td>2180.0</td>
      <td>11.35</td>
      <td>10.0</td>
      <td>0.01</td>
      <td>3.42</td>
      <td>3650.0</td>
      <td>20.0</td>
      <td>160.0</td>
      <td>0.01</td>
      <td>28.0</td>
      <td>5.0</td>
      <td>240.0</td>
      <td>280.0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>4627073.0</td>
      <td>548963.0</td>
      <td>2.76</td>
      <td>1.39</td>
      <td>0.61</td>
      <td>7.0</td>
      <td>21.0</td>
      <td>7480.0</td>
      <td>6.62</td>
      <td>10.0</td>
      <td>0.12</td>
      <td>0.33</td>
      <td>1475.0</td>
      <td>2.0</td>
      <td>230.0</td>
      <td>0.07</td>
      <td>8.0</td>
      <td>67.0</td>
      <td>105.0</td>
      <td>44.0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>4627050.0</td>
      <td>548964.0</td>
      <td>3.20</td>
      <td>1.74</td>
      <td>0.14</td>
      <td>2.0</td>
      <td>60.0</td>
      <td>1030.0</td>
      <td>8.41</td>
      <td>10.0</td>
      <td>0.14</td>
      <td>0.40</td>
      <td>148.0</td>
      <td>5.0</td>
      <td>70.0</td>
      <td>0.07</td>
      <td>11.0</td>
      <td>16.0</td>
      <td>165.0</td>
      <td>48.0</td>
    </tr>
    <tr>
      <td>4</td>
      <td>4627090.0</td>
      <td>548870.0</td>
      <td>3.42</td>
      <td>0.23</td>
      <td>2.41</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>4530.0</td>
      <td>3.62</td>
      <td>10.0</td>
      <td>0.06</td>
      <td>1.16</td>
      <td>286.0</td>
      <td>2.0</td>
      <td>360.0</td>
      <td>0.04</td>
      <td>12.0</td>
      <td>46.0</td>
      <td>26.0</td>
      <td>58.0</td>
    </tr>
  </tbody>
</table>
</div>



# Valores más altos (5%) <a name='valores'/>

<p>Posteriormente, se hizo el cálculo del 5% más alto de los datos por elemento. Los valores obtenidos se  guardaron dentro de un mismo set de datos. Luego, se graficaron, junto con el mapa de coordenadas de todos lo puntos muestreados para obtener la distribución espacial de los datos con mayor concentración de los elementos.</p>
<p>El proceso fue el siguiente:
</p>
<ol>
    <li>Definir el porcentaje de valores altos deseado (5%).</li>
    <li>Calcular el número de datos correspondiente al porcentaje especificado. Debido a que todas las columnas de elementos tiene la misma longitud, es un valor contante en todas los elementos.</li>
    <li>Se selecciona una columna de elementos.</li>
    <li>Se ordena el número de elementos de mayor a menor.</li>
    <li>Selecciona el número de datos calculado en la lista de valores ordenados.</li>
    <li>Se almacenanan los valores de un set de datos.</li>
    <li>Definir el porcentaje de valores altos deseado (5%).</li>
    <li>Se repite para la columna siguiente.</li>
    <li>Una vez terminadas las columnas, se define un nuevo mosaico.</li>
    <li>Se definene las características del mosaico.</li>
    <li>Se selecciona una columna.</li>
    <li>Se grafican los valores más altos en el mapa de coordenadas, junto con todos los datos muestreados.</li>
    <li>Se repite el proceso hasta terminar con todas las columnas de elementos.</li>
    <li>Finalmente, se calcula el número de datos contenidos en cada división del grid para obtener las ubicaciones de mayor concentración</li>
</ol>   





```python
#-----------------------------------------------------Cuantil 95%---------------------------------------------------------------
#Se define el porcentaje de datos que se desean dentro del rango del cuantil (5%)
porc=5
#Se calcula el número de datos
vm=m.ceil((porc*len(datos[datos.columns[0]])/100))
#Se crea un nuevo dataframa donde se almacenarán los valores más altos
df = pd.DataFrame(columns=['norte', 'este', 'Valor', 'elemento'])
#Se utiliza un ciclo para hacer el mismo proceso en todas las columnas menos en dos (norte, este)
for i in range (len(datos.columns)-2):
    #Se ordenan los valores de mayor a menor, se selecciona el número de datos a utilizar
    va=(datos.sort_values([datos.columns[i+2]], ascending=False)[['norte','este',datos.columns[i+2]]].head(vm)).copy()
    va.rename(columns={va.columns[2]:'Valor'},inplace = True)
    #Se define el elemento al que pertenece la selección de datos realizada
    va['elemento']= datos.columns[i+2] 
    #Se guardan los valores en el dataframe creado anteriormente
    df=df.append(va)
#Se reinician los índices
df.reset_index(drop=True, inplace=True)
#Se muestra el dataframe de los datos datos correspondientes al 5%
df.head(15)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>norte</th>
      <th>este</th>
      <th>Valor</th>
      <th>elemento</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>4627016.0</td>
      <td>549188.0</td>
      <td>5.76</td>
      <td>Recvd Wt.</td>
    </tr>
    <tr>
      <td>1</td>
      <td>4626861.0</td>
      <td>548363.0</td>
      <td>5.40</td>
      <td>Recvd Wt.</td>
    </tr>
    <tr>
      <td>2</td>
      <td>4626802.0</td>
      <td>548370.0</td>
      <td>5.36</td>
      <td>Recvd Wt.</td>
    </tr>
    <tr>
      <td>3</td>
      <td>4626991.0</td>
      <td>549010.0</td>
      <td>5.18</td>
      <td>Recvd Wt.</td>
    </tr>
    <tr>
      <td>4</td>
      <td>4627057.0</td>
      <td>549233.0</td>
      <td>5.08</td>
      <td>Recvd Wt.</td>
    </tr>
    <tr>
      <td>5</td>
      <td>4626765.0</td>
      <td>548957.0</td>
      <td>6.26</td>
      <td>Al</td>
    </tr>
    <tr>
      <td>6</td>
      <td>4626963.0</td>
      <td>548960.0</td>
      <td>5.23</td>
      <td>Al</td>
    </tr>
    <tr>
      <td>7</td>
      <td>4626706.0</td>
      <td>548740.0</td>
      <td>5.21</td>
      <td>Al</td>
    </tr>
    <tr>
      <td>8</td>
      <td>4627049.0</td>
      <td>549003.0</td>
      <td>5.13</td>
      <td>Al</td>
    </tr>
    <tr>
      <td>9</td>
      <td>4627090.0</td>
      <td>549009.0</td>
      <td>4.93</td>
      <td>Al</td>
    </tr>
    <tr>
      <td>10</td>
      <td>4626873.0</td>
      <td>548555.0</td>
      <td>6.47</td>
      <td>Ca</td>
    </tr>
    <tr>
      <td>11</td>
      <td>4626653.0</td>
      <td>548248.0</td>
      <td>4.36</td>
      <td>Ca</td>
    </tr>
    <tr>
      <td>12</td>
      <td>4626694.0</td>
      <td>548603.0</td>
      <td>3.21</td>
      <td>Ca</td>
    </tr>
    <tr>
      <td>13</td>
      <td>4627064.0</td>
      <td>549093.0</td>
      <td>2.54</td>
      <td>Ca</td>
    </tr>
    <tr>
      <td>14</td>
      <td>4627090.0</td>
      <td>548870.0</td>
      <td>2.41</td>
      <td>Ca</td>
    </tr>
  </tbody>
</table>
</div>



<p style="text-align: center;font-size: 10px;font-style: oblique;">Tabla 4. Datos más altos seleccionados</p>


```python
#-----------------------------------------------------MOSAICO DE COORDENADAS----------------------------------------------------
temp=0
#Se crea una nueva variable para el mosaico
VA = gridspec.GridSpec(dimy,dimx)
#Se define el tamaño de la figura
plt.figure(figsize=(12,12))
#Se define el espaciamiento entre mapas de coordenadas
plt.subplots_adjust(top = 1.6, right=1.9)
#Se definen los ciclos de graficación para el mosaico
try:
    for i in range(dimy):
        for j in range(dimx):            
            if (temp==len(df)):
                raise BreakAllTheLoops()            
            plt.subplot(VA[i,j])
            plt.title(df.elemento[temp], fontsize=20)
            plt.scatter(datos.este,datos.norte, color='black', s=20, label='Datos muestreados') 
            plt.scatter(df.este[temp:(temp+5)],df.norte[temp:(temp+5)], color='red', s=80,  label='Valores altos') 
            #plt.xlabel('[mE]') Se eliminaron por cuestiones de visualización
            #plt.ylabel('[mN]')
            plt.grid()
            plt.legend()
            temp+=5;
except BreakAllTheLoops:
    pass
#Se guarda el mosaico creado en una imagen
plt.savefig('Mosaico.png')
#Se muestra el mosaico
plt.show()
```


![png](outputImages/output_33_0.png)


<p style="text-align: center;font-size: 10px;font-style: oblique;">Figura 3. Mosaico de coordenadas de muestreo de valores regulares (en color negro) y valores más altos (en color rojo)</p>


```python
#Ubicación de zonas de mayor concentración de valores altos
i=0
j=0
temp2=[]
#Se crea un ciclo que varía la posición del box de coordenadas que cuenta la posición
for j in range(6):
    #Variación de las coordenadas en metros norte
    cy1=4626600+(j*100)
    cy2=4626700+(j*100)
    for i in range (6):
        #Variación de las coordenadas en metros este
        cx1=548200+(i*200)
        cx2=548400+(i*200)
        #Se calcula el número de valores altos dentro del box definido
        temp=len(df[(df['este']>cx1)&(df['este']<cx2)&(df['norte']>cy1)&(df['norte']<cy2)])
        #Se imprimen los resultados obtenidos
        print ('Datos contenidos entre: ',cx1,'-', cx2,'[mE] y', cy1,'-', cy2,'[mN]', 'son:',temp)
        #Se guardan los valores más altos en una variable
        if (temp>8):
            temp2.append((cx1+cx2)/2)
            temp2.append((cy1+cy2)/2)
    print ('\n')
```

    Datos contenidos entre:  548200 - 548400 [mE] y 4626600 - 4626700 [mN] son: 2
    Datos contenidos entre:  548400 - 548600 [mE] y 4626600 - 4626700 [mN] son: 6
    Datos contenidos entre:  548600 - 548800 [mE] y 4626600 - 4626700 [mN] son: 10
    Datos contenidos entre:  548800 - 549000 [mE] y 4626600 - 4626700 [mN] son: 0
    Datos contenidos entre:  549000 - 549200 [mE] y 4626600 - 4626700 [mN] son: 0
    Datos contenidos entre:  549200 - 549400 [mE] y 4626600 - 4626700 [mN] son: 0
    
    
    Datos contenidos entre:  548200 - 548400 [mE] y 4626700 - 4626800 [mN] son: 6
    Datos contenidos entre:  548400 - 548600 [mE] y 4626700 - 4626800 [mN] son: 3
    Datos contenidos entre:  548600 - 548800 [mE] y 4626700 - 4626800 [mN] son: 2
    Datos contenidos entre:  548800 - 549000 [mE] y 4626700 - 4626800 [mN] son: 8
    Datos contenidos entre:  549000 - 549200 [mE] y 4626700 - 4626800 [mN] son: 4
    Datos contenidos entre:  549200 - 549400 [mE] y 4626700 - 4626800 [mN] son: 0
    
    
    Datos contenidos entre:  548200 - 548400 [mE] y 4626800 - 4626900 [mN] son: 5
    Datos contenidos entre:  548400 - 548600 [mE] y 4626800 - 4626900 [mN] son: 4
    Datos contenidos entre:  548600 - 548800 [mE] y 4626800 - 4626900 [mN] son: 0
    Datos contenidos entre:  548800 - 549000 [mE] y 4626800 - 4626900 [mN] son: 1
    Datos contenidos entre:  549000 - 549200 [mE] y 4626800 - 4626900 [mN] son: 2
    Datos contenidos entre:  549200 - 549400 [mE] y 4626800 - 4626900 [mN] son: 0
    
    
    Datos contenidos entre:  548200 - 548400 [mE] y 4626900 - 4627000 [mN] son: 0
    Datos contenidos entre:  548400 - 548600 [mE] y 4626900 - 4627000 [mN] son: 0
    Datos contenidos entre:  548600 - 548800 [mE] y 4626900 - 4627000 [mN] son: 0
    Datos contenidos entre:  548800 - 549000 [mE] y 4626900 - 4627000 [mN] son: 5
    Datos contenidos entre:  549000 - 549200 [mE] y 4626900 - 4627000 [mN] son: 7
    Datos contenidos entre:  549200 - 549400 [mE] y 4626900 - 4627000 [mN] son: 1
    
    
    Datos contenidos entre:  548200 - 548400 [mE] y 4627000 - 4627100 [mN] son: 0
    Datos contenidos entre:  548400 - 548600 [mE] y 4627000 - 4627100 [mN] son: 0
    Datos contenidos entre:  548600 - 548800 [mE] y 4627000 - 4627100 [mN] son: 0
    Datos contenidos entre:  548800 - 549000 [mE] y 4627000 - 4627100 [mN] son: 7
    Datos contenidos entre:  549000 - 549200 [mE] y 4627000 - 4627100 [mN] son: 15
    Datos contenidos entre:  549200 - 549400 [mE] y 4627000 - 4627100 [mN] son: 2
    
    
    Datos contenidos entre:  548200 - 548400 [mE] y 4627100 - 4627200 [mN] son: 0
    Datos contenidos entre:  548400 - 548600 [mE] y 4627100 - 4627200 [mN] son: 0
    Datos contenidos entre:  548600 - 548800 [mE] y 4627100 - 4627200 [mN] son: 0
    Datos contenidos entre:  548800 - 549000 [mE] y 4627100 - 4627200 [mN] son: 0
    Datos contenidos entre:  549000 - 549200 [mE] y 4627100 - 4627200 [mN] son: 0
    Datos contenidos entre:  549200 - 549400 [mE] y 4627100 - 4627200 [mN] son: 0
    
    
    


```python
#-----------------------------------------------VISUALIZACIÓN DE LOS DATOS-----------------------------------------------------
#Se realiza una visualización rápida de las zonas con valores más altos y su ubicación
temp2=(np.array(temp2))
plt.figure(figsize=(5,5)) 
plt.scatter(datos.este,datos.norte, color='black', s=20, label='Datos muestreados') 
plt.scatter(temp2[0::2],temp2[1::2], s=200)
plt.title('Figura 4. Mapa de ubicación de zonas de mayor concentración de valores altos', fontsize=15)
plt.xlabel('[mE]')
plt.ylabel('[mN]')
plt.grid()
plt.show()
```


![png](outputImages/output_36_0.png)


<p style="text-align: center;font-size: 10px;font-style: oblique;">Figura 4. Mapa de coordenadas de zonas de mayor concentración de datos con los valores más altos (en color azul) y valore datos muestreados (en color negro)</p>

<p>Analizando la distribución de los datos muestreados, podemos observar que los datos obtenidos no obedecen un patrón de muestreo definido. No obstante, se observan dos agrupaciones principales de datos ubicadas al noreste y al suroeste respectivamente, de aproximadamente el mismo número de muestreos en cada uno.</p>
<p>En cuanto a las posiciones de los valores con concentraciones más altas notamos que existe una gran distribución de ellos a lo largo de la zona de estudio. A pesar de ello, podemos resaltar tres zonas de ocurrencia máxima de concentración de elementos:
</p>
<ol>
    <li>Zona 1:Datos contenidos entre:  548600 - 548800 [mE] y 4626600 - 4626700 [mN] son: 16</li>
    <li>Zona 2: Datos contenidos entre:  548200 - 548400 [mE] y 4626700 - 4626800 [mN] son: 15</li>
    <li>Zona 3: Datos contenidos entre:  549000 - 549200 [mE] y 4627000 - 4627100 [mN] son: 20</li>
</ol> 
<p>Debido a lo mostrado anteriormente, podemos determinar que la zona de mayor concentración general de elementos se ubica entre las coordenadas 549000 - 549200 [mE] y 4627000 - 4627100 [mN]
</p>


# Boxplots <a name='Boxplots'/>

<p>Se realizó el cálculo de los boxplots de la información dada para poder obtener una visualización rápida 
de la información en cada uno de los elementos, así como también para fines comparativos.</p>
<p>Para la respresentación de los boxplots, se resulta necesario dividir los elementos en los dos grupos de concentración 
principales(% o ppm). Sin embargo, debido a que existen valores de ppb se optó por crear un tercer grupo. Es posible hacer la
conversión de ppb a ppm, pero esto implicaría dividir los valores entre 1000, lo cual generaría una pérdida en la visualización
de la información contenida en el boxplot.
</p>
<p>A pesar de las divisiones realizadas, los rangos de oscilación de las concentraciones de los elementos es amplio, por lo que al graficar los diferentes elementos no es posible visualizar la información contenida en el boxplot con precisión. Debido a esto, se realizaron subdivisiones, estableciendo rangos entre las divisiones previas con base en los valores máximos de cada elemento. 
Los rangos en los que se graficaron los boxplots fueron los siguientes:
</p>
<p>-Concentraciones en porcentaje:
</p>
<ol>
    <li>Concentraciones menores al 5%</li>
    <li>Concentraciones mayores al 5%</li>
</ol> 
<p>-Concentraciones en partes por millón:
</p>
<ol>
    <li>Concentraciones máximas menores a 20 ppm</li>
    <li>Concentraciones máximas menores a 100 ppm y mayores a 20 ppm</li>
    <li>Concentraciones máximas menores a 1000 ppm y mayores a 100 ppm</li>
    <li>Concentraciones máximas mayores a 1000 ppm</li>
</ol>
<p>-Concentraciones en partes por billón</p>
<p>Debido a los problemas de visualización que producían los outliers, también fueron removidos</p>



```python
#Se separan aquellos datos cuya concentración está dada en porcentaje 
cporc=np.where(agregar=='%')
#Se almacenan los datos en un dataframe de porcentaje
dporc=datos[datos.columns[cporc]]
dporc.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Al</th>
      <th>Ca</th>
      <th>Fe</th>
      <th>K</th>
      <th>Mg</th>
      <th>S</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>3.71</td>
      <td>0.09</td>
      <td>10.20</td>
      <td>0.07</td>
      <td>1.65</td>
      <td>0.03</td>
    </tr>
    <tr>
      <td>1</td>
      <td>5.13</td>
      <td>0.02</td>
      <td>11.35</td>
      <td>0.01</td>
      <td>3.42</td>
      <td>0.01</td>
    </tr>
    <tr>
      <td>2</td>
      <td>1.39</td>
      <td>0.61</td>
      <td>6.62</td>
      <td>0.12</td>
      <td>0.33</td>
      <td>0.07</td>
    </tr>
    <tr>
      <td>3</td>
      <td>1.74</td>
      <td>0.14</td>
      <td>8.41</td>
      <td>0.14</td>
      <td>0.40</td>
      <td>0.07</td>
    </tr>
    <tr>
      <td>4</td>
      <td>0.23</td>
      <td>2.41</td>
      <td>3.62</td>
      <td>0.06</td>
      <td>1.16</td>
      <td>0.04</td>
    </tr>
  </tbody>
</table>
</div>



<p style="text-align: center;font-size: 10px;font-style: oblique;">Tabla 5. Set de datos de concentración en porcentaje</p>


```python
#Se separan aquellos datos cuya concentración está dada en ppm
cppm=np.where((agregar=='ppm'))
#Se almacenan los datos en un dataframe de ppm
dppm=datos[datos.columns[cppm]]
dppm.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Co</th>
      <th>Cr</th>
      <th>Cu</th>
      <th>Ga</th>
      <th>Mn</th>
      <th>Ni</th>
      <th>P</th>
      <th>Sc</th>
      <th>Sr</th>
      <th>V</th>
      <th>Zn</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>13.0</td>
      <td>26.0</td>
      <td>6040.0</td>
      <td>10.0</td>
      <td>1700.0</td>
      <td>8.0</td>
      <td>220.0</td>
      <td>12.0</td>
      <td>11.0</td>
      <td>155.0</td>
      <td>139.0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>32.0</td>
      <td>77.0</td>
      <td>2180.0</td>
      <td>10.0</td>
      <td>3650.0</td>
      <td>20.0</td>
      <td>160.0</td>
      <td>28.0</td>
      <td>5.0</td>
      <td>240.0</td>
      <td>280.0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>7.0</td>
      <td>21.0</td>
      <td>7480.0</td>
      <td>10.0</td>
      <td>1475.0</td>
      <td>2.0</td>
      <td>230.0</td>
      <td>8.0</td>
      <td>67.0</td>
      <td>105.0</td>
      <td>44.0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>2.0</td>
      <td>60.0</td>
      <td>1030.0</td>
      <td>10.0</td>
      <td>148.0</td>
      <td>5.0</td>
      <td>70.0</td>
      <td>11.0</td>
      <td>16.0</td>
      <td>165.0</td>
      <td>48.0</td>
    </tr>
    <tr>
      <td>4</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>4530.0</td>
      <td>10.0</td>
      <td>286.0</td>
      <td>2.0</td>
      <td>360.0</td>
      <td>12.0</td>
      <td>46.0</td>
      <td>26.0</td>
      <td>58.0</td>
    </tr>
  </tbody>
</table>
</div>



<p style="text-align: center;font-size: 10px;font-style: oblique;">Tabla 6. Set de datos de concentración en partes por millón</p>


```python
#Se separan aquellos datos cuya concentración está dada en ppm
cppb=np.where((agregar=='ppb'))
#Se almacenan los datos en un dataframe de ppm
dppb=datos[datos.columns[cppb]]
dppb.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
    </tr>
    <tr>
      <td>1</td>
    </tr>
    <tr>
      <td>2</td>
    </tr>
    <tr>
      <td>3</td>
    </tr>
    <tr>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>



<p style="text-align: center;font-size: 10px;font-style: oblique;">Tabla 7. Set de datos de concentración en partes por billón</p>


```python
plt.subplot(1, 3, 1)
plt.subplots_adjust(right=2.3)

dporc.boxplot()
plt.title('Concentración en porcentaje')
plt.ylabel('[%]')
plt.xlabel('Elemento')

plt.subplot(1, 3, 2)
dppm.boxplot()
plt.title('Concentración en ppm')
plt.ylabel('[ppm]')
plt.xlabel('Elemento')

plt.subplot(1, 3, 3)
dppb.boxplot()
plt.title('Concentración en ppb')
plt.ylabel('[ppb]')
plt.xlabel('Elemento')

plt.show()
```


![png](outputImages/output_47_0.png)


<p style="text-align: center;font-size: 10px;font-style: oblique;">Figura 5. Boxplots de las divisiones de datos principales: porcentaje(izquierda), ppm(centro) y ppb(derecha)</p>

<h4>Boxplot de elementos en porcentaje</h4>


```python
dporc.boxplot(figsize=[20,10])
plt.xlabel('Elemento')
plt.ylabel('Concentración [%]')
```




    Text(0, 0.5, 'Concentración [%]')




![png](outputImages/output_50_1.png)


<p style="text-align: center;font-size: 10px;font-style: oblique;">Figura 6. Set de datos de concentraciones en porcentaje</p>


```python
dporc[dporc.columns[np.where((dporc.max()<5)==True)]].boxplot(figsize=[15,5], showfliers=False)
plt.xlabel('Elemento')
plt.ylabel('Concentración[%]')
```




    Text(0, 0.5, 'Concentración[%]')




![png](outputImages/output_52_1.png)


<p style="text-align: center;font-size: 10px;font-style: oblique;">Figura 7. Set de datos de concentración en porcentaje menores al 5%</p>


```python
dporc[dporc.columns[np.where((dporc.max()>5)==True)]].boxplot(figsize=[15,5], showfliers=False)
plt.xlabel('Elemento')
plt.ylabel('Concentración [%]')
```




    Text(0, 0.5, 'Concentración [%]')




![png](outputImages/output_54_1.png)


<p style="text-align: center;font-size: 10px;font-style: oblique;">Figura 8. Set de datos de concentración en porcentaje mayores al 5%</p>

<p>De forma general, en el caso de las concentración dadas en porcentaje, podemos observar que los valores oscilan en un rango de 0.01 - 14.1%.
Sin embargo, la mayoría de los elementos oscilan en valores bajos de porcentaje y con rangos de variación muy pequeños, como es el caso del K, Na, S y Ti. Elementos como el Al y Mg tienen rangos de variación intermedios, con valores ligeramente más altos en porcentaje. En el caso del Fe se presentan los porcentajes más altos, pero también el rango de variación más grande.
</p>
<p>
El valor de las medianas se mantiene en la mayoría de los casos menor al 2%, únicamente sobrepasado por el Al(2.03%) y el Fe(6.76%). La presencia de outliers se observa principalmente en el Ca y S. Siendo este último el que presenta mayor cantidad de outliers. Pero, analizando de forma general, la presencia de outliers es escasa.
</p>

<h4>Boxplot de elementos en partes por millón</h4>


```python
dppm.boxplot(figsize=[20,10])
plt.xlabel('Elemento')
plt.ylabel('Concentración [ppm]')
```




    Text(0, 0.5, 'Concentración [ppm]')




![png](outputImages/output_58_1.png)


<p style="text-align: center;font-size: 10px;font-style: oblique;">Figura 9. Set de datos de concentración en partes por millón</p>


```python
dppm[dppm.columns[np.where((dppm.max()<=20)==True)]].boxplot(figsize=[15,5], showfliers=False)
plt.xlabel('Elemento')
plt.ylabel('Concentración [ppm]')
```




    Text(0, 0.5, 'Concentración [ppm]')




![png](outputImages/output_60_1.png)


<p style="text-align: center;font-size: 10px;font-style: oblique;">Figura 10. Set de datos de leyes en partes por millón menores a 10 ppm</p>


```python
dppm[dppm.columns[np.where(((dppm.max()<=100)&(dppm.max()>20))==True)]].boxplot(figsize=[15,5], showfliers=False)
plt.xlabel('Elemento')
plt.ylabel('Concentración [ppm]')
```




    Text(0, 0.5, 'Concentración [ppm]')




![png](outputImages/output_62_1.png)


<p style="text-align: center;font-size: 10px;font-style: oblique;">Figura 11. Set de datos de concentración en partes por millón mayores a 10 ppm y menores a 100 ppm</p>


```python
dppm[dppm.columns[np.where(((dppm.max()<=1000)&(dppm.max()>100))==True)]].boxplot(figsize=[15,5], showfliers=False)
plt.xlabel('Elemento')
plt.ylabel('Concentración [ppm]')
```




    Text(0, 0.5, 'Concentración [ppm]')




![png](outputImages/output_64_1.png)


<p style="text-align: center;font-size: 10px;font-style: oblique;">Figura 12. Set de datos de leyes en partes por millón mayores a 100 ppm y menores a 1000 ppm</p>


```python
dppm[dppm.columns[np.where(((dppm.max()>1000))==True)]].boxplot(figsize=[15,5], showfliers=False)
plt.xlabel('Elemento')
plt.ylabel('Concentración [ppm]')
```




    Text(0, 0.5, 'Concentración [ppm]')




![png](outputImages/output_66_1.png)


<p style="text-align: center;font-size: 10px;font-style: oblique;">Figura 13. Set de datos de concentración en partes por millón mayores a 1000 ppm </p>

<p>En este caso, es difícil establecer características generales de los datos, debido a que existe demasiada variabilidad en todas
las caracterítisticas observadas. Esto es provocado porque en este caso existen elementos con un rango de variación de hasta 6000 ppm,
mientras que existen otros con el mismo valor en todos los datos. Dentro de lo que se puede observar, los elementos que presentan mayor 
rango de variación son el Cu y Mn, los que presentan menor rango de variación son el B, Cd y Ga. Estos mismos elementos son los que presentan
menor cantidad de outliers. Por otra parte, elementos como Cu, Zn, As, y Ba presentan una gran cantidad de outliers.
</p>
<p>
Analizando los valores máximos menores o iguales a 20 ppm, se observan valores constantes, con poco o nulo rango de variación y con medianas cercanas a 0 o a 10. Los elementos contenidos en este rango son: Ag, B,Bi, Cd, Ga y Mo.
Por otro lado, en los elementos con maximos entre 20 - 100 ppm, se presentan rangos de variación mayores, con variaciones entre 
30 - 50 ppm. Las medianas se mantienen constantes, entre aproximadamente 5 - 15 ppm. En este rango se encuentran: Co, Ni, Sc y Sr.
El siguiente rango(100 - 1000 ppm), elementos como As, Cr y Pb presentan rangos de variación y medianas similares a las del rango anterior. Mientras que los demás elementos del rango (P y V), tienen un rango de variación mayores, de 600 y 300 ppm aproximadamente, así como medianas cercanas a los 200 ppm.
Finalmente, en el rango de valores mayores a 1000 ppm se tiene una alta variación de valores, principalmente en elementos como Cu y Mn
</p>

<h4>Boxplot de elementos en partes por billón</h4>


```python
dppb.boxplot(showfliers=False)
plt.xlabel('Elemento')
plt.ylabel('Concentración [ppb]')
```




    Text(0, 0.5, 'Concentración [ppb]')




![png](outputImages/output_70_1.png)


<p style="text-align: center;font-size: 10px;font-style: oblique;">Figura 13. Set de datos de concentración en partes por billón</p>

<p>Por último, las concentraciones de Au son los valores de concentración más pequeños. Estaría en un rango de 0.005 - 0.030 ppm, con una mediana de 0.0075 ppb. Los rangos de variación son muy cortos.
</p>

<h1 style="text-align: center; font-size: 35px; margin-top: 150px;">Análisis multivariable</h1><a name='AM'/>

<p>Antes de iniciar el análisis mutivariable es necesario realizar una limpieza de los datos. Aquellos elementos que no representen una distribución gaussiana tendrán que ser eliminados. Esto incluye elementos cuya concentración de valores se da en un único valor, dichos elementos corresponden a una desviación estándar de 0.
</p>



```python
#Se eliminan los valores con desviación estándar = 0
datos.drop(datos.columns[np.where(datos.std()==0)[0]], axis=1, inplace=True)
```


```python
#Se muestra el set de datos limpios
datos.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>norte</th>
      <th>este</th>
      <th>Recvd Wt.</th>
      <th>Al</th>
      <th>Ca</th>
      <th>Co</th>
      <th>Cr</th>
      <th>Cu</th>
      <th>Fe</th>
      <th>K</th>
      <th>Mg</th>
      <th>Mn</th>
      <th>Ni</th>
      <th>P</th>
      <th>S</th>
      <th>Sc</th>
      <th>Sr</th>
      <th>V</th>
      <th>Zn</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>4627001.0</td>
      <td>549050.0</td>
      <td>2.78</td>
      <td>3.71</td>
      <td>0.09</td>
      <td>13.0</td>
      <td>26.0</td>
      <td>6040.0</td>
      <td>10.20</td>
      <td>0.07</td>
      <td>1.65</td>
      <td>1700.0</td>
      <td>8.0</td>
      <td>220.0</td>
      <td>0.03</td>
      <td>12.0</td>
      <td>11.0</td>
      <td>155.0</td>
      <td>139.0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>4627049.0</td>
      <td>549003.0</td>
      <td>3.32</td>
      <td>5.13</td>
      <td>0.02</td>
      <td>32.0</td>
      <td>77.0</td>
      <td>2180.0</td>
      <td>11.35</td>
      <td>0.01</td>
      <td>3.42</td>
      <td>3650.0</td>
      <td>20.0</td>
      <td>160.0</td>
      <td>0.01</td>
      <td>28.0</td>
      <td>5.0</td>
      <td>240.0</td>
      <td>280.0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>4627073.0</td>
      <td>548963.0</td>
      <td>2.76</td>
      <td>1.39</td>
      <td>0.61</td>
      <td>7.0</td>
      <td>21.0</td>
      <td>7480.0</td>
      <td>6.62</td>
      <td>0.12</td>
      <td>0.33</td>
      <td>1475.0</td>
      <td>2.0</td>
      <td>230.0</td>
      <td>0.07</td>
      <td>8.0</td>
      <td>67.0</td>
      <td>105.0</td>
      <td>44.0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>4627050.0</td>
      <td>548964.0</td>
      <td>3.20</td>
      <td>1.74</td>
      <td>0.14</td>
      <td>2.0</td>
      <td>60.0</td>
      <td>1030.0</td>
      <td>8.41</td>
      <td>0.14</td>
      <td>0.40</td>
      <td>148.0</td>
      <td>5.0</td>
      <td>70.0</td>
      <td>0.07</td>
      <td>11.0</td>
      <td>16.0</td>
      <td>165.0</td>
      <td>48.0</td>
    </tr>
    <tr>
      <td>4</td>
      <td>4627090.0</td>
      <td>548870.0</td>
      <td>3.42</td>
      <td>0.23</td>
      <td>2.41</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>4530.0</td>
      <td>3.62</td>
      <td>0.06</td>
      <td>1.16</td>
      <td>286.0</td>
      <td>2.0</td>
      <td>360.0</td>
      <td>0.04</td>
      <td>12.0</td>
      <td>46.0</td>
      <td>26.0</td>
      <td>58.0</td>
    </tr>
  </tbody>
</table>
</div>



<p>Ahora bien, una vez que se cuenta con los datos limpios, es necesario hacer una normalización de los mismos.
Esto se debe a que el rango de valores en cada elemento oscila en magnitudes diferentes, por lo que no son comparables.
</p>
<p>Cuando se realiza la normalización se obtienen valores dentro del mismo rango y por lo tanto equiparables para el proceso subsecuente.
</p>


```python
#Se normalizan los datos
datosnor = pd.DataFrame(columns=[datos.columns])
#Para versiones de Jupyter Notebook < 6.0.0
#datosnor[datos.columns]=(datos-datos.mean())/datos.std()
#Para versiones de Jupyter Noteobook > 6.0.0
datosnor=(datos-datos.mean())/datos.std()
datosnor.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>norte</th>
      <th>este</th>
      <th>Recvd Wt.</th>
      <th>Al</th>
      <th>Ca</th>
      <th>Co</th>
      <th>Cr</th>
      <th>Cu</th>
      <th>Fe</th>
      <th>K</th>
      <th>Mg</th>
      <th>Mn</th>
      <th>Ni</th>
      <th>P</th>
      <th>S</th>
      <th>Sc</th>
      <th>Sr</th>
      <th>V</th>
      <th>Zn</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>1.158407</td>
      <td>0.863976</td>
      <td>-1.085915</td>
      <td>0.785483</td>
      <td>-0.605919</td>
      <td>-0.348536</td>
      <td>-0.082882</td>
      <td>1.878843</td>
      <td>1.350309</td>
      <td>-0.671381</td>
      <td>-0.124541</td>
      <td>-0.073522</td>
      <td>-0.145141</td>
      <td>-0.561698</td>
      <td>-0.458977</td>
      <td>-0.290624</td>
      <td>-0.645802</td>
      <td>0.082472</td>
      <td>-0.329398</td>
    </tr>
    <tr>
      <td>1</td>
      <td>1.502234</td>
      <td>0.712588</td>
      <td>-0.407333</td>
      <td>1.621894</td>
      <td>-0.670386</td>
      <td>1.015834</td>
      <td>1.464494</td>
      <td>0.309306</td>
      <td>1.802410</td>
      <td>-1.554589</td>
      <td>1.025327</td>
      <td>1.132232</td>
      <td>0.961980</td>
      <td>-0.927247</td>
      <td>-0.482477</td>
      <td>1.345852</td>
      <td>-1.070580</td>
      <td>1.006639</td>
      <td>-0.022261</td>
    </tr>
    <tr>
      <td>2</td>
      <td>1.674147</td>
      <td>0.583748</td>
      <td>-1.111047</td>
      <td>-0.581049</td>
      <td>-0.127024</td>
      <td>-0.779390</td>
      <td>-0.234585</td>
      <td>2.464370</td>
      <td>-0.057100</td>
      <td>0.064625</td>
      <td>-0.982071</td>
      <td>-0.212647</td>
      <td>-0.698701</td>
      <td>-0.500773</td>
      <td>-0.411976</td>
      <td>-0.699743</td>
      <td>3.318798</td>
      <td>-0.461155</td>
      <td>-0.536334</td>
    </tr>
    <tr>
      <td>3</td>
      <td>1.509397</td>
      <td>0.586969</td>
      <td>-0.558129</td>
      <td>-0.374891</td>
      <td>-0.559872</td>
      <td>-1.138435</td>
      <td>0.948702</td>
      <td>-0.158302</td>
      <td>0.646605</td>
      <td>0.359027</td>
      <td>-0.936596</td>
      <td>-1.033178</td>
      <td>-0.421921</td>
      <td>-1.475571</td>
      <td>-0.411976</td>
      <td>-0.392904</td>
      <td>-0.291819</td>
      <td>0.191198</td>
      <td>-0.527621</td>
    </tr>
    <tr>
      <td>4</td>
      <td>1.795919</td>
      <td>0.284195</td>
      <td>-0.281670</td>
      <td>-1.264315</td>
      <td>1.530691</td>
      <td>-0.994817</td>
      <td>-0.689696</td>
      <td>1.264853</td>
      <td>-1.236493</td>
      <td>-0.818583</td>
      <td>-0.442867</td>
      <td>-0.947848</td>
      <td>-0.698701</td>
      <td>0.291251</td>
      <td>-0.447226</td>
      <td>-0.290624</td>
      <td>1.832073</td>
      <td>-1.320086</td>
      <td>-0.505838</td>
    </tr>
  </tbody>
</table>
</div>



Se confirma que los datos se encuentran normalizados, para ellos deben presentar media cero y una desviación estándar de 1


```python
#Se confirma que estén bien los datos
datosnor.describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>norte</th>
      <th>este</th>
      <th>Recvd Wt.</th>
      <th>Al</th>
      <th>Ca</th>
      <th>Co</th>
      <th>Cr</th>
      <th>Cu</th>
      <th>Fe</th>
      <th>K</th>
      <th>Mg</th>
      <th>Mn</th>
      <th>Ni</th>
      <th>P</th>
      <th>S</th>
      <th>Sc</th>
      <th>Sr</th>
      <th>V</th>
      <th>Zn</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>count</td>
      <td>8.200000e+01</td>
      <td>8.200000e+01</td>
      <td>8.200000e+01</td>
      <td>8.200000e+01</td>
      <td>8.200000e+01</td>
      <td>8.200000e+01</td>
      <td>8.200000e+01</td>
      <td>8.200000e+01</td>
      <td>8.200000e+01</td>
      <td>8.200000e+01</td>
      <td>8.200000e+01</td>
      <td>8.200000e+01</td>
      <td>8.200000e+01</td>
      <td>8.200000e+01</td>
      <td>8.200000e+01</td>
      <td>8.200000e+01</td>
      <td>8.200000e+01</td>
      <td>8.200000e+01</td>
      <td>8.200000e+01</td>
    </tr>
    <tr>
      <td>mean</td>
      <td>1.464295e-12</td>
      <td>-5.481658e-14</td>
      <td>1.137302e-16</td>
      <td>2.677398e-16</td>
      <td>-1.281157e-16</td>
      <td>-9.579058e-17</td>
      <td>-5.415722e-18</td>
      <td>9.477514e-18</td>
      <td>-1.469015e-16</td>
      <td>-5.957294e-17</td>
      <td>-4.907998e-16</td>
      <td>-4.941846e-17</td>
      <td>8.665155e-17</td>
      <td>7.311225e-17</td>
      <td>-8.665155e-17</td>
      <td>-3.757157e-17</td>
      <td>9.883693e-17</td>
      <td>4.129488e-17</td>
      <td>-5.889598e-17</td>
    </tr>
    <tr>
      <td>std</td>
      <td>1.000000e+00</td>
      <td>1.000000e+00</td>
      <td>1.000000e+00</td>
      <td>1.000000e+00</td>
      <td>1.000000e+00</td>
      <td>1.000000e+00</td>
      <td>1.000000e+00</td>
      <td>1.000000e+00</td>
      <td>1.000000e+00</td>
      <td>1.000000e+00</td>
      <td>1.000000e+00</td>
      <td>1.000000e+00</td>
      <td>1.000000e+00</td>
      <td>1.000000e+00</td>
      <td>1.000000e+00</td>
      <td>1.000000e+00</td>
      <td>1.000000e+00</td>
      <td>1.000000e+00</td>
      <td>1.000000e+00</td>
    </tr>
    <tr>
      <td>min</td>
      <td>-1.420295e+00</td>
      <td>-1.719270e+00</td>
      <td>-2.191752e+00</td>
      <td>-1.264315e+00</td>
      <td>-6.795957e-01</td>
      <td>-1.210244e+00</td>
      <td>-8.413994e-01</td>
      <td>-5.738639e-01</td>
      <td>-1.763289e+00</td>
      <td>-1.554589e+00</td>
      <td>-1.183460e+00</td>
      <td>-1.111088e+00</td>
      <td>-7.909616e-01</td>
      <td>-1.780196e+00</td>
      <td>-4.824772e-01</td>
      <td>-1.415701e+00</td>
      <td>-1.212173e+00</td>
      <td>-1.559282e+00</td>
      <td>-6.256433e-01</td>
    </tr>
    <tr>
      <td>25%</td>
      <td>-8.902288e-01</td>
      <td>-8.946929e-01</td>
      <td>-7.340575e-01</td>
      <td>-1.015453e+00</td>
      <td>-6.220361e-01</td>
      <td>-9.230080e-01</td>
      <td>-7.807180e-01</td>
      <td>-5.242568e-01</td>
      <td>-7.313197e-01</td>
      <td>-8.185826e-01</td>
      <td>-9.203544e-01</td>
      <td>-9.407367e-01</td>
      <td>-7.909616e-01</td>
      <td>-6.835475e-01</td>
      <td>-4.707269e-01</td>
      <td>-8.020228e-01</td>
      <td>-8.404917e-01</td>
      <td>-9.014933e-01</td>
      <td>-4.867782e-01</td>
    </tr>
    <tr>
      <td>50%</td>
      <td>-2.419717e-01</td>
      <td>2.487637e-01</td>
      <td>-2.062717e-01</td>
      <td>-2.099649e-01</td>
      <td>-4.493575e-01</td>
      <td>-1.690138e-01</td>
      <td>-5.531628e-01</td>
      <td>-4.217896e-01</td>
      <td>-9.588563e-05</td>
      <td>6.462494e-02</td>
      <td>-2.902004e-01</td>
      <td>-2.203762e-01</td>
      <td>-4.680512e-01</td>
      <td>-3.484606e-01</td>
      <td>-4.237259e-01</td>
      <td>-8.606465e-02</td>
      <td>-2.564212e-01</td>
      <td>2.075066e-01</td>
      <td>-2.694952e-01</td>
    </tr>
    <tr>
      <td>75%</td>
      <td>8.324875e-01</td>
      <td>8.663913e-01</td>
      <td>6.419555e-01</td>
      <td>9.047598e-01</td>
      <td>2.367522e-01</td>
      <td>9.260730e-01</td>
      <td>7.211467e-01</td>
      <td>-8.561991e-02</td>
      <td>5.896008e-01</td>
      <td>5.062287e-01</td>
      <td>8.271861e-01</td>
      <td>7.565935e-01</td>
      <td>5.006798e-01</td>
      <td>9.918869e-01</td>
      <td>-1.329069e-01</td>
      <td>9.111628e-01</td>
      <td>4.161449e-01</td>
      <td>8.816045e-01</td>
      <td>-9.850056e-02</td>
    </tr>
    <tr>
      <td>max</td>
      <td>1.867550e+00</td>
      <td>1.453419e+00</td>
      <td>2.658851e+00</td>
      <td>2.287490e+00</td>
      <td>5.269758e+00</td>
      <td>1.949351e+00</td>
      <td>3.740046e+00</td>
      <td>3.489042e+00</td>
      <td>2.883521e+00</td>
      <td>3.597455e+00</td>
      <td>2.331110e+00</td>
      <td>2.572953e+00</td>
      <td>3.822043e+00</td>
      <td>1.692523e+00</td>
      <td>4.299879e+00</td>
      <td>2.164089e+00</td>
      <td>3.318798e+00</td>
      <td>1.767717e+00</td>
      <td>4.791728e+00</td>
    </tr>
  </tbody>
</table>
</div>



# Scatter plot <a name='Scatter'/>


```python
temp=0
m=len(datos.columns)-3
b=0

#Se definen las dimensiones del grid interior
x=6
y=5
outer_grid = gridspec.GridSpec(m,1)

#se grafica el grid exterior
for i in range(m): 
    ngrid = outer_grid[i,0]   
    plt.subplots_adjust(top = 75, right=3)    
    inner_grid = gridspec.GridSpecFromSubplotSpec(x,y, ngrid, wspace=1)

    # Se grafica el grif interior
    for j in range(x):
        if (b==1):
            b=0
            break
        for k in range(y):
            if (temp==(len(datos.columns)-3)):
                temp=0
                b=1
                break
            ax = plt.subplot(inner_grid[j,k])
            
            ax.scatter(datosnor[datos.columns[i+3]],datosnor[datos.columns[temp+3]])
            plt.xlabel(datos.columns[i+3])
            plt.ylabel(datos.columns[temp+3])
            if (temp==2):
                ax.set_title(datos.columns[i+3], fontsize=30)
            temp+=1
plt.show()

```


![png](outputImages/output_82_0.png)


<p style="text-align: center;font-size: 10px;font-style: oblique;">Figura 14. Scatter plot de elementos contra elementos</p>

<p>Analizando el gráfico anterior podemos obtener una primera aproximación a la correlación existente entre los datos. Dentro de las observaciones principales podemos destacar lo siguiente:
</p>
<ol>
    <li>Los elementos que presentan mayor correlación con otros elementos son: Al, Co, Fe, Mg, Mn, Ni y Sc. Siendo los principales Al, Mn y Mg
    </li>
    <li>Los elementos que presentan menor correlación con otros elementos son: Ca, Cr, Cu, K, P, S, Sr. Siendo los principales K y P</li>
    <li>Elementos como el Cu, P y Sr no presentan ninguna correlación con otros elementos</li>
    <li>El elemento K presenta correclaciones negativas en la mayoría de los casos</li>
</ol> 

<p>Sin embargo, las conclusiones mencionadas anteriormente son únicamente las observaciones percibidas al observar la gráfica. Debido a ello, es necesario determinar valores específicos en cada caso. Esto puede realizarse por medio del cálculo de las matrices de covarianza y correlación
</p>

# Matriz de covarianza <a name='Mcov'/>

<p>Para el cálculo de la matriz de covarianza se realizó el cálculo excluyendo las tres primeras columnas correspondientes a
coordenadas este, coordenadas norte y el peso de las muestras
</p>


```python
#Se calcula la matriz de covarianza
mcov=datosnor.drop(columns=datosnor.columns[0:3], axis=1).cov()
mcov
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Al</th>
      <th>Ca</th>
      <th>Co</th>
      <th>Cr</th>
      <th>Cu</th>
      <th>Fe</th>
      <th>K</th>
      <th>Mg</th>
      <th>Mn</th>
      <th>Ni</th>
      <th>P</th>
      <th>S</th>
      <th>Sc</th>
      <th>Sr</th>
      <th>V</th>
      <th>Zn</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Al</td>
      <td>1.000000</td>
      <td>-0.009584</td>
      <td>0.700752</td>
      <td>0.688654</td>
      <td>-0.184098</td>
      <td>0.535382</td>
      <td>-0.603281</td>
      <td>0.914588</td>
      <td>0.746716</td>
      <td>0.739213</td>
      <td>-0.364677</td>
      <td>-0.170005</td>
      <td>0.814932</td>
      <td>0.071028</td>
      <td>0.777716</td>
      <td>0.387752</td>
    </tr>
    <tr>
      <td>Ca</td>
      <td>-0.009584</td>
      <td>1.000000</td>
      <td>0.408500</td>
      <td>0.032259</td>
      <td>0.000076</td>
      <td>0.054239</td>
      <td>-0.296143</td>
      <td>0.161397</td>
      <td>0.427458</td>
      <td>0.200908</td>
      <td>-0.057399</td>
      <td>-0.051459</td>
      <td>0.349757</td>
      <td>0.415623</td>
      <td>0.197733</td>
      <td>0.329626</td>
    </tr>
    <tr>
      <td>Co</td>
      <td>0.700752</td>
      <td>0.408500</td>
      <td>1.000000</td>
      <td>0.500780</td>
      <td>-0.055038</td>
      <td>0.512521</td>
      <td>-0.641043</td>
      <td>0.763399</td>
      <td>0.894268</td>
      <td>0.686629</td>
      <td>-0.365085</td>
      <td>-0.092047</td>
      <td>0.773096</td>
      <td>0.169552</td>
      <td>0.663249</td>
      <td>0.462747</td>
    </tr>
    <tr>
      <td>Cr</td>
      <td>0.688654</td>
      <td>0.032259</td>
      <td>0.500780</td>
      <td>1.000000</td>
      <td>-0.086608</td>
      <td>0.409009</td>
      <td>-0.478470</td>
      <td>0.720525</td>
      <td>0.443098</td>
      <td>0.857589</td>
      <td>-0.549283</td>
      <td>-0.176265</td>
      <td>0.703535</td>
      <td>-0.009582</td>
      <td>0.656541</td>
      <td>0.208787</td>
    </tr>
    <tr>
      <td>Cu</td>
      <td>-0.184098</td>
      <td>0.000076</td>
      <td>-0.055038</td>
      <td>-0.086608</td>
      <td>1.000000</td>
      <td>0.117543</td>
      <td>0.050792</td>
      <td>-0.189134</td>
      <td>-0.166005</td>
      <td>-0.093689</td>
      <td>-0.170905</td>
      <td>-0.034958</td>
      <td>-0.205959</td>
      <td>0.108863</td>
      <td>-0.178633</td>
      <td>-0.140585</td>
    </tr>
    <tr>
      <td>Fe</td>
      <td>0.535382</td>
      <td>0.054239</td>
      <td>0.512521</td>
      <td>0.409009</td>
      <td>0.117543</td>
      <td>1.000000</td>
      <td>-0.451225</td>
      <td>0.377934</td>
      <td>0.474967</td>
      <td>0.340826</td>
      <td>-0.363667</td>
      <td>-0.152516</td>
      <td>0.506711</td>
      <td>-0.081127</td>
      <td>0.662761</td>
      <td>0.051180</td>
    </tr>
    <tr>
      <td>K</td>
      <td>-0.603281</td>
      <td>-0.296143</td>
      <td>-0.641043</td>
      <td>-0.478470</td>
      <td>0.050792</td>
      <td>-0.451225</td>
      <td>1.000000</td>
      <td>-0.646233</td>
      <td>-0.659757</td>
      <td>-0.576156</td>
      <td>0.177471</td>
      <td>0.237180</td>
      <td>-0.710909</td>
      <td>-0.151509</td>
      <td>-0.629039</td>
      <td>-0.379125</td>
    </tr>
    <tr>
      <td>Mg</td>
      <td>0.914588</td>
      <td>0.161397</td>
      <td>0.763399</td>
      <td>0.720525</td>
      <td>-0.189134</td>
      <td>0.377934</td>
      <td>-0.646233</td>
      <td>1.000000</td>
      <td>0.787779</td>
      <td>0.828014</td>
      <td>-0.369545</td>
      <td>-0.069822</td>
      <td>0.843472</td>
      <td>0.128553</td>
      <td>0.724221</td>
      <td>0.461989</td>
    </tr>
    <tr>
      <td>Mn</td>
      <td>0.746716</td>
      <td>0.427458</td>
      <td>0.894268</td>
      <td>0.443098</td>
      <td>-0.166005</td>
      <td>0.474967</td>
      <td>-0.659757</td>
      <td>0.787779</td>
      <td>1.000000</td>
      <td>0.632818</td>
      <td>-0.266993</td>
      <td>-0.180060</td>
      <td>0.778836</td>
      <td>0.266207</td>
      <td>0.692412</td>
      <td>0.653757</td>
    </tr>
    <tr>
      <td>Ni</td>
      <td>0.739213</td>
      <td>0.200908</td>
      <td>0.686629</td>
      <td>0.857589</td>
      <td>-0.093689</td>
      <td>0.340826</td>
      <td>-0.576156</td>
      <td>0.828014</td>
      <td>0.632818</td>
      <td>1.000000</td>
      <td>-0.454138</td>
      <td>-0.193527</td>
      <td>0.749251</td>
      <td>0.130333</td>
      <td>0.653348</td>
      <td>0.401977</td>
    </tr>
    <tr>
      <td>P</td>
      <td>-0.364677</td>
      <td>-0.057399</td>
      <td>-0.365085</td>
      <td>-0.549283</td>
      <td>-0.170905</td>
      <td>-0.363667</td>
      <td>0.177471</td>
      <td>-0.369545</td>
      <td>-0.266993</td>
      <td>-0.454138</td>
      <td>1.000000</td>
      <td>-0.023872</td>
      <td>-0.363509</td>
      <td>-0.085956</td>
      <td>-0.411401</td>
      <td>-0.095169</td>
    </tr>
    <tr>
      <td>S</td>
      <td>-0.170005</td>
      <td>-0.051459</td>
      <td>-0.092047</td>
      <td>-0.176265</td>
      <td>-0.034958</td>
      <td>-0.152516</td>
      <td>0.237180</td>
      <td>-0.069822</td>
      <td>-0.180060</td>
      <td>-0.193527</td>
      <td>-0.023872</td>
      <td>1.000000</td>
      <td>-0.262592</td>
      <td>-0.136886</td>
      <td>-0.275499</td>
      <td>-0.135064</td>
    </tr>
    <tr>
      <td>Sc</td>
      <td>0.814932</td>
      <td>0.349757</td>
      <td>0.773096</td>
      <td>0.703535</td>
      <td>-0.205959</td>
      <td>0.506711</td>
      <td>-0.710909</td>
      <td>0.843472</td>
      <td>0.778836</td>
      <td>0.749251</td>
      <td>-0.363509</td>
      <td>-0.262592</td>
      <td>1.000000</td>
      <td>0.210042</td>
      <td>0.900059</td>
      <td>0.415537</td>
    </tr>
    <tr>
      <td>Sr</td>
      <td>0.071028</td>
      <td>0.415623</td>
      <td>0.169552</td>
      <td>-0.009582</td>
      <td>0.108863</td>
      <td>-0.081127</td>
      <td>-0.151509</td>
      <td>0.128553</td>
      <td>0.266207</td>
      <td>0.130333</td>
      <td>-0.085956</td>
      <td>-0.136886</td>
      <td>0.210042</td>
      <td>1.000000</td>
      <td>0.172828</td>
      <td>0.348954</td>
    </tr>
    <tr>
      <td>V</td>
      <td>0.777716</td>
      <td>0.197733</td>
      <td>0.663249</td>
      <td>0.656541</td>
      <td>-0.178633</td>
      <td>0.662761</td>
      <td>-0.629039</td>
      <td>0.724221</td>
      <td>0.692412</td>
      <td>0.653348</td>
      <td>-0.411401</td>
      <td>-0.275499</td>
      <td>0.900059</td>
      <td>0.172828</td>
      <td>1.000000</td>
      <td>0.331629</td>
    </tr>
    <tr>
      <td>Zn</td>
      <td>0.387752</td>
      <td>0.329626</td>
      <td>0.462747</td>
      <td>0.208787</td>
      <td>-0.140585</td>
      <td>0.051180</td>
      <td>-0.379125</td>
      <td>0.461989</td>
      <td>0.653757</td>
      <td>0.401977</td>
      <td>-0.095169</td>
      <td>-0.135064</td>
      <td>0.415537</td>
      <td>0.348954</td>
      <td>0.331629</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>



<p style="text-align: center;font-size: 10px;font-style: oblique;">Tabla 8. Matriz de covarianza</p>

# Matriz de correlación <a name='Mcorr'/>

<p>De manera similar al caso anterior, para el cálculo se excluyeron las tres primeras columnas correspondientes a
coordenadas este, coordenadas norte y el peso de las muestras
</p>


```python
#Se calcula la matriz de correlación
mcor=datosnor.drop(columns=datosnor.columns[0:3], axis=1).corr()
mcor
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Al</th>
      <th>Ca</th>
      <th>Co</th>
      <th>Cr</th>
      <th>Cu</th>
      <th>Fe</th>
      <th>K</th>
      <th>Mg</th>
      <th>Mn</th>
      <th>Ni</th>
      <th>P</th>
      <th>S</th>
      <th>Sc</th>
      <th>Sr</th>
      <th>V</th>
      <th>Zn</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Al</td>
      <td>1.000000</td>
      <td>-0.009584</td>
      <td>0.700752</td>
      <td>0.688654</td>
      <td>-0.184098</td>
      <td>0.535382</td>
      <td>-0.603281</td>
      <td>0.914588</td>
      <td>0.746716</td>
      <td>0.739213</td>
      <td>-0.364677</td>
      <td>-0.170005</td>
      <td>0.814932</td>
      <td>0.071028</td>
      <td>0.777716</td>
      <td>0.387752</td>
    </tr>
    <tr>
      <td>Ca</td>
      <td>-0.009584</td>
      <td>1.000000</td>
      <td>0.408500</td>
      <td>0.032259</td>
      <td>0.000076</td>
      <td>0.054239</td>
      <td>-0.296143</td>
      <td>0.161397</td>
      <td>0.427458</td>
      <td>0.200908</td>
      <td>-0.057399</td>
      <td>-0.051459</td>
      <td>0.349757</td>
      <td>0.415623</td>
      <td>0.197733</td>
      <td>0.329626</td>
    </tr>
    <tr>
      <td>Co</td>
      <td>0.700752</td>
      <td>0.408500</td>
      <td>1.000000</td>
      <td>0.500780</td>
      <td>-0.055038</td>
      <td>0.512521</td>
      <td>-0.641043</td>
      <td>0.763399</td>
      <td>0.894268</td>
      <td>0.686629</td>
      <td>-0.365085</td>
      <td>-0.092047</td>
      <td>0.773096</td>
      <td>0.169552</td>
      <td>0.663249</td>
      <td>0.462747</td>
    </tr>
    <tr>
      <td>Cr</td>
      <td>0.688654</td>
      <td>0.032259</td>
      <td>0.500780</td>
      <td>1.000000</td>
      <td>-0.086608</td>
      <td>0.409009</td>
      <td>-0.478470</td>
      <td>0.720525</td>
      <td>0.443098</td>
      <td>0.857589</td>
      <td>-0.549283</td>
      <td>-0.176265</td>
      <td>0.703535</td>
      <td>-0.009582</td>
      <td>0.656541</td>
      <td>0.208787</td>
    </tr>
    <tr>
      <td>Cu</td>
      <td>-0.184098</td>
      <td>0.000076</td>
      <td>-0.055038</td>
      <td>-0.086608</td>
      <td>1.000000</td>
      <td>0.117543</td>
      <td>0.050792</td>
      <td>-0.189134</td>
      <td>-0.166005</td>
      <td>-0.093689</td>
      <td>-0.170905</td>
      <td>-0.034958</td>
      <td>-0.205959</td>
      <td>0.108863</td>
      <td>-0.178633</td>
      <td>-0.140585</td>
    </tr>
    <tr>
      <td>Fe</td>
      <td>0.535382</td>
      <td>0.054239</td>
      <td>0.512521</td>
      <td>0.409009</td>
      <td>0.117543</td>
      <td>1.000000</td>
      <td>-0.451225</td>
      <td>0.377934</td>
      <td>0.474967</td>
      <td>0.340826</td>
      <td>-0.363667</td>
      <td>-0.152516</td>
      <td>0.506711</td>
      <td>-0.081127</td>
      <td>0.662761</td>
      <td>0.051180</td>
    </tr>
    <tr>
      <td>K</td>
      <td>-0.603281</td>
      <td>-0.296143</td>
      <td>-0.641043</td>
      <td>-0.478470</td>
      <td>0.050792</td>
      <td>-0.451225</td>
      <td>1.000000</td>
      <td>-0.646233</td>
      <td>-0.659757</td>
      <td>-0.576156</td>
      <td>0.177471</td>
      <td>0.237180</td>
      <td>-0.710909</td>
      <td>-0.151509</td>
      <td>-0.629039</td>
      <td>-0.379125</td>
    </tr>
    <tr>
      <td>Mg</td>
      <td>0.914588</td>
      <td>0.161397</td>
      <td>0.763399</td>
      <td>0.720525</td>
      <td>-0.189134</td>
      <td>0.377934</td>
      <td>-0.646233</td>
      <td>1.000000</td>
      <td>0.787779</td>
      <td>0.828014</td>
      <td>-0.369545</td>
      <td>-0.069822</td>
      <td>0.843472</td>
      <td>0.128553</td>
      <td>0.724221</td>
      <td>0.461989</td>
    </tr>
    <tr>
      <td>Mn</td>
      <td>0.746716</td>
      <td>0.427458</td>
      <td>0.894268</td>
      <td>0.443098</td>
      <td>-0.166005</td>
      <td>0.474967</td>
      <td>-0.659757</td>
      <td>0.787779</td>
      <td>1.000000</td>
      <td>0.632818</td>
      <td>-0.266993</td>
      <td>-0.180060</td>
      <td>0.778836</td>
      <td>0.266207</td>
      <td>0.692412</td>
      <td>0.653757</td>
    </tr>
    <tr>
      <td>Ni</td>
      <td>0.739213</td>
      <td>0.200908</td>
      <td>0.686629</td>
      <td>0.857589</td>
      <td>-0.093689</td>
      <td>0.340826</td>
      <td>-0.576156</td>
      <td>0.828014</td>
      <td>0.632818</td>
      <td>1.000000</td>
      <td>-0.454138</td>
      <td>-0.193527</td>
      <td>0.749251</td>
      <td>0.130333</td>
      <td>0.653348</td>
      <td>0.401977</td>
    </tr>
    <tr>
      <td>P</td>
      <td>-0.364677</td>
      <td>-0.057399</td>
      <td>-0.365085</td>
      <td>-0.549283</td>
      <td>-0.170905</td>
      <td>-0.363667</td>
      <td>0.177471</td>
      <td>-0.369545</td>
      <td>-0.266993</td>
      <td>-0.454138</td>
      <td>1.000000</td>
      <td>-0.023872</td>
      <td>-0.363509</td>
      <td>-0.085956</td>
      <td>-0.411401</td>
      <td>-0.095169</td>
    </tr>
    <tr>
      <td>S</td>
      <td>-0.170005</td>
      <td>-0.051459</td>
      <td>-0.092047</td>
      <td>-0.176265</td>
      <td>-0.034958</td>
      <td>-0.152516</td>
      <td>0.237180</td>
      <td>-0.069822</td>
      <td>-0.180060</td>
      <td>-0.193527</td>
      <td>-0.023872</td>
      <td>1.000000</td>
      <td>-0.262592</td>
      <td>-0.136886</td>
      <td>-0.275499</td>
      <td>-0.135064</td>
    </tr>
    <tr>
      <td>Sc</td>
      <td>0.814932</td>
      <td>0.349757</td>
      <td>0.773096</td>
      <td>0.703535</td>
      <td>-0.205959</td>
      <td>0.506711</td>
      <td>-0.710909</td>
      <td>0.843472</td>
      <td>0.778836</td>
      <td>0.749251</td>
      <td>-0.363509</td>
      <td>-0.262592</td>
      <td>1.000000</td>
      <td>0.210042</td>
      <td>0.900059</td>
      <td>0.415537</td>
    </tr>
    <tr>
      <td>Sr</td>
      <td>0.071028</td>
      <td>0.415623</td>
      <td>0.169552</td>
      <td>-0.009582</td>
      <td>0.108863</td>
      <td>-0.081127</td>
      <td>-0.151509</td>
      <td>0.128553</td>
      <td>0.266207</td>
      <td>0.130333</td>
      <td>-0.085956</td>
      <td>-0.136886</td>
      <td>0.210042</td>
      <td>1.000000</td>
      <td>0.172828</td>
      <td>0.348954</td>
    </tr>
    <tr>
      <td>V</td>
      <td>0.777716</td>
      <td>0.197733</td>
      <td>0.663249</td>
      <td>0.656541</td>
      <td>-0.178633</td>
      <td>0.662761</td>
      <td>-0.629039</td>
      <td>0.724221</td>
      <td>0.692412</td>
      <td>0.653348</td>
      <td>-0.411401</td>
      <td>-0.275499</td>
      <td>0.900059</td>
      <td>0.172828</td>
      <td>1.000000</td>
      <td>0.331629</td>
    </tr>
    <tr>
      <td>Zn</td>
      <td>0.387752</td>
      <td>0.329626</td>
      <td>0.462747</td>
      <td>0.208787</td>
      <td>-0.140585</td>
      <td>0.051180</td>
      <td>-0.379125</td>
      <td>0.461989</td>
      <td>0.653757</td>
      <td>0.401977</td>
      <td>-0.095169</td>
      <td>-0.135064</td>
      <td>0.415537</td>
      <td>0.348954</td>
      <td>0.331629</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>



<p style="text-align: center;font-size: 10px;font-style: oblique;">Tabla 9. Matriz de correlación</p>

<p>Para poder realizar una mejor interpretación de los datos obtenidos, se graficó en forma de "heatmap". De esta forma, es posible visualizar la información completa de manera objetiva.
</p>
<p>Este proceso se realizó para la matriz de covarianza y la matriz de correlación.
</p>


```python
#Se grafica la matriz de covarianza en forma de heatmap
ax = plt.axes()
sns.heatmap(mcov, xticklabels=mcov.columns, yticklabels=mcov.columns)
ax.set_title('Matriz de covarianza', fontsize=20)
plt.show()
```


![png](outputImages/output_94_0.png)


<p style="text-align: center;font-size: 10px;font-style: oblique;">Figura 15. Matriz de covarianza</p>


```python
#Se grafica la matriz de correlación en forma de heatmap
ax = plt.axes()
sns.heatmap(mcor, xticklabels=mcor.columns, yticklabels=mcor.columns)
ax.set_title('Matriz de correlación', fontsize=20)
plt.show()
```


![png](outputImages/output_96_0.png)


<p style="text-align: center;font-size: 10px;font-style: oblique;">Figura 16. Matriz de correlación</p>

<p>Por otro lado, ya que se cuenta con los datos numéricos, es posible determinar de manera precisa entre qué elementos se presenta mayor correlación, covarianza, así como su valor. Por consiguiente, se optó por crear un nuevo dataframe que contuviera los valores más altos de estos parámetros
</p>


```python
#Se crean los datafrmaes de correlación y covarianza
columnas=[]
for i in range (len(mcor.columns)):
    columnas.append(mcor.columns[i])
    
mcor.columns=columnas
mcov.columns=columnas  

dfcor = pd.DataFrame(columns=['Corr', 'Elemento1', 'Elemento2'])
dfcov = pd.DataFrame(columns=['Cov', 'Elemento1', 'Elemento2'])
```


```python
#Se obtienen los elementos con mayor y menor covarianza y correlación
for i in range(len(mcor.columns)):
    va=(mcor.sort_values([mcor.columns[i]], ascending=False)[[mcor.columns[i]]].head(4)).copy()
    va['Elemento1']=mcor.columns[i]
    va['Elemento2']=va.index
    va.rename(columns={va.columns[0]:'Corr'},inplace = True)
    dfcor=dfcor.append(va)
    
    va=(mcor.sort_values([mcor.columns[i]], ascending=False)[[mcor.columns[i]]].tail(3)).copy()
    va['Elemento1']=mcor.columns[i]
    va['Elemento2']=va.index
    va.rename(columns={va.columns[0]:'Corr'},inplace = True)
    dfcor=dfcor.append(va)
    
    va=(mcor.sort_values([mcor.columns[i]], ascending=False)[[mcor.columns[i]]].head(4)).copy()
    va['Elemento1']=mcov.columns[i]
    va['Elemento2']=va.index
    va.rename(columns={va.columns[0]:'Cov'},inplace = True)
    dfcov=dfcov.append(va)
    
    va=(mcor.sort_values([mcor.columns[i]], ascending=False)[[mcor.columns[i]]].tail(3)).copy()
    va['Elemento1']=mcov.columns[i]
    va['Elemento2']=va.index
    va.rename(columns={va.columns[0]:'Cov'},inplace = True)
    dfcov=dfcov.append(va)
    
dfcor.reset_index(drop=True, inplace=True)
dfcor.drop(dfcor[dfcor[dfcor.columns[0]]==1].index, inplace=True)
dfcor.reset_index(drop=True, inplace=True)

dfcov.reset_index(drop=True, inplace=True)
dfcov.drop(dfcov[dfcov[dfcov.columns[0]]==1].index, inplace=True)
dfcov.reset_index(drop=True, inplace=True)
```


```python
#Se mjestran los elementos con mayor y menor correlación
dfcov.sort_values(['Cov'], ascending=False).iloc[::6, :]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Cov</th>
      <th>Elemento1</th>
      <th>Elemento2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>0.914588</td>
      <td>Al</td>
      <td>Mg</td>
    </tr>
    <tr>
      <td>18</td>
      <td>0.857589</td>
      <td>Cr</td>
      <td>Ni</td>
    </tr>
    <tr>
      <td>1</td>
      <td>0.814932</td>
      <td>Al</td>
      <td>Sc</td>
    </tr>
    <tr>
      <td>13</td>
      <td>0.773096</td>
      <td>Co</td>
      <td>Sc</td>
    </tr>
    <tr>
      <td>30</td>
      <td>0.662761</td>
      <td>Fe</td>
      <td>V</td>
    </tr>
    <tr>
      <td>6</td>
      <td>0.427458</td>
      <td>Ca</td>
      <td>Mn</td>
    </tr>
    <tr>
      <td>66</td>
      <td>0.237180</td>
      <td>S</td>
      <td>K</td>
    </tr>
    <tr>
      <td>38</td>
      <td>0.050792</td>
      <td>K</td>
      <td>Cu</td>
    </tr>
    <tr>
      <td>62</td>
      <td>-0.057399</td>
      <td>P</td>
      <td>Ca</td>
    </tr>
    <tr>
      <td>94</td>
      <td>-0.140585</td>
      <td>Zn</td>
      <td>Cu</td>
    </tr>
    <tr>
      <td>27</td>
      <td>-0.184098</td>
      <td>Cu</td>
      <td>Al</td>
    </tr>
    <tr>
      <td>75</td>
      <td>-0.262592</td>
      <td>Sc</td>
      <td>S</td>
    </tr>
    <tr>
      <td>76</td>
      <td>-0.363509</td>
      <td>Sc</td>
      <td>P</td>
    </tr>
    <tr>
      <td>88</td>
      <td>-0.411401</td>
      <td>V</td>
      <td>P</td>
    </tr>
    <tr>
      <td>23</td>
      <td>-0.549283</td>
      <td>Cr</td>
      <td>P</td>
    </tr>
    <tr>
      <td>39</td>
      <td>-0.646233</td>
      <td>K</td>
      <td>Mg</td>
    </tr>
  </tbody>
</table>
</div>



<p style="text-align: center;font-size: 10px;font-style: oblique;">Tabla 10. Elementos de mayor y menor covarianza</p>


```python
#Se muestran los elementos con mayor y menor correlación
dfcor.sort_values(['Corr'], ascending=False).iloc[::6, :].tail(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Corr</th>
      <th>Elemento1</th>
      <th>Elemento2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>75</td>
      <td>-0.262592</td>
      <td>Sc</td>
      <td>S</td>
    </tr>
    <tr>
      <td>76</td>
      <td>-0.363509</td>
      <td>Sc</td>
      <td>P</td>
    </tr>
    <tr>
      <td>88</td>
      <td>-0.411401</td>
      <td>V</td>
      <td>P</td>
    </tr>
    <tr>
      <td>23</td>
      <td>-0.549283</td>
      <td>Cr</td>
      <td>P</td>
    </tr>
    <tr>
      <td>39</td>
      <td>-0.646233</td>
      <td>K</td>
      <td>Mg</td>
    </tr>
  </tbody>
</table>
</div>



<p style="text-align: center;font-size: 10px;font-style: oblique;">Tabla 11. Elementos de mayor y menor correlación</p>

De los datos y gráficas observadas anteriormente es posible determinar una relación existente entre la presencia de los elementos contenidos en las muestras, con base en su covarianza y correlación calculadas. Dentro de las observaciones más destacables se encuentran:
- La diferencia entre la matriz de correlación y la matriz de convarianza es que la matriz de correlación se encuentra estadarizada. Debido a que se realizó una estandarización de los datos previo al cálculo, la matriz de covarianza y correlación coinciden en valores.
- Los pares de elementos que presentaron una mayor correlación son (Al  Mg), (V - Sc) y (Mn - Co). Es decir, cuando la concentración de uno de estos elementos aumenta, también lo hará la concentración del otro elemento.
- Los pares de elementos que presentaron una menor correlación son (K  Mg), (P - Cr) y (V - P). Es decir, cuando la concentración de uno de estos elementos aumenta, la concentración del otro elemento disminuye.
- El elemento que más tiende a repetirse en altas correlaciones es el Mg. Por lo que la presencia de Mg es un buen indicador de altas concentraciones de los otros elementos correlacionados, como son: Al, Mn, Sc, V y Co.
- El elemento que más tiende a repetirse en correlaciones negativas es el K. Por lo que la presencia de k es un indicador de bajas o nulas concentraciones de Mg y Mn. Además a que están correlacionadas, también se esperaría una baja concentración de los elementos anteriores.
- Otro elemento que tiende a repetirse en correlaciones negativas es el P. Asociado a bajas concentraciones de Sc, Co, V, Ni y Cr

<h1 style="text-align: center; font-size: 35px; margin-top: 150px;">Análisis espacial</h1><a name='AES'/>


```python
#En caso de no contar con la biblioteca
#!pip install scikit-gstat
#Link de instalación
#https://mmaelicke.github.io/scikit-gstat/install.html
```


```python
from skgstat import Variogram
from sklearn.decomposition import PCA
from sklearn.metrics import mean_squared_error
from math import sqrt
from sklearn.cluster import KMeans
```

# Semivariogramas <a name='Semivariograma'/>


```python
#Se grafican los variogramas
temp=[]
temp2=[]
temp3=[]

for i in range(len(datos['este'])):
    temp.append(datos['este'].values[i])
    temp2.append(datos['norte'].values[i])

fig, _a = plt.subplots(4, 4, figsize=(8,8))
plt.subplots_adjust(right=2.5, top = 2.5)
axes = _a.flatten()

for i in range(len(datosnor.columns)-3):
    temp3=[]
    for j in range(len(datosnor[datosnor.columns[i]])):        
        temp3.append(datosnor[datosnor.columns[i+3]].values[j])
    axes[i].set_title(datosnor.columns[i+3], fontsize=20)
    V = Variogram(coordinates=np.vstack((temp, temp2)).transpose(), values=temp3)
    V.plot(axes=axes[i], hist=False, show=True)

plt.show()
```

    C:\ProgramData\Anaconda3\lib\site-packages\skgstat\Variogram.py:1638: UserWarning: Matplotlib is currently using module://ipykernel.pylab.backend_inline, which is a non-GUI backend, so cannot show the figure.
      fig.show()
    


![png](outputImages/output_110_1.png)


<p style="text-align: center;font-size: 10px;font-style: oblique;">Figura 17. Variogramas</p>

De manera general, en gráfico anterior podemos observar que la mayoría de los elementos pierden su correlación espacial muy rápido. Por lo que resulta poco probable encontrar muestras con concentraciones similares a distancias medias y largas. 
- Los elementos que pierden más rápidamente su correlación espacial son Sr, Ni, Cu y S
- Los elementos que guardan una correlación espacial a distancias medianas y largas son el Zn, K y Ca.
- Los elementos con correlaciones importantes pierden correlación espacial a distancias medianas

# Componentes Principales <a name='CP'/>


```python
#Se calculan las componentes principales 
pca=PCA()
pca.fit(datosnor.drop(columns=datosnor.columns[0:3], axis=1))
pca_datos=pca.transform(datosnor.drop(columns=datosnor.columns[0:3], axis=1))
```


```python
#Se calcula el porcentaje de información y porcentaje de información acumulada
per_var=np.round(pca.explained_variance_ratio_*100, decimals=1)
per_var_acum=np.cumsum(per_var)
labels=['PC'+str(x) for x in range(1, len(per_var)+1)]
```


```python
#Se grafican los prcentajes obtenidos anteriormente
fig, ax1 = plt.subplots()
t = np.arange(0.01, 10.0, 0.01)
s1 = np.exp(t)
ax1.bar(x=range(1,len(per_var)+1), height=per_var, tick_label=labels)
ax1.set_xlabel('Componentes Principales')
# Make the y-axis label, ticks and tick labels match the line color.
ax1.set_ylabel('Porcentaje [%]', color='b')
ax1.tick_params('y', colors='b')
plt.xticks(rotation='vertical')

ax2 = ax1.twinx()
ax2.plot(labels,per_var_acum,color='r')
ax2.set_ylabel('Porcentaje acumulado [%]', color='r')
ax2.tick_params('y', colors='r')
plt.xticks(rotation='vertical')

fig.tight_layout()
plt.title('Porcentaje de datos representados')
plt.grid()
plt.show()
```


![png](outputImages/output_116_0.png)


<p style="text-align: center;font-size: 10px;font-style: oblique;">Figura 18. Contenido de información en las componentes principales</p>

Podemos observar que, como era esperado, el mayor porcentaje de información se encuentra en la primera componente (cerca del 50%). Sin embargo, el porcentaje total de información se aproxima de forma lenta al 100%, por lo que para obtener un gran porcentaje de la información sería necesario abarcar numerosas componetes.


```python
#Se crea un data frame de las componentes principales
pca_df=pd.DataFrame(pca_datos, index=datos.index, columns=labels)
pca_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>PC1</th>
      <th>PC2</th>
      <th>PC3</th>
      <th>PC4</th>
      <th>PC5</th>
      <th>PC6</th>
      <th>PC7</th>
      <th>PC8</th>
      <th>PC9</th>
      <th>PC10</th>
      <th>PC11</th>
      <th>PC12</th>
      <th>PC13</th>
      <th>PC14</th>
      <th>PC15</th>
      <th>PC16</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>0.259987</td>
      <td>-1.371718</td>
      <td>1.716030</td>
      <td>-0.839130</td>
      <td>-0.671461</td>
      <td>-1.123032</td>
      <td>0.008988</td>
      <td>-0.039826</td>
      <td>0.367012</td>
      <td>-0.034095</td>
      <td>-0.180933</td>
      <td>-0.693654</td>
      <td>0.181376</td>
      <td>-0.062544</td>
      <td>-0.288514</td>
      <td>-0.141234</td>
    </tr>
    <tr>
      <td>1</td>
      <td>3.813911</td>
      <td>-2.010293</td>
      <td>0.291595</td>
      <td>-0.643573</td>
      <td>-0.685350</td>
      <td>-0.494142</td>
      <td>-0.244044</td>
      <td>0.120202</td>
      <td>0.340137</td>
      <td>-0.219652</td>
      <td>0.106706</td>
      <td>-0.189298</td>
      <td>-0.350655</td>
      <td>0.038171</td>
      <td>-0.067981</td>
      <td>-0.354574</td>
    </tr>
    <tr>
      <td>2</td>
      <td>-1.370037</td>
      <td>1.400552</td>
      <td>3.295659</td>
      <td>-0.228697</td>
      <td>1.176470</td>
      <td>-0.428771</td>
      <td>1.247583</td>
      <td>-1.556516</td>
      <td>0.119820</td>
      <td>-0.189173</td>
      <td>0.476634</td>
      <td>-0.040070</td>
      <td>-0.474474</td>
      <td>-0.388780</td>
      <td>-0.112417</td>
      <td>-0.228781</td>
    </tr>
    <tr>
      <td>3</td>
      <td>-0.906562</td>
      <td>-1.648617</td>
      <td>0.898269</td>
      <td>-0.229162</td>
      <td>0.806615</td>
      <td>1.053126</td>
      <td>0.620034</td>
      <td>0.733797</td>
      <td>0.958076</td>
      <td>0.205908</td>
      <td>0.096714</td>
      <td>-0.198724</td>
      <td>-0.347740</td>
      <td>-0.034267</td>
      <td>-0.178263</td>
      <td>0.038650</td>
    </tr>
    <tr>
      <td>4</td>
      <td>-1.963021</td>
      <td>2.258416</td>
      <td>1.662563</td>
      <td>-0.211428</td>
      <td>0.784855</td>
      <td>0.477740</td>
      <td>-0.964390</td>
      <td>-1.259635</td>
      <td>0.674855</td>
      <td>-0.552977</td>
      <td>-0.165276</td>
      <td>-0.604685</td>
      <td>-0.185193</td>
      <td>0.264874</td>
      <td>0.359657</td>
      <td>-0.032584</td>
    </tr>
  </tbody>
</table>
</div>



<p style="text-align: center;font-size: 10px;font-style: oblique;">Tabla 12. Set de valores de componentes principales</p>


```python
#Se realiza la gráfica de puntuaciones
plt.scatter(pca_df.PC1,pca_df.PC2)
plt.xlabel('Componente Principal 1 - {0}%'.format(per_var[0]))
plt.ylabel('Componente Principal 2 - {0}%'.format(per_var[1]))

for muestra in pca_df.index:
    plt.annotate(muestra, (pca_df.PC1.loc[muestra], pca_df.PC2.loc[muestra]))

plt.title('Gráfica de puntuaciones')
plt.show()
```


![png](outputImages/output_121_0.png)


<p style="text-align: center;font-size: 10px;font-style: oblique;">Figura 19.Gráfica de puntuaciones</p>

Dentro del gráfico de puntuaciones no es posible determinar agrupaciones específicas. Sin embargo, es posible inferir la existencia de dos o tres agrupaciones principales dentro del set de datos. Para concluir sobre las agrupaciones necesarias, se realizará una gráfica de codo.


```python
#Se realiza la gráfica de puntuaciones
plt.scatter(pca_df.PC2,pca_df.PC3)
plt.xlabel('Componente Principal 2 - {0}%'.format(per_var[2]))
plt.ylabel('Componente Principal 3 - {0}%'.format(per_var[3]))

for muestra in pca_df.index:
    plt.annotate(muestra, (pca_df.PC2.loc[muestra], pca_df.PC3.loc[muestra]))

plt.title('Gráfica de puntuaciones')
plt.show()
```


![png](outputImages/output_124_0.png)


<p style="text-align: center;font-size: 10px;font-style: oblique;">Figura 20.Gráfica de puntuaciones</p>

Una vez que graficamos la segunda y tercer componente en la gráfica de puntuaciones, observamos una segunda agrupación correspondiente probablemente a un segundo posible tipo de dato que presenta caracterísiticas similares. Se observa que la presencia de puntos en una agrupación aparece fuera de la agrupación en el otro gráfico. Algunos ejemplos de esto son:
- Punto 12
- Punto 59
- Punto 13


```python
#Se realiza una representación de la primera componente principal en el muestreo de datos
marker_size=15
plt.scatter(datos['este'], datos['norte'], marker_size, c=pca_df.PC1)
plt.title("Puntos de muestreo CP1")
plt.xlabel("[mE]")
plt.ylabel("[mN]")
cbar= plt.colorbar()
cbar.set_label("Peso de la información", labelpad=+1)
plt.grid()
plt.show()
```


![png](outputImages/output_127_0.png)


<p style="text-align: center;font-size: 10px;font-style: oblique;">Figura 21. Primera componente proyectada en los puntos de muestreo</p>

Dentro de los puntos graficados junto con la primera componente principal asociada podemos observar que existe una tendencia de los valores con mayor magnitud de contenido de información. La tendencia descrita corresponde a una dirección de (SW - NE), en las partes centrales de las agrupaciones de puntos, estando rodeadas por puntos con menor contenido de información.


```python
#Se realiza una representación de la segunda componente principal en el muestreo de datos
marker_size=15
plt.scatter(datos['este'], datos['norte'], marker_size, c=pca_df.PC2)
plt.title("Puntos de muestreo CP2")
plt.xlabel("[mE]")
plt.ylabel("[mN]")
cbar= plt.colorbar()
cbar.set_label("Peso de la información", labelpad=+1)
plt.grid()
plt.show()
```


![png](outputImages/output_130_0.png)


<p style="text-align: center;font-size: 10px;font-style: oblique;">Figura 22. Segunda componente proyectada en los puntos de muestreo</p>

Para el caso de la segunda componente principal, considerando que el orden de magnitud presente es la mitad de la obtenida en la primera componente principal, se observa una mayor dispersión de los valores, sin presentar una tendencia o agrupación característica.


```python
#Se realiza una representación de la segunda componente principal en el muestreo de datos
marker_size=15
plt.scatter(datos['este'], datos['norte'], marker_size, c=pca_df.PC3)
plt.title("Puntos de muestreo CP3")
plt.xlabel("[mE]")
plt.ylabel("[mN]")
cbar= plt.colorbar()
cbar.set_label("Peso de la información", labelpad=+1)
plt.grid()
plt.show()
```


![png](outputImages/output_133_0.png)


<p style="text-align: center;font-size: 10px;font-style: oblique;">Figura 23. Tercera componente proyectada en los puntos de muestreo</p>

En general, observamos que los valores altos dentro de la  primera componente principal abarca los valores centrales , mientras que los valores altos en la segunda y tercera componente se presentan en las zonas circundantes de las obtenidas en la primera compoente principal.

# Clustering <a name='clust'/>


```python
#Se grafican las firmas metálicas leídas (cada 5 muestras)
for j in range(len(datos)):
    temp=[]
    for i in range(len(datos.columns)-3):
        temp.append(datosnor[datosnor.columns[i+3]][j])
    #Se grafican las firmas minerales
    if (j%5==0):
        plt.plot(datos.columns[3:],temp)
                 
plt.title('Firma metálicas')
plt.show()        
```


![png](outputImages/output_137_0.png)


<p style="text-align: center;font-size: 10px;font-style: oblique;">Figura 24. Firmas metálicas del estudio</p>


```python
#Se obtiene un arreglo de datos del dataframe de datos normalizados
X=np.array(datosnor[datosnor.columns[3:]])
```


```python
#Se obtiene la gráfica de codo para determinar el número de clusters necesarios
Nc = range(1, 20)
kmeans = [KMeans(n_clusters=i) for i in Nc]
kmeans
score = [kmeans[i].fit(X).score(X) for i in range(len(kmeans))]
score
plt.plot(Nc,score)
plt.xlabel('Numero de Clusters')
plt.ylabel('Score')
plt.title('Curva de Codo (Elbow Curve)')
plt.grid()
plt.show()
```


![png](outputImages/output_140_0.png)


<p style="text-align: center;font-size: 10px;font-style: oblique;">Figura 25. Curva de codo</p>

En la curva de codo obtenida no resulta fácilmente identificable el codo de la gráfica. Debido a que hay una disminución gradual de la pendiente, el último cambio destacable de pendiente resulta en un número de clusters de dos. No obstante, se realizaron pruebas variando el número de clusters para ver diferentes aproximaciones.
En cada una de las aproximaciones con diferente número de clusters, se calcula la suma del error medio cuadrático en el modelo calculado y las muestras. Dicha suma se utilizó como indicador para ver las variaciones de las aproximaciones. Se observó que:
- Con dos clusters el error total acumulado es de aproximadamente 63
- Con tres clusters el error total acumulado es de aproximadamente 59
- Con cuatro clusters el error total acumulado es de aproximadamente 57
- Con cinco clusters el error total acumulado es de aproximadamente 54.5
- Con seis clusters el error total acumulado es de aproximadamente 52.5

Por lo que se decidión utilizar 2 clusters, ya que apartir de ese punto la disminución del error no es significativa


```python
#Se ajusta el número de clusters con la información del dataframe
kmeans = KMeans(n_clusters=3).fit(X)
centroides = kmeans.cluster_centers_
print('Centroides:\n',centroides)
```

    Centroides:
     [[-0.03735576  0.17248574  0.05734057 -0.11454178  0.06740466  0.67685877
      -0.06337615 -0.20447575  0.04283299 -0.17322004 -0.05310738 -0.17492688
       0.10070703 -0.12867983  0.43748458 -0.2602612 ]
     [ 1.14455078  0.23288422  1.08764316  1.11132807 -0.14070416  0.51136767
      -0.92456752  1.24178843  1.05281317  1.16495248 -0.63724462 -0.27426257
       1.18220405  0.18959629  0.96619294  0.65213321]
     [-0.81631138 -0.28791993 -0.83852683 -0.73966885  0.05786167 -0.83388069
       0.72270116 -0.7747579  -0.80310259 -0.73940444  0.5044878   0.31999654
      -0.93739303 -0.05236092 -1.00638144 -0.30345067]]
    


```python
#Se grafican los centroides obtenidos
for i in range(len(centroides)):
    plt.plot(datos.columns[3:],centroides[i], label='Cent '+str(i+1))
plt.title('Centroides')
plt.xlabel("Elementos")
plt.ylabel("Concentración normalizada")
plt.xticks(rotation='vertical')
plt.legend()
plt.show
```




    <function matplotlib.pyplot.show(*args, **kw)>




![png](outputImages/output_144_1.png)


<p style="text-align: center;font-size: 10px;font-style: oblique;">Figura 26.Centroides de las concentraciones minerales</p>

Se obtuvieron dos comportamientos típicos de concentraciones de elementos (centroides) que describen mayormente el comportamiento de las muestras realizadas. Dicho comportamiento se atibuye a que la población de los datos se puede clasificar en 2 grupos que comparten características similares. La agrupación de los datos se define por el contenido de elementos y su proporción, esto podría ser causado por dos razones: 
- Variaciones litológicas
- Alteraciones mineralógicas
Debido a que se realizó una divisón en dos grupos principales, y que resultan composiciones químicas completamente diferentes, se define a las agrupaciones como litologías diferentes. Se realizaron pruebas con mayor número de centroides calculados. Sin embargo, al proyectarlos en el mapa la distribución de los datos resultaba con una gran dispersión.


```python
# Se obtiene una predicción del modelo por cada muestra
labels = kmeans.predict(X)
```


```python
#Se calcula el error acumulado del ajuste realizado
errortot=0
for i in range(len(labels)):
    rms = sqrt(mean_squared_error(X[i,:], centroides[labels[i]]))
    errortot+=rms
errortot
```




    57.256894072043934




```python
#Se agrega la clasificación realizada al dataframe original
datos['Centroide'] = pd.Series(labels, index=datos.index)
```


```python
datos.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>norte</th>
      <th>este</th>
      <th>Recvd Wt.</th>
      <th>Al</th>
      <th>Ca</th>
      <th>Co</th>
      <th>Cr</th>
      <th>Cu</th>
      <th>Fe</th>
      <th>K</th>
      <th>Mg</th>
      <th>Mn</th>
      <th>Ni</th>
      <th>P</th>
      <th>S</th>
      <th>Sc</th>
      <th>Sr</th>
      <th>V</th>
      <th>Zn</th>
      <th>Centroide</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>4627001.0</td>
      <td>549050.0</td>
      <td>2.78</td>
      <td>3.71</td>
      <td>0.09</td>
      <td>13.0</td>
      <td>26.0</td>
      <td>6040.0</td>
      <td>10.20</td>
      <td>0.07</td>
      <td>1.65</td>
      <td>1700.0</td>
      <td>8.0</td>
      <td>220.0</td>
      <td>0.03</td>
      <td>12.0</td>
      <td>11.0</td>
      <td>155.0</td>
      <td>139.0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>4627049.0</td>
      <td>549003.0</td>
      <td>3.32</td>
      <td>5.13</td>
      <td>0.02</td>
      <td>32.0</td>
      <td>77.0</td>
      <td>2180.0</td>
      <td>11.35</td>
      <td>0.01</td>
      <td>3.42</td>
      <td>3650.0</td>
      <td>20.0</td>
      <td>160.0</td>
      <td>0.01</td>
      <td>28.0</td>
      <td>5.0</td>
      <td>240.0</td>
      <td>280.0</td>
      <td>1</td>
    </tr>
    <tr>
      <td>2</td>
      <td>4627073.0</td>
      <td>548963.0</td>
      <td>2.76</td>
      <td>1.39</td>
      <td>0.61</td>
      <td>7.0</td>
      <td>21.0</td>
      <td>7480.0</td>
      <td>6.62</td>
      <td>0.12</td>
      <td>0.33</td>
      <td>1475.0</td>
      <td>2.0</td>
      <td>230.0</td>
      <td>0.07</td>
      <td>8.0</td>
      <td>67.0</td>
      <td>105.0</td>
      <td>44.0</td>
      <td>2</td>
    </tr>
    <tr>
      <td>3</td>
      <td>4627050.0</td>
      <td>548964.0</td>
      <td>3.20</td>
      <td>1.74</td>
      <td>0.14</td>
      <td>2.0</td>
      <td>60.0</td>
      <td>1030.0</td>
      <td>8.41</td>
      <td>0.14</td>
      <td>0.40</td>
      <td>148.0</td>
      <td>5.0</td>
      <td>70.0</td>
      <td>0.07</td>
      <td>11.0</td>
      <td>16.0</td>
      <td>165.0</td>
      <td>48.0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>4</td>
      <td>4627090.0</td>
      <td>548870.0</td>
      <td>3.42</td>
      <td>0.23</td>
      <td>2.41</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>4530.0</td>
      <td>3.62</td>
      <td>0.06</td>
      <td>1.16</td>
      <td>286.0</td>
      <td>2.0</td>
      <td>360.0</td>
      <td>0.04</td>
      <td>12.0</td>
      <td>46.0</td>
      <td>26.0</td>
      <td>58.0</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>



<p style="text-align: center;font-size: 10px;font-style: oblique;">Tabla 13. Set de datos final con clasificación</p>


```python
#Se muestra la clasificación realizada en un mapa espacial de las muestras
marker_size=15
plt.scatter(datos['este'], datos['norte'], marker_size, c=datos.Centroide)
plt.title("Puntos de muestreo")
plt.xlabel("[mE]")
plt.ylabel("[mN]")
cbar= plt.colorbar()
cbar.set_label("Litología", labelpad=+1)
plt.grid()
plt.show()
```


![png](outputImages/output_152_0.png)


<p style="text-align: center;font-size: 10px;font-style: oblique;">Figura 24. División litológica de acuerdo a la clasificación realizada</p>

Finalmente, en la última gráfica podemos observar una clasificación de cada punto muestreado con base en las concentraciones de cada elementos químico que la compone. Como ya se había mencionado, se atribuye la diferencia de características a variaciones litológicas. Asumiendo que es el caso, como en mucho resultados anteriores, se obtiene un eje principal en dirección SW - NE, en el que en los valores centrales corresponden a una misma litología, mientras que los valores exteriores correspnden otra.


```python
datos.to_csv('datosCompletos.csv')
```


```python
datos[['este', 'norte', 'Centroide']].to_csv('datos1.csv')
```

<h1 style="text-align: center; font-size: 35px; margin-top: 150px;">Conclusiones</h1><a name='conc'/>

Se clasificó el set de datos limpios en dos agrupaciones de datos principales que compartían características similares. Se deduce que la diferencia entre clases obtenidas puede estar asociada a dos variaciones principales: Variación litológica y alteraciones hidrotermales. Sin embargo, debido a que la variación de concentraciones de elementos resulta completamente opuesta la una de la otra (Figura 26), se concluye que las agrupaciones de datos son representativos de dos litologías diferentes. A su vez, la división en dos grupos principales es comprobado por las gráficas de puntuaciones realizadas, donde dentro de las componentes principales se observan dos aglomeraciones (Figuras 19 y 20).
Ahora bien, una vez definido que se trata de dos litologías el paso siguiente consiste en determinar a que litología corresponde cada grupo asociandolo a una explicación geológica de acuerdo a los resultados obtenidos. Dentro de lo estudiado, se realizaron 3 posibles explicaciones de los datos:
<ol>
<li>Intrusión de cuerpo máfico:</li>
    De acuerdo a lo observado en las firmas metálicas parámetro (Centroides) se observa que la composición química de las litologías presentes corresponden a composiciones químicas opuestas. Centrándonos en los elementos de minerales formadores de roca, por un lado, una litología presenta alto contenido de elementos como Mg, Fe y Ca, característicos de rocas máficas en la Serie de reacciones de Bowen, así como valores bajos de K. Por el contrario, en la otra litología se obtienen concentraciones inversas a las anteriores, por lo que corresponde al comportamiento de una roca félsica. La relación anterior es descrita por los valores de correlación obtenidos, principalmente en las correlaciones asociada a K, como es el caso de (K - Mg).
    Ahora, analizando la distribución espacial de los datos (Figura 24), se observa que la ubicación de la curva metálica correspondiente a altas concentraciones de M y Fe se encuentra inmersa en la segundo litología (altas concentraciones de K), por lo que se infiere una intrusión de un cuerpo ígneo de composicón máfica entre una roca ígnea de composición félsica. Debido a que se asume una intrusión, la roca ígnea que intruye debería corresponder a una roca intrusiva, mientras que la otra litología correpondería a una roca extrusiva. Tomando lo anterior en consideración, se concluye que una posible explicación a los datos observados sería la intrusión de una roca máfica intrusiva (Gabro) en una roca félsica extrusiva preexistente (Riolita).
<li>Pórfido cuprífero:</li>
    Otra posible explicación podría estar asociado a un yacimiento mineralógico tipo Pórfido Cuprífero. En el cual se esperaría obtener altos valores de concentraciones de Cu, S, Fe, W y Sn. De acuerdo a los elementos muestreados cumple con las condiciones ya que se tiene una firma metálica con altos valores de Cu, S y Fe. Además de presentar normalmente una roca encajonante félsica, lo cual también coincidiría de acuerdo a lo mencionado en la explicación anterior (Figura 24). Cabe mencionar que los pórfido cupríferos son depósitos minerales de gran tonelaje y de baja ley, esto explicaría las concentraciones de concentración regular encontradas, pero constante en ambos grupos clasificados.
<li>Depósito tipo skarn:</li>
    En esta explicación, se entiende que corresponde a un depósito mineral constituido por silicatos de Ca, Mg y Fe derivados de un protolito de calizas y dolomitas en las cuales se ha introducido metasomáticamente grandes cantidades de Si, Al, Fe y Mg, los cuales corresponden con las altas concentraciones observadas de estos elementos.
</ol>
No obstante, la posibles explicaciones presentan aspectos no correspondientes a los valores esperados. Por lo que se realiza una elección de modelo por descarte. 
En el caso de la posible explicación 2 (Pórfido de Cobre) se descarta debido a que no corresponde la geometría, en un pórfido de este tipo, el depósito tiene una forma esférica característica, debido a que se observa una tendencia rasgada (Elipse SW - NE) no se cumplen con las caracteríticas del modelo. Por otro lado, debería existir una mayor cantidad de zonas de alteración asocidas, al momento intentar mapear dichas alteraciones con una mayor cantidad de clusters, no se observó una tendencia de cículos clasificados, sino una dispersión irregular de los datos.
También se descarta la opción del skarn debido a que no se observan las altas concentraciones de Ca esperadas (debido a que está conformado dentro de secuencias calcáreas)en la litología esperada, sino que los valores de altas concentraciones de Ca se encuentra presentes en la otra curva. Otro aspecto de descarte, es que la otra litología no corresponde a las concentraciones esperadas, pero sobretodo, debería existir una tendencia más clara de la formación de 3 agrupaciones (cuerpo intrusivo, skarn, roca calcárea), la cual no fue observada. Finalmente, se concluye que la explicación más aproximada es la primera opción.

