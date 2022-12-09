# Scraping de una web - AD3
## Lina Vargas Vega 
### Máster periodismo digital y de datos 

Esta actividad consistió en hacer web scraping de **El País**. El también denominado rascado web, es una técnica que permite extraer información de sitios de internet. En el caso puntual de esta actividad vamos a extraer titulares de este medio de comunicación, este será un pequeño paso a paso de este proceso. 

## Importar librerías 


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

### Explicación de las librerías 

**Módulos o librerías propias de Python**

[time](https://docs.python.org/es/3/library/time.html): Este módulo proporciona varias funciones relacionadas con el tiempo. 

[csv](https://docs.python.org/es/3/library/csv.html?highlight=csv:) El módulo ```csv``` implementa clases para leer y escribir datos tabulares en formato CSV (Valores Separados por Comas). Es uno de los formtos más populares para la organización de datos.

[re](https://docs.python.org/es/3/library/re.html?highlight=re#module-re) Este módulo proporciona operaciones de coincidencia de expresiones regulares similares a las encontradas en Perl.

[os](https://docs.python.org/es/3/library/os.html?highlight=#module-os): Este módulo provee una manera versátil de usar funcionalidades dependientes del sistema operativo.

**Módulos o librerías externas a Python**

[bs4](tps://pypi.org/project/beautifulsoup4/):Beautiful Soup es una biblioteca que facilita extraer información de páginas web. Se basa en un analizador HTML o XML.

[pandas](https://pandas.pydata.org/): pandas es una herramienta de análisis y manipulación de datos de código abierto rápida, flexible y fácil de usar. Está construida sobre el lenguaje de programación Python.

[termcolor](https://pypi.org/project/termcolor/): Formato de color ANSI (Instituto Nacional de Normalización Estadounidense por sus siglas en inglés) para salida en terminal.

[requests](https://pypi.org/project/requests/) Solicitudes le permite enviar solicitudes HTTP/1.1 con mucha facilidad. No es necesario agregar manualmente cadenas de consulta a sus URL, o codificar sus datos PUT & POST.

## Variables 
Vamos a crear una variable tipo lista ([]) para organizar los resultados de nuestra búsqueda. En este caso la llamaremos 'resultados'


```python
resultados = []
```

## ¿De dónde vendrán los resultados?
Ahora, es momento de decirle a **jupyter** de dónde obtendrá los resultados que buscamos. Esto lo hacemos con la librería requests, que ya importamos, y [get](https://www.w3schools.com/python/ref_dictionary_get.asp), que devuelve el valor del elemento con la clave especificada, en este caso la clave es la página de resultados de **El País**. A este llamado o acción la vamos a denominar req de ahora en adelante.. Además le diremos que traiga solo los resultados con código 200 que son los  satisfactorios. Así como 404 es una página no encontrada, 200 es una página con resultados satisfactorios.

```python
req = requests.get("https://resultados.elpais.com")
# Si el estatus code no es 200 no se puede leer la página
if (req.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
```

## BeautifulSoup

Según [Wikipedia](https://es.wikipedia.org/wiki/Beautiful_Soup),Beautiful Soup es una librería de Python para analizar documentos HTML (incluyendo los que tienen un marcado incorrecto -por eso se especifíca que solo envíe resultados "200"-). Esta biblioteca crea un árbol con todos los elementos del documento y puede ser utilizado para extraer información. Por lo tanto, esta biblioteca es útil para realizar web scraping — extraer información de sitios web. Con la orden 'findAll("h2"), se le está pidiendo que encuentre todos los h2, es decir los titulares de **El País**

```python
soup = BeautifulSoup(req.text, 'html.parser')

tags = soup.findAll("h2")
```

Con la siguiente indicación le diremos que por cada h2 que salga en los tags nos devuelva un texto, es decir el titular. Al ejecutar esta acción ya tendremos el resultado final.


```python
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
```

    El Mundial, en vivo
    Universo Mundial
    El calendario a seguir desde América
    ¿Quién va a ganar? Simulador y pronóstico de cada selección 
    La ‘newsletter’
    Nicolás Maduro condiciona la celebración de elecciones libres al levantamiento de sanciones
    La prohibición del maíz transgénico abre un nuevo foco de tensión entre Estados Unidos y México
    Superviviente de una mara hondureña: “No me enteré de que a mi hija la habían violado a punta de pistola hasta que salimos del país”
    Video | ¿Qué hay en la cabeza de un soldado ucranio? Estas son sus heridas mentales
    El historiador Serhii Plokhy: “El destino de la guerra está claro: Rusia se debilita y Ucrania será independiente”
    España se ve lanzada  
    ¡Viva mi desgracia!
    Sí, se puede ser feliz por una clasificación a octavos de final
    Y una tarde, los argentinos recuperaron la alegría
    Un barrio popular, un partido de México a pie de calle y la rutina de la derrota 
    Elon Musk asegura que en seis meses se implantará el primer chip en un cerebro humano
    Video | Crea un lector y tendrás un comprador
    Cecilia Vicuña: “Siempre fui catalogada de ridícula porque sabía que había que plantar bosques”
    Daniela Tarazona: “No soy una escritora ejemplar. Me he empeñado en desobedecer”
    El desafío de envejecer con VIH: “Las personas de largo recorrido sufrimos depresión, ansiedad y deterioro cognitivo”
    Pacientes de VIH contra Guatemala: “Si fuera por el Estado, estaríamos muertos”
    La deforestación de la Amazonia cae un 11% en el último balance anual de la era Bolsonaro
    Arranca la legislatura en Quebec sin los diputados que rehusaron prestar juramento al rey Carlos III
    ‘As bestas’, de Rodrigo Sorogoyen, lidera la carrera a los Premios Goya 2023 con 17 nominaciones
    Cómo detectar un ataque de ansiedad y qué hacer frente a él
    Hallan en Transilvania una nueva especie de dinosaurio enano herbívoro que vivió hace 70 millones de años
    A Juan Luis Guerra se le sube la bilirrubina
    Ideas de regalos para el jardinero
    Guía de regalos para el ‘foodie’
    La FIL, fobias y filias
    Estamos jodidos
    Carta de socorro
    Las píldoras de Enzensberger
    La historia de una niña que soñó con ser pianista
    Pon nombre a lo que te pasa para sentirte mejor
    Volver a empezar
    Alemania suaviza su ley de inmigración para atraer trabajadores extranjeros de fuera de la Unión Europea
    Hungría evita la confrontación con la Comisión Europea y confía en desbloquear los fondos en 2023
    Al menos 30 desaparecidos tras un deslizamiento de tierra en el sur de Brasil  
    El número de españoles en cárceles extranjeras repunta un 15% tras el fin de la pandemia
    Quince periodistas e integrantes de ‘El Faro’ demandan al fabricante del software espía Pegasus 
    Las islas del Caribe colombiano que desaparecen por el cambio climático
    ‘Que sepan que sabemos’: traducen los megaproyectos de López Obrador a lenguas indígenas
    América Latina puede convertirse en un referente mundial de la transición energética justa
    ‘Mujeres de armas tomar’: la historia de la víctima, que desesperada por justicia, se vengó de sus violadores
    La respuesta a la guerra en Ucrania también es literaria
    Felix Klieser: un virtuoso de la trompa (sin brazos)
    De piezas aztecas a teotihuacanas: México busca detener una subasta de 85 objetos arqueológicos en Francia
    Una investigación saca a la luz 170 escritos inéditos del poeta español Miguel Hernández
    ¿Selfie sí o selfie no en Art Basel?
    De las galerías a las calles: así se hizo importante Miami para el arte del mundo
    Spotify Wrapped, el selfi musical que monopoliza la conversación en Twitter y acaricia el ego
    Al norte del Norte: por la remota Ruta de la Costa Ártica de Islandia 
    Series de diciembre de 2022: ‘Alice in Borderland’, ‘Fácil’ y otros estrenos
    Aló Comidista: “Si dejas la piña boca abajo, ¿se pone más dulce?”
    La tragedia en la frontera de Melilla: el papel de Marruecos y España en las muertes del 24-J
    Un tatuaje y un bebé en el nombre de Mircea Cărtărescu
    ‘Argentina, 1985’, un debate nacional entre la ficción y la memoria
    Madrid, la nueva Miami: así se han hecho con la capital española los ricos latinoamericanos
    Qatar 2022, el único Mundial donde es posible ver dos partidos en un mismo día
    ¿Qué estrella tuvo el mejor debut en el Mundial? El ‘tier list’ de Jesús Gallego, Bruno Alemany y Aritz Gabilondo
    Enredados en el Mundial | Los jugadores iraníes vuelven a cantar el himno y ha nacido una estrella en EE UU
    Venezuela tiene escasez de sacerdotes
    Puerto Rico lucha por mantener sus playas públicas
    Tres claves para entender las protestas por el censo en Bolivia
    Breast cancer expert: ‘Public health is decided at the polls’
    The unfortunate case of Anne Hathaway, or why we hate some female celebrities for no reason
    Why does finding a partner seem impossible these days?
    Ukraine’s farm of horrors: 2,000 dead cows and fields of anti-tank mines 
    Americanas: Reportajes y noticias sobre feminismo e historias con enfoque de género de la región
    Toda la actualidad científica en el boletín de Materia
    Letras Americanas: la actualidad literaria de un continente vista por el escritor Emiliano Monge
    Ideas: reportajes y entrevistas para entender el mundo
    El Kremlin silencia el dolor de las mujeres rusas que critican la guerra de Ucrania 
    El primer ministro de Rumania: “Necesitamos un esfuerzo común en la OTAN para defender cada centímetro de territorio frente a Rusia”  
    La justicia investiga si el Gobierno de Macron favoreció a consultoras privadas en las dos últimas campañas presidenciales en Francia
    Aquí puede buscar si ha sido engañado por la estafa internacional de las criptomonedas
    El secreto de la pirámide
    El futuro de las compras: más redes sociales, pago a plazos y vuelta de la tienda física
     El Papa detecta fallos en Caritas Internationalis y cesa a sus responsables 
    La Iglesia italiana da por primera vez cifras de los abusos cometidos por el clero
    El Senado de EE UU da un paso clave para blindar el matrimonio homosexual frente al Supremo
    Muere Hans Magnus Enzensberger, gran intelectual alemán del siglo XX
    ‘Close’, una mirada conmovedora a la fragilidad de la preadolescencia
    La gran retrospectiva de Vermeer en Ámsterdam aviva la polémica sobre la autoría de ‘Muchacha con flauta’
    75 años de la muerte de G.H. Hardy, un matemático excéntrico y brillante
    Hallan en Transilvania una nueva especie de dinosaurio enano herbívoro que vivió hace 70 millones de años
    Un fármaco experimental contra el alzhéimer confirma efectos positivos, pero puede estar detrás de la muerte de dos pacientes 
    Más dunas y más arena: el Dakar 2023 aumenta la complejidad del recorrido
    Jugar para tu país
    ‘Mi tío Ricardo y la antena de la tele’, por Rafa Cabeleira
    La prohibición del maíz transgénico abre un nuevo foco de tensión entre EE UU y México
    Un plan de choque de 356 millones para intentar salvar Doñana de su creciente declive 
    Portugal, el secreto de un país sin bicicletas que se convirtió en el mayor fabricante europeo
    Ronna, ronto, quetta y quecto, los nuevos prefijos para magnitudes extraordinarias
    La Policía de San Francisco contará con robots con capacidad de matar
    Damian Burns, director de Twitch Europa: “No me importaría que mi hija fuera ‘streamer’”
    El tiempo en el puente de diciembre: primero frío y, después, lluvias generalizadas
    Defensa invertirá 80 millones en su futuro Sistema de Combate en el Ciberespacio
    Xavier Trias ficha a Alsina, Calvet y Tremosa para la candidatura de Junts al Ayuntamiento de Barcelona
    Borja Cobeaga: “Tardé más en sacarme el carné que en hacer esta serie”
    ‘El presidente: juego de la corrupción’, los entresijos del fútbol’, por Ángel Sánchez-Harguindey
    Radiografía de ‘El hormiguero’: entretenimiento blanco de éxito… y polémicas por machismo
    Windsor desvela su decoración navideña en las primeras festividades sin Isabel II y con Carlos III como rey
    Camila y los 1.000 osos Paddington: la reina entrega a una ONG infantil los peluches en homenaje a Isabel II
    Stallone vs. Schwarzenegger, la “competición” eterna: “Éramos antagonistas. Soñaba con pegarle y él con pegarme a mí”
    Muchos hombres, una sola mujer
    El misterio de Manuel Carrasco: cómo salir de una barriada de pescadores y acabar siendo un cantante que llena estadios
    La polarización es como las drogas: engancha
    Descansar está mal visto: el arte perdido del reposo
    Madrid, la nueva Miami: así se han hecho con la capital los ricos latinoamericanos
    Si no tiene ahorros, esta es una de las pocas alternativas para comprar casa
    Roe Ethridge, el fructífero intercambio entre el arte y el comercio
    ‘La última vez’, de Guillermo Martínez: un laberinto con final milagrosamente imprevisible
    La economía solidaria: un nuevo modelo financiero contra la desigualdad y la emergencia climática
    Alima tiene bronquiolitis, pero no hay oxígeno para evitar su asfixia en el hospital de Etiopía
    Oyambre, Son Bou y otras nueve playas españolas para un baño de historia
    Cómo actuar ante el caos de los aeropuertos: qué hacer frente a retrasos y cancelaciones de un vuelo
    El lamentable caso de Anne Hathaway o por qué se odia a algunas famosas sin justificación ninguna
    Samantha Hudson: “Me va bien económicamante, pero no soy Paula Echevarría”
    “Tobogán de piojos”, “genocida de oxígeno”... Por qué cada Mundial recuerda el poderío del español como idioma para insultar
    Alejandro Jato, el actor que será Camilo Sesto: “Da pena la imagen que los jóvenes tienen de él, con lo grande que fue”
    Lentejas con gambas
    ¿Son saludables todas las conservas de verduras y legumbres?
    Hildalgo pide más dinero por Air Europa y retrasa la venta
    El emotivo mensaje de un padre a los médicos que salvaron a su hijo: “Me han dado la vida”
    Oskar Matute explica qué piensa cuando le dicen que es de ETA y habla así de claro sobre la banda
    João ya tiene precio: 100 millones
    ¿Sirve para algo el boicot personal e intransferible al Mundial de Qatar?
    Confirmados los juegos gratis de PS Plus en diciembre de 2022 para PS5 y PS4
    

## Esto lo podemos replicar en diferentes secciones de El País 

## Internacional 


```python
internacional = requests.get("https://resultados.elpais.com/internacional")
# Si el estatus code no es 200 no se puede leer la página
if (internacional.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup = BeautifulSoup(internacional.text, 'html.parser')

tags = soup.findAll("h2")
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
```

    Rusia advierte del “enorme” riesgo de una guerra nuclear por el conflicto en Ucrania
    El historiador Serhii Plokhy: “El destino de la guerra ya está claro: Ucrania será independiente y Rusia quedará tremendamente debilitada”
    Hungría evita la confrontación con Bruselas y confía en desbloquear los fondos europeos en 2023 
    Una globalización centrada en los seres humanos
    Autocracia en crisis
    “¿De qué color es el hambre, mamá?”
    China: una protesta nacional
    Publicar no es un delito
    Bruselas promueve un tribunal especial para juzgar a la cúpula de Putin por sus crímenes en Ucrania
    Bruselas exige congelar fondos europeos a Hungría por las violaciones del Estado de derecho
    Hungría acelera las medidas anticorrupción para rebajar la magnitud del castigo europeo
    China relaja las medidas anticovid en Guangzhou para frenar nuevas protestas
    Nicolás Maduro condiciona la celebración de elecciones libres al levantamiento de sanciones 
    Muere el expresidente de China Jiang Zemin
    La doble respuesta de China a las protestas: acelerar la vacunación de los mayores y descabezar las manifestaciones 
    La selección del jurado popular marca el inicio del juicio por los atentados yihadistas de 2016 en Bruselas
    La OTAN redobla su alerta sobre una China en ebullición
    La tragedia de los menores en zona de guerra: 230 millones de niños viven en los conflictos más cruentos del mundo
    Macron viaja a Washington para “resincronizarse” con Biden ante el impacto de la guerra en Ucrania
    Quince periodistas e integrantes de ‘El Faro’ demandan al fabricante del software espía Pegasus 
    La hija de Hugo Chávez califica de “grotesco” un video oficialista en homenaje a su padre
    Nueva York internará en psiquiátricos contra su voluntad a las personas sin hogar con trastornos mentales graves
    El líder del grupo ultraderechista Oath Keepers, culpable de sedición por el asalto al Capitolio
    Estados Unidos prepara una ley para evitar una huelga de ferrocarriles en vísperas de Navidad
    

## Economía



```python
economia = requests.get("https://resultados.elpais.com/economia")
# Si el estatus code no es 200 no se puede leer la página
if (economia.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup = BeautifulSoup(economia.text, 'html.parser')

tags = soup.findAll("h2")
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
```

    La pensión máxima subirá un 3% hasta 2050 y la cotización más alta, un 35%
    El euríbor cierra noviembre en su mayor nivel desde 2008: la cuota de los que revisen ahora su pago crecerá un 44%
    Cepsa invertirá 3.000 millones en el mayor proyecto de hidrógeno verde de Europa
    Transportes reforzará en 2023 las inspecciones para garantizar que los camioneros no trabajen a pérdidas
    Los desafíos al proceso de transición ecológica
    La moda de la sostenibilidad
    Impuesto erróneo a la banca
    Sufrimientos evitables
    El coste de la vida
    La caída de la producción frena la actividad industrial española
    La OCDE pide facilitar el despliegue de los planes de pensiones para garantizar la sostenibilidad del sistema
    Cosecha de ingresos en las constructoras y concesionarias: facturan 6.000 millones más hasta septiembre
    Arabia Saudí encarga otros cinco buques de guerra a Navantia 
    Los ingresos tributarios hasta octubre ya superan lo recaudado en todo 2021
    Prisa presenta el plan para redoblar su apuesta por la sostenibilidad
    Estos picos han enamorado al chef José Andrés y están hasta en las mesas de Corea del Sur
    El BBVA gana a Merlin el arbitraje sobre la Operación Chamartín
    Breton exige a Musk que refuerce su política de moderación de contenidos y desinformación en Twitter
    Una celestina solar para instalar paneles
    España, en el podio de países de la OCDE con mayor alza de la presión fiscal en una década
    Madrid, la nueva Miami: así se han hecho con la capital los ricos latinoamericanos
    Si no tiene ahorros, esta es una de las pocas alternativas para comprar casa
    Los efectos de una economía de andar por casa
    Caballos al galope: un negocio con un impacto económico de 7.400 millones de euros
    Los impuestos que se necesitan para construir la sociedad del futuro
    Hildalgo pide más dinero por Air Europa y retrasa la venta
    Órdago de los agentes sociales a Escrivá: sin acuerdo político no negocian el cálculo de la pensión
    Telefónica estudiará nuevas medidas de ahorro ante el escenario inflacionario
    Barceló irrumpe en la venta de Hotelatelier con una oferta de 200 millones de euros
    Por primera vez un juez declara nulo un contrato de alquiler por aplicación de la perspectiva de género
    Visitas a centros de acogida y muchas horas de estudio: así se forman los jueces en materia de género
    La justicia obliga a Iberia a controlar el peso de las maletas para que los azafatos no se lesionen
    ‘Phishing’, ‘malware’ o ‘ransomware’: el reto de formar en ciberseguridad tras la pandemia
    La difícil tarea de recuperar el talento investigador que un día hizo las maletas
    Descubre las formaciones de marketing ‘online’ más buscadas de 2023
    Logística de ida y vuelta, cuando la solución está en reciclar menos y reutilizar más
    Logística y digitalización, el manual de supervivencia para pymes
    Empleados satisfechos, empresas de éxito
    Diez claves del bienestar corporativo para empresas de éxito
    Las aventuras de un par de calcetines que dan empleo a todo un pueblo
    Cultura financiera como punto de partida para volver a empezar
    Cómo aplicar el ‘design thinking’ a cualquier negocio
    Las soluciones digitales que necesita mi negocio 
    Alexia Putellas, un Balón de Oro a base de “esfuerzo, constancia, disciplina y saber reinventarse”
    El método Muñoz para triunfar en cada reto que se propone
    ¿Por qué muchos inversores están volviendo a confiar en Japón?
    ¿Por qué se está enfriando la economía china?
    No una, sino cinco ‘startups’ para la esperanza
    Si tú lo imaginas, yo te lo imprimo
    Cómo garantizar la seguridad en un mundo de amenazas híbridas
    ¿Cuáles son los dilemas éticos del uso de la inteligencia artificial?
    El propósito empresarial, una seña de identidad
    Las ciudades españolas ganan en salud respecto al año pasado
    Alimentación, empresa y deporte se unen en el pódium de la competitividad
    Cómo conseguir ayudas a la digitalización para autónomos y pymes con Kit Digital
    Un brindis con vino español por la sostenibilidad, la economía y el empleo
    

## Deportes 


```python
deportes = requests.get("https://resultados.elpais.com/deportes")
# Si el estatus code no es 200 no se puede leer la página
if (deportes.status_code != 200):
 raise Exception("No se puede hacer Web Scraping en"+ URL)
soup = BeautifulSoup(deportes.text, 'html.parser')

tags = soup.findAll("h2")
for h2 in tags:
    print(h2.text)
    resultados.append(h2.text)
```

    Última hora
    El calendario
    Vídeo en directo
    Las predicciones
    La ‘newsletter’
    España se ve lanzada  
    Morata encuentra su paz
    Japón y el dios del pie buscan al mejor Kamada
    Sí, se puede ser feliz por una clasificación a octavos de final
    Mi tío Ricardo y la antena de la tele
    Jugar para tu país
    El mejor Messi juega, no golea 
    Croacia - Bélgica: Mundial de Qatar 2022, en directo | La subcampeona del mundo se mide con la selección belga en un partido muy igualado con los octavos en juego
    Canadá - Marruecos: Mundial de Qatar 2022, en directo | Los canadienses recortan distancias antes del descanso gracias a un gol en propia puerta
    Joshua Kimmich bajo vigilancia
    Mundial de Qatar 2022, últimas noticias en directo | Dani Alves: “Poder cerrar el círculo jugando una Copa del Mundo me hace muy feliz”
    Argentina se gana los octavos 
    Más dunas y más arena: el Dakar 2023 aumenta la complejidad del recorrido
    “Si Lewandowski fuera argentino, igual mete cinco”
    México se queda a un gol del pase 
    Tata Martino lleva a México a su gran fracaso en el Mundial
    ¡Viva mi desgracia!
    Kevin de Bruyne, entre la frustración y el miedo ante el día decisivo de Bélgica contra Croacia
    Olga Viza: “Llevé la copa del Mundial de España 82 de copiloto en mi coche con la L en el cristal”
    Túnez desnuda a los suplentes de Francia
    Última hora del Mundial de Qatar: vídeo en directo | Qatar en Juego
    ¿Volverá Benzema al Mundial de Qatar tras su lesión?
    ¿Morata o Asensio? dos cromos muy diferentes para la misma posición
    La moneda que cambió para siempre el destino de España en los Mundiales
    Cómo no perderte ni un solo minuto del Mundial
    Las promesas del baloncesto español piden consejo a Sergio Scariolo
    A toda mecha hacia el mundial de la consagración
    Cómo disfrutar de la montaña con todas las garantías
    Y una tarde, los argentinos recuperaron la alegría
    Un Mundial de ida y vuelta con Caparrós y Villoro
    ¡Viva mi desgracia!
    México se queda a un gol del pase 
    Tata Martino lleva a México a su gran fracaso en el Mundial
    El mejor Messi juega, no golea 
    Los vídeos de los goles y el resumen de la jornada 11 del Mundial de Qatar 2022
    Argentina se gana los octavos 
    
