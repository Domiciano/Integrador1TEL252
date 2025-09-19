# Taller 3 (15%)
En este taller debe implementar un flujo de datos que nace en un dispositivo de hardaware y se almacena finalmente en una base de datos.

## Contexto
La empresa de energía `Enerbit` lo ha contratado para almacenar un flujo de datos de uno de sus últimos productos que son unas unidades medidoras de energía.

Estas unidades consisten en un sensor de efecto hall que mide la corriente en Amperios y es capaz de medir desde -50 amperes hasta +50 amperes. Además la unidad cuenta con un sensor de voltaje que es capaz de medir entre -500 y +500 volts.

`Enerbit` ha observado que la frencuencia natural máxima del fenómeno es de 100Hz. Adicionalmente se quieren almacenar 2 segundos de muestra

Cada unidad está diseñada para conectarse a un toma corriente de modo que se pueda registrar el consumo por toma en un hogar.

Su objetivo va a ser trabajar en una sola unidad, para lo cual debe considerar estos pasos


## Simulación de sensores [10%] 

Dado que no tiene los sensores, simule el comportamiento del sensor usando la función de random. Aquí hay unos ejemplo de uso
```cpp
int valor1 = random(10);      // entre 0 y 9
int valor2 = random(5, 15);   // entre 5 y 14
```
Tenga en cuenta los rangos suministrados en el problema

## Algoritmo de muestreo [10%]

Cree un algoritmo que permita muestrear la señal de acuerdo a lo exigido por `Enerbit`

## Base de datos [20%]

Defina una `@Entity` que le permita almacenar la muestra

## Backend [30%]

Cree un endpoint de tipo `POST` que le permita recibir la muestra de 2 segundos y almacenarla en `Postgres`

## Post request desde ESP32 [30%]

Finalmente con todo el escenario envíe la muestra y almacénela en la base de datos
