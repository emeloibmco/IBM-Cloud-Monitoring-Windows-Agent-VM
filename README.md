# IBM-Cloud-Monitoring-Windows-Agent-VM :cloud:
Introducción
Requisitos
Contenido
# [Imagen](#Imagen)
Paso 1. Configuracion Motionpro y ping (P)
Paso 2. remote (P)
Paso 3. prometheus wmi (P)
Paso 4. recopilar métricas opción 2 (D)
-Descargar el .zip
-Crear archivo apikey
-Editar archivo prometheus
-Habilitar metricas de sysdig
Paso 5. Ejecutar y esperar (D)
Paso 6. Visualizar dashboard (D)

# Imagen
<p align="center"><img width="520" src="https://github.com/emeloibmco/IBM-Cloud-Monitoring-Windows-Agent-VM/blob/main/prueba_Trim.gif"></p>



## Paso 4. Recopilar métricas ejecutando Prometheus :hammer:
Para monitorear un sistema Windows con *IBM Cloud Monitoring with Sysdig* se emplea *Prometheus*, que es un sistema de supervisión para enviar y recopilar las métricas desde Windows. A continuación, se presentan los pasos para realizar la recopilación.

1. Descargue el sistema de supervisión [*prometheus-2.24.1.windows-amd64.zip*.](https://prometheus.io/download/) 
2. Descomprima el archivo descargado y guardelo en una carpeta a la que pueda acceder fácilmente. 
3. Cree un archivo llamado *sysdigapikey* en un bloc de notas, agregue el *Sysdig Monitor API Token* (que lo puede encontrar como se observa en la imagen) y guarde el archivo en la misma carpeta del paso 2. Posteriormente abra una ventana en *PowerShell*, ingrese a la carpeta donde se encuentra ubicado el archivo y copie el siguiente comando 
```
.\sysdigapikey.txt sysdigapikey
```
   Con este comando puede eliminar la extensión .txt del archivo creado. 

4. Ingrese a la carpeta del paso 2 y busque el archivo *prometheus.yml*. Abralo en un block de notas sin cambiar el formato y modifique la sección *scrape_configs*, con la siguiente información:
```
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: 'wmi_exporter'

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
    - targets: ['localhost:9182']

      labels:
        region: "us-south"
        instance: "vsi-windows-sd"
        job: "windows"

remote_write:
  - url: "https://ingest.us-south.monitoring.cloud.ibm.com/api/prometheus/write"

    bearer_token_file: C:\Users\Administrator\Downloads\sysdigapikey
```




