# tdse-tp1_2026_1ercuatrimestre_Colodro_dosReis_Toscan
 FIUBA - Electrónica - Taller de Sistemas  Embebidos - Trabajo Práctico N°: 1 – Diagramas de  Estado - Modelado 
# FIUBA - Electrónica - Taller de Sistemas Embebidos 
## Trabajo Práctico N°: 1 - Diagramas de Estado - Modelado 
### 2026 - 1er Cuatrimestre - Grupo: Colodro, dos Reis y Toscan
### Responsable de la entrega: 
| Padrón | Apellidos, Nombres      | Fecha    | Deadline  | 
| 106433 | Colodro, Felipe         | :------: | :-------: | 
| 111545 | dos Reis, Mariana       |          | Semana 04 | 
| 111106 | Toscan, Uma             |          | Semana 04 |


Eventos

Los eventos representan cambios en la posición del pulsador (entrada binaria):

EV_BTN_PRESSED: se genera cuando el botón pasa de no presionado a presionado (flanco ascendente)
EV_BTN_RELEASED: se genera cuando el botón pasa de presionado a no presionado (flanco descendente)
Estados del modelo
ST_BTN_RELEASED: botón en estado no presionado
ST_BTN_PRESSED: botón presionado
Timer

Se define un contador de tiempo:

tick: contador incrementado cada 1 ms

Este timer se utiliza para implementar un retardo de estabilización (debouncing), evitando múltiples detecciones falsas debido al rebote mecánico del pulsador.

Por ejemplo, se puede utilizar una constante:

DEL_BTN_DEBOUNCE
Acciones

Las acciones del módulo Sensor se ejecutan en cada ciclo de muestreo:

Lectura del estado actual del botón
Comparación con el estado anterior
Actualización del estado interno
Generación de eventos
Inicialización o actualización de temporizadores asociados
Relación Eventos / Acciones (trigger → effect)
EV_BTN_PRESSED
→ Generación de señal hacia el sistema (EV_SYS_BTN_PRESSED)
→ Inicialización del timer de debounce
EV_BTN_RELEASED
→ Generación de señal hacia el sistema (EV_SYS_BTN_RELEASED)
→ Reinicio del timer
