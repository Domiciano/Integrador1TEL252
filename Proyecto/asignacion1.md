# Flujo de datos de sensor
Usted y su equipo deben realizar el montaje del sensor de modo que por medio del ESP32 puedan extraer datos de movimiento.

## Conexión de sensores [20%]
Realiza el montaje de sensores de momento en protoboard 

## Algoritmo de muestreo [20%]
Cree un algoritmo que permita muestrear la señal de acuerdo a lo exigido por el proyecto

## Base de datos [20%]
La muestra debe quedar almacenada en la base de datos y adicionalmente debe quedar asignada a un paciente. Por lo tanto considere las entidades `Patient`, `Sample`, `RawData` de modo que un paciente puede tener muchas muestras y que cada muestra tenga muchos datos.

Para la demo, puede utilizar un paciente ya creado en la base de datos de modo que todas las muestras que se tomen sean para ese paciente

## Backend [20%]
Cree un endpoint de tipo POST que le permita recibir la muestra de 2 segundos y almacenarla en Postgres

## Post request desde ESP32 [20%]
Finalmente con todo el escenario envíe la muestra y almacénela en la base de datos

## Sustentación [100%] Factor multiplicativo 
Esta entrega deberá ser sustentada para la evaluación individual del proyecto.

## Fecha de entrega
2 de octubre de 2025

## Referencias útiles
Puede usar este documento para hacer la conexión y uso de su sensor

https://randomnerdtutorials.com/esp32-mpu-6050-accelerometer-gyroscope-arduino/
