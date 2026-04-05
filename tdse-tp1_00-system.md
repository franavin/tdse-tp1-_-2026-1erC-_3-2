## Parking Ticket Dispenser Machine (Entry) - Módulo de Sistema
**-Módulo de código C:** Temporizado  
**-Período de escrutinio:** 1 ms
***************************************************************************************************************************************************************************************************************************
## 1. Descripción del Modelo

***************************************************************************************************************************************************************************************************************************
## 2. Estados
* **`ST_SYS_CLOSED`**: Reposo. La barrera está abajo.
* **`ST_SYS_OPENING`**: Transitorio. Se envió la orden de subir y se espera confirmación.
* **`ST_SYS_OPEN`**: Barrera totalmente levantada.
* **`ST_SYS_CLOSING`**: Transitorio. Se envió la orden de bajar y se espera confirmación.
  
## 3. Eventos
* **`EV_SYS_XX_UP`**: Señal desde el sensor. El auto ya no está bajo la barrera.
* **`EV_SYS_XX_DOWN`**: Señal desde el sensor. Hay un auto cerca de la barrera.
* **`EV_SYS_READY`**: Señal de feedback desde el actuador. Se envía cuando la tarea abrir o cerrar barrera está completada.
  
## 4. Acciones
### Acciones Externas
* **`EV_ACT_OPEN`**: Orden al actuador para iniciar la apertura de la barrera.
* **`EV_ACT_CLOSE`**: Orden al actuador para iniciar el cierre de la barrea.



### System Statechart - State Transition Table

| Current State | Event | [Guard] | Next State | Actions |
| :--- | :--- | :--- | :--- | :--- |
| ST_SYS_CLOSED  | EV_SYS_XX_DOWN | |ST_SYS_OPENING  |raise EV_ACT_OPEN |
| ST_SYS_OPENING | EV_SYS_READY | |ST_SYS_OPEN | |
| ST_SYS_OPEN | EV_SYS_XX_UP | |ST_SYS_CLOSING |raise EV_ACT_CLOSE |
| ST_SYS_CLOSING| EV_SYS_READY | |ST_SYS_CLOSED| |
