# Clase de bits y bytes

![](https://i.pinimg.com/originals/7f/7f/28/7f7f2882899755a705a2953b6fcfc263.gif)

_lluvia digital de Matrix_

## De interruptores a números

Cómo hemos visto en otras partes del curso, la unidad mínima de información en cualquier sistema informatizado es el bit. Un interruptor donde "encendido" es 1 y "apagado" es 0.

Con esto, podemos hacer números. Pero ¿cómo?

![imagen](https://user-images.githubusercontent.com/60569015/113589090-40396800-9631-11eb-9f52-a75d79419586.png)

Los números normalmente los expresamos con un sistema decimal, pero existen otros sistemas. En la pizarra explicaremos los sistemas binario, octal, hexadecimal y su comparación con el decimal. 

El binario sólo tiene 0 y 1. 

El octal tiene cifras del 0 al 7. 

El hexadecimal tiene cifras del 0 al 6 y de la a a la e para representar las 5 siguientes cifras. 

### Ejercicio con arduino. Hacer números con interruptores

Con esto podemos hacer un pequeño programa para hacer números con interruptores. Utilizaremos 5 interruptores. Este ejercicio no está _acabado_ sino que se pide crear un programa propio que combine los diferentes estadoBoton para generar números de diferentes formas. 

``` C++

const int pinBotones[] = {2,3,4,5,6};
const int numeroBotones = 5;
int estadoBoton1 = 0;
int estadoBoton2 = 0;
int estadoBoton3 = 0;
int estadoBoton4 = 0;
int estadoBoton5 = 0;

void setup() {
 for(int x=0; x<=numeroBotones; x++){
  pinMode(pinBotones[x],INPUT);
 }
Serial.begin(9600);
}

void loop() {
  //leer botones
  estadoBoton1 = digitalRead(pinBotones[0]);
  estadoBoton2 = digitalRead(pinBotones[1]);
  estadoBoton3 = digitalRead(pinBotones[2]);
  estadoBoton4 = digitalRead(pinBotones[3]);
  estadoBoton5 = digitalRead(pinBotones[4]);
  int numero;
// Espacio en blanco para hacer operaciones con números


//Mandamos al ordenador el número
  Serial.print("El número que sale con los botones es: ");
  Serial.println(numero);
}
``` 
Opciones posibles: sumarlos todos. Sumar algunos y restar otros. Sumar unos y multiplicarlos por otros. Multiplicar algunos de ellos por números más grandes (por ejemplo que el botón 4 signifique 432). 


Una vez tenemos _números_ podemos hacer cosas. Al menos representar números. Pero nos falta representar otras cosas como imágenes, sonidos o letras. 

La siguiente vuelta es hacer un traspase de números a letras

## De números a letras 



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


