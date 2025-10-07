# Laboratorio-Nro.4
Programación de rutinas en Arduino

Integrante: Michael Esteban Viveros, 
            Daniel Esteban Moran,
            Jairo Daniel Cabrera
            
# Descripción general
 Este laboratorio tuvo como objetivo aplicar los conocimientos adquiridos en el manejo de rutinas y control de periféricos utilizando la plataforma Tinkercad
 y el entorno de programación de Arduino.
 Se desarrolló un circuito que permite medir la temperatura ambiente con un sensor TMP36, mostrar el valor en una pantalla LCD, y controlar un LED y un ventilador según tres condiciones de temperatura predefinidas.
 
# Componentes electrónicos utilizados
1.Arduino UNO 
2.Sensor TMP36 
3.Pantalla LCD 16x2 
4.LED
5.Motor DC 
6.Resistencias 
7.Protoboard y cables 

# Procedimiento
1. Se construyó el circuito en Tinkercad, siguiendo el diagrama propuesto en la guía del laboratorio.  
2. Se implementó el código base proporcionado y se añadió la rutina que obtiene la temperatura del sensor TMP36.  
3. Se utilizó la función `lcd_1_print()` para mostrar la temperatura en la pantalla LCD.  
4. Se desarrollaron las siguientes validaciones:
   
  **Validación 1**. Si la temperatura es menor o igual a 10 °C, el LED debe parpadear con un delay
   de medio segundo y el ventilador (motor) debe estar apagado.
   **Validación 2**. Si la temperatura está entre 11 °C y 25 °C, el led y el ventilador (motor) debe
   estar apagado.
   **Validación 3**. Si la temperatura es mayor o igual a 26 °C, el led debe permanecer encendido y
   el ventilador (Motor), debe encenderse.

   

# Link tinkercad
https://www.tinkercad.com/things/5HdrLNMMkhx-mighty-trug/editel?returnTo=https%3A%2F%2Fwww.tinkercad.com%2Fdashboard&sharecode=ZeYCyP36xZKtMY-Lp8MqxeVH5VZ4cNN4kS9hQBEjOAc


# Imagen circuito 

https://github.com/viverosmva/Laboratorio-Nro.4/blob/main/Mighty%20Trug.png?raw=true

# Código fuente
#include <LiquidCrystal.h>
// Configuración de pines del LCD: RS, E, D4, D5, D6, D7
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

// Pines del sistema
const int TMP36_PIN = A0; // Sensor TMP36
const int LED_PIN = 7; // LED indicador
const int FAN_PIN = 9; // Motor/ventilador

float temperatura = 0;

void setup() {
lcd.begin(16, 2);
pinMode(LED_PIN, OUTPUT);
pinMode(FAN_PIN, OUTPUT);
pinMode(TMP36_PIN, INPUT);

lcd.print("Sensor TMP36");
delay(2000);
lcd.clear();
}

void loop() {
int valorADC = analogRead(TMP36_PIN);
// Conversión para TMP36: (Vout en mV - 500) / 10 = °C
float voltaje = (valorADC * 5.0) / 1024.0;
temperatura = (voltaje - 0.5) * 100.0;

lcd.setCursor(0, 0);
lcd.print("Temp: ");
lcd.print(temperatura);
lcd.print(" C");

// Validación 1: Temperatura <= 10°C
if (temperatura <= 10) {
digitalWrite(FAN_PIN, LOW);
digitalWrite(LED_PIN, HIGH);
delay(500);
digitalWrite(LED_PIN, LOW);
delay(500);
lcd.setCursor(0, 1);
lcd.print("Frio FAN:OFF ");
}

// Validación 2: 11°C <= Temp <= 25°C
else if (temperatura > 10 && temperatura <= 25) {
digitalWrite(LED_PIN, LOW);
digitalWrite(FAN_PIN, LOW);
lcd.setCursor(0, 1);
lcd.print("Normal FAN:OFF");
}

// Validación 3: Temperatura >= 26°C
else if (temperatura >= 26) {
digitalWrite(LED_PIN, HIGH);
digitalWrite(FAN_PIN, HIGH);
lcd.setCursor(0, 1);
lcd.print("Calor FAN:ON ");
}

delay(1000);
}


   
