# PRACTICA 7.1: Reproducción desde memoria interna

## Codigo

```
#include "Arduino.h"
#include "FS.h"
#include "HTTPClient.h"
#include "SPIFFS.h"
#include "SD.h"
#include "AudioGeneratorAAC.h"
#include "AudioOutputI2S.h"
#include "AudioFileSourcePROGMEM.h"
#include "sampleaac.h"
AudioFileSourcePROGMEM *in;
AudioGeneratorAAC *aac;
AudioOutputI2S *out;
void setup(){
Serial.begin(9600);
in = new AudioFileSourcePROGMEM(sampleaac, sizeof(sampleaac));
aac = new AudioGeneratorAAC();
out = new AudioOutputI2S();
out -> SetGain(0.125);
out -> SetPinout(26,25,22);
aac->begin(in, out);
}
void loop(){
if (aac->isRunning()) {
aac->loop();
} else {
aac -> stop();
Serial.printf("Sound Generator\n");
delay(1000);
}
}
```

# Funcionamiento

Primero de todo anañadimos las 9 librerias. Las ultimas 4 son de la biblioteca de audio de ESP8266 de Earle F.Philhower.

```
#include "Arduino.h"
#include "FS.h"
#include "HTTPClient.h"
#include "SPIFFS.h"
#include "SD.h"
#include "AudioGeneratorAAC.h"
#include "AudioOutputI2S.h"
#include "AudioFileSourcePROGMEM.h"
#include "sampleaac.h"
```

Seguidamente declaramos tres punteros, con el primero `AudioFileSourcePROGMEM `
leeremos un archivo de una matriz **Progrem** , el segundo `AudioGeneratorAAC` reproduce un archivo AAc mono o estereo utilizando un decodificador AAC de punto fijo Helix y el tercero `AudioOutputI2` envia señales mono o estereo a cualquier frecuencia.

```
AudioFileSourcePROGMEM *in;
AudioGeneratorAAC *aac;
AudioOutputI2S *out;
```

En el `void setup()` inicializamos una comunicación en serie con una velocidad de 115200 bauds. Declaramos el puntero **in** como una variable. Después utilizaremos el puntero **aac** para realizar la comunicación entre la placa y el decodificador. Por ultimo con el puntero **out** declararemos la ganancia, los punes de salida y la salida mono.

```
void setup(){
Serial.begin(9600);
in = new AudioFileSourcePROGMEM(sampleaac, sizeof(sampleaac));
aac = new AudioGeneratorAAC();
out = new AudioOutputI2S();
out -> SetGain(0.125);
out -> SetPinout(26,25,22);
aac->begin(in, out);
}
```

En el `void loop()` crearemos un if donde miraremos si la variable Running es true para saber que no hay ningun error. Si no da error, llamamos a la función `loop()`.
Si la variable fuese **false** , se llamaria a la función `stop()` para cerrar el fichero de audio.

```
void loop(){
if (aac->isRunning()) {
aac->loop();
} else {
aac -> stop();
Serial.printf("Sound Generator\n");
delay(1000);
}
}
```
