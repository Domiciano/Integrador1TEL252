# Endpoint HTTP
Individualmente haga un endpoint GET HTTP que consiste en:

El cliente hacer HTTP GET al siguiente endpoint 
```http
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
```http
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

> [!IMPORTANT]
> Debe resolver primero este problema para obtener el 50% de la calificación de la actividad
