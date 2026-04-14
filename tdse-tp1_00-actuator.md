# Modelo Actuator (un solo LED)

El módulo Actuator se encarga de recibir señales del System y ejecutar acciones físicas sobre el LED.  
Las acciones pueden ser:  
- Signals (eventos) hacia otros modelos.  
- Funciones internas de control del LED.  
- Inicialización / modificación de variables de control (ej. temporizador tick).  

---

## Estados del modelo Actuator

Los estados representan las fases operativas del LED:
- **ST_LED_1_OFF** → LED apagado, sin actividad.  
- **ST_LED_1_ON** → LED encendido de forma continua, apertura de la barrera.  
- **ST_LED_1_BLINK** → LED titilando, procesando.  

---

##  Eventos del modelo Actuator

- **EV_LED_1_Loading** → evento interno para iniciar procesamiento.  
- **EV_LED_1_Barrier** → evento interno que permite la apertura de barrera (tick = 5).  
- **EV_LED_1_ERROR1** → error de procesamiento (tick == 0).  
- **EV_LED_1_ERROR2** → error de apertura de la barrera (tick == 0).  
- **EV_LED_1_Reset** → evento interno que apaga el LED y retorna al estado inicial.

---

## Tabla de Estados y Excitaciones – Modelo Actuator

| Estado Actual     | Evento / Condición   | [Guard]        | Próximo Estado   | Acciones                                                                 |
|-------------------|----------------------|----------------|------------------|--------------------------------------------------------------------------|
| **ST_LED_1_OFF**    | EV_LED_1_Loading       | [tick = 5]   | ST_LED_1_BLINK     |                    |
| **ST_LED_1_BLINK**     | EV_LED_1_Barrier       | [tick = 5]     | ST_LED_1_ON       | Tick--                                           |
| **ST_LED_1_BLINK**  | EV_LED_1_ERROR1       | [tick == 0]     | ST_LED_1_OFF       |                  |
| **ST_LED_1_ON**     | EV_LED_1_ERROR2        | [tick == 0]    | ST_LED_1_BLINK       |                                  |
| **ST_LED_1_ON**  | EV_LED_1_Reset        | [tick == 0]    | ST_LED_1_OFF       |                 |
