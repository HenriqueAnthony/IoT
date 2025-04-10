# IoT

// Pinos
const int sensorTemp = A0;           // TMP36/LM35 no pino A0
const int pinoVentilador = 3;        // Motor do ventilador no pino 3
const int pinoLed = 4;               // LED vermelho no pino 4
const int pinoBuzina = 5;            // Buzzer no pino 5

float temperatura = 0;

void setup() {
  pinMode(pinoVentilador, OUTPUT);
  pinMode(pinoLed, OUTPUT);
  pinMode(pinoBuzina, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  int leitura = analogRead(sensorTemp);
  
  // TMP36: temperatura em °C = ((leitura * 5.0 / 1023.0) - 0.5) * 100
  // LM35: temperatura em °C = (leitura * 5.0 / 1023.0) * 100
  temperatura = (leitura * 5.0 / 1023.0) * 100.0; // Supondo LM35

  Serial.print("Temperatura: ");
  Serial.println(temperatura);

  // Ventilador ligado se >= 30 °C
  digitalWrite(pinoVentilador, temperatura >= 30 ? HIGH : LOW);

  // Alarme de emergência se > 50 °C
  if (temperatura > 50) {
    digitalWrite(pinoLed, HIGH);
    digitalWrite(pinoBuzina, HIGH);
  } else {
    digitalWrite(pinoLed, LOW);
    digitalWrite(pinoBuzina, LOW);
  }

  delay(1000);
}
