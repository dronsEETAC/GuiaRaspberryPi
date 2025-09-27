Configuraci�n b�sica de la Raspberry Pi e instalaci�n en el dron Hexsoon EDU 

1. Introducci�n
Con frecuencia un dron lleva computaci�n a bordo, que le permite tomar decisiones durante el vuelo como, por ejemplo, cambiar el plan de vuelo previsto si detecta algo a trav�s de los sensores del dron. Naturalmente, una c�mara es uno de esos sensores m�s habituales. 
Una opci�n ideal para esto es la Raspberry Pi (RPi), a la que se le puede instalar una c�mara y adem�s puede conectarse a internet. En esta gu�a vamos a describir paso a paso c�mo configurar la RPi de manera que podamos conectarnos con ella desde la estaci�n de tierra a trav�s de internet, c�mo conectarle una c�mara o como conectarla con el autopiloto para controlar el dron desde un programa en Python. 
El kit RPi que vamos a utilizar es el que se muestra en la figura 1. Consiste en una RPi, un m�dulo de c�mara, un cable de alimentaci�n, cable para conectar la RPi a un monitor por HDMI, un adaptador WIFI y una tarjeta microSD. 

Figura 1: The RPi kit
En algunos de los pasos de esta gu�a se una una RPi contenida en una caja roja como la mostrada en la figura. Esta caja contiene los conectores para alimentar la RPi y para conectarla al autopiloto. Tambi�n contiene un bot�n y unos leds, que aprenderemos a controlar en la secci�n x de esta gu�a.


2. Instalaci�n del sistema operativo Raspberry Pi OS (antes conocido como Raspbian)
El sistema operativo debe instalarse en una tarjeta microSD que posteriormente se insertar� en la RPi. Para ello debemos conectar la microSD al ordenador desde el que se realizar� la instalaci�n. 
El c�digo del sistema operativo puede encontrarse aqu�:

https://www.raspberrypi.com/software/
En primer lugar debemos descargar Raspberry Pi Imager. Al ejecutarlo, podremos elegir el modelo de RPi, el sistema operativo que queremos instalar (que debe ser Raspberry Pi OS de 64 bits) y la unidad en la que se ha conectado la microSD. Para instalar el sistema operativo en la microSD desde nuestro port�til podemos utilizar dispositivos como los mostrados en la figura 2.



Figura 2: Dispositivos para instalar el sistema operativo en la microSD
La aplicaci�n preguntar� si queremos hacer alguna configuraci�n. Le diremos que no. Todas las configuraciones necesarias las haremos en los pasos siguientes. La figura 3 contiene im�genes que pueden ayudar en este proceso. 



Figura 3: Im�genes de la instalaci�n del sistema operativo en la microSD
3. Conexi�n de los dispositivos necesarios para realizar la configuraci�n
El siguiente paso es insertar la microSD en la que hemos instalado el sistema operativo en la ranura correspondiente de la RPi. Adem�s, debemos conectar la RPi a:
* Un monitor (con el cable HDMI-microHDMI)
* Un teclado (a uno de los puertos USB de la RPi)
* Un rat�n (a otro de los puertos USB)

Y naturalmente conectar la RPi a la alimentaci�n. La figura 4 muestra algunas im�genes que pueden ayudar en este paso.



Figura 4: Conexiones necesarias

4. Configuraci�n inicial 
Al conectar la RPi nos aparecer�n en pantalla una serie de men�s que nos permitir�n hacer algunas configuraciones iniciales b�sicas (pa�s, idioma, zona horaria) y tambi�n crear un usuario. Tambi�n nos preguntar� si queremos configurar una conexi�n a internet, pero nos saltaremos ese paso (Skip).
Despu�s nos pedir� que elijamos el navegador que debe usar. Finalmente, intentar� realizar una actualizaci�n del software pero no podr� hacerlo porque a�n no se ha configurado la conexi�n a internet. Acabado este proceso la RPi se reiniciar� y aparecer� la pantalla de bienvenida.
Las im�genes de la figura 5 pueden ayudar en este proceso.










