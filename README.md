# Arduino Remote Car Starter

## Descripción
Proyecto educativo para encender un automóvil de forma remota usando Arduino y un módulo RF. Controla contacto y arranque mediante relés, permitiendo aprender sobre electrónica automotriz y control remoto de manera segura. Solo fines educativos.

## Características
- Encendido remoto del motor mediante señal RF  
- Control independiente de contacto y arranque mediante relés  
- Código Arduino modular y adaptable a distintos vehículos  
- Uso de fusibles y relés para seguridad eléctrica  

## Componentes necesarios
| Componente | Cantidad | Notas |
|------------|---------|-------|
| Arduino Nano/Uno | 1 | Microcontrolador principal |
| Módulo RF 433 MHz receptor | 1 | Compatible con control remoto |
| Relé 30A | 2 | Uno para contacto, otro para arranque |
| Fuente 12V → 5V | 1 | Step-down para Arduino |
| Botón remoto / llavero RF | 1 | Para enviar señal al Arduino |
| Cables y fusibles | - | Para conexiones seguras |

## Diagrama conceptual

[Control Remoto RF] --> [Receptor RF] --> [Arduino]
|-- Pin 7 --> [Relé Contacto] --> Switch IGN ON
|-- Pin 8 --> [Relé Arranque] --> Motor Arranque
Arduino alimentado por Step-down 12V->5V desde batería del auto

Instrucciones de montaje

Conectar módulo RF al Arduino (DATA → D2).

Conectar relés a los pines 7 y 8.

Relé contacto → Switch IGN ON, Relé arranque → Motor de arranque.

Alimentar Arduino con step-down 12V→5V.

Insertar fusibles en serie con los relés.

Subir código al Arduino y probar con control remoto.

Recomendaciones de seguridad

Nunca conectar Arduino directamente al motor de arranque.

Usar fusibles de 5–10A en cada línea de relé.

Desconectar batería del auto mientras se hacen conexiones.

Probar primero en simulador o vehículo de bajo riesgo.


---

Si querés, puedo generarte un **archivo `.zip` listo para descargar** con este README y un ejemplo de código Arduino, así solo lo descomprimes y subes al repo.  

¿Querés que haga eso?


## Código de ejemplo
```cpp
#include <RCSwitch.h>
RCSwitch mySwitch = RCSwitch();

const int releContacto = 7;
const int releArranque = 8;

void setup() {
  pinMode(releContacto, OUTPUT);
  pinMode(releArranque, OUTPUT);
  digitalWrite(releContacto, LOW);
  digitalWrite(releArranque, LOW);

  mySwitch.enableReceive(0); // Pin D2 para receptor RF
}

void loop() {
  if (mySwitch.available()) {
    long code = mySwitch.getReceivedValue();
    if (code == 123456) { // Código del control remoto
      encenderAuto();
    }
    mySwitch.resetAvailable();
  }
}

void encenderAuto() {
  digitalWrite(releContacto, HIGH);
  delay(500);
  digitalWrite(releArranque, HIGH);
  delay(1500);
  digitalWrite(releArranque, LOW);
}

