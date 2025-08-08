# Springboot

Vamos a iniciar con Springboot, para ello haremos las siguiente configuraciones

# Agregue el padre al proyecto Maven

Use el Springboot Initializer para crear un nuevo proyecto desde IntellJ Ultimate </br></br>

A la lista de dependencias, agregue
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```
De esta forma nuestro repositorio tendrá lo necesario para comenzar un RestAPI HTTP

### Punto de inicio
Método main de nuestro repo
```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.web.servlet.support.SpringBootServletInitializer;

@SpringBootApplication
public class App extends SpringBootServletInitializer {

    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }

}
```

Ya con todas las condiciones iniciales puede agregar un controller
```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {
    @GetMapping("hello")
    public String hello() {
        return "Integrador 1";
    }
}

```



# Leer Datos desde el Request en Spring Boot

En Spring Boot, podemos extraer datos de las peticiones HTTP de varias maneras. A continuación, se presentan las formas más comunes:

## 1. Lectura de Headers
Podemos leer los headers de la petición utilizando la anotación `@RequestHeader`.

```java
@RestController
@RequestMapping("/api")
public class HeaderController {

    @GetMapping("/headers")
    public ResponseEntity<String> getHeaders(@RequestHeader("User-Agent") String userAgent) {
        return ResponseEntity.ok("User-Agent: " + userAgent);
    }
}
```

- `@RequestHeader("User-Agent")`: Extrae el valor del header `User-Agent` enviado en la petición.
- Retorna el valor del header en la respuesta.

---

## 2. Path Variables
Podemos extraer valores desde la URL usando `@PathVariable`.

```java
@RestController
@RequestMapping("/api")
public class PathVariableController {

    @GetMapping("/users/{id}")
    public ResponseEntity<String> getUserById(@PathVariable("id") Long userId) {
        return ResponseEntity.ok("Usuario con ID: " + userId);
    }
}
```

- `@PathVariable("id")`: Captura el valor `{id}` desde la URL.
- Retorna el ID en la respuesta.

---

## 3. Request Params
Podemos obtener parámetros de consulta (`query params`) con `@RequestParam`.

```java
@RestController
@RequestMapping("/api")
public class RequestParamController {

    @GetMapping("/search")
    public ResponseEntity<String> search(@RequestParam("query") String query) {
        return ResponseEntity.ok("Búsqueda: " + query);
    }
}
```

- `@RequestParam("query")`: Captura el parámetro de consulta `query`.
- Retorna el valor del parámetro en la respuesta.

---

## 4. Body en JSON
Para recibir datos en formato JSON, utilizamos `@RequestBody`.



```java
@RestController
@RequestMapping("/api")
public class RequestBodyController {

    @PostMapping("/users")
    public ResponseEntity<String> createUser(@RequestBody User user) {
        return ResponseEntity.ok("Usuario creado: " + user.getName() + ", Edad: " + user.getAge());
    }

}
```

```java
public class User {

    private String name;
    private int age;

    // Getters y setters
        
}
```

- `@RequestBody`: Convierte el JSON enviado en la petición a un objeto Java.
- `User`: Clase modelo para deserializar los datos JSON.
- Retorna la información del usuario recibido.



# Uso de ResponseEntity en Spring Boot

`ResponseEntity` es una clase de Spring utilizada para representar toda la respuesta HTTP, incluyendo el cuerpo, los encabezados y el código de estado.

## 1. Retornar una Respuesta Exitosa (200 OK)

```java
@RestController
@RequestMapping("/api")
public class ResponseEntityController {

