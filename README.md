# Configuración básica de la Raspberry Pi e instalación en la plataforma dron Hexsoon EDU  

## 1. Introducción
Con frecuencia un dron lleva computación a bordo, que le permite tomar decisiones durante el vuelo como, por ejemplo, cambiar el plan de vuelo previsto si detecta algo a través de los sensores del dron. Naturalmente, una cámara es uno de esos sensores más habituales.   
  
Una opción ideal para esto es la Raspberry Pi (RPi), a la que se le puede instalar una cámara y además puede conectarse a internet. En esta guía vamos a describir paso a paso cómo configurar la RPi de manera que podamos conectarnos con ella desde la estación de tierra a través de internet, cómo conectarle una cámara o como conectarla con el autopiloto para controlar el dron desde un programa en Python.   
 
El kit RPi que vamos a utilizar es el que se muestra en la figura. Consiste en una RPi, un módulo de cámara, un cable de alimentación, cable para conectar la RPi a un monitor por HDMI, un adaptador WIFI y una tarjeta microSD.   

 <img width="504" height="378" alt="image" src="https://github.com/user-attachments/assets/db071ec1-1607-4f89-8429-8a82ddc66d5b" />


En algunos de los pasos de esta guía se usa una RPi contenida en una caja roja como la mostrada en la figura. Esta caja contiene los conectores para alimentar la RPi y para conectarla al autopiloto. También contiene un botón y unos leds, que aprenderemos a controlar en la sección 15 de esta guía.    

 
<img width="506" height="217" alt="image" src="https://github.com/user-attachments/assets/0a0caac6-389e-4feb-9103-04b10edc465c" />


## 2. Instalación del sistema operativo Raspberry Pi OS (antes conocido como Raspbian)
El sistema operativo debe instalarse en una tarjeta microSD que posteriormente se insertará en la RPi. Para ello debemos conectar la microSD al ordenador desde el que se realizará la instalación. El código del sistema operativo puede encontrarse aquí:   

<img width="308" height="181" alt="image" src="https://github.com/user-attachments/assets/9ae9e3bc-595b-4186-b9fa-50d02e62cdea" />

https://www.raspberrypi.com/software/   
 
En primer lugar debemos descargar Raspberry Pi Imager. Al ejecutarlo, podremos elegir el modelo de RPi, el sistema operativo que queremos instalar (que debe ser Raspberry Pi OS de 64 bits) y la unidad en la que se ha conectado la microSD. Para instalar el sistema operativo en la microSD desde nuestro portátil podemos utilizar dispositivos como los mostrados en la siguiente figura.

<img width="521" height="221" alt="image" src="https://github.com/user-attachments/assets/55e1d4c3-d4a7-415c-93a3-350acaacd609" />


La aplicación preguntará si queremos hacer alguna configuración. Le diremos que no. Todas las configuraciones necesarias las haremos en los pasos siguientes. La figura siguiente contiene imágenes que pueden ayudar en este proceso. 

<img width="1076" height="241" alt="image" src="https://github.com/user-attachments/assets/659e59d6-0c74-423d-87a9-3b70a30ca1fb" />


## 3. Conexión de los dispositivos necesarios para realizar la configuración
El siguiente paso es insertar la microSD en la que hemos instalado el sistema operativo en la ranura correspondiente de la RPi. Además, debemos conectar la RPi a:    
 
* Un monitor (con el cable HDMI-microHDMI)
* Un teclado (a uno de los puertos USB de la RPi)
* Un ratón (a otro de los puertos USB)

Y naturalmente conectar la RPi a la alimentación. La figura siguiente muestra algunas imágenes que pueden ayudar en este paso.


<img width="705" height="451" alt="image" src="https://github.com/user-attachments/assets/9e57fd13-7856-4e4e-9a40-4a05ccf0f43e" />


## 4. Configuración inicial 
Al conectar la RPi nos aparecerán en pantalla una serie de menús que nos permitirán hacer algunas configuraciones iniciales básicas (país, idioma, zona horaria) y también crear un usuario. También nos preguntará si queremos configurar una conexión a internet, pero nos saltaremos ese paso (Skip). Después nos pedirá que elijamos el navegador que debe usar. Finalmente, intentará realizar una actualización del software pero no podrá hacerlo porque aún no se ha configurado la conexión a internet. Acabado este proceso la RPi se reiniciará y aparecerá la pantalla de bienvenida. Las imágenes siguientes pueden ayudar en este proceso.   
 
<img width="645" height="498" alt="image" src="https://github.com/user-attachments/assets/eca8ccc0-e9ba-49e8-b68d-3660e12fb145" />

