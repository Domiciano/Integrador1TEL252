# Clase Integrador 3 de Septiembre

En esta clase aprenderemos a trabajar con **PlatformIO**, la comunicación serial, la conexión a WiFi y finalmente a consumir un servicio REST utilizando un `GET Request`.

---

## 1. Instalar PlatformIO

PlatformIO es un ecosistema para desarrollo embebido que se integra muy bien con **Visual Studio Code**.  
Pasos básicos:

1. Ir a las extensiones y buscar **PlatformIO**.
2. Instalar la extensión y reiniciar VSCode.

---

## 2. Crear un primer proyecto en PlatformIO

1. Abrir PlatformIO desde la barra lateral izquierda de VSCode.
2. Seleccionar **New Project**.
3. Configurar:
   - Nombre del proyecto.
   - Placa (ejemplo: ESP32 Dev Module).
   - Framework: **Arduino**.
4. Finalizar y esperar que se cree la estructura de carpetas.

---

## 3. Configurar el `baudrate` en el archivo `platformio.ini`

En el archivo `platformio.ini` agrega:

```ini
monitor_speed = 115200
```

Esto asegura que el monitor serial use la misma velocidad que configuremos en el código.

---

## 4. Usar eventos seriales

Código de ejemplo para probar la comunicación serial con `serialEvent`:

```cpp
#include <Arduino.h>

void setup() {
  Serial.begin(115200);
}

void loop() {
  
}

void serialEvent() {
  if (Serial.available() > 0) {
    String data = Serial.readStringUntil('\n');
    Serial.println(data);
  }
}
```

Prueba escribiendo texto en el monitor serial y verifica que se imprima de vuelta.

---

## 5. Conectándose a una red WiFi

Ejemplo de cómo conectar el ESP32 a una red WiFi:

```cpp
#include <Arduino.h>
#include <WiFi.h>

const char* ssid = "SU_SSID";
const char* password = "SU_CONTRASENA";

void initWiFi() {
  WiFi.begin(ssid, password);
  Serial.print("Connecting to WiFi ..");
  while (WiFi.status() != WL_CONNECTED) {
    Serial.print('.');
    delay(1000);
  }
  Serial.println("Connected!!");
  Serial.println(WiFi.localIP());
}

void setup() {
  Serial.begin(115200);
  WiFi.mode(WIFI_STA);
  initWiFi();
}

void loop() {
  
}
```

---

## 6. Hacer un GET Request sencillo (ejemplo con Fake Store API)

Código de ejemplo para realizar una petición GET:

```cpp
#include <Arduino.h>
#include <WiFi.h>
#include <HTTPClient.h>

const char* ssid = "PUBLICA";
const char* password = "";

String url = "https://fakestoreapi.com/products";

void initWiFi() {
  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, password);
  Serial.print("Connecting to WiFi ..");
  while (WiFi.status() != WL_CONNECTED) {
    Serial.print('.');
    delay(1000);
  }
  Serial.println("Connected!!");
  Serial.println(WiFi.localIP());
}

void setup() {
  Serial.begin(115200);
  Serial.println("Inicializando");
  initWiFi();
  
  HTTPClient http;
  http.begin(url.c_str());
  int httpResponseCode = http.GET();

  if (httpResponseCode > 0) {
    Serial.print("HTTP Response code: ");
    Serial.println(httpResponseCode);
    String payload = http.getString();
    Serial.println(payload);
  } else {
    Serial.print("Error code: ");
    Serial.println(httpResponseCode);
  }
  http.end();
}

void loop() {
  
}
```
