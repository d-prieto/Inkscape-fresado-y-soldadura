# Clase de bits y bytes

![](https://i.pinimg.com/originals/7f/7f/28/7f7f2882899755a705a2953b6fcfc263.gif)
_lluvia digital de Matrix_

Cómo hemos visto en otras partes del curso, la unidad mínima de información en cualquier sistema informatizado es el bit. Un interruptor donde "encendido" es 1 y "apagado" es 0.

Estos números se pueden agrupar de muchas formas. Pueden crear sonido, pueden modelar imágenes y pueden modelar texto. 

Así que vamos a hacer operaciones con números 

![imagen](https://user-images.githubusercontent.com/60569015/113589090-40396800-9631-11eb-9f52-a75d79419586.png)

La siguiente unidad después del bit dentro de la informática es el byte, que son 8 bits. 


Hoy vamos a hacer un par de ejercicios para pasar información de botones a bits y mandarlos al ordenador como letras y la operación inversa. 

Primero decir que existe una conversión (creo que Arduino utiliza ANSI, pero puede que use ASCII). He modificado un código que he encontrado aquí: https://forum.arduino.cc/index.php?topic=647786.msg4369705#msg4369705


``` C++


void setup() {
  Serial.begin(9600);

}

void loop() {
  if(Serial.available()){
      int c = Serial.read();
    if (c != -1) { // if we have received a byte
      Serial.print((char) c);
      Serial.print(F("\t0x"));
      Serial.print((byte) c, HEX);
      Serial.print(F("\t0b"));
      for (char aBit = 7; aBit >= 0; aBit--) {
        Serial.print(bitRead(c, aBit));
      }
      Serial.print(F("\t"));
      for (char aBit = 7; aBit >= 0; aBit--) {
        if (bitRead(c, aBit)) Serial.write('*'); else Serial.write('_');
      }
      Serial.println();
    }
  }
}
```

Este código habla a través del Serial y cuando escribimos algo, nos lo transforma en código hexadecimal y en bits de dos formas diferentes. 

Una versión con ocho leds sería así: 




``` C++
const int firstLedPin = 2;




void setup() {
  Serial.begin(9600);
  for (int x = firstLedPin; x <= firstLedPin +8; x++){
    pinMode(x, OUTPUT);
  }
}

void loop() {
  if(Serial.available()){
      int c = Serial.read();
    if (c != -1) { // if we have received a byte
      Serial.print((char) c);
      Serial.print(F("\t0x"));
      Serial.print((byte) c, HEX);
      Serial.print(F("\t0b"));
      for  (char aBit = 7; aBit >= 0; aBit--) {
        Serial.print(bitRead(c, aBit));
      }
      Serial.print(F("\t"));
      int pinLed = 2;
      for (char aBit = 7;  aBit >= 0; aBit--) {
        if (bitRead(c, aBit)) {
          Serial.write('*');
          digitalWrite(pinLed, HIGH);
        }
        else{
          Serial.write('_');
          digitalWrite(pinLed, LOW);
        }
        pinLed++;
      }
      Serial.println();
      delay(1000);
    }
    Serial.println("Fin del mensaje");
  }
}
```



Ahora vamos a intentar hacer lo contrario para lo cual vamos a necesitar 8 botones o 1 botón y cables con posible conexión manual a 5V o GND. 