Figura 5: Im�genes del proceso de configuraciones b�sicas iniciales
5. Configuraci�n de las conexiones a internet
La RPi puede conectarse a internet mediante un cable Ethernet, pero no vamos a usar esta conexi�n. Por otra parte, tiene una interfaz interna para la conexi�n a una Wifi (la que no quisimos configurar en el paso anterior). Esta interfaz permitir� a la RPi conectarse a internet y poder enviar a la estaci�n de tierra, por ejemplo, las im�genes que tome con la c�mara. La RPi tiene una segunda interfaz Wifi que se puede configurar como punto de acceso (hotspot). Este hotspot permitir� a dispositivos externos (por ejemplo, nuestro port�til) conectarse a la RPi para instalar software o para poner ese software en marcha. Veamos ahora c�mo configurar estas dos interfaces. Pero, en primer lugar, tenemos que conectar el adaptador Wifi a uno de los puertos USB de la RPi que quedan disponibles.
Las im�genes de la figura 6 ilustran los pasos para configurar la interfaz interna para conectar la RPi a una Wifi. Para iniciar la configuraci�n y que aparezca la pantalla de la figura 6� hay que clicar en el icono de las dos marcas rojas que hay en la esquina superior derecha, entre el icono de bluetooth y el icono de sonido. La figura 6a muestra que hay dos interfaces disponibles. La interna (BroadcomBCM43438) es la que vamos a configurar para conectar la RPi a una de las Wifis disponibles. Hay que elegir una de ellas e introducir la contrase�a, tal y como muestran las figuras 6a y 6b. Conviene ahora comprobar que la RPi tiene conexi�n a internet. Para ello se puede abrir un terminal y ejecutar el comando:
ping google.com
La segunda interfaz (Ralink RT5370) corresponde al adaptador Wifi que hemos conectado al puerto USB. Este modelo de adaptador (que se muestra en la figura 6c) tiene la ventaja de que el driver necesario ya est� incluido en el sistema operativo, de manera que solo es necesario conectar el adaptador para que est� operativo. En el caso de otros modelos de adaptador puede ser necesario instalar los drivers. En el anexo se describen algunos casos. 
Las figuras 6d, 6e y 6f muestran c�mo configurar el punto de acceso (hotspot), al cual le hemos puesto el nombre MiHotSpot. Este ser� el nombre de la Wifi que ser� visible por cualquier dispositivo externo que quiera conectarse a la RPi.
Finalmente, es necesario indicarle al sistema operativo que al iniciar la RPi active autom�ticamente el hotspot. Las figuras 6g, 6h y 6i muestran c�mo hacerlo. 
La figura 6j muestra las IP de cada una de las conexiones, una vez configuradas. Particularmente importante es la IP del hotspot (10.42.0.1), porque es la que debe usar el dispositivo externo que quiera conectarse a la RPi.









(a)
(b)


(c)
(d)



(e)
(f)






(h)
(i)


(j)


Figura 6: Im�genes del proceso de configuraci�n de las conexiones a internet
6. Conexi�n a la RPi desde un dispositivo externo v�a SSH 
Antes de realizar una conexi�n con la RPi, debemos indicar al sistema operativo que permita las conexiones SSH. Para eso, en un terminal de la RPi debe ejecutarse el comando siguiente:
sudo raspi-config

Aparecer� en pantalla un men� de opciones como el que muestra la figura 7, que tambi�n muestra las opciones a elegir para habilitar las conexiones SSH. La figura 7 no muestra la tercera pantalla en la que se indica si se desea poner el marcha el servidor SSH, en la que hay que contestar que si.



