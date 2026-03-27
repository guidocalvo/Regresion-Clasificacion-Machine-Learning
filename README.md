# Modelos de regresión y clasificación de Machine Learning

## 📖 Descripción del Proyecto

Este proyecto se basa en datos sobre el rendimiento académico de 1000 alumnos.

Se aplicarán dos modelos:

- Modelo de regresión para obtener la ‘nota_final’ (variable continua entre 0 y 100).
- Modelo de clasificación para obtener el ‘aprobado’ (variable binaria: 1 si la nota es ≥ 60, 0 en caso contrario).


## 📂 Descripción de los Datos

Los datos se encuentran en una tabla de 1000 filas y 11 columnas.

Sus columnas son:

-	`nivel_dificultad`: Nivel de dificultad del alumno para estudiar.
-	`nota_anterior`: Nota del examen anterior.
-	`tasa_asistencia`: Tasa de asistencia a clase en porcentaje.
-	`edad`: Edad del alumno.
-	`horario_estudio_semanal`: Promedio de horas de estudio por semana.
-	`horas_sueno`: Promedio de horas de sueño por día.
-	`tiene_tutor`: Indica si el alumno cuenta con tutor.
-	`horario_estudio_preferido`: Horario preferido de estudio.
-	`estilo_aprendizaje`: Forma de estudio que emplea el alumno.
-	`nota_final`: Nota de examen final.
-	`aprobado`: Indicador binario (1/0): 1 si el alumno está aprobado, 0 en caso contrario.


## 🗂️ Estructura del Proyecto

├── data/ # Archivos de datos originales y procesados

├── notebooks/ # Jupyter Notebooks del proyecto

├── modelos/ # Modelos de regresión y clasificación

└── README.md # Descripción del proyecto


## 🔍 EDA preliminar

Aquí se realiza un análisis exploratorio de los datos:

-	Identificación las variables numéricas y categóricas.
-	Estadísticas descriptivas.
-	Gráficos de histogramas para variables numéricas.
-	Gráficos de barras para variables categóricas.
-	Matriz de correlación.

También se realizan gráficos de relaciones cruzadas basados en las variables objetivo ‘nota_final’ y ‘aprobado’.


## 🔄 Preproceso de datos

### Tratamiento de nulos

Para las columnas categóricas: 'horario_estudio_preferido' y 'estilo_aprendizaje', se utiliza el valor genérico "Unknown" para reemplazar los valores nulos.

Para la columna numérica 'horas_sueno' se reemplazan los nulos por la mediana.

Esta estrategia de imputación preserva la integridad del conjunto de datos mientras proporciona un tratamiento apropiado según el tipo de variable, evitando la eliminación de filas con información parcialmente completa.

### Codificación

Se utilizan los mismos métodos de codificación en ambos modelos reemplazando la variable objetivo:

-	OneHot-encoding a 'tiene_tutor' ya que es una columna binaria.
-	Target-encoding a 'horario_estudio_preferido' y 'estilo_aprendizaje' ya que cuentan con más de dos categorías.
-	Label-encoding a ‘nivel_dificultad’ así se mantiene la escala de las dificultades.

Luego, utilizamos el método de escalado ‘MinMaxScaler’.


## 📉 Regresión

Se realiza un modelo de regresión para obtener la ‘nota_final’ (variable continua entre 0 y 100). Para esto, se elimina la columna ‘aprobado’ por ser una variable dependiente de la nota final que no aporta información adicional y puede introducir ruido. 

El siguiente paso es la división de datos (ya codificados y escalados):

-	X: Todas las columnas excepto 'nota_final'.
-	y: Únicamente la columna 'nota_final'.

Luego, se separan las filas de las dos tablas, el 80% será para el entrenamiento y el resto para el testeo del modelo. 

-	Tabla 1 – X_train: Serán las primeras 800 filas de la tabla X, se utilizarán para el entrenamiento del modelo.
-	Tabla 2 – y_train: Serán los 800 primeros valores de la tabla Y, se utilizarán como respuestas para entrenar al modelo.
-	Tabla 3 – X_test: Serán las 200 filas restantes de la tabla X, se utilizarán para realizar el testeo.
-	Tabla 4 – y_test: Serán los 200 valores restantes de la tabla Y, se utilizarán para compararlos con los valores que nos devuelve el modelo.

Se realiza el entrenamiento y las validaciones.

### Validación inicial

Gráficos de dispersión entre valores reales y predichos.

<img width="479" height="388" alt="image" src="https://github.com/user-attachments/assets/7e5d7543-d81c-44b1-ae09-e24d499aa847" />     


Comparación de distribuciones (histogramas/KDE).

<img width="612" height="332" alt="image" src="https://github.com/user-attachments/assets/3866677f-f73e-4bb3-b6dd-0d82e771b031" />

Análisis de residuos (residuos vs. predicciones y QQ-plot).

<img width="535" height="424" alt="image" src="https://github.com/user-attachments/assets/bcfe479c-1a23-4c2f-b4d2-af1f650010fc" />

Exploración de la importancia de cada característica (coeficientes).
 
<img width="723" height="410" alt="image" src="https://github.com/user-attachments/assets/314f2553-acf9-494e-91a6-1c479a6e7cea" />

Se calcula R², MAE y RMSE tanto en entrenamiento como en prueba, para evaluar la calidad del ajuste y detectar posible sobreajuste.

### Entrenamiento final y optimización

Una vez validadas las métricas, reentrenamos el modelo sobre el conjunto completo para su uso en producción.

Exploramos técnicas de regularización (Ridge, Lasso), ajuste de hiperparámetros (GridSearchCV/RandomizedSearchCV) y posibles transformaciones o ingeniería de características para mejorar la generalización.


## 📊 Clasificación

Se separan los datos y se crean las cuatro tablas de igual manera que para la regresión, pero utilizando como variable objetivo la columna ‘aprobado’.

Luego, se realiza el entrenamiento y la validación del modelo.

<img width="507" height="455" alt="image" src="https://github.com/user-attachments/assets/42bff4ab-1673-470a-87b1-24b560ea8208" />


Verdaderos negativos (TN = 8): 8 muestras negativas fueron detectadas correctamente.

Falsos positivos (FP = 7): 7 muestras negativas fueron clasificadas como positivas.

Falsos negativos (FN = 0): no hay ejemplos reales positivos clasificados como negativos.

Verdaderos positivos (TP = 185): las 185 muestras positivas se detectaron correctamente.

### Importancia de las características
Se valora la importancia de las características, tomando en cuenta:

-	Extraer coeficientes del modelo y tomar su valor absoluto.
-	Ordenar variables según magnitud del coeficiente.
-	Visualizar un diagrama de barras con la importancia de cada variable.

<img width="739" height="411" alt="image" src="https://github.com/user-attachments/assets/1a5466e6-20f1-4241-8588-0b9120ac8a13" />

Como último paso, se realiza el entrenamiento final con el 100% de los datos y la optimización final del modelo.


## 🤝 Contribuciones

Las contribuciones son bienvenidas. Puedes sugerir nuevas consultas, correcciones o mejoras estructurales.


## ✍️ Autor

Guido Julián Calvo Sio
