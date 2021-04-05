# PRÁCTICA 4B: 
## CÓDIGO DEL PROGRAMA:

```
#include <Arduino.h>

const int led1 = 2; // Pin del LED

SemaphoreHandle_t xBinarySemaphore;

void LedOnTask( void * pvParameters);
void LedoffTask( void * pvParameters);

void setup()
{
  Serial.begin(115200);
  pinMode(led1 ,OUTPUT);
  xBinarySemaphore = xSemaphoreCreateBinary();
  xTaskCreate(LedOnTask, "ON",1000,NULL,1,NULL);
  xTaskCreate(LedoffTask, "OFF", 1000,NULL,1,NULL);
  xSemaphoreGive(xBinarySemaphore);
}

void loop(){
    delay(1000);
}
void LedOnTask(void *pvParameters)
{
  while(1)
  {
   xSemaphoreTake(xBinarySemaphore,portMAX_DELAY);
   Serial.println("ON");
   digitalWrite(led1,HIGH);
   xSemaphoreGive(xBinarySemaphore);
   vTaskDelay(1000);
  }
}
void LedoffTask(void *pvParameters)
{
  while(1)
  {
    xSemaphoreTake(xBinarySemaphore,portMAX_DELAY);
    Serial.println("OFF");
    digitalWrite(led1,LOW);
    xSemaphoreGive(xBinarySemaphore);
    vTaskDelay(1000);
  }
}

```