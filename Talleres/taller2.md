# Endpoint HTTP
Individualmente haga un endpoint GET HTTP que consiste en:

El cliente hacer HTTP GET al siguiente endpoint 
```
http://<IP>:<PORT>/measurements/4`
```

La respuesta del cliente debe ser
```
{
  "id":4,
  "message":"El cliente ha pedido la medición 4",
  "method":"GET"
}
```

No solo con el 4, sino con cualquier número de medida que el cliente haga. Por ejemplo si el cliente hace GET a 
```
http://<IP>:<PORT>/measurements/9123`
```

La respuesta será
```
{
  "id":9123,
  "message":"El cliente ha pedido la medición 9123",
  "method":"GET"
}
```

> [!NOTE]
> Este endpoint será consumido por el ESP32

> [!IMPORTANT]
> Debe resolver primero este problema para obtener el 50% de la calificación de la actividad


# Cliente ESP32
En grupos de dos, creen un cliente HTTP desde ESP32

El cliente consiste en:
- El ESP32 recibe por puerto serie una serie de comandos
- Si el ESP32 recibe `w` o `W`, el ESP32 se conecta al WiFi
- Si el ESP32 recibe `1`, `2`, `3`, `4` y `5`, el ESP32 hace un GET al endpoint del estudiante A
- Si el ESP32 recibe `6`, `7`, `8`, `9` y `10`, el ESP32 hace un GET al endpoint del estudiante B

> [!IMPORTANT]
> Esta parte equivalete al restante 50%









