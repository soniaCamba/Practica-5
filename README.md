# Practica-5

## CÓDIGO
```
#include <Arduino.h>
// Esta libreria nos permitirá comunicarnos con I2C.
#include <Wire.h>

void setup()
{
  Wire.begin();
 
  Serial.begin(115200);
  while (!Serial);             // Leonardo: wait for serial monitor
  Serial.println("\nI2C Scanner");
}
 
 
void loop()
{
  byte error, address;
  int nDevices;
 
  Serial.println("Scanning...");
 
  nDevices = 0;
  for(address = 1; address < 127; address++ )
  {
    // The i2c_scanner uses the return value of
    // the Write.endTransmisstion to see if
    // a device did acknowledge to the address.
    Wire.beginTransmission(address);
    error = Wire.endTransmission();
 
    if (error == 0)
    {
      Serial.print("I2C device found at address 0x");
      if (address<16)
        Serial.print("0");
      Serial.print(address,HEX);
      Serial.println("  !");
 
      nDevices++;
    }
    else if (error==4)
    {
      Serial.print("Unknown error at address 0x");
      if (address<16)
        Serial.print("0");
      Serial.println(address,HEX);
    }    
  }
  if (nDevices == 0)
    Serial.println("No I2C devices found\n");
  else
    Serial.println("done\n");
 
  delay(5000);           // wait 5 seconds for next scan
}
```
## FUNCIONAMIENTO

Primero definimos librerias. Luego dentro del setup inicializamos Wire.
Dentro del loop creamos un bucle, donde adress parte de 1 y maximo puede llegar a 126.
```
 for(address = 1; address < 127; address++ )
 ```
 Dentro del bucle mencionado anteriormente, el i2c_scanner utiliza el valor de retorno de endTransmisstion para ver si algun dispositivo reconoce la dirección.
 ```
 {
    Wire.beginTransmission(address);
    error = Wire.endTransmission();
```
Cuando el error sea nulo imprimiremos por pantalla "I2C device found at address 0x". Y si la dirección es menor a 16, al contador nDevices se le sumará 1.
```
if (error == 0)
    {
      Serial.print("I2C device found at address 0x");
      if (address<16)
        Serial.print("0");
      Serial.print(address,HEX);
      Serial.println("  !");
 
      nDevices++;
    }
```
Si el error es 4 impimirá "Unknown error at address 0x", que significa que no reconoce la dirección. Y si la dirección es inferior a 16, imprimirá 0 junto a la dirección.
```
else if (error==4)
    {
      Serial.print("Unknown error at address 0x");
      if (address<16)
        Serial.print("0");
      Serial.println(address,HEX);
    }
```
Si el contador nDevices es 0, imprimirá por pantalla "No I2C devices found", es decir, no se ha encontrado ningun I2C.
```
if (nDevices == 0)
    Serial.println("No I2C devices found\n");
```
Pero si no lo es imprimirá un "done"
```
else
    Serial.println("done\n");
```
Y por ultimo hay un delay de 5000ms
```
delay(5000);
```