    @GetMapping("/success")
    public ResponseEntity<?> successResponse() {
        return ResponseEntity.ok(Map.of("message", "Operación exitosa"));
    }
}
```

### Explicación
- `ResponseEntity.ok(Map.of(...))`: Retorna un código de estado `200 OK` con un cuerpo en formato `Map`.

---

## 2. Retornar un Código de Estado Personalizado
Podemos personalizar el código de estado utilizando `ResponseEntity.status()`.

```java
@GetMapping("/custom-status")
public ResponseEntity<?> customStatus() {
    return ResponseEntity.status(HttpStatus.CREATED).body(Map.of("message", "Recurso creado correctamente"));
}
```

### Explicación
- `ResponseEntity.status(HttpStatus.CREATED).body(Map.of(...))`: Retorna `201 Created` con un mensaje en formato `Map`.

---

## 3. Retornar una Respuesta con Headers Personalizados
Podemos agregar headers adicionales a la respuesta.

```java
@GetMapping("/with-headers")
public ResponseEntity<?> responseWithHeaders() {
    HttpHeaders headers = new HttpHeaders();
    headers.add("Custom-Header", "ValorPersonalizado");
    return ResponseEntity.ok().headers(headers).body(Map.of("message", "Respuesta con headers"));
}
```


- `HttpHeaders`: Permite agregar headers personalizados.
- `headers(headers)`: Agrega los headers a la respuesta.

---

## 4. Retornar un Código de Error (400 Bad Request)
Podemos manejar errores devolviendo un código HTTP adecuado.

```java
@GetMapping("/bad-request")
public ResponseEntity<?> badRequest() {
    return ResponseEntity.badRequest().body(Map.of("error", "Solicitud incorrecta"));
}
```


- `ResponseEntity.badRequest()`: Retorna `400 Bad Request` con un mensaje de error en `Map`.

---

## 5. Retornar un Código 404 Not Found
Si el recurso no existe, podemos devolver un `404 Not Found`.

```java
@GetMapping("/not-found")
public ResponseEntity<?> notFound() {
    return ResponseEntity.status(HttpStatus.NOT_FOUND).body(Map.of("error", "Recurso no encontrado"));
}
```


- `HttpStatus.NOT_FOUND`: Retorna un `404` con un mensaje adecuado en `Map`.

---

## 6. Retornar un No Content (204 No Content)
Si no hay contenido que devolver, podemos usar `204 No Content`.

```java
@GetMapping("/no-content")
public ResponseEntity<?> noContent() {
    return ResponseEntity.noContent().build();
}
```


- `ResponseEntity.noContent().build()`: Retorna `204 No Content` sin cuerpo.


Con estas técnicas, podemos estructurar mejor nuestras respuestas en una API RESTful.






# Archivo de configuración
En el archivo de configuración se pueden cambiar parámetros estáticos del servidor. Como por ejemplo el puerto por el que escucha las solicitudes
```
server.port=8081
```


# HTTP Springboot Client
Springboot también puede ser cliente de otro API si se requiere

Para eso debe crear un archivo de configuración en un paquete config
```java
@Configuration
public class AppConfig {
    @Bean
    public RestTemplate restTemplate() {
        return new RestTemplate();
    }
}
```
Luego, puede usar algún método que haga el llamado. Por ejemplo un POST Request
```java
    public String httpRequest(String url, String body) throws IOException {
        HttpHeaders headers = new HttpHeaders();
        headers.set("Content-Type", "application/json; UTF-8");

        HttpEntity<String> request = new HttpEntity<>(json, headers);
        ResponseEntity<String> response = restTemplate.postForEntity(url, request, String.class);

        if (response.getStatusCode() == HttpStatus.OK) {
            return response.getBody();
        } else {
            throw new IOException("HTTP Error: " + response.getStatusCode() + ", " + response.getBody());
        }
    }
```

# Comando útiles
Liste los programas corriendo sobre un puerto<br><br>
Unix
```
lsof -i:8080
```
Windows
```
netstat -ano | findstr :<PORT>
```

Mate el proceso en el puerto elegido<br><br>
Unix
```
kill $(lsof -t -i:8080)
```
Windows
```
taskkill /PID <PID> /F
```



