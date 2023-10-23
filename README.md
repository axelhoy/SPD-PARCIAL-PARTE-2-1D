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

## 💻 Función principal
El contador suma del 0 al 99 (o 97 si es primo), de a 1 o en numeros primos, dependiendo de la posición del deslizante.
Cuando el display se encuentra en numeros naturales, y llega a 5, este prende el motor, y si el sensor de flexion no esta flexionado, este no prende

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
~~~
### keypressed
Devuelve lo que recibe por parametro, si se presiona el boton para subir la flag pasa a valer 1, y pregunta
Si se presiono (sumar = 0), entonces es diferente a la flag
Si es diferente a la flag, la flag pasa a valer 0
Cuando se suelta, sumar vuelve a valer 1, y la flag vuelve a 1 tambien ya que lo convierte en lo mismo
~~~ C
int sumar;
int flagsumar;
int restar;
int flagrestar;
int reset;
int flagreset;

int keypressed(void){
  	sumar = digitalRead(SUBIRLO);
  	restar = digitalRead(BAJARLO);
  	reset = digitalRead(RESET);
  	if (sumar)
      flagsumar = 1;
  	if (restar)
      flagrestar = 1;
  	if(reset)
      flagreset = 1;
 	
  if(sumar != flagsumar)
  {
    flagsumar = sumar;
      return SUBIRLO;

(...)

  return 0;
}
~~~
### Numeros primos
#### esPrimo
recibe por parametro un numero, si la resta de la division del numero divido por 2 es 0, entonces devuelve false
tambien se incluye el for, el cual verifica al numero 2, ya que si es 2 este se salta la iteracion y returna directamente true

#### primoSuma
Recibe start(En este caso es countPrimo, el cual esta incializado en 0), luego le suma 1, si ese numero es primo, lo retorna, si no es, lo va sumando hasta encontrar un numero primo
0+1 = 1, no es primo, 1+1 = 2, es primo, lo retorna

#### primoResta
Misma funcionalidad que el anterior, pero va restando hasta encontrar el siguiente numero primo a retornar
~~~ C
bool esPrimo(int n) {
  if (n <= 1) {
    return false;
  }
  for (int i = 2; i * i <= n; i++) {
    if (n % i == 0) {
      return false;
    }
  }
  return true;
}

int primoSuma (int start) {
  int num = start + 1;
  while (true) {
    if (esPrimo(num)) {
      return num;
    }
    num++;
  }
}
int primoResta(int start) {
  int num = start - 1;
  while (true) {
    if (esPrimo(num)) {
      return num;
    }
    num--;
  }
}
~~~
### void loop()
Se crean las variables, luego pregunta si el sensor esta flexionado.
Si este se encuentra flexionado pregunta por la posicion del deslizante, entrando a la funcion de sumar los digitos en numeros naturales o primos.
Como ultimo, si el digito es 5 y es natural un motor de CC es prendido
~~~ C

void loop()
{
  lectura = analogRead(SENSOR);
  int pressed = keypressed();
  int deslizanteON = digitalRead(4);
  
 if(lectura >=500){
    if (deslizanteON==0){
    if(pressed==SUBIRLO){
        countDigit++;
        if(countDigit>99)
          countDigit=0;
      }
      else if(pressed==BAJARLO){
        countDigit--;
        if(countDigit<0)
          countDigit=99;
      }
      else if(pressed==RESET){
        countDigit=0;
      }
      printCount(countDigit);
    }
    else{
    if(pressed==SUBIRLO){
        countPrimo = primoSuma(countPrimo);
        if(countPrimo>97)
          countPrimo=0;
      }
      else if(pressed==BAJARLO){
        countPrimo = primoResta(countPrimo);
        if(countPrimo>99)
          countPrimo=97;
      }
      else if(pressed==RESET){
        countPrimo=0;
      }
            printCount(countPrimo);
    }
      

   if (countDigit == 5) {
    digitalWrite(MOTOR, HIGH);
  } else {
    digitalWrite(MOTOR, LOW);
  }
  }
}
~~~ 
## 🔑 Link 

 - [Proyecto](https://www.tinkercad.com/things/219u1bS1KuL-parcial-domiciliario-2-grupo-9-1d/editel?sharecode=vHDrqeMG8JtjAcvGix0EM_X-mfbsCjIE4na5r9iMePs)