Figura 7: Configuraci�n necesaria para permitir conexiones v�a SSH
Ahora, cualquier dispositivo externo (nuestro m�vil o nuestro port�til) deber�a ver la Wifi MiHotSpot. Si nos conectamos a esa Wifi ahora podemos conectarnos a la RPi v�a SSH. La figura 8 muestra c�mo hacerlo utilizando PuTTY (esta aplicaci�n se puede descargar de https://putty.org/).


Figura 8: Conexi�n a la RPi v�a SSH usando PuTTY 

7. Instalaci�n de Python
Una vez hemos entrado en la RPi (usando las claves de usuario configuradas en su momento) podemos ya instalar el software que necesitemos. Empezaremos instalando Python. Desde un terminal de la RPi haremos (tener en cuenta que el segundo comando puede tardar unos minutos en ejecutarse):
sudo apt update 
sudo apt upgrade �y 
sudo apt install libgl1-mesa-glx 
sudo apt install python3-pip python3-dev 

Para comprobar que la instalaci�n se ha realizado correctamente, prepararemos un fichero hello.py con la sentencia:	
       
      print (�Hello world�)

Y ejecutamos el programa desde el terminal haciendo:

python3 hello.py

Desarrollar programas en Python en modo comandos puede parecer a muchos algo tedioso (aunque otros lo prefieren). Requiere aprender a usar editores de texto tales como pico o nano, que vienen instalados en el sistema operativo. En el caso de que tengamos un terminal conectado a la RPi, entonces podemos usar Thonny Python IDE, que tambi�n est� instalado en el sistema operativo y nos ofrece un entorno de desarrollo m�s amigable, basado en ventanas.

8. Conexi�n de la RPi con el simulador SITL
Uno de los objetivos de instalar la RPi a bordo del dron es poner controlar el autopiloto mediante nuestros programas ejecutados en la RPi. Esto nos permitir� tomar decisiones de forma r�pida sin necesidad de que intervenga la estaci�n de tierra. Por ejemplo, podemos hacer que el dron aterrice inmediatamente si detecta la imagen de un determinado objeto (usando la c�mara que aprenderemos a instalar en las pr�ximas secciones). Esta operaci�n tambi�n puede hacerse sin una RPi, haciendo que un transmisor de video env�e la imagen de la c�mara a la estaci�n de tierra, que analizar� la imagen y en caso de detectar el objeto en cuesti�n se enviar� la orden de aterrizaje a trav�s del enlace de telemetr�a. Obviamente, esta segunda alternativa va a ser m�s lenta, lo cual puede hacerla inviable en determinadas operaciones que requieren acci�n inmediata.
Para controlar el dron usaremos la librer�a dronLink, que se est� desarrollando en la UPC. Esta librer�a se encuentra en un repositorio de github, convenientemente documentada:

https://github.com/dronsEETAC/dronLink

El v�deo siguiente muestra los pasos que hay que dar para instalar la librer�a en la RPi y ejecutar un programa que controle el simulador SITL de Ardupilot. Los pasos son los siguientes:
1. Conectar el port�til al hotspot de la RPi y tambi�n a la misma red local a la que est� conectada la RPi. Para realizar estar dos conexiones necesitaremos un adaptador WiFi externo.
2. Poner en marcha Mission Planner y una instancia del simulador SITL.
3. Entrar en la RPi y crear una carpeta para desarrollar la demo.
4. Clonar en esa carpeta el repositorio en el que est� la librer�a dronLink. El repositorio clonado tiene muchos ficheros que incluyen programas de test de las funciones de la librer�a y tambi�n programas m�s complejos con demostradores de uso de la librer�a. En realidad, para usar la librer�a solo se necesita el fichero dron.py y la carpeta modules. En la demo se usa tambi�n el fichero test_libreria.py que contiene un ejemplo de programa que usa algunas funciones b�sicas de la librer�a.
5. Averiguar la IP asignada al port�til, en la misma red local a la que est� conectada la RPi y poner esa IP en el programa de test.
6. Crear un entorno virtual para instalar las librer�as que necesite, que en este caso solo es la librer�a pymavlink, que necesita dronLink para implementar las funciones que est�n en la carpeta modules. ATENCI�N: hay librer�as que no pueden instalarse en un entorno virtual y deben instalarse con apt install (como se ha hecho antes con python3-pip, por ejemplo). Esas librer�as no ser�an accesibles cuando se trabaja en un entorno virtual. Si queremos que esas librer�as tambi�n puedan ser usadas desde un entorno virtual, entonces hay que crear el entorno virtual de la siguiente forma:

python3 -m venv venv --system-site-packages

Puesto que m�s Adelante en este tutorial vamos a usar una librer�a con estas caracter�sticas, conviene crear ya el entorno virtual como se acaba de indicar y no como muestra el v�deo.

7. Ejecutar el programa test_libreria.py

Usando esta estrategia podemos probar con el simulador SITL nuestros programas ejecut�ndolos directamente en la RPi. Esos mismos programas funcionar�n correctamente con el dron real, cambiando solo una l�nea de c�digo, que es la que conecta la RPi con el simulador o con el autopiloto. Pero antes tenemos que aprender a conectar la RPi con el autopiloto.
 
9. Conexi�n de la RPi con el autopiloto
Para realizar este paso usaremos una RPi montada en la caja roja (ver apartado 1). La RPi y el autopiloto se van a comunicar mediante un enlace de comunicaci�n serie que usa el protocolo UART (Universal Asynchronous Receiver/Transmitter). Este protocolo establece unas normas de comunicaci�n bit a bit entre dos dispositivos. Es un protocolo muy simple que solo necesita dos hilos (para enviar informaci�n en ambos sentidos) y una conexi�n com�n a tierra, tal y como muestra la figura.



Se puede encontrar m�s informaci�n sobre este protocolo aqu�:
https://www.analog.com/en/analog-dialogue/articles/uart-a-hardware-communication-protocol.html 

Tanto la RPi como el autopiloto tienen puertos para comunicaci�n UART. En concreto, por parte del autopiloto usaremos el puerto TELEM2, cuya especificaci�n se muestra en la figura:



De los 6 pines del puerto, el protocolo UART solo usa 3: GND (los pines en los extremos), TX (pin 2) y RX (pin 3). Estos son los pines que deberemos conectar a la RPi. No es necesario conectar el pin VCC porque tanto el autopiloto como la RPi estar�n alimentadas por la misma placa de distribuci�n de energ�a.
The RPi 4 has 40 pins (GPIO) available to interact with peripherals. Figure 2 shows the position of the pins on the board and the pin configuration. Some of the pins have a specific purpose (for example pins that supply 5 volts, or pins for serial communication). But several are general purpose and we can use them to control the LEDs and the button.

La configuraci�n de pines de la RPi se muestra en la figura. Esta figura muestra que los pines dedicados a la comunicaci�n serie son GPIO14 y GPIO15. En realidad, otros pines pueden configurarse para comunicaci�n serie, de manera que la RPi puede tener hasta 4 canales de comunicaci�n serie con diferentes dispositivos. 




Por otro lado, la caja roja tiene un conector (mostrado en la figura) que es el que se usar� para conectar la RPi con el autopiloto mediante el cable de telemetr�a. Si numeramos los pines de ese conector del 1 al 4, de izquierda a derecha seg�n la imagen, al hacer la conexi�n mediante el cable de telemetr�a,  podemos ver que la se�al GND llegar� al pin 4, la se�al RX del autopiloto llegar� al pin 3 y la se�al TX llegar� al pin 2. 






Por tanto, tal y como muestra la figura, ser� necesario conectar el pin 4 a cualquiera de los pines de la RPi etiquetados como GND (por ejemplo, el 14), el pin 3 a GPIO14 (RX de autopiloto con TX de RPi) y el pin 2 a GPIO15 (TX de autopiloto con RX de RPi). 



Una vez establecidas esas conexiones, solo queda realizar un par de configuraciones. Por una parte, hay que indicarle al autopiloto que usar� TELEM2 para comunicaci�n UART. Los dos par�metros que hay que cambiar (usando Mission Planner) son:
SERIAL2_PROTOCOL�= 2 (el valor por defecto) 
SERIAL2_BAUD�= 921 para comunicarse con la RPi a una velocidad de 921600 baud.


Por otro lado hay que configurar el puerto UART en la RPi. Para ello volvemos a ejecutar en la RPi:
sudo raspi-config

Y seleccionamos las opciones mostradas en las im�genes. 




Finalmente, hay que contestar NO cuando nos pregunte �Would you like a login shell to be accessible over serial?�, y hay que contestar SI cuando nos pregunte �Would you like the serial port hardware to be enabled?�.

Acabado este proceso es necesario reiniciar la RPi. Ahora ya podemos ejecutar nuestros programas para enviar directamente las ordenes al autopiloto. Para ello, hay que cambiar el connection_string y la velocidad en la llamada a la operaci�n de conexi�n:

connection_string = �/dev/serial0�
baud = 921600
dron.connect(connection_string, baud)

Para probar el correcto funcionamiento de la conexi�n podemos usar el programa test_telemetria.py que simplemente se conecta al autopiloto (habr� que cambiar los par�metros de la conexi�n tal y como se ha indicado) y obtiene varios datos de telemetr�a (naturalmente, no podemos probar el programa test_libreria.py hasta que no estemos en disposici�n de volar el dron en el droneLab).

10. Captura del stream de video de una webcam

En esta y las siguientes secciones aprenderemos a capturar el stream de video de una c�mara conectada a la RPi y enviar ese stream de video a una estaci�n de tierra. 
Empezaremos instalando la librer�a OpenCV que nos ayudar� a capturar y procesar las im�genes de la c�mara. La instalaci�n debe hacerse dentro del entorno virtual que hemos creado (tal y como se mostr� en el v�deo del apartado 0). 
pip install opencv-python
Ahora Podemos conectar una WebCam a cualquiera de los puertos USB de la RPi. Podemos usar el c�digo siguiente para capturar y mostrar en pantalla el stream de video de la WebCam. Es importante tener en cuenta que si bien las instalaciones de las librer�as o la edici�n del programa puede hacerse indistintamente desde el port�til (conectado a la RPi) o desde el teclado conectado directamente a la RPi, la puesta en marcha del programa debe hacerse necesariamente desde el teclado de la RPi, para que el c�digo encuentre el monitor en el que mostrar� el stream de video. 
El programa simplemente abre la c�mara y entra en un bucle en el que captura un frame, pasa la imagen a escala de grises (una de las m�ltiples cosas que se pueden hacer con opencv) y lo muestra en pantalla.

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

Figura 9: C�digo para mostrar el stream de video de la web cam

11. Transmisi�n del stream de video por MQTT
Como es obvio el programa anterior solo nos sirve para comprobar que estamos capturando correctamente el stream de video, pero no nos va a servir cuando el dron est� volando, a menos que seamos capaces de montar tambi�n en el dron un monitor de v�deo conectado a la RPi, que adem�s debe ser grande para poder verlo desde lejos (cosa que nosotros no hemos conseguido). 
Lo correcto es enviar el stream de video a la estaci�n de tierra para que se muestre en la pantalla del port�til en el que se ejecuta esa estaci�n de tierra. En este apartado vamos a aprender como hacer esto usando el protocolo MQTT y las librer�as adecuadas.
Veamos primero el c�digo que debe ejecutarse en la RPi (emisor).


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
        print("No se pudo abrir la c�mara")
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

El c�digo emisor usa la librer�a opencv para capturar (en esta ocasi�n cada 200 milisegundos) un frame. El frame no lo pasa a escala de grises pero si que ajusta el nivel de calidad a 40 para que el volumen de informaci�n a enviar no sea excesivamente grande. El nivel de calidad puede subirse hasta 100, con lo que la imagen que se recibe ser� de m�s calidad pero la fluidez del stream ser� menos por saturaci�n de la comunicaci�n. La fluidez de la comunicaci�n tambi�n se puede controlar haciendo que el c�digo env�e menos frames por segundo, ajustando el valor del par�metro de time.sleep (0.2).
Lo importante es que cuando tenemos un frame preparado se publica en un br�ker con el topic "raspi/camera/stream". Este es el principio b�sico de la comunicaci�n usando el protocolo MQTT. El emisor publica informaci�n asign�ndole un topic (un string cualquiera). Esa informaci�n la recibe el br�ker (que es un servidor MQTT). El br�ker suministrar� esa informaci�n a cualquier programa (en nuestro caso, el receptor) que se haya suscrito al topic. Naturalmente, tanto el emisor como el receptor deben estar conectados a internet y deben usar el mismo br�ker para las publicaciones y suscripciones. En el ejemplo, se usa el br�ker p�blico y gratuito "broker.hivemq.com". Hay otros muchos br�ker p�blicos y gratuitos, e incluso puede hacerse la instalaci�n de un br�ker en alg�n servidor propio. 
Puede observarse que el c�digo, antes de publicar cada frame publica un mensaje de texto con el topic "raspi/camera/message". De esta manera se ilustra c�mo enviar otros tipos de informaci�n usando el protocolo MQTT.
Para poner en marcha el programa emisor ser� necesario instalar en el entorno virtual la librer�a paho-mqtt:
pip install paho-mqtt

Veamos ahora el c�digo del receptor, que se ejecuta en el port�til que act�a como estaci�n de tierra.

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
    client.subscribe([(TOPIC_MSG, 0), (TOPIC_FRAME, 0)])  # suscripci�n doble

    try:
        client.loop_forever()
    except KeyboardInterrupt:
        pass
    finally:
        cv2.destroyAllWindows()

if __name__ == "__main__":
    main()

El emisor tambi�n usa las librer�as opencv y paho-mqtt. Debe usar el mismo br�ker que el emisor y se suscribe a los dos topic (el correspondiente a los mensajes de texto y el correspondiente a los frames del stream de video). Cada vez que el receptor recibe un mensaje ejecuta la funci�n on_message. Alli averigua primero que tipo de mensaje es, mirando el topic (solo ha dos opciones). Si es un mensaje de texto lo recupera y lo escribe en consola. Si es un frame lo recupera y lo muestra en una ventana que se mostrar� en la pantalla del port�til.

Si se ponen en marcha estos c�digos se observar� que el stream de video no es especialmente fluido. Eso es debido a que MQTT no est� pensado para enviar grandes cantidades de datos como pueden ser los frames de video. Est� pensado para mensajes m�s cortos, como los que suelen enviarse en un escenario IoT. No obstante, como ya se ha indicado antes, la fluidez de la implementaci�n mostrada puede mejorarse ajustando adecuadamente el nivel de calidad de los frames que se env�an y la frecuencia con la que se env�an los frames.
En todo caso, veamos ahora una tecnolog�a alternativa para implementar la comunicaci�n del stream de video, que va a mejorar notablemente la fluidez.

12. Transmisi�n del stream de video por WebSockets
Un websocket es un canal de comunicaci�n entre el emisor y el receptor a trav�s de internet pero que no requiere de ning�n otro intermediario, como es el caso del br�ker necesario en el caso de MQTT. Esto hace que para el caso de la transmisi�n de stream de video sea una opci�n mejor. 
El protocolo de websocket se apoya en el protocolo HTTP sobre el que funciona la web. Por eso, en la comunicaci�n debe hacer un servidor y un cliente que se conecta al servidor. En nuestro caso, el servidor ser� el emisor que se ejecuta en la RPi (que sirve el stream de video a quien se conecte). El cliente ser� el receptor, que se ejecuta en el port�til.
Veamos el c�digo del servidor.

import cv2
import base64
import asyncio
import websockets

async def video_stream(websocket):
    cap = cv2.VideoCapture(0)  # c�mara
    if not cap.isOpened():
        print("No se pudo abrir la c�mara")
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

El servidor abre un websocket en el puerto 8765, al que puede conectarse cualquier dispositivo externo. En el momento en el que alguien se conecta se pone en marcha el c�digo de la funci�n video_stream. Esa funci�n es b�sicamente un bucle que captura un frame y lo env�a por el websocket. En este caso, no hay topic. Tampoco hay mensaje de texto previo, aunque podr�a enviarse uno sin problemas a trav�s del websocket. Observese que en este caso no reducimos la calidad del frame y adem�s enviamos 30 frames por segundo, a pesar de lo cual veremos que la fluidez del stream de video es mejor que la del caso de MQTT.
Para que este c�digo puede ejecutarse es necesario instalar la librer�a websockets en el entorno virtual:
pip install websockets


Veamos ahora el c�digo del cliente:

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

Este c�digo tambi�n requiere de la instalaci�n de la librer�a websockets. El c�digo se conecta al websocket y entra en un bucle en el que recibe frames y los muestra en una ventana que aparece en la pantalla del port�til. La conexi�n debe hacerse al puerto 8765 del servidor. Para ello hay que averiguar la IP que le ha sigo asignada a la RPi en la red local a la que se ha conectado, que debe ser la misma a la que se ha conectado el port�til. Para averiguar la IP basta ejecutar el comando ifconfig en un terminal de la RPi.
Si se ponen en marcha estos c�digos podr� observarse que efectivamente la fluidez del stream de v�deo es mucho mejor que en el caso de la comunicaci�n v�a MQTT.

No obstante, hay herramientas espec�ficas para implementar video streaming con resultados a�n mejores, pero de mayor complejidad, por lo que quedan fuera del alcance de este tutorial.

13. Instalaci�n del m�dulo de c�mara 3 de la RPi
La calidad de la imagen puede mejorarse si se sustituye la webcam por un m�dulo de c�mara espec�fico para la RPi. En este apartado vamos a ver como conectar el modulo de c�mara, versi�n 3. En el apartado siguiente veremos lo mismo, pero para la versi�n 2.

El m�dulo de c�mara para RPi (versi�n 3) muestra en la figura 10, La figura 10 muestra c�mo instalar esta c�mara en la RPi. Es muy importante prestar atenci�n a la correcta orientaci�n de la cinta de conexi�n. Las letras que aparecen en esa cinta nos pueden ayudar a no cometer errores.



Figura 10: Instalaci�n del m�dulo de c�mara

Una vez instalada, podemos comprobar que funciona correctamente ejecutando el comando siguiente, que nos mostrar� en la pantalla conectada a la RPi el stream de video durante unos segundos:

libcamera-hello


Para poder utilizar esta c�mara en un programa en Python tenemos que instalar en el entorno virtual la librer�a picamara2:

sudo apt install �y python3-picamera2

Esta librer�a es un ejemplo de librer�a que no se puede instalar en un entorno virtual. Por tanto, si queremos usarla dentro de un entorno virtual, ese entorno debe haber sido creado seg�n se ha explicado en el paso 6 del apartado 8. 

Para verificar que la c�mara funciona correctamente podemos ejecutar el c�digo de la figura 11, que debe mostrar el stream de video. Pero antes debemos desconectar la web cam que hemos usado en un paso anterior. Adem�s, el c�digo que captura el stream de video de la webcam no va a funcionar si tenemos conectado el m�dulo de c�mara de la RPi.

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

Figura 11: Codigo que muestra el stream de video usando el  m�dulo de c�mera de la RPi

De nuevo, este programa hay que ejecutarlo en el entorno virtual que hemos creado y hay que hacerlo desde el teclado conectado directamente a la RPi, para que el stream de v�deo aparezca en el monitor conectado a la RPi.

M�s informaci�n sobre la librer�a picamera2 puede obtenerse aqu�:

https://github.com/raspberrypi/picamera2


14. Instalaci�n del m�dulo de c�mara versi�n 2 de la RPi
Veamos tambi�n c�mo conectar una c�mara RPi pero versi�n 2, que tiene un proceso de instalaci�n diferente. La c�mara se conecta a la RPi exactamente igual que la versi�n 3 (recu�rdese la importancia de orientar bien la cinta de conexi�n). Despu�s, es necesario a�adir al fichero /boot/firmware/config.txt la l�nea siguiente:
start_x=1 
Esta edici�n habr� que hacerla con permisos de administrador (sudo).
Finalmente hay que instalar las librer�as siguientes:

sudo apt install libraspberrypi-bin libraspberrypi0 libraspberrypi-dev 
sudo pip3 install picamera

Despu�s de reiniciar la RPi el mismo programa que se mostr� en el apartado 10 para capturar el stream de video de la webcam deber�a funcionar con la c�mara RPi versi�n 2.

15. Configuraci�n de los LEDs y del bot�n
Tal y como se mostr� en el apartado 1, la caja roja contiene 5 LEDs RGB y un bot�n. Estos dispositivos pueden controlarse por programa para hacer, por ejemplo, que al pulsar el bot�n se tome una foto con la c�mara o encender los LEDS de colores espec�ficos para se�alar determinadas circunstancias (como, por ejemplo, nivel bajo de bater�a en el dron o conexi�n a internet disponible). 


Los 5 LEDs RGB se controlan mediante 4 se�ales (los 5 pines de la derecha, que se muestran en la figura. Los pines son: GND, DIN (se�al de control), 5VDC y nuevamente GND. Estos pones est�n claramente identificados en la placa negra. Por otro lado el bot�n se controla con dos pines que est�n en el lado izquierdo: GND y DOUT (hay dos pines m�s que no usaremos). 
Para controlar los LEDs y el bot�n ser� necesario conectar los pines indicados a los pines apropiados de la RPi. Por ejemplo, el pin 5VDC puede conectarse al pin 2 de la RPi y el pin DIN puede conectarse a cualquiera de los pines de prop�sito general, como por ejemplo el 12, que se corresponde con GPIO18). La figura muestra precisamente estas dos conexiones (cables rojo y amarillo). De la misma forma se conectan los pines GND. El pin DOUT tambi�n puede conectarse a cualquiera de los de prop�sito general, como por ejemplo el pin 03 (GPIO2).
Para poder controlar los LEDs y el bot�n por programa es necesario instalar la librer�a gpiozero:
sudo apt update 
sudo apt install python3-gpiozero

El siguiente programa muestra como usar el bot�n:

from gpiozero import Button 
from signal import pause 
def say_hello(): 
   print ("Hello") 
button = Button (2) 
print ("Press button") 
button.when_pressed = say_hello 
pause()

El siguiente ejemplo muestra c�mo controlar los LEDs:
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

La ejecuci�n de estos programas requiere la instalaci�n de las librer�as siguientes:
sudo pip3 install rpi_ws281x adafruit-circuitpython-neopixel 
sudo python3 -m pip install --force-reinstall adafruit-blinka

Ademas, los programas deben ejecutarse con permiso de administrador:
sudo python3 ejemplo.py

Mas informaci�n sobre la librer�a gpiozero puede encontrarse aqui: https://gpiozero.readthedocs.io/en/stable/index.html

16. Integraci�n de la RPi en la plataforma










Anexo
Otros adaptadores Wifi
Hay otros muchos modelos de adaptadores Wifi, cuyo uso puede requerir la instalaci�n en la RPi de los drivers correspondientes. Veamos aqu� dos casos concretos, con las instrucciones necesarias para instalar sus drivers.




Antena Realtek RTL8188FTV (300 Mbps)
https://github.com/kelebek333/rtl8188fu

sudo apt-get install build-essential git dkms linux-headers-$(uname -r)
git clone https://github.com/kelebek333/rtl8188fu
sudo dkms install ./rtl8188fu
sudo cp ./rtl8188fu/firmware/rtl8188fufw.bin /lib/firmware/rtlwifi/


Tp-link AC1300

sudo apt-get install -y raspberrypi-kernel-headers bc build-essential dkms git
git clone https://github.com/morrownr/88x2bu-20210702.git
cd 88x2bu*
sudo ./install-driver.sh

