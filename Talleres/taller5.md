# Taller 5 · 20% de Implementación

Debe implementar una tablero de medición IoT. 
Cada estudiante: `robledo`, `mayor`, `mosquera`, `barbosa`, `garcia` y `rengifo` tiene una planta a la que se le está midiendo el Voltaje y la Corriente.

Usted verá en pantalla el estado de su sensor, el objetivo es que usted:

1. Encienda el sistema de medición

2. Una vez encendido, el sistema de medición comienza a enviar datos a un topic específico.

3. Debe almacenar las muestras recibidas en el almacen de datos

4. Debe poder apagar el sistema de medición.

[15%]
Envío de comando de activación (ON)

Cree una interfaz HTML tipo tablero donde por medio de un botón `ON` encienda el sistema de medición
Para hacerlo debe enviar el siguiente mensaje
```json
{ "action": "ON", "deviceId": "robledo" }
```
al topic `icesi/sensors/control` usando MQTT.

[15%]
Envío de comando de apagado (OFF)
Use un botón para enviar el comando de OFF:
```json
{ "action": "OFF", "deviceId": "robledo" }
```
Este comando detiene el flujo de datos del estudiante correspondiente.
El cambio de estado debe reflejarse visualmente en el tablero 

[40%]
Visualización de datos en tiempo real
Adicional a los botones de ON y OFF, su página debe tener textos que permitan observar el valor actual del medidor **en tiempo real**.


[30%]
Almacenamiento de datos (POST)

Implemente un algoritmo que permita que en el momento de recibir una medida del broker MQTT, se envíe automáticamente una solicitud HTTP POST que permita almacenar los datos de voltaje y corriente en el almacén.

El JSON que recibe desde el broker es el siguiente
```json
{
  "deviceId": "robledo",
  "voltaje": 121.3,
  "corriente": 6.2,
  "timestamp": "2025-10-30T16:12:00Z"
}
```
El endpoint es
```
POST http://192.168.131.159:8080/examen
```
Header necesario
```
Content-Type: application/json
```


# Parámetros de conexión

La conexión es al broker público

```
host: broker.emqx.io
port: 8083
user: icesidomicianorincon123123
```
Topic de datos
```
icesi/sensors/rengifo
icesi/sensors/barbosa
icesi/sensors/mosquera
icesi/sensors/robledo
icesi/sensors/mayor
icesi/sensors/garcia
```
