
**DESCRIPCIONES**
***************************************************************************************************************************************************************************************************************************
**-Intelligent Parking Management System**

Su estructura consta de un servidor, (recomendado: 16G RAM, HDD 2T, CPU: Intel I7) que administra los otros módulos del sistema (totem de entrada, salida, toll computer, sistema de pago automático), mediante la última tecnología base web. Acepta pago en efectivo, tarjeta y pago electrónico.  
El software de gestión inteligente y flexible, tiene la capacidad de integrarse con sistemas de terceros. Puede generar todo tipo de infores de transferencia en formato PDF o EXCEL.  
Compatible con sistema de boletos, sistema de tarjetas, sistema de estacionamiento sin boletos: LPR o ANPR, sistema de estacionamiento RID de largo alcance, sistema de estacionamiento de automóviles en la calle, etc.

**-Automated Parking System**

El sistema de estacionamiento automatico principalmente consiste de dos partes: la entrada y salida de las maquinas que entregan tickets, y el pago del tipo auto-servicio. Primero, cuando un vehiculo entra a la playa de estacionamiento, la máquina de la entrada se imprime un ticket sin necesidad de manipulación por parte del usuario, logrando una incremantación en la eficiencia del flujo de tráfico.  
Cuando el usuario decide retirarse, escanea el ticket que le dieron anteriormente en la máquina de pago de auto-servicio. De esta manera, el sistema calcula el cargo del servicio de manera automatica, basandose en el tiempo de duración del estacionamiento, y ya el usuario podría pagar con el método de pago que más cómodo le sea. Después de que el pago sea exitoso, el sistema garantiza un tiempo de gracia para que el usuario pueda salir sin problemas. Por último, lo que quedaría es escanear el ticket de entrada en la máquina de salida, y la barrera se levanta automaticamente.

**-Parking Ticket Dispenser Machine (Entry)**

La máquina expedidora de tickets de estacionamiento suele estar ubicada en la entrada del parqueadero. Esta emite un ticket impreso cada vez que un radar detecta el vehículo y el usuario temporal presiona el botón de solicitud. El ticket consiste generalmente en un código de barras o código QR único que incluye la fecha y la hora de entrada. El panel de la máquina cuenta con un lector de tarjetas mensuales especialmente diseñado para usuarios fijos; con tan solo acercar la tarjeta mensual, la barrera se levantará de forma automática.

***************************************************************************************************************************************************************************************************************************
**IMPLEMENTACION Parking Ticket Dispenser Machine (Entry)**

En una primera instancia, tenemos una implementación básica para poder visualizar fisicamente qué es lo que ocurre con uno de los tantos escenarios que podemos encontrarnos. El "problema" que podría haber con este tipo de implementación es que está completamente ligado con el hardware aplicado, ya que si quisieramos cambiar de sensor, en el módulo de procesar, tiene que seguir creyendo que es el sensor viejo.  
Luego, cuando pasamos a la segunda instancia de módulos, ya vemos que esto no sucedería porque agrupa todos los sensores en una misma interfaz, pero aún neceistamos ser más específicos respecto a los mensajes que se envían entre los módulos.  
Por última instancia, tenemos el modelo final, aquí ya tenemos prácticamente todo lo que necesitamos para poder implementar la lógica programable detrás de este sistema. Tenemoos un arquitectura que se basa principalmente en la utilización del Cyclic Executive con una comunicación basada en cola de eventos/mensajes. Este tipo de implementación nos garantiza tiempos de respuestas fijos, ya que cada 1ms el sistema revisará los sensores, proocesará la lógica y actualizará los
actuadores.

***************************************************************************************************************************************************************************************************************************
**Modelos - Parking Ticket Dispenser Machine (Entry)**

**Modelo de sensor (escrutar)**  
Objetivo: Monitorear el estado físico de las entradas y filtrar señales no deseadas. Ej: (Camera, Button y Sensor Coil)  
Entrada: Señal eléctrica directa del hardware.  
Salida: Evento lógico de sistema una vez validado.

**Modelo de sistema (procesar)**  
Objetivo: Actuar como el "cerebro" o lógica de control de la máquina de tickets. Recibe los eventos ya "limpios" del modelo Sensor. Basándose en el estado actual de la máquina (ej. esperando vehículo, esperando impresión) y en condiciones lógicas (Guards), decide qué acción debe realizarse. Es el encargado de la "inteligencia" del negocio (verificar si hay lugar, validar tarjetas, etc.).  
Entrada: Eventos provenientes del Sensor.  
Salida: Eventos dirigidos al Actuador.

**Modelo de actuator (actuar)**  
Objetivo: Traducir las órdenes lógicas del sistema en acciones físicas.  
Entrada: Eventos provenientes del sistema.  
Salida: Cambio de estado en los periféricos de salida (Display, Printer, Barrier y Sever).

