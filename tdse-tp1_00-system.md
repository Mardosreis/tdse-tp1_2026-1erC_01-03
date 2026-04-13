## Modelo System (Parking Ticket Dispenser Machine)

El módulo System se implementa como una máquina de estados finitos (FSM) que corre en un ciclo temporizado de 1 ms.  
Su función es procesar los eventos provenientes del Sensor y ejecutar acciones que afectan al Actuator.

---

### Eventos del modelo System
Los eventos son los triggers que disparan transiciones en la FSM del System:

- EV_SYS_BTN_DOWN     -> recibido desde el Sensor cuando el botón fue validado como presionado.  
- EV_SYS_BTN_UP       -> recibido desde el Sensor cuando el botón fue validado como liberado.  
- EV_SYS_TICKET_VALID -> evento interno cuando el ticket emitido es reconocido por el sistema.  
- EV_SYS_PAYMENT_OK   -> evento cuando el pago se confirma en la estación de autoservicio.  
- EV_SYS_TIMEOUT      -> evento interno si expira un temporizador de seguridad o de espera.  

---

### Acciones del modelo System
Las acciones reflejan los efectos que el System ejecuta al procesar los eventos:

- **Signals hacia el Actuator**  
  - `raise EV_ACT_PRINT` → activar impresora de tickets (LED).  
  - `raise EV_ACT_BARRIER_OPEN` → abrir barrera de salida (LED).  
  - `raise EV_ACT_DISPLAY_UPDATE` → actualizar display con instrucciones o estado.  

- **Funciones internas**  
  - Registrar ticket emitido en la base de datos.  
  - Calcular tarifa según duración del estacionamiento.  
  - Validar pago y otorgar tiempo libre de salida.  

- **Inicialización / modificación de variables de control**  
  - `tick = DEL_SYS_MAX` → inicializar temporizador de seguridad o espera.  
  - `tick--` → decremento periódico del temporizador en cada ciclo de 1 ms.  
  - Banderas de control:  
    - `ticket_ready = true` → indica que el ticket fue emitido.  
    - `payment_done = true` → indica que el pago fue confirmado.  
    - `barrier_open = true` → indica que la barrera está habilitada.  

---

###  Ejemplo de Transiciones 
| Estado Actual   | Evento             | [Guard]       | Estado Siguiente | Acción                        |
|-----------------|--------------------|---------------|-----------------|-------------------------------|
| ST_IDLE         | EV_SYS_BTN_DOWN    |               | ST_TICKET_ISSUE | raise EV_ACT_PRINT, ticket_ready=true |
| ST_TICKET_ISSUE | EV_SYS_TICKET_VALID|               | ST_WAIT_PAYMENT | tick = DEL_SYS_MAX            |
| ST_WAIT_PAYMENT | EV_SYS_PAYMENT_OK  | [tick>0]      | ST_EXIT_READY   | raise EV_ACT_BARRIER_OPEN     |
| ST_EXIT_READY   | EV_SYS_BTN_UP      |               | ST_IDLE         | barrier_open=false             |

---

✅ Con esta estructura se cumplen los requisitos de la consigna N08:  
- Se enuncian y describen los **Eventos del modelo System**.  
- Se detallan las **Acciones del modelo System**, incluyendo señales hacia el Actuator, funciones internas y modificación de variables de control.  
- Se muestra cómo se integran en un módulo temporizado de 1 ms con guards y efectos.




## Eventos del modelo System
- **EV_SYS_BTN_DOWN** → recibido desde el Sensor cuando el botón fue validado como presionado.  
- **EV_SYS_BTN_UP** → recibido desde el Sensor cuando el botón fue validado como liberado.  
- (opcional) **EV_SYS_TIMEOUT** → generado internamente si expira un temporizador de control.  

## Acciones del modelo System
- **Signals hacia el Actuator**  
  - `raise EV_ACT_PRINT` → activar impresora (LED).  
  - `raise EV_ACT_BARRIER_OPEN` → abrir barrera (LED).  
  - `raise EV_ACT_DISPLAY_UPDATE` → actualizar display (LED).  

- **Funciones internas**  
  - Registrar ticket emitido.  
  - Actualizar estado del sistema (ej. bandera de “ticket listo”).  

- **Inicialización / modificación de variables de control**  
  - `tick = DEL_SYS_MAX` → inicializar temporizador de seguridad.  
  - `tick--` → decremento periódico del temporizador.  
  - Banderas de control para evitar múltiples tickets en una misma pulsación larga.  









Las Acciones del modelo System son signals (Eventos) para el modelo Actuator o bien ejecutar funciones o
bien “inicialización/modificación” de variables de control (timer); variables que pueden usarse como guard para
condicionar transiciones (trigger [guard] / effect
 
  Eventos del modelo System:
  Estos eventos son señales que luego utiliza el módulo System para decidir las transiciones de su máquina de estados. 
  Los principales eventos que se pueden detectar son:
    Botón presionado: cuando el valor pasa de 0 a 1 (flanco ascendente).
    Botón liberado: cuando el valor pasa de 1 a 0 (flanco descendente).
    Botón mantenido: cuando el botón permanece presionado durante un tiempo mayor a un umbral definido.
    Sin actividad: cuando no se detectan cambios en el botón durante un período prolongado


  Evento del modelo System:
  Son las respuestas que se ejecutan cuando ocurren estos eventos. 
  Pueden ser de tres tipos:
    Señales hacia el Actuator: por ejemplo, activar la impresora (LED) o abrir la barrera (LED).
    Funciones internas: como registrar el evento en la base de datos o actualizar el display.
    Modificación de variables de control: inicializar o actualizar contadores y banderas, como un temporizador para medir la duración de la
    pulsación, una bandera que indique si el ticket está listo, o un mecanismo de seguridad para evitar múltiples tickets en una misma 
    pulsación larga





    09) En el archivo tdse-tp1_00-system.md, editar y modificar su contenido:
        Enunciar la tabla de Estados y Excitaciones del modelo System (un solo sistema) necesarios para describir el
        comportamiento del módulo de código C del tipo temporizado (Update by Time Code, period = 1mS) para “procesar”,
        necesario para la implementación referida en el TP1 – Actividad 00 – Paso 05.


        Estado actual	Evento	[Guard]	Próximo estado	Acciones
        Idle (espera)	Botón presionado	Ticket no generado	TicketRequested	Generar ticket, registrar evento
        TicketRequested	Ticket listo	—	TicketReady	Encender LED impresora
        TicketReady	Botón liberado	Ticket generado correctamente	BarrierOpen	Abrir barrera
        BarrierOpen	Tiempo de apertura	—	Idle	Apagar LED barrera, volver a espera
                  
