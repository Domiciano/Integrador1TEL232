<img width="256" src="https://www.icesi.edu.co/launiversidad/images/La_universidad/logo_icesi.png">

# Protocolo MQTT
Para crear un servidor MQTT simple cree una imagen de docker usando la siguiente estructura de carpetas:


```
* images
   * mosquitto
      - mosquitto.conf
      - Dockerfile  
* composes
   * mosquitto
      - docker-compose.yml
```
Convención
```
* Carpeta
- Archivo
```

En el archivo Dockerfile use esta plantilla

```
FROM eclipse-mosquitto:2

# Copiar el archivo de configuración personalizado
COPY mosquitto.conf ./mosquitto/config/mosquitto.conf

# Exponer los puertos
EXPOSE 1883 8883 9001
```

Y en el archivo mosquitto.conf escriba lo siguiente
```
allow_anonymous true
listener 1883 0.0.0.0
```

Lo que está haciendo es construyendo su propia imagen del MQTT server con su propio archivo de configuración


## Despliegue local


Una vez haya creado su imagen y la haya subido a dockerhub, usted podrá usar el servicio en su docker compose:

```
version: '3.8'
services:
    mosquitto:
        image: domi0620/mqttserver:0.0.1
        ports:
            - 1883:1883
            - 8883:8883
            - 9001:9001
```


Luego use
```
docker-compose up
```

Esto creará un contenedor con el servidor en ejecución y se podrá conectar a él usando la url

```
mqtt://127.0.0.1:1883
```

A partir de esto podrá suscribirse y enviar datos a un topic

# Cliente MQTT

Puede usar cualquier cliente de su elección. MQTTX es adecuado para testear los envíos y las suscripciones.
```
https://mqttx.app/
```
