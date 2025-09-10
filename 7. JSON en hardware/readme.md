# JSON en ESP32
Vamos a instalar la librería <a href="https://github.com/arduino-libraries/Arduino_JSON">Arduino_JSON</a>

En `PlatformIO` debemos ir a la sección de Libraries, buscar `Arduino_JSON` y la agregamos a nuestro proyecto

# Codificando en JSON
Puede armar un objeto simple usando el siguiente código
```cpp
#include <Arduino_JSON.h>

...

JSONVar object;
object["key1"] = "value1";
object["key2"] = "value2";
object["key2"] = "value3";
String jsonString = JSON.stringify(object);
Serial.println(jsonString);
```
O si requiere un arreglo lo puede hacer así
```cpp
#include <Arduino_JSON.h>

...

JSONVar jsonArray;
jsonArray[0] = 20;
jsonArray[1] = 214;
jsonArray[2] = 317;
jsonArray[3] = 498;
...
```
Incluso puede hacer objetos compuestos que tengan arreglos y objetos
```cpp
#include <Arduino_JSON.h>

...

JSONVar object;
object["key1"] = "value1";

JSONVar jsonArray;
jsonArray[0] = 142;
jsonArray[1] = 45;
jsonArray[2] = 1020;

object["key2"] = jsonArray;
```

# POST Request
Regularmente necesitaremos enviar estos objetos via POST a un endpoint de recepción de datos. Puede usar el método post del HTTPClient


```cpp
HTTPClient http;
int httpResponseCode = http.POST("{\"api_key\":\"tPmAT5Ab3j7F9\",\"sensor\":\"BME280\",\"value1\":\"24.25\",\"value2\":\"49.54\",\"value3\":\"1005.14\"}");
```