## 5. Configuración de las conexiones a internet
La RPi puede conectarse a internet mediante un cable Ethernet, pero no vamos a usar esta conexión. Por otra parte, tiene una interfaz interna para la conexión a una Wifi (la que no quisimos configurar en el paso anterior). Esta interfaz permitirá a la RPi conectarse a internet y poder enviar a la estación de tierra, por ejemplo, las imágenes que tome con la cámara. La RPi tiene una segunda interfaz Wifi que se puede configurar como punto de acceso (hotspot). Este hotspot permitirá a dispositivos externos (por ejemplo, nuestro portátil) conectarse a la RPi para instalar software o para poner ese software en marcha. Veamos ahora cómo configurar estas dos interfaces. Pero, en primer lugar, tenemos que conectar el adaptador Wifi a uno de los puertos USB de la RPi que quedan disponibles.   
 
Las imágenes de la siguiente figura ilustran los pasos para configurar la interfaz interna para conectar la RPi a una Wifi.   
<img width="1018" height="1081" alt="image" src="https://github.com/user-attachments/assets/b39b73e9-d42f-4bf2-95b5-40bd4871df8b" />

Para iniciar la configuración y que aparezca la pantalla de la figura (a) hay que clicar en el icono de las dos marcas rojas que hay en la esquina superior derecha, entre el icono de bluetooth y el icono de sonido. La figura 6a muestra que hay dos interfaces disponibles. La interna (BroadcomBCM43438) es la que vamos a configurar para conectar la RPi a una de las Wifis disponibles. Hay que elegir una de ellas e introducir la contraseña, tal y como muestran las figuras (a) y (b). Conviene ahora comprobar que la RPi tiene conexión a internet. Para ello se puede abrir un terminal y ejecutar el comando:   
```
ping google.com
```
La segunda interfaz (Ralink RT5370) corresponde al adaptador Wifi que hemos conectado al puerto USB. Este modelo de adaptador (que se muestra en la figura (c) tiene la ventaja de que el driver necesario ya está incluido en el sistema operativo, de manera que solo es necesario conectar el adaptador para que esté operativo. En el caso de otros modelos de adaptador puede ser necesario instalar los drivers. En el anexo se describen algunos casos.   
 
Las figuras (d), (e) y (f) muestran cómo configurar el punto de acceso (hotspot), al cual le hemos puesto el nombre MiHotSpot. Este será el nombre de la Wifi que será visible por cualquier dispositivo externo que quiera conectarse a la RPi.    
 
Finalmente, es necesario indicarle al sistema operativo que al iniciar la RPi active automáticamente el hotspot. Las figuras (g), (h) y (i) muestran cómo hacerlo.   
 
La figura (6) muestra las IP de cada una de las conexiones, una vez configuradas. Particularmente importante es la IP del hotspot (10.42.0.1), porque es la que debe usar el dispositivo externo que quiera conectarse a la RPi.   

## 6. Conexión a la RPi desde un dispositivo externo vía SSH 
Antes de realizar una conexión con la RPi, debemos indicar al sistema operativo que permita las conexiones SSH. Para eso, en un terminal de la RPi debe ejecutarse el comando siguiente:   
```
sudo raspi-config
```
Aparecerá en pantalla un menú de opciones como el que muestra la figura siguiente, que también muestra las opciones a elegir para habilitar las conexiones SSH. Esta figura no muestra la tercera pantalla en la que se indica si se desea poner el marcha el servidor SSH, en la que hay que contestar que si.   

<img width="919" height="319" alt="image" src="https://github.com/user-attachments/assets/5b031a28-1bc9-483b-87a7-5b3e0f6fdcaf" />

Ahora, cualquier dispositivo externo (nuestro móvil o nuestro portátil) debería ver la Wifi MiHotSpot. Si nos conectamos a esa Wifi ahora podemos conectarnos a la RPi vía SSH. La figura siguiente muestra cómo hacerlo utilizando PuTTY. Esta aplicación se puede descargar de:   
https://putty.org/

 <img width="527" height="525" alt="image" src="https://github.com/user-attachments/assets/647bdab8-cf97-485b-bec3-890e02b0554c" />


## 7. Instalación de Python
Una vez hemos entrado en la RPi (usando las claves de usuario configuradas en su momento) podemos ya instalar el software que necesitemos. Empezaremos instalando Python. Desde un terminal de la RPi haremos (tener en cuenta que el segundo comando puede tardar unos minutos en ejecutarse):   
```
sudo apt update 
sudo apt upgrade –y 
sudo apt install libgl1-mesa-glx 
sudo apt install python3-pip python3-dev
```
Para comprobar que la instalación se ha realizado correctamente, prepararemos un fichero hello.py con la sentencia:	
```       
print (‘Hello world”)
```
Y ejecutamos el programa desde el terminal haciendo:
```
python3 hello.py
```
Desarrollar programas en Python en modo comandos puede parecer a muchos algo tedioso (aunque otros lo prefieren). Requiere aprender a usar editores de texto tales como pico o nano, que vienen instalados en el sistema operativo. En el caso de que tengamos un terminal conectado a la RPi, entonces podemos usar Thonny Python IDE, que también está instalado en el sistema operativo y nos ofrece un entorno de desarrollo más amigable, basado en ventanas.

## 8. Conexión de la RPi con el simulador SITL
Uno de los objetivos de instalar la RPi a bordo del dron es poner controlar el autopiloto mediante nuestros programas ejecutados en la RPi. Esto nos permitirá tomar decisiones de forma rápida sin necesidad de que intervenga la estación de tierra. Por ejemplo, podemos hacer que el dron aterrice inmediatamente si detecta la imagen de un determinado objeto (usando la cámara que aprenderemos a instalar en las próximas secciones). Esta operación también puede hacerse sin una RPi, haciendo que un transmisor de video envíe la imagen de la cámara a la estación de tierra, que analizará la imagen y en caso de detectar el objeto en cuestión se enviará la orden de aterrizaje a través del enlace de telemetría. Obviamente, esta segunda alternativa va a ser más lenta, lo cual puede hacerla inviable en determinadas operaciones que requieren acción inmediata.   
 
Para controlar el dron usaremos la librería dronLink, que se está desarrollando en la UPC. Esta librería se encuentra en un repositorio de github, convenientemente documentada:   

https://github.com/dronsEETAC/dronLink

El vídeo siguiente muestra los pasos que hay que dar para instalar la librería en la RPi y ejecutar un programa que controle el simulador SITL de Ardupilot.   
https://youtu.be/Rv5vDt5qYPg
   
[![](https://markdown-videos-api.jorgenkh.no/url?url=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3DnBh4BNyI6vw)](https://www.youtube.com/watch?v=Rv5vDt5qYPg)

[![](https://markdown-videos-api.jorgenkh.no/url?url=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3DnRv5vDt5qYPg)](https://www.youtube.com/watch?v=Rv5vDt5qYPg)
 
Los pasos son los siguientes:   
1. Conectar el portátil al hotspot de la RPi y también a la misma red local a la que está conectada la RPi. Para realizar estar dos conexiones necesitaremos un adaptador WiFi externo.
2. Poner en marcha Mission Planner y una instancia del simulador SITL.
3. Entrar en la RPi y crear una carpeta para desarrollar la demo.
4. Clonar en esa carpeta el repositorio en el que está la librería dronLink. El repositorio clonado tiene muchos ficheros que incluyen programas de test de las funciones de la librería y también programas más complejos con demostradores de uso de la librería. En realidad, para usar la librería solo se necesita el fichero dron.py y la carpeta modules. En la demo se usa también el fichero test_libreria.py que contiene un ejemplo de programa que usa algunas funciones básicas de la librería.
5. Averiguar la IP asignada al portátil, en la misma red local a la que está conectada la RPi y poner esa IP en el programa de test.
6. Crear un entorno virtual para instalar las librerías que necesite, que en este caso solo es la librería pymavlink, que necesita dronLink para implementar las funciones que están en la carpeta modules. ATENCIÓN: hay librerías que no pueden instalarse en un entorno virtual y deben instalarse con apt install (como se ha hecho antes con python3-pip, por ejemplo). Esas librerías no serían accesibles cuando se trabaja en un entorno virtual. Si queremos que esas librerías también puedan ser usadas desde un entorno virtual, entonces hay que crear el entorno virtual de la siguiente forma:
```
python3 -m venv venv --system-site-packages
```
> Puesto que más adelante en este tutorial vamos a usar una librería con estas características, conviene crear ya el entorno virtual como se acaba de indicar y no como muestra el vídeo.

7. Ejecutar el programa test_libreria.py

Usando esta estrategia podemos probar con el simulador SITL nuestros programas ejecutándolos directamente en la RPi. Esos mismos programas funcionarán correctamente con el dron real, cambiando solo una línea de código, que es la que conecta la RPi con el simulador o con el autopiloto. Pero antes tenemos que aprender a conectar la RPi con el autopiloto.
 
## 9. Conexión de la RPi con el autopiloto
Para realizar este paso usaremos una RPi montada en la caja roja (ver apartado 1). La RPi y el autopiloto se van a comunicar mediante un enlace de comunicación serie que usa el protocolo UART (Universal Asynchronous Receiver/Transmitter). Este protocolo establece unas normas de comunicación bit a bit entre dos dispositivos. Es un protocolo muy simple que solo necesita dos hilos (para enviar información en ambos sentidos) y una conexión común a tierra, tal y como muestra la figura.   

<img width="336" height="229" alt="image" src="https://github.com/user-attachments/assets/7de8408c-dd19-41b1-80c1-bfe8324dee53" />

Se puede encontrar más información sobre este protocolo aquí:    
 
https://www.analog.com/en/analog-dialogue/articles/uart-a-hardware-communication-protocol.html 

Tanto la RPi como el autopiloto tienen puertos para comunicación UART. En concreto, por parte del autopiloto usaremos el puerto TELEM2, cuya especificación se muestra en la figura:

<img width="599" height="271" alt="image" src="https://github.com/user-attachments/assets/1aeb801b-c256-4f28-910b-066d06845efd" />


De los 6 pines del puerto, el protocolo UART solo usa 3: GND (los pines en los extremos), TX (pin 2) y RX (pin 3). Estos son los pines que deberemos conectar a la RPi. No es necesario conectar el pin VCC porque tanto el autopiloto como la RPi estarán alimentadas por la misma placa de distribución de energía.   
 
The RPi 4 has 40 pins (GPIO) available to interact with peripherals. Figure 2 shows the position of the pins on the board and the pin configuration. Some of the pins have a specific purpose (for example pins that supply 5 volts, or pins for serial communication). But several are general purpose and we can use them to control the LEDs and the button.

La RPi tiene 40 pines (GPIO) disponibles para la comunicación con dispositivos periféricos. La figura siguiente muestra la disposición de los pines. Algunos de ellos son de propósito específico (por ejemplo, los que proporcionan 5 voltios, o los pines específicos para comunicación serie). Pero muchos son de propósito general y pueden usarse para controlar, por ejemplo, los LEDs RGB, tal y como veremos más adelante.   

 <img width="554" height="309" alt="image" src="https://github.com/user-attachments/assets/805085c5-bada-4a7c-9abf-ad2b7bcce500" />

  
Para implementar la conexión serie con la RPi usaremos los pines GPIO14 y GPIO15. En realidad, otros pines pueden configurarse también para comunicación serie, de manera que la RPi puede tener hasta 4 canales de comunicación serie con diferentes dispositivos.  
  
Por otro lado, la caja roja tiene un conector (mostrado en la figura siguiente) que es el que se usará para conectar la RPi con el autopiloto mediante el cable de telemetría. Si numeramos los pines de ese conector del 1 al 4, de izquierda a derecha según la imagen, al hacer la conexión mediante el cable de telemetría,  podemos ver que la señal GND llegará al pin 4, la señal RX del autopiloto llegará al pin 3 y la señal TX llegará al pin 2. 

<img width="886" height="472" alt="image" src="https://github.com/user-attachments/assets/75efdbba-2eae-422b-be4a-2979c6641484" />


Por tanto, tal y como muestra la figura siguiente, será necesario conectar el pin 4 a cualquiera de los pines de la RPi etiquetados como GND (por ejemplo, el 14), el pin 3 a GPIO14 (RX de autopiloto con TX de RPi, mediante el cable amarillo de la figura) y el pin 2 a GPIO15 (TX de autopiloto con RX de RPi). 

<img width="352" height="338" alt="image" src="https://github.com/user-attachments/assets/7b73282e-d60e-4989-a6d5-519e038851eb" />


Una vez establecidas esas conexiones, solo queda realizar un par de configuraciones. Por una parte, hay que indicarle al autopiloto que usará TELEM2 para comunicación UART. Los dos parámetros que hay que cambiar (usando Mission Planner) son:   
 
SERIAL2_PROTOCOL = 2 (el valor por defecto)   
SERIAL2_BAUD = 921 para comunicarse con la RPi a una velocidad de 921600 baud.   

Por otro lado hay que configurar el puerto UART en la RPi. Para ello volvemos a ejecutar en la RPi:    
```
sudo raspi-config
```

Y seleccionamos las opciones mostradas en la figura siguiente: 

<img width="1533" height="523" alt="image" src="https://github.com/user-attachments/assets/4d92c136-67b3-4760-ae72-96f9c2e174a3" />



Finalmente, hay que contestar NO cuando nos pregunte “Would you like a login shell to be accessible over serial?”, y hay que contestar SI cuando nos pregunte “Would you like the serial port hardware to be enabled?”.   
 
Acabado este proceso es necesario reiniciar la RPi. Ahora ya podemos ejecutar nuestros programas para enviar directamente las ordenes al autopiloto. Para ello, hay que cambiar el connection_string y la velocidad en la llamada a la operación de conexión:
```
connection_string = “/dev/serial0”
baud = 921600
dron.connect(connection_string, baud)
```

Para probar el correcto funcionamiento de la conexión podemos usar el programa test_telemetria.py (en la carpeta de tests que acompaña a la librería dronLink) que simplemente se conecta al autopiloto (habrá que cambiar los parámetros de la conexión tal y como se ha indicado) y obtiene varios datos de telemetría (naturalmente, no podemos probar el programa test_libreria.py hasta que no estemos en disposición de volar el dron en el droneLab).   


## 10. Captura del stream de video de una webcam
En esta y las siguientes secciones aprenderemos a capturar el stream de video de una cámara conectada a la RPi y enviar ese stream de video a una estación de tierra.   
 
Empezaremos instalando la librería OpenCV que nos ayudará a capturar y procesar las imágenes de la cámara. La instalación debe hacerse dentro del entorno virtual que hemos creado (tal y como se mostró en el vídeo del apartado 0).   
```
pip install opencv-python
```
Ahora Podemos conectar una WebCam a cualquiera de los puertos USB de la RPi. Podemos usar el código siguiente para capturar y mostrar en pantalla el stream de video de la WebCam.   
```
import numpy as np
import cv2 as cv # libreria opencv
cap = cv.VideoCapture(0)
if not cap.isOpened():
    print("Cannot open camera")
    exit()
while True:
    # Capture frame-by-frame
    ret, frame = cap.read()
    # if frame is read correctly ret is True
    if not ret:
        print("Can't receive frame (stream end?).   Exiting ...")
        break
    # Our operations on the frame come here
    gray = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
    # Display the resulting frame
    cv.imshow('frame', gray)
    if cv.waitKey(1) == ord('q'):
        break
# When everything done, release the capture
cap.release()
cv.destroyAllWindows()
```

Es importante tener en cuenta que si bien las instalaciones de las librerías o la edición del programa puede hacerse indistintamente desde el portátil (conectado a la RPi) o desde el teclado conectado directamente a la RPi, la puesta en marcha del programa debe hacerse necesariamente desde el teclado de la RPi, para que el código encuentre el monitor en el que mostrará el stream de video.   
 
El programa simplemente abre la cámara y entra en un bucle en el que captura un frame, pasa la imagen a escala de grises (una de las múltiples cosas que se pueden hacer con opencv) y lo muestra en pantalla.

## 11. Transmisión del stream de video por MQTT
Como es obvio el programa anterior solo nos sirve para comprobar que estamos capturando correctamente el stream de video, pero no nos va a servir cuando el dron esté volando, a menos que seamos capaces de montar también en el dron un monitor de vídeo conectado a la RPi, que además debe ser grande para poder verlo desde lejos (cosa que nosotros no hemos conseguido). Lo correcto es enviar el stream de video a la estación de tierra para que se muestre en la pantalla del portátil en el que se ejecuta esa estación de tierra. En este apartado vamos a aprender como hacer esto usando el protocolo MQTT y las librerías adecuadas.   
 
Veamos primero el código que debe ejecutarse en la RPi (emisor).
```
import cv2
import base64
import paho.mqtt.client as mqtt
import time

BROKER = "broker.hivemq.com"
PORT = 1883
TOPIC_FRAME = "raspi/camera/stream"
TOPIC_MSG = "raspi/camera/message"

def main():
    client = mqtt.Client()
    client.connect(BROKER, PORT, 60)

    cap = cv2.VideoCapture(0)
    if not cap.isOpened():
        print("No se pudo abrir la cámara")
        return

    try:
        counter = 0
        while True:
            ret, frame = cap.read()
            if not ret:
                continue

            # --- Enviar mensaje de texto antes de cada frame ---
            message = f"Frame #{counter} enviado desde Raspberry"
            client.publish(TOPIC_MSG, message)

            # --- Comprimir frame en JPEG ---
            _, buffer = cv2.imencode(".jpg", frame, [cv2.IMWRITE_JPEG_QUALITY, 40])
            jpg_b64 = base64.b64encode(buffer).decode("utf-8")

            client.publish(TOPIC_FRAME, jpg_b64)
            counter += 1
            time.sleep(0.2)  # ~5 FPS
    except KeyboardInterrupt:
        pass
    finally:
        cap.release()
        client.disconnect()

if __name__ == "__main__":
    main()
```
El código emisor usa la librería opencv para capturar (en esta ocasión cada 200 milisegundos) un frame. El frame no lo pasa a escala de grises pero si que ajusta el nivel de calidad a 40 para que el volumen de información a enviar no sea excesivamente grande. El nivel de calidad puede subirse hasta 100, con lo que la imagen que se recibe será de más calidad pero la fluidez del stream será menos por saturación de la comunicación. La fluidez de la comunicación también se puede controlar haciendo que el código envíe menos frames por segundo, ajustando el valor del parámetro de time.sleep (0.2).   
 
Lo importante es que cuando tenemos un frame preparado se publica en un bróker con el topic "raspi/camera/stream". Este es el principio básico de la comunicación usando el protocolo MQTT. El emisor publica información asignándole un topic (un string cualquiera). Esa información la recibe el bróker (que es un servidor MQTT). El bróker suministrará esa información a cualquier programa (en nuestro caso, el receptor) que se haya suscrito al topic. Naturalmente, tanto el emisor como el receptor deben estar conectados a internet y deben usar el mismo bróker para las publicaciones y suscripciones. En el ejemplo, se usa el bróker público y gratuito "broker.hivemq.com". Hay otros muchos bróker públicos y gratuitos, e incluso puede hacerse la instalación de un bróker en algún servidor propio.    
 
Puede observarse que el código, antes de publicar cada frame publica un mensaje de texto con el topic "raspi/camera/message". De esta manera se ilustra cómo enviar otros tipos de información usando el protocolo MQTT.   
 
Para poner en marcha el programa emisor será necesario instalar en el entorno virtual la librería paho-mqtt:   
```
pip install paho-mqtt
```
Veamos ahora el código del receptor, que se ejecuta en el portátil que actúa como estación de tierra.   
```
import cv2
import base64
import numpy as np
import paho.mqtt.client as mqtt

BROKER = "broker.hivemq.com"
PORT = 1883
TOPIC_FRAME = "raspi/camera/stream"
TOPIC_MSG = "raspi/camera/message"

def on_message(client, userdata, msg):
    if msg.topic == TOPIC_MSG:
        text = msg.payload.decode("utf-8")
        print(f"[MENSAJE] {text}")

    elif msg.topic == TOPIC_FRAME:
        try:
            jpg_b64 = msg.payload.decode("utf-8")
            jpg_bytes = base64.b64decode(jpg_b64)
            nparr = np.frombuffer(jpg_bytes, np.uint8)
            frame = cv2.imdecode(nparr, cv2.IMREAD_COLOR)

            if frame is not None:
                cv2.imshow("Stream desde Raspberry (HiveMQ)", frame)
                if cv2.waitKey(1) & 0xFF == ord("q"):
                    client.disconnect()
        except Exception as e:
            print("Error decodificando frame:", e)

def main():
    client = mqtt.Client()
    client.on_message = on_message
    client.connect(BROKER, PORT, 60)
    client.subscribe([(TOPIC_MSG, 0), (TOPIC_FRAME, 0)])  # suscripción doble

    try:
        client.loop_forever()
    except KeyboardInterrupt:
        pass
    finally:
        cv2.destroyAllWindows()

if __name__ == "__main__":
    main()
```
El receptor también usa las librerías opencv y paho-mqtt. Debe usar el mismo bróker que el emisor y se suscribe a los dos topic (el correspondiente a los mensajes de texto y el correspondiente a los frames del stream de video). Cada vez que el receptor recibe un mensaje ejecuta la función on_message. Alli averigua primero que tipo de mensaje es, mirando el topic (solo ha dos opciones). Si es un mensaje de texto lo recupera y lo escribe en consola. Si es un frame lo recupera y lo muestra en una ventana que se mostrará en la pantalla del portátil.         
 
Si se ponen en marcha estos códigos se observará que el stream de video no es especialmente fluido. Eso es debido a que MQTT no está pensado para enviar grandes cantidades de datos como pueden ser los frames de video. Está pensado para mensajes más cortos, como los que suelen enviarse en un escenario IoT. No obstante, como ya se ha indicado antes, la fluidez de la implementación mostrada puede mejorarse ajustando adecuadamente el nivel de calidad de los frames que se envían y la frecuencia con la que se envían los frames.   
 
En todo caso, veamos ahora una tecnología alternativa para implementar la comunicación del stream de video, que va a mejorar notablemente la fluidez.   

## 12. Transmisión del stream de video por WebSockets
Un websocket es un canal de comunicación entre el emisor y el receptor a través de internet pero que no requiere de ningún otro intermediario, como es el caso del bróker necesario en el caso de MQTT. Esto hace que para el caso de la transmisión de stream de video sea una opción mejor.  
 
El protocolo de websocket se apoya en el protocolo HTTP sobre el que funciona la web. Por eso, en la comunicación debe hacer un servidor y un cliente que se conecta al servidor. En nuestro caso, el servidor será el emisor que se ejecuta en la RPi (que sirve el stream de video a quien se conecte). El cliente será el receptor, que se ejecuta en el portátil.   
 
Veamos el código del servidor.
```
import cv2
import base64
import asyncio
import websockets

async def video_stream(websocket):
    cap = cv2.VideoCapture(0)  # cámara
    if not cap.isOpened():
        print("No se pudo abrir la cámara")
        return

    try:
        while True:
            ret, frame = cap.read()
            if not ret:
                continue

            _, buffer = cv2.imencode('.jpg', frame)
            frame_b64 = base64.b64encode(buffer).decode("utf-8")

            await websocket.send(frame_b64)
            await asyncio.sleep(0.03)  # ~30 FPS
    finally:
        cap.release()

async def main():
    async with websockets.serve(video_stream, "0.0.0.0", 8765):
        print("Servidor WebSocket corriendo en ws://0.0.0.0:8765")
        await asyncio.Future()  # run forever

if __name__ == "__main__":
    asyncio.run(main())
```
El servidor abre un websocket en el puerto 8765, al que puede conectarse cualquier dispositivo externo. En el momento en el que alguien se conecta se pone en marcha el código de la función video_stream. Esa función es básicamente un bucle que captura un frame y lo envía por el websocket. En este caso, no hay topic. Tampoco hay mensaje de texto previo, aunque podría enviarse uno sin problemas a través del websocket. Observese que en este caso no reducimos la calidad del frame y además enviamos 30 frames por segundo, a pesar de lo cual veremos que la fluidez del stream de video es mejor que la del caso de MQTT.   
 
Para que este código puede ejecutarse es necesario instalar la librería websockets en el entorno virtual:
```
pip install websockets
```

Veamos ahora el código del cliente:
```
import cv2
import base64
import numpy as np
import asyncio
import websockets

async def receive_video():
    uri = "ws://<IP_DE_RASPBERRY>:8765"
    async with websockets.connect(uri) as websocket:
        while True:
            frame_b64 = await websocket.recv()
            frame_bytes = base64.b64decode(frame_b64)
            nparr = np.frombuffer(frame_bytes, np.uint8)
            frame = cv2.imdecode(nparr, cv2.IMREAD_COLOR)

            cv2.imshow("Video desde Raspberry", frame)
            if cv2.waitKey(1) & 0xFF == ord("q"):
                break

    cv2.destroyAllWindows()

if __name__ == "__main__":
    asyncio.run(receive_video())
```
Este código también requiere de la instalación de la librería websockets. El código se conecta al websocket y entra en un bucle en el que recibe frames y los muestra en una ventana que aparece en la pantalla del portátil. La conexión debe hacerse al puerto 8765 del servidor. Para ello hay que averiguar la IP que le ha sigo asignada a la RPi en la red local a la que se ha conectado, que debe ser la misma a la que se ha conectado el portátil. Para averiguar la IP basta ejecutar el comando ifconfig en un terminal de la RPi.   
 
Si se ponen en marcha estos códigos podrá observarse que efectivamente la fluidez del stream de vídeo es mucho mejor que en el caso de la comunicación vía MQTT. No obstante, hay herramientas específicas para implementar video streaming con resultados aún mejores, pero de mayor complejidad, por lo que quedan fuera del alcance de este tutorial.   

## 13. Instalación del módulo de cámara 3 de la RPi
La calidad de la imagen puede mejorarse si se sustituye la webcam por un módulo de cámara específico para la RPi. En este apartado vamos a ver como conectar el modulo de cámara, versión 3. En el apartado siguiente veremos lo mismo, pero para la versión 2.   
 
El módulo de cámara para RPi (versión 3) muestra en la figura siguiente en la que se ve también cómo instalar esta cámara en la RPi. Es muy importante prestar atención a la correcta orientación de la cinta de conexión. Las letras que aparecen en esa cinta nos pueden ayudar a no cometer errores.

<img width="775" height="302" alt="image" src="https://github.com/user-attachments/assets/bd71b74e-e11a-42d6-81a3-ac18b5d9685a" />

Una vez instalada, podemos comprobar que funciona correctamente ejecutando el comando siguiente, que nos mostrará en la pantalla conectada a la RPi el stream de video durante unos segundos:    
```
libcamera-hello
```
Para poder utilizar esta cámara en un programa en Python tenemos que instalar en el entorno virtual la librería picamara2:
```
sudo apt install –y python3-picamera2
```
Esta librería es un ejemplo de librería que no se puede instalar en un entorno virtual. Por tanto, si queremos usarla dentro de un entorno virtual, ese entorno debe haber sido creado según se ha explicado en el paso 6 del apartado 8.   
 
Para verificar que la cámara funciona correctamente podemos ejecutar el código siguiente, que debe mostrar el stream de video. Pero antes debemos desconectar la web cam que hemos usado en un paso anterior. Además, el código que captura el stream de video de la webcam no va a funcionar si tenemos conectado el módulo de cámara de la RPi.
```
import cv2
from picamera2 import Picamera2
# Grab images as numpy arrays and leave everything else to OpenCV.
cv2.startWindowThread()
picam2 = Picamera2()
picam2.configure(picam2.create_preview_configuration(main={"format": 'XRGB8888', "size": (640, 480)}))
picam2.start()

while True:
    im = picam2.capture_array()
    grey = cv2.cvtColor(im, cv2.COLOR_BGR2GRAY)
    cv2.imshow("Camera", im)
    cv2.waitKey(1)
```

De nuevo, este programa hay que ejecutarlo en el entorno virtual que hemos creado y hay que hacerlo desde el teclado conectado directamente a la RPi, para que el stream de vídeo aparezca en el monitor conectado a la RPi. Más información sobre la librería picamera2 puede obtenerse aquí:   
https://github.com/raspberrypi/picamera2

## 14. Instalación del módulo de cámara versión 2 de la RPi
Veamos también cómo conectar una cámara RPi pero versión 2, que tiene un proceso de instalación diferente. La cámara se conecta a la RPi exactamente igual que la versión 3 (recuérdese la importancia de orientar bien la cinta de conexión). Después, es necesario añadir al fichero /boot/firmware/config.txt la línea siguiente:   
```
start_x=1
```
Esta edición habrá que hacerla con permisos de administrador (sudo). Finalmente hay que instalar las librerías siguientes:
```
sudo apt install libraspberrypi-bin libraspberrypi0 libraspberrypi-dev 
sudo pip3 install picamera
```

Después de reiniciar la RPi el mismo programa que se mostró en el apartado 10 para capturar el stream de video de la webcam debería funcionar con la cámara RPi versión 2.

## 15. Configuración de los LEDs y del botón
Tal y como se mostró en el apartado 1, la caja roja contiene 5 LEDs RGB y un botón. Estos dispositivos pueden controlarse por programa para hacer, por ejemplo, que al pulsar el botón se tome una foto con la cámara o se reinicie la RPi. Los LEDS pueden usarse para indicar con colores específicos, por ejemplo, si hay o no conexión a internet o si la batería se está agotanto.   
 
Los 5 LEDs RGB se controlan mediante 4 señales (los 5 pines de la derecha, que se muestran en la figura.   

<img width="886" height="378" alt="image" src="https://github.com/user-attachments/assets/6e7197d0-2afc-4098-ad14-0b5472612077" />

Los pines son: GND, DIN (señal de control), 5VDC y nuevamente GND. Estos pones están claramente identificados en la placa negra. Por otro lado el botón se controla con dos pines que están en el lado izquierdo: GND y DOUT (hay dos pines más que no usaremos).    
 
Para controlar los LEDs y el botón será necesario conectar los pines indicados a los pines apropiados de la RPi. Por ejemplo, el pin 5VDC puede conectarse al pin 2 de la RPi y el pin DIN puede conectarse a cualquiera de los pines de propósito general, como por ejemplo el 12, que se corresponde con GPIO18). La figura anterior muestra precisamente estas dos conexiones (cables rojo y amarillo). De la misma forma se conectan los pines GND. El pin DOUT también puede conectarse a cualquiera de los de propósito general, como por ejemplo el pin 03 (GPIO2).    
 
Para poder controlar los LEDs y el botón por programa es necesario instalar la librería gpiozero:   
```
sudo apt update 
sudo apt install python3-gpiozero
```
El siguiente programa muestra como usar el botón:  
```
from gpiozero import Button 
from signal import pause 
def say_hello(): 
   print ("Hello") 
button = Button (2) 
print ("Press button") 
button.when_pressed = say_hello 
pause()
```
El siguiente ejemplo muestra cómo controlar los LEDs:   
```
from gpiozero import Button 
from signal import pause 
import board 
import neopixel 
import time 
def lights(): 
      print ("Button pressed") 
#red 
pixels[0] =(255,0,0) 
time.sleep(2) 
pixels[0] = (0,0,0) 
#green 
pixels[1] = (0,255,0) 
time.sleep (2) 
pixels[1] = (0,0,0) 
#blue 
pixels[2] = (0, 0, 255) 
time.sleep (2) 
pixels[2] = (0,0,0) 
pixels = neopixel.NeoPixel (board.D18,5) 
button = Button (2) 
print ("Press button") 
button.when_pressed = lights pause()
```
La ejecución de estos programas requiere la instalación de las librerías siguientes:   
```
sudo pip3 install rpi_ws281x adafruit-circuitpython-neopixel 
sudo python3 -m pip install --force-reinstall adafruit-blinka
```
Ademas, los programas deben ejecutarse con permiso de administrador:   
```
sudo python3 ejemplo.py
```
Mas información sobre la librería gpiozero puede encontrarse aqui:  
https://gpiozero.readthedocs.io/en/stable/index.html

## 16. Integración de la RPi en la plataforma










# Anexo: Otros adaptadores Wifi
Hay otros muchos modelos de adaptadores Wifi, cuyo uso puede requerir la instalación en la RPi de los drivers correspondientes. Veamos aquí dos casos concretos, con las instrucciones necesarias para instalar sus drivers.   
## Antena Realtek RTL8188FTV (300 Mbps)
<img width="254" height="390" alt="image" src="https://github.com/user-attachments/assets/9da699e2-c950-46a4-8f7a-ae8de93b2f87" />

https://github.com/kelebek333/rtl8188fu
```
sudo apt-get install build-essential git dkms linux-headers-$(uname -r)
git clone https://github.com/kelebek333/rtl8188fu
sudo dkms install ./rtl8188fu
sudo cp ./rtl8188fu/firmware/rtl8188fufw.bin /lib/firmware/rtlwifi/
```

## Tp-link AC1300 
<img width="368" height="368" alt="image" src="https://github.com/user-attachments/assets/180cdb51-ed40-43e5-9b54-2e0b9b5e651f" />

```
sudo apt-get install -y raspberrypi-kernel-headers bc build-essential dkms git
git clone https://github.com/morrownr/88x2bu-20210702.git
cd 88x2bu*
sudo ./install-driver.sh
```
