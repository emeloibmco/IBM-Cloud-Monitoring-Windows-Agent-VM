# IBM-Cloud-Monitoring-Windows-Agent-VM :cloud: :computer:
IBM Cloud Monitoring with Sysdig es un sistema que permite obtener una visión operativa ofreciendo funciones de supervisión, solución de problemas, definición de alertas y diseñar paneles personalizados. Esta guía esta enfocada a supervisar un entorno Windows con IBM Cloud Monitoring with Sysdig, se debe tener en cuenta que el agente sysdig no se puede instalar en una plataforma Windows, por ello se debe instalar la herramienta llamada Prometheus WMI Exporter para obtener las métricas del sistema Windows.

## Índice  :bookmark_tabs:
1. [Pre-Requisitos](#Pre-Requisitos-pencil).
2. [Paso 1. Verificación de conectividad](#Paso-1).
3. [Paso 2. Ingreso a interfaz gráfica](#Paso-2).
4. [Paso 3. Instalación de herramienta Prometheus y selección de colectores](#Paso-3)
5. [Paso 4. Recopilación de métricas ejecutando Prometheus](#Paso-4)
6. [Paso 5. Ejecución de Prometheus](#Paso-5)
7. [Paso 6. Visualización de métricas en Sysdig](#Paso-6)

## Pre-Requisitos :pencil:
+ MotionPro en el ordenador
+ Microsoft Remote Desktop en el ordenador
+ Contar con una cuenta en IBM Cloud
+ Crear la instancia de monitoring with sysdig en IBM Cloud.
+ Contar con los permisos para acceder a la API KEY del agente de Sysdig.
## Paso 1
### Verificación de conectividad :white_check_mark:
El acceso VPN de IBM Cloud permite a los usuarios gestionar todos los servidores de forma remota y segura en la red privada de IBM Cloud. Para tener acceso remoto a la VSI de Windows se debe establecer primero la conexion VPN, esta se realiza por medio de **MotionPro**. 
Para añadir una nueva conexion VPN seleccione **Add profile**. A continuación, ingrese: **site name**, **host**, **username** y **password**. Verfique que **mode** este en VPN. <br>
<p align="center"><img width="600" src="https://github.com/emeloibmco/IBM-Cloud-Monitoring-Windows-Agent-VM/blob/main/windowssysdig/Paso1.gif"></p>

Nota: Para consultar el **host** según la región consulte <a href="https://cloud.ibm.com/docs/iaas-vpn?topic=iaas-vpn-available-vpn-endpoints">Puntos finales de VPN disponibles</a>. El **username** y **password** se pueden consultar en la plataforma de IBM Cloud Catalogo > Acceso (IAM) > Usuarios > Detalles de Usuario > Contraseña de VPN . <br>
Una vez termine la configuración, podrá conectar la VPN. Para confirmar que la conexión es correcta puede realizar PING a la IP de la VSI.
<p align="center"><img width="300" src="https://github.com/emeloibmco/IBM-Cloud-Monitoring-Windows-Agent-VM/blob/main/windowssysdig/ping.JPG"></p>
La VPN debe permanecer conectada durante el proceso.

## Paso 2
### Ingreso a interfaz gráfica :globe_with_meridians:
Para acceder a la interfaz gráfica de la VSI Linux se puede emplear la herramienta **Remote Desktop**, y en Add > PCs agregar el acceso a la VSI mediante la IP privada de la máquina.<br>
Una vez se cuenta con el acceso, se debe ingresar el 

<p align="center"><img width="600" src="https://github.com/emeloibmco/IBM-Cloud-Monitoring-Windows-Agent-VM/blob/main/windowssysdig/Paso2.gif"></p>
## Paso 3
### Instalación de herramienta Prometheus y selección de colectores  :paperclip:




## Paso 4
### Recopilación de métricas ejecutando Prometheus :hammer:
Para monitorear un sistema Windows con *IBM Cloud Monitoring with Sysdig* se emplea *Prometheus*, que es un sistema de supervisión para enviar y recopilar las métricas desde Windows. A continuación, se presentan los pasos para realizar la recopilación.

1. Descargue el sistema de supervisión [*prometheus-2.24.1.windows-amd64.zip*.](https://prometheus.io/download/) 
2. Descomprima el archivo descargado y guardelo en una carpeta a la que pueda acceder fácilmente (por ejemplo la carpeta de Descargas).
3. Cree un archivo llamado *sysdigapikey* en un bloc de notas, agregue el *Sysdig Monitor API Token* (que lo puede encontrar como se observa en la imagen) y guarde el archivo en la misma carpeta del paso 2. Posteriormente abra una ventana en *PowerShell*, ingrese a la carpeta donde se encuentra ubicado el archivo y copie el siguiente comando: 
```
.\sysdigapikey.txt sysdigapikey
```
   Con este comando puede copiar el mismo archivo sin ninguna extensión determinada. 
   
   <p align="center"><img width="600" src="https://github.com/emeloibmco/IBM-Cloud-Monitoring-Windows-Agent-VM/blob/main/windowssysdig/apitoken.gif"></p>
   

4. Ingrese a la carpeta del paso 2, acceda a la carpeta de *Prometheus* y busque el archivo *prometheus.yml*. Abralo en un bloc de notas sin cambiar el formato y modifique la sección *scrape_configs*, de la siguiente manera:
```
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: 'wmi_exporter'

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
    - targets: ['localhost:9182']

      labels:
        region: "<REGION>"
        instance: "<HOSTNAME>"
        job: "<JOBNAME>"

remote_write:
  - url: "ENDPOINT/api/prometheus/write"

    bearer_token_file: C:\Users\Administrator\Downloads\sysdigapikey
```
Donde,
- REGION es la región. Un ejemplo puede ser *us_south.*
- HOSTNAME es el nombre del sistema Windows.
- JOBNAME es un atributo personalizado que identifica el rol del nodo cuyos datos está recopilando.
- ENDPOINT es el punto final del recopilador de Sysdig y se determina según la región. Para ver la lista de puntos finales consulte [Regiones y Puntos Finales](https://cloud.ibm.com/docs/Monitoring-with-Sysdig?topic=Monitoring-with-Sysdig-endpoints#endpoints_ingestion).

>**_NOTA_**: Como puede observar, en este caso en la linea de comando *bearer_token_file:* el archivo *sysdigapikey* se encuentra ubicado en la carpeta de descargas. Coloque la ubicación del archivo en base a su carpeta. Por otro lado, tenga cuidado con la identación en el archivo *prometheus.yml*, de lo contrario puede presentar problemas durante la ejecución de *Prometheus*.

5. Habilite las métricas de Sysdig en la plataforma [IBM Cloud](https://cloud.ibm.com/login). Para ello, siga los pasos que se presentan a continuación:
- Inicie sesión.
- Haga click en el menú de navegación o "menú de hamburguesa" y elija la opción *Observabilidad*.
- Haga click en la pestaña *Supervisión*.
- Haga click en el botón *Configurar métricas de la plataforma*, seleccione la instancia de la región y la opción *IBM Cloud Monitoring with Sysdig*, tal y como se muestra en la imagen.

<p align="center"><img width="520" src="https://github.com/emeloibmco/IBM-Cloud-Monitoring-Windows-Agent-VM/blob/main/windowssysdig/metricas.gif"></p>


## Paso 5
### Ejecución de Prometheus :heavy_check_mark:
Para ejecutar *Prometheus*, abra una ventana de *PowerShell* y acceda a la carpeta en donde se encuentran los archivos. Posteriormente ejecute el comando:
```
.\prometheus.exe 
```
>**_NOTA 1_**: Es importante que los colectores del **Paso 3**, se estén ejecutando. 

>**_NOTA 2_**: Una vez ejecute *Prometheus*, no cierre la ventana la de *PowerShell*.

## Paso 6
### Visualización de métricas de Windows en Sysdig :chart_with_upwards_trend:
Para visualizar las métricas de Sysdig en la plataforma [IBM Cloud](https://cloud.ibm.com/login), ingrese a *Observabilidad* y posteriormente a *Supervisión*. Dirijase a la opción *visualizar panel de control* y de click en *Visualizar Sysdig*. Un vez se encuentre en la ventana del agente Sysdig, seleccione la pestaña *Dashboards* y en el ícono de búsqueda :mag_right: escriba *Windows Node Overview*. Si realizó todos los pasos de manera correcta podrá visualizar resultados similares a los que se muestran en la siguiente imagen.

<p align="center"><img width="520" src="https://github.com/emeloibmco/IBM-Cloud-Monitoring-Windows-Agent-VM/blob/main/windowssysdig/'Windows%20Node%20Overview%20-%20Dashboards%20-%20Sysdig'%20.jpg"></p>

>**_NOTA_**: En caso de no poder visualizar la opción de *Windows Node Overview* de forma inmediada a la ejecución de *Prometheus*, por favor tenga paciencia. La visualización de las métricas puede tardar varios minutos.





 
 



