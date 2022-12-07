# Trabajo final - Periodismo de Datos II 
## Lina Vargas Vega 
### Metodología 

Este trabajo final consta de la compilación de cuatro actividades que involucran lenguajes de Markdown y Python desde un nivel básico hasta uno avanzado. Repasamos una a una las actividades para conocer cómo fue su proceso. 

El trabajo final, así como las cuatro actividades anteriores, están depositados en GitHub, una plataforma que permite usar lenguajes informáticoas como Markdown

## [AD1](ad1.md)
Pese a que, en retrospectiva, esta fue la actividad más sencilla, en su momento fue desafiante. No conocía Markdown y no había escuchado de GitHb, enfrentarme por primera vez a un documento .md fue confuso al principio. La actividad en sí era sencilla: escribir entre 300 y 500 palabras analizando  una pieza de periodismo de datos. Realicé un escrito común y corriente en un archivo de word que después copié y pegué en el [pad](https://pad.riseup.net/p/nebrija-2223-keep) sin ningún tratamiento.

Con el paso de las clases y el aprendizaje del curso fui modificando el archivo poco a poco para que su escritura tuviera más sentido con el formato: añadí el encabezamiento de primer nivel, inserté imagenes y las etiqueté correctamente y fui capaz de hacer un pequeño análisis de los gráficos presentes en el informe [El empleo reflota en octubre: el paro baja en 27.000 personas, la mayor caída en este mes de la historia](https://elpais.com/economia/2022-10-04/el-mercado-laboral-se-enfria-en-septiembre-el-paro-sube-en-18000-personas-con-el-fin-de-los-contratos-de-verano.html) de **El País**, a través de la lectura del código fuente de la página. 

Esta fue una de las actividades, a lo largo de este máster, en las que más pulsé la función de 'Inspeccionar' en una página web. Fue todo un reto aprender a identificar correctamente qué contenedor alojaba el gráfico, lo que al principio creí que era un div resultó siendo un iframe. Aprendí también que si veo el famoso !DOCTYPE en medio del documento y no en la parte inicial como es usual se debe a que hay un elemento embebido. Incluso perfeccioné la técnica de darle click en 'Inspeccionar' justo en el lugar de donde queremos extraer la información. 

Gracias a esta actividad logré familiarizarme más con este lenguaje informático que hace unas pocas semanas no conocía. Si bien estoy consciente de que hay muchos códigos y funciones más que desconozco, ya me siento con la fluidez de producir un documento directamente acá y no pegándolo desde un editor de textos más tradicional como word. 


## [AD2](ad2.md)

Para la realización de la AD2 ya tenía un poco más de confianza con el editor de GitHub, pude resolver algunos detalles técnicos como las imágenes y los encabezados muy rápido. Esta vez el reto fue el análisis de la información desde el inspector del sitio web. En esta ocasión elegí escribir el análisis de [Más variados, originales y cortos: así se han popularizado 1.200 nombres en 25 años](https://www.eldiario.es/nidos/variados-originales-cortos-han-popularizado-1-200-nombres-25-anos_1_9207661.html) de **elDiario.es.**, una pieza periodística con bastantes gráficos que me llamaron la atención desde la primera vez que los vi. 

Algunos de los gráficos en este reportaje son más complejos que otros. Me llamó especialmente la atención un estilo de gráfico compuesto de más de 30 histogramas pequeños, pues no recuerdo haber visto algo similar antes. Fue interesante ir al link original del gráfico -siempre ubicado en el inspector o algunas veces en el pie de foto de la gráfica- pues en el sitio web original, la data se deja visualizar mejor y alguien -como yo- que está aprendiendo del mundo de la visualización de datos puede tener más ideas de organización de la información, herramientas y  tipos de gráficos

![Infografía nombres comunes](https://user-images.githubusercontent.com/118140811/205188928-0f9da747-90b2-4766-babc-f7d07c43da97.png)


## [AD3](ad3.md)

Acá ya entramos un poco más en materia. Para iniciar la AD3 fue necesario descargar **Anaconda**, un software de procesamiento de datos en diferentes lenguajes de programación. Una vez se instaló [correctamente](https://docs.anaconda.com/anaconda/install/windows/) la plataforma, pudimos acceder desde allí a **Jupyter**, una aplicación web que permite generar cuadernos de Python y Markdown. Otro nuevo reto: aprender a manejar Jupyter, sin embargo con el previo conocimiento de **GitHub** y un repaso por el lenguaje de *Python*, fue más sencilla la familiarización. 

En esta primera actividad en Jupyter hicimos un scraping de una web. El resultado mejor posicionado de este término -[Wikipedia](https://es.wikipedia.org/wiki/Web_scraping)- define esta actividad como "una técnica utilizada mediante programas de software para extraer información de sitios web". Muy útil para la investigación de un tema en concreto pues funciona a través de los tags con los que se identifican las noticias. Para este ejercicio en particular hicimos scraping o rascado de **El Páis**

### Primer paso 

Para trabajar el rascado de datos necesitaremos algunas librerías que nos ayudarán a obtener, organizar y diferenciar la información que requerimos. La explicación de cada librería está disponible en la [AD3](ad3.md). 

```python
import requests
import time
import csv
import re
from bs4 import BeautifulSoup
import os
import pandas as pd
from termcolor import colored
```
### Segundo Paso 

Es fundamental crear una variable tipo lista ([]), que eventualmente organizará los titulares que encontremos en nuestra búsqueda. Para esta actividad esa variable se denominó "resultados". 

```python
resultados = []
```
### Tercer paso 

Lo que haremos a continuación será decirle a **jupyter** dónde buscar la data que solicitamos. Con [get](https://www.w3schools.com/python/ref_dictionary_get.asp), que devuelve el valor del elemento con la clave especificada: donde los elementos serán los titulares y la clave será [la página de resultados de **EL País**]("https://resultados.elpais.com"). Además pediremos que solo se obtengan resultados satisfactorios (200).

```python
req = requests.get("https://resultados.elpais.com")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
```

### Cuarto paso 

Entra BeautifulSoup al juego. Esta es la librería que hace la magia en el Web Scraping. Además con la orden 'findAll("h2"), se le está pidiendo que encuentre todos los titulares de **El País**

```python
soup = BeautifulSoup(req.text, 'html.parser')

tags = soup.findAll("h2")
```

### Quinto paso 

Este es el paso final. Daremos la instrucción de que por cada h2 nos devuelva el texto, al ejecutar la acción tendremos la lista de resultados. Resultados disponibles en la [AD3](ad3.md). 


```python
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
```
Se repitió el mismo proceso con la seeción de 'Internacional', 'Economía' y 'Deportes'


## [AD4](ad4.md)

Desde mi punto de vista esta fue la actividad más interesante, también la más desafiante, pero la de mejores resultados. Para llevar a cabo este ejercicio usamos una API o una Interfaz de "Programación de Aplicaciones", en este caso la [API del Covid](https://covid19api.com/). Este trabajo lo hicimos a través de la librería Pandas.

### Primer paso 

Teniendo en cuenta que Pandas es la herramienta que nos ayudará a ejecutar este ejercicio, lo primero es importarla. Ya que estamos trabajando en Anaconda, podemos usar !conda para invocarla, si estamos desde otra plataforma, lo correcto será usar !pip. 

```python
!pip install pandas
```

    Requirement already satisfied: pandas in c:\users\usuario\anaconda3\lib\site-packages (1.4.4)
    Requirement already satisfied: python-dateutil>=2.8.1 in c:\users\usuario\anaconda3\lib\site-packages (from pandas) (2.8.2)
    Requirement already satisfied: pytz>=2020.1 in c:\users\usuario\anaconda3\lib\site-packages (from pandas) (2022.1)
    Requirement already satisfied: numpy>=1.18.5 in c:\users\usuario\anaconda3\lib\site-packages (from pandas) (1.21.5)
    Requirement already satisfied: six>=1.5 in c:\users\usuario\anaconda3\lib\site-packages (from python-dateutil>=2.8.1->pandas) (1.16.0)

### Segundo paso 

Ahora es necesario configurar Pandas, la vamos a importar como pd, para poder usarla nombrando esta abreviatura de ahora en adelante. 

```python
import pandas as pd 
```
También es importante decirle con qué datos vamos a trabajar. Vamos a crear una variable que vamos a denominar miurl, esta variable tendrá el valor del link donde están alojados los datos de la [API Covid 19](https://api.covid19api.com/countries)

```python
miurl = "https://api.covid19api.com/countries"
```

### Tercer paso 

Ya tenemos la herramienta que nos ayudará a ejecutar la actividad y los datos que vamos a trabajar, ahora necesitamos el espacio donde vamos a alojar la información: un dataframe (df) que es literalmente un marco de datos para encerrar, organizar y eventualmente ilustrar nuestra información. Le indicaremos a ese dataframe que debe leer json, que es el lenguaje en el que están los datos que vamos a usar.

```python
df = pd.read_json (miurl)
```

### Cuarto paso 

Tecnicamente este paso no suma mucho al resultado final, pero es indispensable y no debe ser pasado por alto. Una vez tenemos los datos alojados en un dataframe podemos jugar con ellos, conocerlos, explorarlos. Esto será importante para saber que no hay ningún dato fuera del grupo correspondiente o "dañado", también nos permite conocer nueva información de la data y saber qué hay y con qué podemos trabajar. Se pueden conocer los primeros datos, los últimos o alguno en concreto. Para conocer cómo se puede explorar esta información, visitar la [AD4](ad4.md).

### Quinto paso 


## Conclusiones 
