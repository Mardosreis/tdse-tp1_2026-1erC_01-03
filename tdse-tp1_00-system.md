## Modelo System (Parking Ticket Dispenser Machine)

El módulo System se implementa como una máquina de estados finitos (FSM) que corre en un ciclo temporizado de 1 ms.  
Su función es procesar los eventos provenientes del Sensor y ejecutar acciones que afectan al Actuator.

---
##  Estados del modelo System

Los estados representan las fases operativas del sistema:

- ST_SYS_IDLE → estado de espera, sin actividad.  
- ST_SYS_Processing → estado de procesamiento inicial luego de la pulsación del botón.  
- ST_SYS_CreatingTicket → estado de creación del ticket.  
- ST_SYS_Ticket_PNT → se emite la impresión del ticket.  
- ST_SYS_Barrier → estado donde la barrera (LED) se habilita para la salida.  

---

### Eventos del modelo System
Los eventos son los triggers que disparan transiciones en la FSM del System:

- EV_SYS_1_Loading → evento interno al iniciar el procesamiento de ticket.  
- EV_SYS_1_Print → evento interno cuando se genera la impresión del ticket (tick = 5).  
- EV_SYS_1_Barrier → evento interno que habilita la apertura de la barrera (tick = 5).  
- EV_SYS_ERRORx → eventos de error (1 a 4) cuando expira un temporizador de seguridad (tick == 0).
     - ERROR1: Error de procesamiento del ticket.
     - ERROR2: Error de creación del ticket.
     - ERROR3: Error de impresión del ticket.
     - ERROR4: Error de apertura de la barrera.
- EV_SYS_reset → evento interno que indica que finalizo el sistema y que vuelve al estado inicial, a la espera del próximo vehiculo.  

---

##  Tabla de Estados y Excitaciones – Modelo System


| Estado Actual          | Evento / Condición       | [Guard]        | Próximo Estado        | Acciones                                                                 |
|------------------------|--------------------------|----------------|-----------------------|--------------------------------------------------------------------------|
| **ST_SYS_IDLE**        | EV_SYS_1_Loading         | —              | ST_SYS_Processing     | Inicializar `tick = DEL_SYS_MAX`, raise `EV_ACT_DISPLAY_UPDATE`          |
| **ST_SYS_Processing**  | EV_SYS_1_Loading         | [tick = 5]     | ST_SYS_CreatingTicket | Registrar solicitud, raise `EV_ACT_PRINT`                                |
| **ST_SYS_Processing**  | EV_SYS_ERROR1           | [tick == 0]    | ST_SYS_IDLE           | Reset del sistema, raise `EV_ACT_DISPLAY_UPDATE` con mensaje de error    |
| **ST_SYS_CreatingTicket** | EV_SYS_1_Print        | [tick = 5]     | ST_SYS_Ticket_PNT     | Encender LED impresora, `ticket_ready = true`                            |
| **ST_SYS_CreatingTicket** | EV_SYS_ERROR2        | [tick == 0]    | ST_SYS_Prossesing           | Reset del sistema, raise `EV_ACT_DISPLAY_UPDATE` con mensaje de error    |
| **ST_SYS_Ticket_PNT**  | EV_SYS_1_Barrier         | [tick > 0]     | ST_SYS_Barrier        | raise `EV_ACT_BARRIER_OPEN`, `barrier_open = true`                       |
| **ST_SYS_Ticket_PNT**  | EV_SYS_ERROR3           | [tick == 0]    | ST_SYS_CreatingTicket | Reset parcial, volver a creación de ticket                               |
| **ST_SYS_Barrier**     | EV_SYS_1_Barrier         | [tick = 5]     | ST_SYS_IDLE           | Apagar LED barrera, `barrier_open = false`, volver a espera              |
| **ST_SYS_Barrier**     | EV_SYS_ERROR4           | [tick == 0]    | ST_SYS_Processing     | Reset parcial, volver a procesamiento inicial                            |
| **Cualquier estado**   | EV_SYS_reset             | —              | ST_SYS_IDLE           | Reset completo del sistema                                               |


| Estado Actual          | Evento / Condición       | [Guard]        | Próximo Estado        | Acciones                                                                 |
|------------------------|--------------------------|----------------|-----------------------|--------------------------------------------------------------------------|
| **ST_SYS_IDLE**        | EV_SYS_BTN_DOWN          | —              | ST_SYS_Processing     | Inicializar `tick = DEL_SYS_MAX`, raise `EV_ACT_DISPLAY_UPDATE`          |
| **ST_SYS_Processing**  | EV_SYS_1_Loading         | —              | ST_SYS_CraftingTicket | Registrar solicitud, raise `EV_ACT_PRINT`                                |
| **ST_SYS_CraftingTicket** | EV_SYS_1_Impression   | [tick == 5]    | ST_SYS_Ticket_PNT     | Encender LED impresora, `ticket_ready = true`                            |
| **ST_SYS_Ticket_PNT**  | EV_SYS_TICKET_VALID      | —              | ST_SYS_Barrier        | raise `EV_ACT_BARRIER_OPEN`, `barrier_open = true`                       |
| **ST_SYS_Barrier**     | EV_SYS_1_Barrier         | [tick == 5]    | ST_SYS_IDLE           | Apagar LED barrera, `barrier_open = false`, volver a espera              |
| **Cualquier estado**   | EV_SYS_ERRORx            | [tick == 0]    | ST_SYS_IDLE           | Reset del sistema, raise `EV_ACT_DISPLAY_UPDATE` con mensaje de error    |

---
