# Internet Of Things ~ Pedro Ángel Castiñeira Ruiz

Proyecto para la clase de Implantación de aplicaciones web sobre la práctica InternetOfThings.

## Preparando la máquina

Necesitaremos una máquina con mínimo 4GB de memoria con los puertos 8888, 8086, 3000, y 1883 abiertos.

## Montando la infraestructura

Necesitamos tener instaladas las herramientas docker y docker-compose en la máquina, para instalarlas ejecutamos el comando

>sudo apt update && sudo apt install docker docker-compose -y

Cuando se hayan instalado las herramientas correspondientes pasaremos a montar la infraestructura.

Los archivos necesarios y la estructura de carpetas necesarias para levantar los contenedores correspondientes se encontrarán en el [repositorio de github](https://github.com/vaeruiz/iaw-IOT).

## Escuchando los mensajes y enviando datos

Dado que no disponemos de los sensores correspondientes que propone la práctica, podemos emular ese envío de información a través de contenedores docker, necesitamos dos contendores

- **Un contenedor de escucha.** El cual estará atento a los mensajes que le enviemos y los mostrará.

- **Un contenedor cliente.** Este contenedor será el que envíe los valores al contenedor que escucha.

Ambos contenedores utilizan un protocolo llamado MQTT, se trata de un protocolo de mensajería ligero que utilizan muchas aplicaciones de Internet, sigue el patrón publish/subscribe que consiste en publicar mensajes enviados por clientes que pertenecen a un topic y mostrarlos a los clientes suscritos.

Para utilizar el contenedor del tipo publish ejecutamos el siguiente comando:

>sudo docker run --init -it --rm efrecon/mqtt-client pub -h dirección_IP -p 1883 -t "iescelia/aula22/co2" -m 30

Donde dirección_IP es la dirección IP pública del host.

Para utilizar el contenedor del tipo subscribe ejecutamos el siguiente comando:

>sudo docker run --init -it --rm efrecon/mqtt-client sub -h direccion_IP -t "iescelia/#"

## Monitorizando datos con Grafana.

Tenemos un contenedor que es un GBD encargado de almacenar los datos que, en este caso, los publish envían. Existe un complemento con el que podemos reflejar estos datos en gráficas, de eso se encarga Grafana, un servicio web que permite tener un panel de control con los datos que se han registrado en nuestra base de datos, de esta forma podemos mostrar los datos recogidos de una forma más gráfica.

Para entrar a Grafana basta con que pongamos nuestra dirección IP/DNS de nuestra máquina y el puerto 3000 (si es el que hemos abierto para el servicio). Por defecto, la contraseña y el usuario son admin, al entrar por primera vez se nos pedirá que cambiemos la contraseña, cuando la actualicemos, estaremos dentro del panel de control.

Para revisar el gráfico de nuestra aplicación IOT, vamos a la sección Dashboards>Manage, ahí veremos nuestro dashboard ya creado bajo el nombre CO2 Dashboard.

Cuando entremos veremos los gráficos correspondientes con los datos que hemos introducido.

![Captura de demostración](/imagenes/captura1.png)
