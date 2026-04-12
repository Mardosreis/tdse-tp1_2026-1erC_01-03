## Modelo Sensor (un solo botón)

El modelo Sensor se implementa como un módulo temporizado (Update by Time Code, período = 1 ms) que escruta el estado de un pulsador binario, definiendo eventos y acciones.  

###  Eventos del Sensor
El botón binario genera dos eventos principales asociados a su posición física:

- **EV_BTN_UP** -> el botón está en posición no presionado.  
- **EV_BTN_DOWN** -> el botón está en posición presionado.  
  
Estos eventos accionan transiciones en la máquina de estados del Sensor.

--

###  Acciones del Sensor
Las acciones reflejan cambios de posición del botón y pueden ser:

- **Inicialización / modificación de variables de control**  
  - tick = DEL_BTN_MAX → al detectar transición hacia falling, se inicializa el temporizador de debounce.  
  - tick-- → decremento periódico del temporizador mientras se mantiene en estado transitorio.

- **Generación de señales hacia el System**  
  - `raise EV_SYS_BTN_DOWN` → notifica al módulo System que el botón fue presionado de manera válida.  
  - (Opcional) `raise EV_SYS_BTN_UP` → notifica liberación del botón, según diseño.

---

### 🗂️ Tabla de Transiciones (simplificada)
| Current State   | Event        | [Guard]     | Next State     | Actions                |
|-----------------|--------------|-------------|----------------|------------------------|
| ST_BTN_UP       | EV_BTN_UP    |             | ST_BTN_UP      |                        |
| ST_BTN_UP       | EV_BTN_DOWN  |             | ST_BTN_FALLING | tick = DEL_BTN_MAX     |
| ST_BTN_FALLING  | EV_BTN_UP    | [tick > 0]  | ST_BTN_FALLING | tick--                 |
| ST_BTN_FALLING  | EV_BTN_UP    | [tick = 0]  | ST_BTN_UP      |                        |
| ST_BTN_FALLING  | EV_BTN_DOWN  | [tick = 0]  | ST_BTN_DOWN    | raise EV_SYS_BTN_DOWN  |
| ST_BTN_DOWN     | EV_BTN_DOWN  |             | ST_BTN_DOWN    |                        |
| ST_BTN_DOWN     | EV_BTN_UP    |             | ST_BTN_RISING  | tick = DEL_BTN_MAX     |

---

### ✅ Conclusión
- Los **Eventos** (`EV_BTN_UP`, `EV_BTN_DOWN`) reflejan el valor binario del pulsador.  
- Las **Acciones** inicializan/modifican el temporizador (`tick`) y generan señales hacia el módulo System (`EV_SYS_BTN_DOWN`).  
- El temporizador actúa como **guard** para filtrar rebotes (*debouncing*), asegurando que solo se reconozcan transiciones válidas.

