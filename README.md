# Mantenimiento-Flota-Vehicular

# 	CASO DEL NEGOCIO
Una empresa de transportes de pasajeros tiene un parque automotor de buses en los cuales hay vehículos flota propia y vehículos de afiliados donde el mantenimiento esta a cargo toda un área de mantenimiento en cabeza de un ingeniero mecánico, se cuenta con un patio taller con mecánicos y un almacén.
El registro de los mantenimientos, repuestos y manos de obra realizados los vehículos se hace mediante un ERP en la nube (cloudfleet)

## Objetivo General
Llevar a cabo una solución de Big Data a través de Databricks aplicando la arquitectura medallion para procesar toda la información diaria que llega y se genera referente al mantenimiento de los vehiculos para finalmente facilitar la toma de decisiones mediante Dashboards con KPis para la gerencial y para el área técnica para el seguimiento del negocio.

## Objetivos Especificos
• Aplicar la Analítica e inteligencia de negocios (BI) a los procesos del area de mantenimiento vehicular.

• Combinar data semiestructurada de históricos de mantenimientos realizados que reposan en archivos de Excel con la data que proviene de la nube.

• Automatizar mediante pipeline la ingesta diaria de datos para actualizar los Dashboards.

# 	RELACION BENEFICIO/ COSTO
Tener la visualización de la información casi que en tiempo real nos Permite:

• Almacén: Revisar salida y entrada de repuestos, ver el comportamiento de la facturacion y hacer mejor control a los proveedores.

• Taller: Visualizar y llevar el control de la facturación de la mano de obra por mecánico, poder comparar los costos con los históricos cuando se tercerizaba los trabajos vs con los Mecánicos Propios.

• Flota: Hacer seguimiento al gasto de los vehículos, por sistemas, por cantidad de ingresos a taller y numero de horas, comparar los mantenimientos correctivos vs los preventivos.

 Todo esto anteriormente con un Software contable y el ERP de mantenimiento no se podía hacer, lo que en la actualidad representa información valiosa para hacer seguimiento al negocio y tomar decisiones en el momento oportuno.

# PIPELINE DE INGESTA DE DATOS
Se perfiló cada Excel antes de cargar (hojas, esquemas, conteos, rangos de fechas, estados de proceso), descubriendo que 452 filas carecían de número de orden (todas con valor 0) y que el histórico no desglosa mano de obra ni repuestos.

Carga bronze desde Excel** con `read_files(format => 'excel', ...)`, normalizando los nombres de columna a snake_case.

Ingesta del API de CloudFleet.** El token se almacena en Databricks Secrets (nunca en texto plano); la descarga es paginada por ventanas de máximo 6 meses y respeta el límite de 30 peticiones/minuto.

![03_ingesta_cloudfleet](03_ingesta_cloudfleet.png)

Cada orden se normaliza al esquema unificado y se persiste con MERGE por clave (`CF-<número de orden>`); las órdenes anuladas se eliminan para no contaminar los KPIs

![04_merge_voided](04_merge_voided.png)



