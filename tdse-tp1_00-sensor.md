## Parking Ticket Dispenser Machine (Entry) - Módulo de Sensores
**-Módulo de código C:** Temporizado  
**-Período de escrutinio:** 1 ms
***************************************************************************************************************************************************************************************************************************
## 1. Descripción del Modelo
En el siguiente modulo lo que tratamos de escrutar o sensar es el estado físico en el que se encuentra el botón de entrada de la máquina
dispensadora de tickets. Dado que los contactos métalicos no son perfectos, y pueden generar rebotes que son considerados también como ruidos
eléctricos, en el modulo debemos de implmentar una maquina de estados que tenga en cuenta éstas anomalías y las pueda filtrar.  
El módulo tendrá un tiempo de accionamiento de 1 ms, en este tiempo lo que hará es evaluar el estado "físico" del pin y gestionar los ticks del
sistema para poder asegurar la información.  
Una vez que el sistema detecte que el estado físico es estable durante un tiempo determinado usando `DEL_BTN_XX_MAX`, el módulo sensor emite
una "Signal" (un mensaje o evento de sistema) hacia el módulo de procesamiento.  

***************************************************************************************************************************************************************************************************************************
## 2. Eventos
En este caso, los eventos del modelo Sensor representan el comportamiento del botón, y más enspecífico, su posicionamiento para con la entrada del microcontrolador, ya que al ser un sensor del tipo binario, solamente nos genera dos eventos base:  
* **`EV_BTN_XX_UP`**: Se detecta un nivel lógico alto en el pin (El botón está físicamente no está presionado o liberado).
* **`EV_BTN_XX_DOWN`**: Se detecta un nivel lógico bajo en el pin (El botón está físicamente presionado).

## 3. Variables de Control
Para comprobar los eventos físicos a lo largo del tiempo, el módulo utiliza una variable de control la cual condiciona las transiciones de estado:
* **`tick`**: Variable temporizadora de cuenta regresiva.
* **`DEL_BTN_XX_MAX`**: Es una constante que define el tiempo de validación anti-rebote.

## 4. Acciones
Las acciones son las respuestas de la máquina de estados ante un Evento válido que cumple con su condición de guarda. Se dividen en manejo interno y 
señales al sistema central (Signals).  

### Acciones Internas
Son las acciones encargadas de manejar el temporizador.

* **`tick = DEL_BTN_XX_MAX`**: Inicializa la variable de control y ejecuta cuando se detecta el primer cambio de estado físico (ej. de UP a FALLING) 
para comenzar a medir el tiempo del rebote.
* **`tick--`**: Decrementa la variable de control. Se ejecuta en cada ciclo de 1 ms mientras el estado físico se mantenga durante el período de 
validación.  

### Acciones Externas
Estas acciones son los mensajes en los que el módulo de escrutar envía hacia el modulo de procesar, reflejando de esta manera su cambio de posición válido y ya confirmado.  

* **`raise EV_SYS_XX_DOWN`**: Emite la señal al sistema indicando que el botón fue **presionado** exitosamente (pasó el tiempo de anti-rebote sin que 
el pin vuelva a subir). Ocurre al transicionar al estado estable `ST_BTN_XX_FALLING`.
* **`raise EV_SYS_XX_UP`**: Emite la señal al sistema indicando que el botón fue **soltado** exitosamente (pasó el tiempo de anti-rebote sin que el 
pin vuelva a bajar). Ocurre al transicionar al estado estable `ST_BTN_XX_UP`.


