# VendingMachine

# PRACTICA 3: MAQUINA EXPENDEDORA

 Irene Diez de Toro
 
 Noviembre - Diciembre 2024 - Third Year of Robotic Software Robotics Engineering

 Sistemas Empotrados y de Tiempo Real.


# 0. INTRODUCTION

El objetivo general de esta práctica ha sido diseñar e implementar un controlador para una máquina expendedora basada en Arduino UNO, utilizando sensores y actuadores del kit. Debe cumplir funcionalidades específicas relacionadas con interacción usuario-máquina, sensores, actuadores y un sistema de administración.
Este programa se dividia en tres funcionalidades principales:
- **ARRANQUE**: En este caso se debía de encender una luz y parpadear tres veces, mientras en pantalla se mostraba en el display "CARGANDO".
- **SERVICIO**: En este segundo caso se debia de comprobar si habia un cliente delante de la máquina, mostrar la temperatura si ese era el caso y pasar al menú de bebidas. Donde se podía ver cuales habia. Tambien se permitia la seleccion de cualquiera, donde para simular la preparación, se encendía un led de manera incremental, mientras se mostraban los correspondientes mensajes en la pantalla.
- **ADMINISTRADOR**: A este se accedia mediante el accionado de un boton durante 5 segundos. Una vez dentro se encendian los leds durante toda su ejecución. En este se mostraba en el display el menu de opciones. Donde se podia ver la temperatura, la distancia del sensor de distancia, el contador (donde se mostraba el tiempo que llevaba la placa encendida) y por ultimo se podian modificar los precios. En este ultimo, se motraba otra vez el menu de bebidas, y una vez seleccionada una, se podia incrementar o decrementar los precios. Actualizandose cuando se volvieran a mostrar. Se salia de la misma forma que para entrar en el modo.


# 1. MI PROGRAMA

Mi código de la máquina expendedora implementa un sistema basado en **máquinas de estados** para gestionar las operaciones de la máquina, permitiendo una transición eficiente entre distintos modos de funcionamiento. La estructura principal de estas máquinas de estados está compuesta por varios estados, cada uno de los cuales representa una acción o condición específica dentro del ciclo de vida de la máquina expendedora. Además, cada caso correspondiente en el codigo que necesite una comprobación, esta manejado con flags.

La primera máquina de estados, tiene las siguientes transiciones:

- **WAITING_CLIENT**: El sistema está a la espera de la interacción del usuario. En este estado, la máquina espera a que el cliente seleccione una opción del menú.
- **TEMPERATURE**: El sistema muestra la temperatura y la humedad actuales en la pantalla LCD, que puede servir para proporcionar información útil al cliente.
- **MENU**: En este estado, el sistema ofrece al cliente las opciones de bebidas disponibles, permitiendo al usuario elegir entre varias opciones, como café o té.
- **MAKING_COFFEE**: Una vez que el cliente selecciona su bebida, la máquina entra en este estado para preparar el café, gestionando el proceso de calentamiento y dosificación de ingredientes.
- **ADMINISTRATOR**: En este estado, se proporcionan funciones administrativas como la gestión de inventarios, ajustes o configuración del sistema.
- **RESTART**: Si se detecta algún error o el sistema necesita reiniciarse, este estado reinicia la máquina a un estado inicial, restableciendo todas las condiciones necesarias para comenzar de nuevo.

La segunda, tiene:

- **OPTIONS**: En este estado, la máquina espera a que el administrador seleccione una opción del menú.
- **TEMPERATURE**: El sistema muestra la temperatura y la humedad actuales en la pantalla LCD, que puede servir para proporcionar información útil al administrador.
- **DISTANCE**: El sistema muestra la distancia a la que esta detectando al administrados el sensor.
- **COUNTER**: En este estado, el sistema muestra el tiempo que lleva encendida la placa.
- **PRICES**:En este estado, la máquina espera a que el cliente seleccione una bebida del menú..
- **CHANGE_PRICE**: En cuanto seleccione una bebida, esta sola se motrará en pantalla y se podrá cambiar el precio correspondiente.

Y la ultima:

- **MAKING**: En este estado, la máquina simula la preparación de la bebida.
- **READY_TO_SERVE**: En este ultimo estado, se le indica al cliente que recoja su bebida.

El sistema también hace uso de **interrupciones** para gestionar de forma eficiente las transiciones entre estados sin tener que estar comprobando constantemente el estado del sistema en un bucle. Por ejemplo, cuando el cliente presiona un botón o realiza alguna acción con el joystick, una interrupción puede cambiar el estado de la máquina de manera inmediata, lo que mejora la eficiencia y reduce el uso innecesario de recursos. Por ello, al presionar los dos botones, salta una interrupción correspondiente a cada una. En el caso del joystick, para manejar la opcion deseada en el menu correspondiente. Mientras que en el otro boton se implementa el reset o el admin time.

El **watchdog timer** es otro componente clave en el sistema. Este temporizador está configurado para detectar si el sistema se queda bloqueado o si no responde correctamente. Si el sistema no recibe señales de actividad en un período de tiempo determinado (de 8 segundos en mi caso), el watchdog genera un reinicio, restableciendo el sistema automáticamente para evitar que la máquina se quede inactiva debido a un fallo o error imprevisto.

A través de estas técnicas, como las máquinas de estados, las interrupciones y el watchdog, el código asegura que la máquina expendedora funcione de manera eficiente, responda rápidamente a las interacciones del cliente y se recupere de posibles fallos sin intervención manual. Este enfoque permite una gestión fluida de las operaciones, optimizando el tiempo de respuesta y mejorando la fiabilidad del sistema en general.

Cabe destacar que en el menu de las bebidas dentro del menu del admin, el de los precios, a veces le cuesta entender la señal de la interrupcion par seleccionar la bebiba a la que quiere cambiarse. Los textos del display han tenido que ser modificados y acortados para que puedieran caber en la pantalla del lcd.

# 3. FRIZTING

Componentes usados:
- Arduino UNO
- LCD
- Joystick
- Sensor Temperatura/Humedad DHT11
- Sensor Ultrasonidos
- Botón
- 2 LEDS normales (LED1, LED2)

A continuacion, se muestra la imagen del esquema del circuito. 
<p align="center">
  <img src="empotrados3.png" alt="CONEXIONADO" width="50%" style="border: 2px solid black;">
  &nbsp;&nbsp;&nbsp;
</p>

Y la foto del circuito real:

<p align="center">
  <img src="dia.png" alt="CONEXIONADO" width="40%" style="border: 2px solid black;">
  &nbsp;&nbsp;&nbsp;
  <img src="noche.png" alt="OTRA VISTA" width="40%" style="border: 2px solid black;">
  &nbsp;&nbsp;&nbsp;
</p>


# 4. DIFICULTADES

Durante la realización de esta práctica, he tenido una serie de problemas que explicaré a continuación:

- *Adaptarme a Arduino y a sus componentes* fue un desafío al principio. Tuve algunas dificultades iniciales con el conexionado, ya que en mi kit tanto el display como el sensor de temperatura estaban dañados. Esto complicó un poco los primeros pasos. Sin embargo, una vez solucionado ese inconveniente, logré entender cómo se conectaban los componentes, cómo interactuaban con Arduino, y también me familiaricé con sus librerías y con el entorno de desarrollo (IDE).
- Me costó muchísimo entender cómo *contar el tiempo necesario* para las distintas funcionalidades. Al principio, lo intenté utilizando bucles while, pero rápidamente me di cuenta de que estaba utilizando espera activa, lo que bloqueaba el flujo del programa principal. Esto causaba problemas, especialmente con tareas como las interrupciones, que se veían bloqueadas. Después de una tutoría y de darle muchas vueltas al problema, logré encontrar una solución utilizando estructuras if en lugar de bucles, lo que me permitió medir el tiempo sin bloquear el programa principal.
- Por último, implementar la *funcionalidad del botón de reinicio y administrador* fue un desafío. Al principio, no lograba que ambos botones funcionaran correctamente; a veces solo uno respondía y el otro no. Después de investigar y probar diferentes enfoques, conseguí identificar el problema y ajustar el código.

# 5. VIDEO OF THE VENDING MACHINE

Haz click en el link para verlo! -> [MAQUINA EXPENDEDORA](https://urjc-my.sharepoint.com/:v:/g/personal/i_diezd_2022_alumnos_urjc_es/EepZyyvT2Y5CuTeeIXFXMX8BVGGi0csNQJd0kgF7qjUQKQ?e=nnHyuL&nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJTdHJlYW1XZWJBcHAiLCJyZWZlcnJhbFZpZXciOiJTaGFyZURpYWxvZy1MaW5rIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXcifX0%3D) :)

Adicional, ya que no he enseñado en el otro si el sensor no me detecta: [MAQUINA EXPENDEDORA 2](https://urjc-my.sharepoint.com/:v:/g/personal/i_diezd_2022_alumnos_urjc_es/EWTQl6uHVtdAibGVrHgEFS8B8riWhGQyhKHGBJLpt04q0w?e=LHAbB6&nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJTdHJlYW1XZWJBcHAiLCJyZWZlcnJhbFZpZXciOiJTaGFyZURpYWxvZy1MaW5rIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXcifX0%3D)
