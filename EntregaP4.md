# Parte de gitlab
Jose Javier Garcia Torrejon y
Samuel Fernandez Peinado 
## Paso 1: Preparación del Repositorio
1. Crea un nuevo repositorio en GitLab para este proyecto.
2. Lo he nombrado Vss

## Paso 2: Datos Científicos
1. Subir el conjunto de datos a su repositorio GitLab y agregar una breve descripción en un archivo `README.md`.
![readme](https://github.com/josejavier1059/grafica/assets/72498118/31a99760-246f-4623-8728-ecd5e2867557)

## Paso 3: Transformación de Datos
1. Escribe un script en Python (u otro lenguaje) para realizar alguna gráfica simple de los datos.
```python
import pandas as pd
import matplotlib.pyplot as plt

# Aqui se prodice la carga de los datos
data = pd.read_csv('Vss/SensorData.csv')

# Estas son las columnas que considero significativas
columna_x = 'pm1ugSPS30'
columna_y = 'pm25ugSPS30'

# Creamos la gráfica
plt.figure(figsize=(10, 6))
plt.plot(data[columna_x], data[columna_y], marker='o')
plt.title('Relación entre ' + columna_x + ' y ' + columna_y)
plt.xlabel(columna_x)
plt.ylabel(columna_y)
plt.grid(True)
```

## Paso 4: Configuración del Pipeline CI/CD en GitLab
1. Configurar un pipeline en GitLab CI/CD que realice las siguientes tareas:
   - Clone el repositorio desde GitLab.  SI
   - Ejecute el script de transformación de datos.  SI
   - Genere gráficos a partir de los datos transformados (usando Python u otro lenguaje).  SI
   - Suba los gráficos resultantes a un servicio de alojamiento de datos abierto como (Figshare). NO
   - Genere documentación a partir de un archivo Markdown (`README.md`) en el repositorio. SI
   - Incruste las gráficas en la documentación. SI
   - Genere un artículo en formato HTML a partir del `README.md`. SI
  
2. Este es el pipeline:

```
stages:
  - process

process_job:
  image: python:3.9 
  stage: process
  script:
    - apt-get update -y && apt-get install -y python3 python3-pip pandoc
    - pip3 install pandas matplotlib 
    - echo "Ejecutando el script para transformar datos"
    - python3 script_analisis.py
    - echo "Generando documentación"
    - cp README.md documentacion.md
    - echo "Grafica resultante" >> documentacion.md
    - echo "![](grafica_sensor_data.png)" >> documentacion.md
    - pandoc documentacion.md -o output.html
  artifacts:
    paths:
      - grafica_sensor_data.png
      - output.html
      - documentacion.md

```
Instalo pandoc primeramente pues lo voy a usar para convertir los .md a html, pip3 install pandas matplotlib es para la generacion de graficas, ejecuto el script el cual me generará el png, acto seguido para no tocar el readme lo copio creando la documentacion a la cual despues le añadimos la gráfica resultante.Seleccionamos los artifacts que serán los archivos de descargaremos.
![Captura](https://github.com/josejavier1059/grafica/assets/72498118/c32c316a-3fc5-4187-a210-b4cf39476c19)

![22](https://github.com/josejavier1059/grafica/assets/72498118/4639954c-e9f0-49f2-8879-73f9d906c9b1)

3. Eso nos dará como resultado los artifacts que usaremos en el siguiente punto
![arifa](https://github.com/josejavier1059/grafica/assets/72498118/12038599-9fa3-4d1f-bdcc-de30958dfc29)

4. Como por ejemplo el documentos y el html, donde podemos ver que las gráfica se cargan en estos:
   ![grffff](https://github.com/josejavier1059/grafica/assets/72498118/4d021320-a9c5-4fca-beac-9632e128f1ce)

  ![aire](https://github.com/josejavier1059/grafica/assets/72498118/839ff202-3c74-40ce-af99-0babfd2a8b71)


## Paso 5: Despliegue en GitHub Pages
1. Creamos un proyecto en github y en la rama main introducimos los elementos resultantes del artifact, el html y el documentos.md, es importante que el html se le cambie el nombre a index.html
2. Nos vamos a settings y activamos el github pages
![pages](https://github.com/josejavier1059/grafica/assets/72498118/5af07626-2d2f-45db-8c83-70151c51fd6d)
3.Creamos este archivo en este sistema de directorios y lo rellenamos con el pipeline
![yma](https://github.com/josejavier1059/grafica/assets/72498118/0c86396c-b077-42a5-b0ba-468a34667776)
```
Evento de Activación (on): push: Se activa al hacer push
branches: - main: Específicamente, un push al main
uses: actions/checkout@v2: Este paso utiliza la acción checkout en su versión 2, el cual permite al flujo de trabajo acceder al contenido de mi repositorio en GitHub.
uses: JamesIves/github-pages-deploy-action@4.1.3: Aquí se utiliza la acción de despliegue de GitHub Pages de James Ives, versión 4.1.3. Esta acción automatiza el proceso de despliegue de archivos a GitHub Pages.
with:Especifica los parámetros de entrada para la acción.
branch: gh-pages: Indica que los archivos se desplegarán en la rama gh-pages.
folder: .: Todo el contenido de la carpeta actual (raíz del repositorio) se desplegará.
token: ${{ secrets.GITHUB_TOKEN }}: Utiliza un token de GitHub generado automáticamente para autenticación.
```
4.Resultados:
![build](https://github.com/josejavier1059/grafica/assets/72498118/7eb2046f-f49f-432b-81e2-f95c1f93e9f4)

![report](https://github.com/josejavier1059/grafica/assets/72498118/611703b8-3390-4b01-a0b1-3fbbba001e8b)

![deploy](https://github.com/josejavier1059/grafica/assets/72498118/24be326b-f811-41d8-a97f-2be59b72a86b)

![htm](https://github.com/josejavier1059/grafica/assets/72498118/f36b2da4-b5eb-4776-8545-5ff64971bf8c)


