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
Almacén: revisar salida y entrada de repuestos, ver el comportamiento de la facturacion y hacer mejor control a los proveedores.
Taller:

# PIPELINE DE INGESTA DE DATOS
Se realiza una API mediante el genie para que extraiga la información de la nube cloudfleet y tener una data actualizada todos los días

![03_ingesta_cloudfleet](03_ingesta_cloudfleet.png)
Cada orden se normaliza al esquema unificado (desglose totalCostLabors/totalCostParts → precio_mano_obra/precio_repuestos) y se carga con MERGE idempotente; las órdenes anuladas se eliminan:

