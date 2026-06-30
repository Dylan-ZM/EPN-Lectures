//Contexto
Se desea realizar un registro climatológico de la ciudad de
Quito. Para esto, escriba un script de Python/Java que permita obtener datos climatológicos desde el API de
[[https://openweathermap.org/current#one][openweathermap]]. Considere la latitud y la longitud de Quito como
(-0.2299, -78.5249), respectivamente. Los resultados obtenidos de
la consulta al API se escriben en un archivo
/clima-quito-hoy.csv/. Cada ejecución del script debe almacenar
nuevos datos en el archivo. Investigue sobre *crontab* y utilícelo
para obtener datos del API de /openweathermap/ cada 5 minutos
mediante la ejecución de un archivo ejecutable denominado
/get-weather.sh/. Verifique los resultados. Todas las operaciones
se realizan en Linux o en el WSL
//Preguntas
Como seria un script en Phyton o en Java, dentro de Emacs, que realice una consulta a una API del clima en internet, escribiendo los resultados de dicha consulta en un archivo csv

Aquí tienes un **ejemplo en Python** sencillo, ideal para ejecutarlo desde Emacs o cualquier terminal. Este script consulta la API de OpenWeatherMap y almacena los datos en un archivo `clima-quito-hoy.csv`, añadiendo una nueva línea en cada ejecución.

**Requisitos previos:**
- Solicita una API key gratis en https://openweathermap.org/api
- Instala la librería requests si no la tienes:  
  `pip install requests`

```python
import requests
import csv
import os
from datetime import datetime

API_KEY = 'TU_API_KEY_AQUI'  # Reemplaza con tu propia API Key
LAT = -0.2299
LON = -78.5249
URL = f"https://api.openweathermap.org/data/2.5/weather?lat={LAT}&lon={LON}&appid={API_KEY}&units=metric"

CSV_FILE = "clima-quito-hoy.csv"

def obtener_clima():
    resp = requests.get(URL)
    data = resp.json()
    temp = data['main']['temp']
    humedad = data['main']['humidity']
    desc = data['weather'][0]['description']
    timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    return [timestamp, temp, humedad, desc]

def guardar_en_csv(info):
    file_exists = os.path.isfile(CSV_FILE)
    with open(CSV_FILE, mode='a', newline='') as f:
        writer = csv.writer(f)
        if not file_exists:
            writer.writerow(['fecha_hora','temperatura_C','humedad_%','descripcion'])
        writer.writerow(info)

if __name__ == "__main__":
    datos = obtener_clima()
    guardar_en_csv(datos)
    print("Datos de clima guardados.")
```

**Cómo ejecutarlo desde Emacs:**
1. Guarda el archivo, por ejemplo como `get_weather.py`
2. Ejecuta con:  
   `M-x shell`  
   `python get_weather.py`

**¿Quieres ejemplo en Java?**

Pide la versión Java si prefieres.

Como se crearia un ejecutable get-weather.sh para ejecutar el programa en Phyton

Claro, aquí tienes un ejemplo sencillo de un archivo ejecutable `get-weather.sh` para ejecutar tu script de Python:

```bash
#!/bin/bash
python3 /ruta/completa/a/get_weather.py
```

**Pasos:**

1. **Crea el archivo:**  
   Abre Emacs, crea el archivo llamado `get-weather.sh` y pega el contenido de arriba. Cambia `/ruta/completa/a/get_weather.py` por la ruta real donde guardaste el script de Python.
2. **Hazlo ejecutable:**  
   En una terminal, navega hasta donde guardaste el archivo y ejecuta:
   ```bash
   chmod +x get-weather.sh
   ```

Ahora puedes ejecutar el script con:
```bash
./get-weather.sh
```

¡Listo! Este archivo ejecutará tu programa de Python cada vez que lo lances.

Y si esta dentro de un bloque de ejecucion?

Si lo ejecutas en un bloque de ejecución (por ejemplo, en **org-mode** con org-babel), puedes llamar el script `get-weather.sh` así:

```org
#+begin_src sh
./get-weather.sh
#+end_src
```

O si quieres ejecutar el script Python directamente desde un bloque:

```org
#+begin_src python
exec(open("/ruta/completa/a/get_weather.py").read())
#+end_src
```

Ambas formas funcionan; la primera usa el archivo `sh`, la segunda ejecuta Python directamente desde org-babel.



Y como se configuraria el crontab para adquirir los datos, que comando lo configura?

Para ejecutar tu script cada 5 minutos usando **crontab**, haz lo siguiente:

1. **Abre el editor de crontab** (en terminal):
   ```sh
   crontab -e
   ```

2. **Agrega esta línea** al final del archivo (ajusta la ruta):
   ```
   */5 * * * * /ruta/completa/a/get-weather.sh
   ```
   Esto ejecuta `get-weather.sh` cada 5 minutos.

3. **Guarda y cierra** el archivo.  
   ¡Listo! El cron ejecutará tu script automáticamente.

**Resumen del comando:**
- `crontab -e`: Edita el cron del usuario.
- `*/5 * * * *`: Cada 5 minutos.
- `/ruta/completa/a/get-weather.sh`: Ruta absoluta al ejecutable.

**Tip:** Asegúrate que el script es ejecutable (`chmod +x get-weather.sh`).