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

# Paso 1. Configuración Motion Pro :pushpin:
El acceso VPN de IBM Cloud permite a los usuarios gestionar todos los servidores de forma remota y segura en la red privada de IBM Cloud. Para tener acceso remoto a la VSI de Windows se debe establecer primero la conexion VPN, esta se realiza por medio de **MotionPro**. 
Para añadir una nueva conexion VPN seleccione **Add Profile**. A continuación, ingrese: **Site Name**, **Host**, **Username** y **Password**. Verfique que **mode** este en VPN.

## Paso 4. Recopilar métricas ejecutando Prometehus :hammer:
Para monitorear un sistema Windows con *IBM Cloud Monitoring with Sysdig* se emplea *Prometheus*, que es un sistema de supervisión para enviar y recopilar las métricas desde Windows. A continuación, se presentan los pasos para realizar la recopilación.

1. Descargar el sistema de supervisión [*prometheus-2.24.1.windows-amd64.zip*.](https://prometheus.io/download/)
2. Descomprima el archivo descargado.
3. 

