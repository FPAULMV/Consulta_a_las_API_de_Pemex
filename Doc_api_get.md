
# Consulta a las API de P-----

## **Propósito:**  
Las API de P----- solo están disponibles para sus comercializadores afiliados. Este repositorio se crea mientras trabajo para uno de estos comercializadores y tiene como objetivo automatizar las consultas a la API para descargar y almacenar la información obtenida.  

## **Contenido:**  
- [Dependencias](#dependencias)
- [Instalación](#Instalación)
- [Descripción de funciones](#descripcion-de-funciones)
- [Configuración](#configuracion)
- [Uso](#uso)
- [Errores comunes y soluciones](#errores-comunes-y-soluciones)
   
### Dependencias  

**Librerías requeridas:** *`python-decouple`*, *`requests`*, *`json`*, *`os`*, *`datetime`*, *`re`*, *`time`*, *`sys`*, *`logging`*.  

Instalar las librerías `requests` y `python-decouple` vía pip:  
```bash
pip install requests python-decouple
```
*`json`*, *`os`*, *`datetime`* (de la cual *`datetime`* y *`timedelta`* son partes), *`re`*, *`time`*, *`sys`*, y *`logging`* son parte de la biblioteca estándar de Python, por lo que no necesitan instalación a través de pip.  

### Instalación 

Puedes clonar este repositorio a través de git: 
```bash
git clone https://github.com/FPAULMV/Consulta_a_las_API_de_P-----_Y_Registro_de_Informacion.
```

### Descripción de funciones  

- ***get_dmy():***  
Devuelve día, mes, y año de una fecha dada.
```python
def get_dmy(fecha_str: str):
```
**Requiere:** una fecha *(como str)* en formato `'ddmmaaaa'`.  
```bash
'01012024'
```
**Retorna:** El día *(como int)*, el nombre del mes en español *(como str)*, el año *(como int)*.
```bash
dia = 1
mes = 'Enero'
año = 2024
```

- ***validar_path():***
Valida si una ruta a un directorio existe. Si no, la crea.
```python
def validar_path(path: str):
```
**Requiere:** Ruta del directorio a validar *(como str)*.
```bash
C:\Ruta\a_validar\de\tu_directorio\
```
**Retorna:** No retorna ningún valor.  

- ***get_version():***
Encuentra el siguiente número de versión para los archivos que siguen el patrón dado dentro de un directorio específico. El propósito de la función es evitar archivos con nombres duplicados agregando un número de versión. 
```python
def get_version(file_base: str, directory: str):
```  
**Requiere:** 
   - **file_base:** *(como str)* El nombre base que llevará el archivo descargado, a este se le agregará `"_V_"` más el número de versión `'1'`.
   - **directory:** *(como str)* El directorio donde se encuentran los archivos.
```bash
file_base = 'Mi_archivo'
directory = 'C:\Usuarios\Mi_Usuario\Documentos\'
```
**Retorna:** La ruta del directorio más el nombre del nuevo archivo con el sufijo de versión.  
```bash
nuevo_nombre = 'C:\Usuarios\Mi_Usuario\Documentos\Mi_archivo_V_1'
```

- ***url_embarques():***
Genera URLs de la API a consultar, agregando el parámetro de búsqueda para la API, que en este caso es una fecha en formato `'ddmmaaaa'` y una lista de fechas en formato `'ddmmaaaa'` dentro de un rango de fechas proporcionado.
```python
def url_embarques(start_date: str =None, end_date: str =None):
```  

**Requiere:**  
   - **start_date:** *(como str, opcional)* Fecha de inicio en formato `'ddmmaaaa'`. Si no se proporciona, se toma el valor de ***`query_days`*** en el archivo `.env`.
   - **end_date:** *(como str, opcional)* Fecha de fin en formato `'ddmmaaaa'`. Si no se proporciona, se toma el valor de la fecha actual.
```bash
start_date = '01012024'
end_date = '03012024'
```

**Retorna:**
   - Lista de URLs a consultar más el parámetro de búsqueda. 
   - Lista de fechas en formato `'ddmmaaaa'`.
```bash
URLs:
(https://api_a_consultar/01012024, https://api_a_consultar/02012024, https://api_a_consultar/03012024)
Fechas:
(01012024, 02012024, 03012024)
```

- ***timer():***
Muestra un temporizador en la consola que cuenta hacia atrás, iniciando desde el número de segundos especificado. Esto sirve para interactuar con el usuario y evitar el error HTTP 429 por *'Demasiadas solicitudes'*. Está predeterminado en 60 segundos. 
```python
def timer(segundos):
```
**Requiere:**
   - **segundos:** *(cualquier valor)* Este valor se proporciona en la variable ***`time_sleep`*** en el archivo `.env`.
```bash
segundos = 5
```
**Retorna:** Cuenta regresiva en consola. 
```bash
Tiempo restante para la siguiente consulta: 5 segundos.
Tiempo restante para la siguiente consulta: 4 segundos.
Tiempo restante para la siguiente consulta: 3 segundos.
Tiempo restante para la siguiente consulta: 2 segundos.
Tiempo restante para la siguiente consulta: 1 segundo.

- ¡Puede volver a consultar!
```

### Configuración  

Configura las variables de entorno para el script con un archivo `.env` ubicado en el directorio raíz donde se ejecuta el código. 

***user:*** Nombre de usuario proporcionado por P----- para autenticarnos en la API.
```bash
user = mi_usuario
```
***passw:*** Contraseña proporcionada por P-----, del usuario con el que nos autenticamos en la API.
```bash
passw = m1Passw0rd!
```
***url:*** URL de la API proporcionada por P-----. 
```bash
url = https://api_a_consultar/
```
***query_days:*** Utilizada en la función *`url_embarques()`*, indica el rango de días para los cuales se realizará una consulta en la API. Predeterminado en *1*
```bash
query_days = 1 
```
***time_sleep:*** Utilizada en la función *`timer()`*, indica el número de segundos para la cuenta regresiva. Predeterminado en *60*.
```bash
time_sleep = 60 
```
***download_path:*** Utilizada en la variable *`path`* de la función *`main()`*. Define la ruta donde se descargará la información de la API y el nombre que va a tener cada archivo según su versión. 
```bash
download_path = C:\Usuarios\Mi_Usuario\Documentos\
```  
***name_api:*** Utilizada en la variable *`path`* de la función *`main()`*. Define, después de la ruta principal, el nombre de la carpeta donde se guardarán los archivos. Esto permite controlar dónde se guardan los documentos de cada API. 
```bash
name_api = mi_api
```
***name_dir:*** Utilizada en la variable *`path`* de la función *`main()`*. Define, el nombre que tendra el documento una vez la informacion sea descargada por la API. 
```bash
name_dir = Mi_archivo
```
***num_try:*** Utilizada en la variable *`max_retries`* de la función *`main()`*. Define el número de intentos que se realizarán a la API en caso de que la solicitud HTTP devuelva un valor diferente de *200*. Puede cambiar el valor dependiendo de sus necesidades. 
```bash
num_try = 1
```

El archivo `.env` resultante se verá así:
```bash
user = mi_usuario
passw = m1Passw0rd!
url = https://api_a_consultar/
query_days = 1
time_sleep = 60
download_path = C:\Usuarios\Mi_Usuario\Documentos\
name_api = mi_api
name_dir = Mi_archivo
num_try = 1
```

### Uso  

Se planea que el uso del scrip, se ejecute en una tarea programada para que consulte automaticamente la consulta a las API segun las nececidades de cada comercializador.   

Aquí se presentan ejemplos de cómo utilizar el scrip disponible en este repositorio. 

#### **1. Configuración del entorno**

Antes de ejecutar el script principal, asegúrate de que el archivo `.env` esté configurado correctamente con las credenciales y parámetros necesarios:

```bash
user = mi_usuario
passw = m1Passw0rd!
url = https://api_a_consultar/
query_days = 1
time_sleep = 60
download_path = C:\Usuarios\Mi_Usuario\Documentos\
name_api = mi_api
name_dir = Mi_archivo
num_try = 1
```

#### **2. Ejecución del script principal**

Para ejecutar el script principal y comenzar a descargar datos desde la API de P-----, usa el siguiente comando en la terminal:

```bash
python main.py
```

El script consultará la API en el rango de fechas especificado, descargará los datos y los almacenará en la carpeta indicada en el archivo `.env`.


#### Errores comunes y soluciones

- **Error 429: Demasiadas solicitudes:**  
  Si encuentras este error, puedes ajustar la variable `time_sleep` en el archivo `.env` para aumentar el tiempo de espera entre solicitudes y evitar este problema.

- **Error de autenticación:**  
  Verifica que las credenciales en el archivo `.env` sean correctas. Si cambian, deberás actualizarlas.






