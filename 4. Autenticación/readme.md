# Taller práctico
Entrega el 29 de Agosto

https://miro.com/app/board/uXjVI6otAiE=/?share_link_id=751940168503

El objetivo del taller práctico apunta al módulo de autenticación del proyecto integrador. 

El taller es **Individual**

Representa el 5% de la calificación total del curso

Debe implementar un sistema de autenticación sencillo, sin base de datos sino con información efímera. Por medio de cualquier tipo de `Collection` de Java.

## Objetivo
Con este ejercicio, podrán practicar el manejo de autenticación y autorización en una aplicación SpringBoot. También aprenderán cómo crear endpoints para el registro de usuarios, el inicio de sesión y la gestión de usuarios registrados, y cómo crear una clase Repository para manejar los datos de la aplicación de forma local.

## Tareas
Genere los siguientes endpoints

### Endpoint para el registro de usuarios 
Este endpoint debe permitir la entrada de un usuario a través del endpoint.

El objeto de usuario debe estar compuesto por un `name`, `email` y `password`.

El usuario debe almacenarse en una `Collection` de su programa.

Si otro usuario intenta registrarse con el mismo email, el servidor debe lanzar un stutus code `4XX` (encuentre el código adecuado a responder)

### Endpoint para el login

Este endpoint debe permitir recibir un objeto con las variables `email`, `password`. 

Si el email y password concuerdan con un usuario ya registrado, genere un UUID que servirá de `token` y devuélvalo en la respuesta.

Un cliente con token simboliza que está autenticado

### Endpoint para listar usuarios
Este endpoint debe entrega toda la lista de usuarios.

Solo se entrega esta lista si en el `request` se presenta un `token` generado por el backend.

Primero el servidor debe verificar si este token existe.

Si es así, entonces se devuelve la lista de usuarios.

De lo contrario el server debe lanzar un stutus code `4XX` (encuentre el código adecuado a responder)

### Endpoint para eliminar usuario
Este endpoint debe permitir eliminar a un usuario por su ID. 

Solo se elimina al usuario, si en el request se presenta un `token` generado por el backend.

De lo contrario el server debe lanzar un stutus code `4XX` (encuentre el código adecuado a responder)

### Repository
Cree una clase auxiliar llamada `UserRepository` que permita almacenar los usuarios y los token.
