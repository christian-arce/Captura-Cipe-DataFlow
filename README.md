# Instalación y Configuración de Apache Airflow para Integración CAPTURA - CIPE

## Requisitos Previos

Antes de comenzar, asegúrate de tener instalados los siguientes programas:

- Python (versión 3.6 o superior)
- `pip` (gestor de paquetes de Python)
- `virtualenv` (puede instalarse con `apt-get install virtualenv` si no lo tienes)
- Tener levantado Captura y CIPE

## Pasos para la Instalación de Airflow y Configuraciones para la Sincronización de Datos
### Crear la Carpeta "airflow", una Variable desde la Interfaz Web de Airflow

1. Desde la línea de comandos, crea la carpeta "airflow". Por ejemplo:/home/tu-user/airflow

```
mkdir /home/tu-user/airflow/dags
```

### Crear y Activar un Entorno Virtual
- Crear un nuevo entorno virtual llamado "airflow_env"
```
virtualenv airflow_env
```

### Activar el entorno virtual
```
source airflow_env/bin/activate
```
### Clonar el siguiente repositorio dentro de la carpeta "airflow"
```
git clone https://github.com/Christian-Arce/Captura-Cipe-DataFlow.git
```
### Renombrar la carpeta clonada "Captura-Cipe-Dataflow" a "dags"
```
mv Captura-Cipe-Dataflow dags
```
### Definir la variable de entorno AIRFLOW_HOME
```
export AIRFLOW_HOME=/home/tu-user/airflow
```
### Instalar Apache Airflow
```
pip install apache-airflow
```
### Instalar Dotenv 
```
pip install python-dotenv
```
### Inicializar la base de datos de Airflow
```
airflow db init
```
### Crear un usuario administrador para poder acceder a la interfaz web de Airflow
```
airflow users create \
    --username admin \
    --firstname Nombre \
    --lastname Apellido \
    --role Admin \
    --email admin@example.com \
    --password tu_contraseña
```
### Iniciar el servidor web de Airflow
```
airflow webserver -p 9090
```
### Verificar /home/tu-user/airflow/airflow.cfg
Dentro de airflow.cfg, verificar directorio correcto para dags folder y modificar load_examples a False
```
dags_folder = /home/tu-user/airflow/dags
```
```
load_examples = False
```
  
### Dentro del entorno virtual, activar el scheduler
```
source airflow_env/bin/activate
```
```
airflow scheduler
```
### Crear formulario en Captura
Campos necesarios para formulario de prueba de bache:
- Descripción (caja de texto)
- Tipo de ruta (desplegable)
- Ubicación (location)

### Personalizar captura_data.env de acuerdo a sus datos
- Obtener las ids, elements correctos y versiones deseadas desde la API de captura
```
CAPTURA_LOGIN=http://localhost:8080/mf/api/authentication/login
CAPTURA_LIST=http://localhost:8080/mf/api/document/list
FORM_ID=5
VERSION=1
ROW_ID=0
ROWS=100
CAPTURA_USER=chake@feltesq.com
CAPTURA_PASSWORD=123456
DESCRIPTION = element15
TYPE_OF_ROAD = element14
ALTITUDE = element13
```
### Personalizar cipe_data.env de acuerdo a sus datos
```
CIPE_POST=http://localhost:8000/api/scientist/
CIPE_TOKEN=http://localhost:8000/api/get-user-token/
CIPE_USER=superuser
CIPE_PASSWORD=superuser
```
- observación: superuser se crea al levantar cipe y modificar en cipe el .env.dev
- Se puede usar otro usuario
```
DJANGO_SUPERUSER_PASSWORD=superuser
```
```
DJANGO_SUPERUSER_USERNAME=superuser
```

### Crear Variable "processes_ids" desde la Interfaz Web de Airflow

1. Ve a la interfaz web de Airflow.

2. En el menú de navegación izquierdo, selecciona "Admin".

3. Selecciona "Variables" en el menú desplegable.

4. Haz clic en el botón "+".

5. Llena los campos requeridos:

   - **Key (Clave)**: Ingresa el nombre de la variable, "processed_ids".

   - **Value (Valor)**: Ingresa el valor inicial,`[]` (una lista vacía).

6. Haz clic en el botón "Save".

### Ejecutar la tarea

En el apartado dags de la interfaz del airflow aparecera la tarea "airflow-captura-to-cipe" y se podrá ejecutar
- Para revisar los outputs de test-airflow.py, hacer click en el número que aparece luego de ejecutar la tarea
- Dentro se pueden revisar los gráficos y consultar los logs de las diferentes tareas
