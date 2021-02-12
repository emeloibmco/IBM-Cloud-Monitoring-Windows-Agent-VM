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



## Paso 4. Recopilar métricas ejecutando Prometehus :hammer:
Para monitorear un sistema Windows con *IBM Cloud Monitoring with Sysdig* se emplea *Prometheus*, que es un sistema de supervisión para enviar y recopilar las métricas desde Windows. A continuación, se presentan los pasos para realizar la recopilación.

### 1. Descargar el sistema de supervisión *Prometheus*
