## Parking Ticket Dispenser Machine (Entry) - Módulo de Actuador
**-Módulo de código C:** Temporizado  
**-Período de escrutinio:** 1 ms
***************************************************************************************************************************************************************************************************************************
## 1. Descripción del Modelo
LED que se prende al resibir una petición del sistema para abrir la barrera, la cual se mantiene prendida hasta que la barrera vuelva al estado de reposo inicial. Una vez abierta la barrera en su totalidad, espera una petición del sistema para cerrarla. Cada vez que se termina de mover la barrera (abrir/cerrar), envía un evento al sistema avisando que terminó la acción.
***************************************************************************************************************************************************************************************************************************
## 2. Estados
* **`ST_ACT_IDLE`**: Reposo. LED apagado, barrera abajo.
* **`ST_ACT_OPENING`**: Transitorio. LED encendido, la barrera se está abriendo.
* **`ST_ACT_UP`**: Barrera completamente abierta. LED encendido.
* **`ST_ACT_CLOSING`**: Transitorio. LED encendido, la barrera se está cerrando.

## 3. Eventos
* **`EV_ACT_OPEN`**: Evento desde el Sistema: "Abrí la barrera".
* **`EV_ACT_CLOSE`**: Evento desde el Sistema: "Cerrá la barrera".
* **`EV_SYS_READY`**: Feedback: Señal enviada al Sistema al terminar el movimiento.

## 4. Variables de Control
* **`tick`**: Variable temporizadora de cuenta regresiva.
* **`DEL_LED_BARRERA_MAX`**: Es una constante que define el tiempo en que la barrera tarda en abrir/cerrar.

## 5. Acciones

### Acciones Internas
* **`tick = DEL_BTN_BARRERA_MAX`**: Inicializa la variable de control.
* **`tick--`**: Decrementa la variable de control.
* **`LED_ON`**: Prende LED.
* **`LED_OFF`**: Apaga LED.

### Acciones Externas

* **`raise EV_SYS_READY`**: Emite la señal al sistema.
  
### Actuator Statechart - State Transition Table

| Current State | Event | [Guard] | Next State | Actions |
| :--- | :--- | :--- | :--- | :--- |
| ST_ACT_IDLE  |EV_ACT_CLOSE |  | ST_ACT_IDLE | |
| | EV_ACT_OPEN |  | ST_ACT_OPENING |LED_ON; tick = DEL_LED_BARRERA_MAX|
| ST_ACT_OPENING | | tick>0 |ST_ACT_OPENING | tick--|
|  | |tick==0  | ST_ACT_UP|raise EV_SYS_READY |
|ST_ACT_UP  |EV_ACT_CLOSE | |ST_ACT_CLOSING  | tick = DEL_LED_BARRERA_MAX|
|  ST_ACT_CLOSING| |tick>0  |ST_ACT_CLOSING | tick--|
|  | |tick==0  | ST_ACT_IDLE|LED_OFF; raise EV_SYS_READY |
