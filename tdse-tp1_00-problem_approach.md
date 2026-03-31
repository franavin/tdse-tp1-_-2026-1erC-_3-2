
**DESCRIPCIONES**
***************************************************************************************************************************************************************************************************************************
**-Intelligent Parking Management System**

Su estructura consta de un servidor, (recomendado: 16G RAM, HDD 2T, CPU: Intel I7) que administra los otros módulos del sistema (totem de entrada, salida, toll computer, sistema de pago automático), mediante la última tecnología base web. Acepta pago en efectivo, tarjeta y pago electrónico.  
El software de gestión inteligente y flexible, tiene la capacidad de integrarse con sistemas de terceros. Puede generar todo tipo de infores de transferencia en formato PDF o EXCEL.  
Compatible con sistema de boletos, sistema de tarjetas, sistema de estacionamiento sin boletos: LPR o ANPR, sistema de estacionamiento RID de largo alcance, sistema de estacionamiento de automóviles en la calle, etc.

**-Automated Parking System**

El sistema de estacionamiento automatico principalmente consiste de dos partes: la entrada y salida de las maquinas que entregan tickets, y 
el pago del tipo auto-servicio. Primero, cuando un vehiculo entra a la playa de estacionamiento, la máquina de la entrada se imprime un ticket
sin necesidad de manipulación por parte del usuario, logrando una incremantación en la eficiencia del flujo de tráfico.
Cuando el usuario decide retirarse, escanea el ticket que le dieron anteriormente en la máquina de pago de auto-servicio. De esta manera,
el sistema calcula el cargo del servicio de manera automatica, basandose en el tiempo de duración del estacionamiento, y ya el usuario
podría pagar con el método de pago que más cómodo le sea. Después de que el pago sea exitoso, el sistema garantiza un tiempo de gracia 
para que el usuario pueda salir sin problemas. Por último, lo que quedaría es escanear el ticket de entrada en la máquina de salida, y 
la barrera se levanta automaticamente.

**-Parking Ticket Dispenser Machine (Entry)**



***************************************************************************************************************************************************************************************************************************
**IMPLEMENTACION Parking Ticket Dispenser Machine (Entry)**

En una primera instancia, tenemos una implementación básica para pdoer visualizar fisicamente qué es lo que ocurre con uno de los tantos escenarios que
podemos encontrarnos. El "problema" que podría haber con este tipo de implementación es que está completamente ligado con el hardware aplicado, ya que
si quisieramos cambiar de sensor, en el módulo de procesar, tiene que seguir creyendo que es el sensor viejo.
Luego, cuando pasamos a la segunda instancia de módulos, ya vemos que esto no sucedería porque agrupa todos los sensores en una misma interfaz, pero aún
neceistamos ser más específicos respecto a los mensajes que se envían entre los módulos.
Por última instancia, tenemos el modelo final, aquí ya tenemos prácticamente todo lo que necesitamos para poder implementar la lógica programable detrás de
este sistema. Tenemoos un arquitectura que se basa principalmente en la utilización del Cyclic Executive con una comunicación basada en cola de eventos/mensajes.
Este tipo de implementación nos garantiza tiempos de respuestas fijos, ya que cada 1ms el sistema revisará los sensores, proocesará la lógica y actualizará los
actuadores.

