## *COMA Electronics: Automated Parking System*

   El sistema de COMA Electronics consiste en una solución automatizada de control de acceso para estacionamientos, basada en una máquina de tickets ubicado en la entrada. Cuando un vehículo es detectado por sensores, el sistema genera un ticket con identificación única y registra la información en una base de datos central. Una vez retirado el ticket, la barrera de acceso se abre automáticamente, permitiendo el ingreso del vehículo. Este proceso se integra con sistemas de pago y control de salida, permitiendo la gestión completa del estacionamiento.
   

## *Parking Ticket Dispenser Machine (Entry)*
El sistema se organiza en tres módulos principales, ejecutados periódicamente con un período de 1 ms (Update by Time Code):

- Módulo Sensor: se modela como un sistema de muestreo periódico (polling), encargado de leer las entradas digitales en cada ciclo de ejecución.
- Módulo System: se modela mediante una máquina de estados finitos (FSM), que define el comportamiento del sistema y las transiciones entre estados en función de las entradas y del estado actual.
- Módulo Actuator: se modela como lógica de salida dependiente del estado del sistema, encargada de accionar los dispositivos físicos.



Para la implementación en laboratorio, los dispositivos reales pueden ser reemplazados por entradas y salidas digitales:

Entradas (Digital Inputs):
- Sensor de vehículo -> pulsador o DIP switch
- Botón de ticket -> pulsador
- Cámara / identificación -> DIP switch

Salidas (Digital Outputs):
- Impresora -> LED
- Barrera -> LED
- Display / sistema -> LED
