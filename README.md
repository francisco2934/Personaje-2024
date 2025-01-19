# Personaje-2024
![image](https://github.com/user-attachments/assets/e29b5e0f-570b-4209-835e-caab9e2f7b16)

## Nombre del Personaje
Mickey Mouse
## Creador
Francisco Javier Torres Suarez


## Explicacion de funcionamiento

El código permite que Mickey Mouse interactúe con el entorno de manera divertida. Cuando detecta movimiento, Mickey mueve una de sus orejas mientras se encienden dos LEDs que cambiean de colores automaticamente, simulando una reacción animada. Además, se reproduce una melodía navideña para ambientar el momento. Si no se detecta movimiento, las orejas vuelven a su posición inicial y los LEDs se apagan. 

## Materiales a utilizar
|Material|Imagen|Cantidad|Precio|
|--|--|--|--|
|ESP32|<img src="https://github.com/user-attachments/assets/2fb063fd-c57e-492e-98c4-027652228051" width="50" />|1|120|
|Servo|<img src="https://github.com/user-attachments/assets/d67ff593-fb93-4d22-81e1-a65a9ee8abc4" width="50" />|1|50|
|Led|<img src="https://github.com/user-attachments/assets/cb4cb7c3-463b-4c8d-8a79-3035dc46dd4f" width="50" />|2|40|
|Buzzer|<img src="https://github.com/user-attachments/assets/cc56f6cf-d453-4dc7-9f74-329fd744fc03" width="50" />|1|10|

## Software a utilizar
|Software|version|
|--|--|
|Thonny|	3.3.0|

## Dibujo del personaje
Imagen hecho a mano o software

## Imagenes de procedimeinto de creacion
![Imagen de WhatsApp 2024-12-06 a las 13 43 45_a96b9cb2](https://github.com/user-attachments/assets/d5690fe8-ee90-47c3-9efc-5c937910144e)
![Imagen de WhatsApp 2024-12-06 a las 13 42 34_d7a2d27b](https://github.com/user-attachments/assets/819cfc73-3f6c-42c6-abee-f15bb1829fb0)
![Imagen de WhatsApp 2024-12-06 a las 13 42 33_657377ee](https://github.com/user-attachments/assets/89399e8c-c246-4c0f-9628-10164fcdac44)


## Enlaces de la simulacion de wokwi de personaje:
https://wokwi.com/projects/420476867650120705

## Videos
https://drive.google.com/drive/folders/1ELphYLnZHq20B_ujfN3cbAIVe-y9Yj27?usp=drive_link

## Video de TikTok
https://vm.tiktok.com/ZMkdrtd5E/

## Videos de practicas de clase
https://drive.google.com/drive/folders/1fvg5iQsfrLAGnyksl76WLwoRzDL3q6rc?usp=drive_link

## Codigo de thonny
from machine import Pin, PWM, time_pulse_us
from time import sleep

#Configuración del buzzer en GPIO 5
buzzer = PWM(Pin(5))
buzzer.duty(0)  # Asegurar que el buzzer esté apagado al inicio

#Configuración de los LEDs en GPIO 19
led = Pin(19, Pin.OUT)
led.off()  # Asegurar que los LEDs estén apagados al inicio

#Configuración del servo en GPIO 17
servo = PWM(Pin(21), freq=50)  # Frecuencia estándar para servos

#Función para mover el servo a un ángulo específico (0 a 180 grados)
def move_servo(angle):
    min_duty = 20  # Ajusta estos valores según las especificaciones del servo
    max_duty = 120
    duty = int(min_duty + (angle / 180) * (max_duty - min_duty))
    servo.duty(duty)

#Configuración del sensor ultrasónico
TRIG_PIN = 17  # GPIO para el TRIG
ECHO_PIN = 18  # GPIO para el ECHO

#Configuración de pines del sensor ultrasónico
trig = Pin(TRIG_PIN, Pin.OUT)
echo = Pin(ECHO_PIN, Pin.IN)

#Notas musicales con sus frecuencias (en Hz)
NOTES = {
    "B0": 31,
    "C1": 33, "D1": 37, "E1": 41, "F1": 44, "G1": 49, "A1": 55, "B1": 62,
    "C2": 65, "D2": 73, "E2": 82, "F2": 87, "G2": 98, "A2": 110, "B2": 123,
    "C3": 131, "D3": 147, "E3": 165, "F3": 175, "G3": 196, "A3": 220, "B3": 247,
    "C4": 262, "D4": 294, "E4": 330, "F4": 349, "G4": 392, "A4": 440, "B4": 494,
    "C5": 523, "D5": 587, "E5": 659, "F5": 698, "G5": 784, "A5": 880, "B5": 988,
    "C6": 1047, "D6": 1175, "E6": 1319, "F6": 1397, "G6": 1568, "A6": 1760, "B6": 1976,
    "C7": 2093, "D7": 2349, "E7": 2637, "F7": 2794, "G7": 3136, "A7": 3520, "B7": 3951,
    "C8": 4186, "D8": 4699
}

#Melodía de "Jingle Bells" con las notas y duraciones
melody = [
    ("E4", 8), ("E4", 8), ("E4", 4), 
    ("E4", 8), ("E4", 8), ("E4", 4), 
    ("E4", 8), ("G4", 8), ("C4", 8), ("D4", 8), ("E4", 2),
    ("F4", 8), ("F4", 8), ("F4", 8), ("F4", 8), ("F4", 8), ("E4", 8), ("E4", 8), 
    ("E4", 8), ("E4", 8), ("D4", 8), ("D4", 8), ("E4", 8), ("D4", 4), ("G4", 4)
]

#Función para reproducir una nota
def play_tone(note, duration):
    if note == "":  # Silencio
        buzzer.duty(0)
    else:
        buzzer.freq(NOTES[note])
        buzzer.duty(512)  # Volumen (ajustar si necesario)
    sleep(duration)
    buzzer.duty(0)  # Apagar el buzzer después de la nota
    sleep(0.05)  # Pequeña pausa entre notas

#Función para medir distancia con el sensor ultrasónico
def medir_distancia():
    trig.off()
    sleep(0.002)  # Pausa de 2 µs
    trig.on()
    sleep(0.00001)  # Pulso de 10 µs
    trig.off()
    
    duracion = time_pulse_us(echo, 1, 30000)  # Tiempo en µs (máx. 30 ms)
    if duracion <= 0:
        return -1  # Error en la medición
    distancia = (duracion / 2) * 0.0343  # Convertir a cm
    return distancia

#Bucle principal
try:
    while True:
        distancia = medir_distancia()  # Medir distancia
        
        if distancia == -1:
            print("Error: No se detecta distancia válida.")
            led.off()  # Apagar LEDs si no hay distancia válida
        else:
            print("Distancia: {:.2f} cm".format(distancia))
            
            if distancia <= 20:  # Reproducir melodía y encender LEDs si está a menos de 20 cm
                print("¡Objeto detectado a menos de 20 cm! Reproduciendo melodía y encendiendo LEDs...")
                led.on()  # Encender LEDs
                
                # Reproducir la melodía y mover el servo al mismo tiempo
                for note, duration in melody:
                    move_servo(90)  # Mover el servo a 90 grados
                    play_tone(note, 0.5 / duration)  # Reproducir melodía
                    move_servo(0)  # Mover el servo a la posición inicial después de cada nota
                    sleep(0.1)  # Pausa para el movimiento del servo

            else:
                buzzer.duty(0)  # Apagar buzzer
                led.off()  # Apagar LEDs si está fuera del rango
                move_servo(0)  # Asegurar que el servo esté en la posición inicial

        sleep(0.1)  # Breve pausa antes de la siguiente medición
except KeyboardInterrupt:
    buzzer.deinit()
    servo.deinit()
    print("Programa detenido")
 
## Codigo node-red
import network
from umqtt.simple import MQTTClient
from machine import Pin, PWM, time_pulse_us
from time import sleep

# Propiedades para conectar a un cliente MQTT
MQTT_BROKER = "broker.hivemq.com"
MQTT_USER = ""
MQTT_PASSWORD = ""
MQTT_CLIENT_ID = "esp32_control"
MQTT_TOPIC = "utng/arg/dht11"  # Tópico para la distancia
MQTT_LED_TOPIC = "utng/arg/led"  # Tópico para el control del LED
MQTT_MUSIC_TOPIC = "utng/arg/music"  # Tópico para controlar el buzzer
MQTT_SERVO_TOPIC = "utng/arg/servo"  # Tópico para controlar el servo
MQTT_PORT = 1883

# WiFi
sta_if = None

# Notas musicales con sus frecuencias (en Hz)
NOTES = {
    "B0": 31,
    "C1": 33, "D1": 37, "E1": 41, "F1": 44, "G1": 49, "A1": 55, "B1": 62,
    "C2": 65, "D2": 73, "E2": 82, "F2": 87, "G2": 98, "A2": 110, "B2": 123,
    "C3": 131, "D3": 147, "E3": 165, "F3": 175, "G3": 196, "A3": 220, "B3": 247,
    "C4": 262, "D4": 294, "E4": 330, "F4": 349, "G4": 392, "A4": 440, "B4": 494,
    "C5": 523, "D5": 587, "E5": 659, "F5": 698, "G5": 784, "A5": 880, "B5": 988,
    "C6": 1047, "D6": 1175, "E6": 1319, "F6": 1397, "G6": 1568, "A6": 1760, "B6": 1976,
    "C7": 2093, "D7": 2349, "E7": 2637, "F7": 2794, "G7": 3136, "A7": 3520, "B7": 3951,
    "C8": 4186, "D8": 4699
}

# Melodía de "Jingle Bells" con las notas y duraciones
melody = [
    ("E4", 8), ("E4", 8), ("E4", 4), 
    ("E4", 8), ("E4", 8), ("E4", 4), 
    ("E4", 8), ("G4", 8), ("C4", 8), ("D4", 8), ("E4", 2),
    ("F4", 8), ("F4", 8), ("F4", 8), ("F4", 8), ("F4", 8), ("E4", 8), ("E4", 8), 
    ("E4", 8), ("E4", 8), ("D4", 8), ("D4", 8), ("E4", 8), ("D4", 4), ("G4", 4)
]

# Inicializar componentes
led = Pin(19, Pin.OUT)
led.value(0)
buzzer = PWM(Pin(5))
buzzer.duty(0)
servo = PWM(Pin(21), freq=50)

# Función para conectar a WiFi
def conectar_wifi():
    global sta_if
    print("Conectando a WiFi...")
    sta_if = network.WLAN(network.STA_IF)
    sta_if.active(True)
    sta_if.connect('UTNG_GUEST', 'R3d1nv1t4d0s#UT')  # Reemplaza con tu contraseña
    while not sta_if.isconnected():
        print(".", end="")
        sleep(0.5)
    print("\nWiFi Conectada!")

# Función para medir distancia
def medir_distancia():
    trig = Pin(17, Pin.OUT)
    echo = Pin(18, Pin.IN)
    
    trig.off()
    sleep(0.002)
    trig.on()
    sleep(0.00001)
    trig.off()

    duracion = time_pulse_us(echo, 1, 30000)
    if duracion <= 0:
        return -1
    return (duracion / 2) * 0.0343

# Función para conectar al broker MQTT
def conectar_mqtt():
    try:
        client = MQTTClient(MQTT_CLIENT_ID, MQTT_BROKER, port=MQTT_PORT,
                            user=MQTT_USER, password=MQTT_PASSWORD)
        client.connect()
        print(f"Conectado a MQTT Broker: {MQTT_BROKER}")
        return client
    except Exception as e:
        print(f"Error al conectar al broker MQTT: {e}")
        return None

# Función para manejar mensajes MQTT
servo_activado = False
def llegada_mensaje(topic, msg):
    global servo_activado
    print(f"Mensaje recibido -> Tópico: {topic.decode()}, Mensaje: {msg.decode()}")
    
    # Control del LED
    if topic == MQTT_LED_TOPIC.encode():
        led.value(1 if msg == b'true' else 0)
    
    # Control del buzzer (melodía)
    elif topic == MQTT_MUSIC_TOPIC.encode():
        if msg == b'true':
            print("Reproduciendo melodía...")
            for note, duration in melody:
                buzzer.freq(NOTES[note])
                buzzer.duty(512)
                sleep(0.2 / duration)  # Tiempo reducido para acelerar
                buzzer.duty(0)
                sleep(0.05)
        elif msg == b'false':
            buzzer.duty(0)
    
    # Control del servo
    elif topic == MQTT_SERVO_TOPIC.encode():
        servo_activado = msg == b'true'

# Función para movimiento continuo del servo
def movimiento_continuo_servo():
    global servo_activado
    potencia = 60  # Puedes ajustar este valor de potencia (40 a 115)
    
    if servo_activado:
        for angle in range(40, 100):  # Movimiento hacia adelante
            duty = int(potencia + (angle / 180) * (115 - potencia))  # Ajuste de potencia
            servo.duty(duty)
            sleep(0.02)
        for angle in range(100, 40, -1):  # Movimiento hacia atrás
            duty = int(potencia + (angle / 180) * (115 - potencia))  # Ajuste de potencia
            servo.duty(duty)
            sleep(0.02)
    else:
        servo.duty(0)  # Apagar servo si no está activo


# Subscripción a tópicos MQTT
def subscribir(client):
    client.set_callback(llegada_mensaje)
    client.subscribe(MQTT_TOPIC)
    client.subscribe(MQTT_LED_TOPIC)
    client.subscribe(MQTT_MUSIC_TOPIC)
    client.subscribe(MQTT_SERVO_TOPIC)
    print("Subscripción completa.")

# Conexión a WiFi y MQTT
conectar_wifi()
client = conectar_mqtt()
if client:
    subscribir(client)

# Ciclo principal
while True:
    try:
        # Verificar conexión WiFi
        if not sta_if.isconnected():
            print("Reconectando WiFi...")
            conectar_wifi()

        # Verificar conexión MQTT
        if client:
            client.check_msg()

        # Medición de distancia
        distancia = medir_distancia()
        if distancia >= 0:
            print(f"Distancia: {distancia} cm")
            # Publicar la distancia en el tópico MQTT
            client.publish(MQTT_TOPIC, str(distancia))
        
        # Mover el servo si está activado
        movimiento_continuo_servo()
        
        sleep(1)

    except Exception as e:
        print(f"Error en el ciclo principal: {e}")
        sleep(1)

## Imagenes de Node-RED
![image](https://github.com/user-attachments/assets/a1b6529c-989d-4afa-9da7-a3822860a528)
![image](https://github.com/user-attachments/assets/e2c70979-0e1c-4a73-85ac-c930f6cc92e5)


## Imagen de la captura de cisco c
![image](https://github.com/user-attachments/assets/571b9ac8-cbb7-4cb7-baf7-4079c448bd2c)
![image](https://github.com/user-attachments/assets/55891139-7460-4ec5-b6f5-747d7e50b86b)
![image](https://github.com/user-attachments/assets/ebe707fd-1d37-4687-8cf8-772487000206)
![image](https://github.com/user-attachments/assets/0e98a4d0-8e44-4624-8757-7be5cbe64212)
![image](https://github.com/user-attachments/assets/f829f259-f523-4038-ac7d-dd7eb7781369)
![image](https://github.com/user-attachments/assets/f662d001-8be4-4e89-83de-b93faa5a9f21)






## Imagen de la captura de cisco Python


![image](https://github.com/user-attachments/assets/f98d62de-7c0d-4bfa-a74b-4395530532d2)
![image](https://github.com/user-attachments/assets/5437f9da-dfc9-4e78-b458-1b15e5e41010)
![image](https://github.com/user-attachments/assets/fc60238a-0794-4c7a-aeb2-d73296356b8e)
![image](https://github.com/user-attachments/assets/732fb939-0303-41f4-ab8a-dbd4b709205e)
![image](https://github.com/user-attachments/assets/4f1d2dae-f3d7-43ca-a581-a7f5bfbf0f96)



## Imagen de la captura de JavaScript Essentials
![image](https://github.com/user-attachments/assets/f8748741-67b6-40e3-a980-860c814dba43)
![image](https://github.com/user-attachments/assets/12a867d2-e3a9-4654-b921-83e02856fe51)
![image](https://github.com/user-attachments/assets/e06e9a46-9f3c-44c3-b12b-6c0329538927)
![image](https://github.com/user-attachments/assets/326feddf-272e-4946-ae3a-9fcf61ab343d)
![image](https://github.com/user-attachments/assets/ccccd4ce-93a5-4094-8864-cdcb026fb721)
![image](https://github.com/user-attachments/assets/281720af-1c10-4621-b3f1-8ce3d09fa3f8)

