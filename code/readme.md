```cpp
#include <Arduino.h>
#include <WiFi.h>
#include <Arduino_JSON.h>
#include <HTTPClient.h>


const char* ssid = "Domi iPhone";
const char* password = "alfabetagama";
String url = "http://172.20.10.3:8080/sensors";


void setup() {
  Serial.begin(115200);
  WiFi.mode(WIFI_STA);
}

void loop() {
  
}

void initWifi(){
  Serial.println("Conectandome a wifi...");
    WiFi.begin(ssid, password);
    while (WiFi.status() != WL_CONNECTED) {
      Serial.print('.');
      delay(1000);
    }
    Serial.println("Conectado");
    Serial.println(WiFi.localIP());

}

//Leo sensor
int readSensor(){
  return random(1024);
}

void sample() {
  JSONVar array;
  Serial.println("Inicio de la muestra...");
  for(int i=0 ; i<40 ; i++){ //Duracion
    JSONVar rawdata;
    rawdata["gx"] = readSensor();
    rawdata["gy"] = readSensor();
    rawdata["gz"] = readSensor();
    array[i] = rawdata;
    delay(50); //Periodo de muestreo
  }
  Serial.println("Fin de la muestra");
  Serial.println(JSON.stringify(array));

  //ENVIO POST
  HTTPClient http;
  http.begin(url.c_str());
  http.addHeader("Content-Type","application/json");
  int httpResponseCode = http.POST(JSON.stringify(array));
  Serial.println(httpResponseCode);
  if (httpResponseCode == 200) {
    Serial.println("Envio exitoso");
  }
  http.end();
}

void measure(){
  //Construir el objeto de RawData
  JSONVar rawdata1;
  rawdata1["gx"] = readSensor();
  rawdata1["gy"] = readSensor();
  rawdata1["gz"] = readSensor();
  Serial.println(JSON.stringify(rawdata1)); 

  HTTPClient http;
  http.begin(url.c_str());
  http.addHeader("Content-Type","application/json");
  int httpResponseCode = http.POST(JSON.stringify(rawdata1));
  Serial.println(httpResponseCode);
  if (httpResponseCode == 200) {
    Serial.println("Envio exitoso");
  }
  http.end();
}

void serialEvent() {
  if (Serial.available() > 0) {
    String data = Serial.readStringUntil('\n');
    if(data == "wifi"){
      initWifi();
    }else if(data == "send"){
      measure();
    }
    else if(data == "samp"){
      sample();
    }
    Serial.println(data);
  }
}
```


```java
package edu.co.icesi.flujodatossensorapi.repo;

import edu.co.icesi.flujodatossensorapi.entity.RawData;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface SensorDataRepo extends JpaRepository<RawData, Integer> { }
```

```java
package edu.co.icesi.flujodatossensorapi.repo;

import edu.co.icesi.flujodatossensorapi.entity.RawData;
import edu.co.icesi.flujodatossensorapi.entity.Sample;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface SampleRepo extends JpaRepository<Sample, Integer> { }
```

```java
package edu.co.icesi.flujodatossensorapi.entity;


import jakarta.persistence.*;

import java.util.List;

@Entity
@Table(name = "sample")
public class Sample {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private int id;

    private double samplingRate;
    private long timestamp;

    @OneToMany(mappedBy = "sample")
    private List<RawData>  sensorData;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public double getSamplingRate() {
        return samplingRate;
    }

    public void setSamplingRate(double samplingRate) {
        this.samplingRate = samplingRate;
    }

    public long getTimestamp() {
        return timestamp;
    }

    public void setTimestamp(long timestamp) {
        this.timestamp = timestamp;
    }

    public List<RawData> getSensorData() {
        return sensorData;
    }

    public void setSensorData(List<RawData> sensorData) {
        this.sensorData = sensorData;
    }
}
```

```java
package edu.co.icesi.flujodatossensorapi.entity;

import jakarta.persistence.*;

@Entity
@Table(name = "rawdata")
public class RawData {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private int id;

    private double gx;
    private double gy;
    private double gz;
    //JSON
    //{"gx":3.5,"gy":1.4,"gz":0.01}

    @ManyToOne
    @JoinColumn(name = "sampleid")
    private Sample sample;

    public Sample getSample() {
        return sample;
    }

    public void setSample(Sample sample) {
        this.sample = sample;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public double getGx() {
        return gx;
    }

    public void setGx(double gx) {
        this.gx = gx;
    }

    public double getGy() {
        return gy;
    }

    public void setGy(double gy) {
        this.gy = gy;
    }

    public double getGz() {
        return gz;
    }

    public void setGz(double gz) {
        this.gz = gz;
    }
}
```

```java
package edu.co.icesi.flujodatossensorapi.controller;

import edu.co.icesi.flujodatossensorapi.entity.RawData;
import edu.co.icesi.flujodatossensorapi.entity.Sample;
import edu.co.icesi.flujodatossensorapi.repo.SampleRepo;
import edu.co.icesi.flujodatossensorapi.repo.SensorDataRepo;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

import java.util.Date;
import java.util.List;

@RestController
public class SensorDataController {

    @Autowired
    private SensorDataRepo sensorDataRepo;

    @Autowired
    private SampleRepo sampleRepo;

    //Envio un dato a la base de datos
    @PostMapping("sensor")
    public ResponseEntity<?> addSensorData(@RequestBody RawData sensorData) {
        sensorDataRepo.save(sensorData);
        return ResponseEntity.status(200).body(sensorData);
    }



    //Enviar un ArrayList de datos y almacenlos en la base de datos
    @PostMapping("/sensors")
    public ResponseEntity<?> addSensorData(@RequestBody List<RawData> sensorData) {
        //Creamos la instacia de la muestra
        Sample sample = new Sample();
        sample.setSamplingRate(20.0);
        sample.setTimestamp(new Date().getTime());
        sampleRepo.save(sample);

        for(int i=0;i<sensorData.size();i++) {
            sensorData.get(i).setSample(sample);
        }
        sensorDataRepo.saveAll(sensorData);
        return ResponseEntity.status(200).body("Datos almacenados");
    }

    @GetMapping("/hello")
    public ResponseEntity<?> healthCheck() {
        return ResponseEntity.status(200).body("Hello World");
    }

}
```
