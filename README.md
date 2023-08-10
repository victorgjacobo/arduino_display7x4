# Arduino y display 7 segmentos 4 digitos

# Tabla de contenidos
1. [Materiales](#Materiales)
2. [Descripción](#Descripción)
3. [Código](#Código)

## Materiales
- Arduino Uno
- Protoboard
- Display 7 segmentos 4 digitos
- Jumpers 

## Descripción
El display 7x4 está compuesto de siete segmentos que se pueden encender o apagar individualmente. A diferencia de otros display este cuenta con 4 dígitos que aumentan y facilitan la visualización de datos. 

![display7x4](https://github.com/victorgjacobo/arduino_display7x4/assets/141197135/6a028808-7614-4c62-8d56-5ce46bdb282f)


En la figura observamos la conexión del arduino uno con el display 7 segmentos 4 digitos.  Los digitos están conectados de la siguiente manera: `D1 -> GPIO 10`, `D2 -> GPIO 11`, `D3 -> GPIO 12`, `D4 -> GPIO 13`. Para el caso de los segmentos, estos están conectados de la siguiente manera, `A -> GPIO 2`, `B -> GPIO 3`, `C -> GPIO 4`, `D -> GPIO 5`,`E -> GPIO 6`, `F -> GPIO 7`, `G -> GPIO 8`.

![arduinodisplay7x4](https://github.com/victorgjacobo/arduino_display7x4/assets/141197135/66c6d5fd-223d-4aad-a681-607db8fcf7c0)

## Código
```C
"""
  Nombre de la práctica: Contador con display 7 segmentos 4 digitos
  Microcontrolador Arduino Uno
  Autor: Ing. Víctor González Jacobo
"""
/////////////////////////A  B  C  D  E  F  G 
byte pines_display [] = {2, 3, 4, 5, 6, 7, 8};
////////////////   D1  D2  D3  D4
byte Display [] = {10, 11, 12, 13};
byte const digi_display_catodo[] = {0x3f,0x06,0x5b,0x4f,0x66,0x6d,0x7d,0x07,0x7f,0x67};

int cuenta, x, segundos, minutos = 0;
int unidad, decena, centena, millar;

void sel_display(int pos, int val);
void desplegar_numero(int val);

void setup() {
  for (int i = 0; i <= sizeof(pines_display); i++)
  {
    pinMode(pines_display[i], OUTPUT);
    digitalWrite(pines_display[i], HIGH);
  } 
  for (int i = 0; i <= sizeof(Display); i++)
  {
    pinMode(Display[i], OUTPUT);
    digitalWrite(Display[i], HIGH);
  }
}

void loop() {
  x = millis()/1000;
  segundos = x%60;
  minutos =  x/60;
  cuenta  = minutos * 100 + segundos;
  conversion(cuenta);
}

void conversion(int conteo)
{
  unidad = conteo%10;
  decena = (conteo%100)/10;
  centena = (conteo%1000)/100;
  millar = conteo/1000;

  sel_display(4, unidad);
  sel_display(3, decena);
  sel_display(2, centena);
  sel_display(1, millar);
}

void sel_display(int pos, int val)
{
  for (int i = 0; i < sizeof(Display); i++)
  {
    digitalWrite(Display[i], HIGH);
  }  
  desplegar_numero(val);
  digitalWrite(pos + 9, LOW);
}

void desplegar_numero(int val)
{
  byte numero2 = digi_display_catodo[val];
    for (int i = 0; i <7; i++)
    {
      int bit =  bitRead(numero2, i);
      digitalWrite(pines_display[i], bit);
    }
}
```

![C](https://img.shields.io/badge/c-%2300599C.svg?style=for-the-badge&logo=c&logoColor=white)
![C++](https://img.shields.io/badge/c++-%2300599C.svg?style=for-the-badge&logo=c%2B%2B&logoColor=white)
![Arduino](https://img.shields.io/badge/-Arduino-00979D?style=for-the-badge&logo=Arduino&logoColor=white)
![](https://img.shields.io/github/watchers/victorgjacobo/arduino_display7x4)
