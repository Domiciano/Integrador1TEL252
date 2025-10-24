# Conexión a un MQTT Broker

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    
    <input id="messageInput" type="text" placeholder="Escribe tu mensaje"><br>
    <button id="sendButton">Enviar</button>

    <div id="messageContainer"></div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/paho-mqtt/1.0.1/mqttws31.js" type="text/javascript"></script>
    <script src="./sampleControl.js"></script>
</body>
</html>
```

```js
const url = "broker.emqx.io";
const port = Number(8083);
const username = "icesidomicianorincon123123";

const messageInput = document.getElementById('messageInput');
const sendButton = document.getElementById('sendButton');
const messageContainer = document.getElementById('messageContainer');

const client = new Paho.MQTT.Client(url, port, username);

//Callback
client.onMessageArrived = function(payload){
    console.log(payload.payloadString);
    messageContainer.innerHTML += `<p>${payload.payloadString}</p>`;
}

client.connect({
    onSuccess:function(){
        console.log("Conectado");
        client.subscribe("icesi/telematica");
    }
});

sendButton.addEventListener("click", sendMessage);

function sendMessage(){
    let msg = messageInput.value;
    let mqttPackage = new Paho.MQTT.Message(msg);
    mqttPackage.destinationName = "icesi/telematica";
    client.send(mqttPackage);
}
```
