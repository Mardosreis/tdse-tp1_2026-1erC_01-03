08) Enunciar y describir los Eventos y Acciones del modelo Sensor (un solo botón) necesarios para describir el
comportamiento del módulo de código C del tipo temporizado (Update by Time Code, period = 1mS) para
“escrutar”, necesario para la implementación referida en el TP1 - Actividad 00 - Paso 05.😁
Las Acciones del modelo System pueden ser signals (Eventos) para el modelo Actuator o bien ejecutar funciones o
bien “inicialización/modificación” de variables de control (timer); variables que pueden usarse como guard para
condicionar transiciones (trigger [guard] / effect



  RESPUESTA: 
  Eventos del modelo Sensor:
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
                  
