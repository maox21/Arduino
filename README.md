//Arduino-HC-SR04
===============

//Arduino HC SR04 sensor to distance measurement


#define SOUND_SPD 344
volatile long t_inicio;
volatile long t_fin;
volatile double lastDist;

void setup()
{
  Serial.begin(9600);
  pinMode(3,INPUT);
  pinMode(2,OUTPUT);
  attachInterrupt(1,detection,CHANGE);
  digitalWrite(2,LOW);
}

void detection() {
  if(t_inicio==0) {
    t_inicio = micros();
  } else {
    t_fin = micros();
    lastDist = (((t_fin-t_inicio)/1000000.0)*SOUND_SPD)/2;
    Serial.write("Distance: ");
    Serial.print(lastDist);
    Serial.write("m\r\n");
  }
}

void startSensor()
{
  t_inicio = 0;
  digitalWrite(2,HIGH);
  delayMicroseconds(10);
  digitalWrite(2,LOW);
}

void loop()
{
  startSensor();
  delay(1000);
}
