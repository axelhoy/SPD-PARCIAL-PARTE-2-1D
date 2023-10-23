# spdparcialgrupo
# Contador binario Parcial SPD 1D

![ARDUINO-BASICO-_-ESPACIO-MAKER-1024x576](https://github.com/axelhoy/spdparcialgrupo/assets/121445908/3e5ca593-c383-400d-85de-061c515ea6cb)

## Integrantes 
1D
- Cannavina Axel
- Velazco Franco
- Dante Tucci
- Alexis Portillo

## Proyecto: Contador binario

![arduino](https://github.com/axelhoy/spdparcialgrupo/assets/121445908/6086e640-e2bf-49c9-ac2e-6f4265dded9b)

## Descripción
Contador binario creado con arduino, implementando el uso de displays 7 segmentos, sensor de flexion, pulsadores y un boton deslizante.

## Función principal
Funciones principales a continuacion:

### printDigit

Esta funcion se encarga de encender los correspondientes leds de un display 7 segmentos, para formar el digito enviado por parametro (digit)

~~~ C 
void printDigit(int digit)
{
  digitalWrite(A, LOW);
....
  digitalWrite(F, HIGH);
  break;
}
~~~
### prendeDigito
Recibe si debe prender el display decimo, o el display de la unidad, sino, apagar ambos
~~~ C 
void prendeDigito(int digito){
    if (digito == UNIDAD){
      digitalWrite(UNIDAD, LOW);
      digitalWrite(DECIMO, HIGH);
      delay(TIEMPO);
    }
    else if (digito == DECIMO){
      digitalWrite(UNIDAD, HIGH);
      digitalWrite(DECIMO, LOW);
      delay(TIEMPO);
    }
    else
    {
      digitalWrite(UNIDAD, HIGH);
      digitalWrite(DECIMO, HIGH);
    }
  }
~~~

### printCount

Recibe el numero a prender, se divide primero por 10 y a partir de ese numero imprime el decimo
luego se le resta 10 y se multiplica por el numero dividido 10 (parse int)
Ej:

14
14/10 
= 1,4 (Prende 1)

Prender decimo

(14-10) * (14/10)
4 * 1,4 
4 * 1 (se parsea a int, borrando los decimales)
= 4 (Prende 4)

Prender unidad

~~~ C
void printCount(int count){
  	prendeDigito(OFF);
    printDigit(count/10);
   	prendeDigito(DECIMO);
    prendeDigito(OFF);
    printDigit(count - 10 * ((int)count/10));
    prendeDigito(UNIDAD);
  }
