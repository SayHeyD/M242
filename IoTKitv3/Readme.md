# IoTKitV3

## Erstes Programm auf Mikroprozessor anwenden (K3)

Als erstes Programm haben wir uns für das Programm "[HTS221](https://os.mbed.com/compiler/#import:/teams/IoTKitV3/code/HTS221/)" entschieden. Das Programm liest über den vorhandenen Sensor auf dem PCB die Lufttemparatur und Feuchtigkeit aus.

Um das Programm auf dem Board zu installieren müssen fooglenden Schritte befolt werden:

### Importieren des Programms in das Online IDE.

Der Sourcecode des Programms kann [über GitHub](https://github.com/mc-b/IoTKitV3/blob/master/i2c/HTS221/src/main.cpp) eingesehen werden. Allerdings können wir, falls wir uns bei [ARM Mbed](https://ide.mbed.com/) ein Konto erstellt haben das Programm direkt in die IDE importieren, was uns etwas Arbeit abnimmt.

Wenn wir nun also auf den [import-link](https://os.mbed.com/compiler/#import:/teams/IoTKitV3/code/HTS221/) klicken, sollte sich direkt die Web-Oberfläche öffnen.

Nun sollten wir folgendes sehen:

<img src="https://github.com/SayHeyD/M242/raw/main/IoTKitv3/Bildschirmfoto%202021-01-11%20um%2021.53.53.png" alt="Programm-Import">

Hier müssen wir nur Sicherstellen das wir bei "Import as", "Programm" auswählen, danach können wir auf "Import" klicken.

### Betrachtung des Programmcodes

Nun können wir links in der "Program Workspace" das Programm "HTS221" auswählen, wenn wir auch den Source-Code sehen möchten können wir auf das "+" klicken und dann die "main.cpp" auswählen:

<img src="https://github.com/SayHeyD/M242/raw/main/IoTKitv3/Bildschirmfoto%202021-01-11%20um%2021.58.45.png" alt="Programm-Workspace und Dateien">

### Kompilieren des Codes

Um den Code auf dem Mikroprozessor ausführen zu können, müssen wir ihn zu einer Binär-Datei compilen. Dies können wir in der in der Online IDE eifach über den Button Compilen machen.

<img src="https://github.com/SayHeyD/M242/raw/main/IoTKitv3/Bildschirmfoto%202021-01-11%20um%2022.25.58.png" alt="Compile Button in Workspace">

Danach läuft der Compile-Prozess und falls keine Fehler auftreten startet nach dem compilen der Download. Diese ".bin" Datei kann man danach auf den Mikroprozessor übertragen.

### Übertragung auf Mikroprozessor

Um das Programm auf den Mikrprozessor zu übertragen, muss zwischen dem Gerät auf das die Datei gedownloaded wurde und dem Board eine Verbindung per USB bestehen. Hierfür sollte ein USB-Kable im IoTKitV* enthalten sein. Sobald man die USB-Verbindung hergestellt hat, sollte der Mikroprozessor automatisch und unabhängig vom Betriebssystem als USB-Stick erkannt werden. Nun kann man die ".bin" Datei einfach auf dieses neu erkannte USB-Gerät kopieren.

<img src="https://github.com/SayHeyD/M242/raw/main/IoTKitv3/Bildschirmfoto%202021-01-11%20um%2022.43.46.png" alt="File copy onto Board">

### Starten des Programms

Nach dem Kopieren der Datei sollte sich der Mikroprozessor automatisch von PC trennen. Danach müssen wir nur noch den Reset-Knopf am Board drücken. Danach sollte das Programm starten.

<img src="https://github.com/SayHeyD/M242/raw/main/IoTKitv3/IMG_2985.jpg" width="800px" alt="Reset Switch on Board">

### Überprüfen der Funktionalität

Um zu überprüfen ob das Programm richtig funktioniert, können wir 2 Dinge tun:

1. Überpprüfen ob das Display Temperatur und Luftfeuchtigkeit anzeigt
2. Einen Finger auf den Sensor halten und auf dem Display überprüfen ob die Temperatur steigt

<img src="https://github.com/SayHeyD/M242/raw/main/IoTKitv3/IMG_2986.jpg" width="800px" alt="Display with Temperature and Humidity">

## Abändern eines bestehenden Programms (K3)

Um ein Programm abzuändern müssen wir den SourceCode verändern. Ich habe mich hier entschieden nur den Output auf den OLED-Display, des Programms HTS221, zu ändern.

Dafür müssen wir das "main.cpp" file ändern. Wie wir das "main.cpp" file aufrufen ist [oben](https://github.com/SayHeyD/M242/tree/main/IoTKitv3#betrachtung-des-programmcodes) schon beschrieben.

Nun können wir den Sourcecode verändern. Ich habe das Programm folgendermassen angepasst:

Ich habe die erste Ausgabe auf dem OLED leicht verändert und bei der sich wiederholenden Nachricht ein "°" nach dem "C" hinzugeüfgt.

Dies:
```cpp
oled.printf( "Temp/Hum Sensor\n" );
```

Zu:
```cpp
oled.printf( "Temp | Hum Sensor\n" );
```

Und dies:
```cpp
printf("HTS221:  [temp] %.2f C, [hum]   %.2f%%\r\n", value1, value2);
```

Zu:
```cpp
printf("HTS221:  [temp] %.2f C°, [hum]   %.2f%%\r\n", value1, value2);
```

Also sieht mein Programm momentan so aus:
```cpp
#include "mbed.h"
#include "HTS221Sensor.h"
#include "OLEDDisplay.h"

// UI
OLEDDisplay oled( MBED_CONF_IOTKIT_OLED_RST, MBED_CONF_IOTKIT_OLED_SDA, MBED_CONF_IOTKIT_OLED_SCL );

static DevI2C devI2c( MBED_CONF_IOTKIT_I2C_SDA, MBED_CONF_IOTKIT_I2C_SCL );
static HTS221Sensor hum_temp(&devI2c);

int main()
{
    uint8_t id;
    float value1, value2;
    
    oled.clear();
    oled.printf( "Temp | Hum Sensor\n" );
    
    /* Init all sensors with default params */
    hum_temp.init(NULL);
    hum_temp.enable();
    
    hum_temp.read_id(&id);
    printf("HTS221  humidity & temperature    = 0x%X\r\n", id);

    while (true) 
    {
        hum_temp.get_temperature(&value1);
        hum_temp.get_humidity(&value2);
        printf("HTS221:  [temp] %.2f C°, [hum]   %.2f%%\r\n", value1, value2);
        oled.cursor( 1, 0 );
        oled.printf( "temp: %3.2f\nhum : %3.2f", value1, value2 );
        wait( 1.0f );
    }
}
```

## Eigener Service (K3)

Anstatt ein Programm zu erweitern haben wir uns dazu entschieden einen eigenen Service zu entwicklen. Hierfür müssen folgende Funktionalitäten im Programm für den Mikroprozessor implementiert werden:

* Aktuelle Zeit holen
* Wiederholtes senden der aktuellen Daten
* HTTPS POST Request
  * Auth-Token Header
  * Temperatur
  * Gerätename
  * Aktuelle Temperatur
* Response Handling
  * Zusätzlicher Text bei Fehlern
  * LED an- und ausschalten je nach Fehler

Hier haben wir als erstes damit angefangen einen HTTPS Request auszuführen.

Hierfür haben wir die [mbed-http](https://os.mbed.com/teams/sandbox/code/mbed-http/) library verwendet.

Um dies zu testen haben wir folgenden Code verwendet:

```cpp
#include "mbed.h"
#include "https_request.h"
#include "network-helper.h"
#include "TCPSocket.h"
#include "HTS221Sensor.h"
#include "MbedJSONValue.h"

// Name of the Device shown on Website
char* DeviceName = "IoTKitV3-david";

// API Token
char* token = "Bearer 6bdbV6PMqCr1yEcPluNCR41ETakDpIgP8zShDlET";

// Route to submit Temp
char* ApiRoute = "https://alarm.davidlab.ch/api/temperature";

// Trusted Root Certs
const char* DST_ROOT_CA_X3 = "-----BEGIN CERTIFICATE-----\n"
    "MIIDSjCCAjKgAwIBAgIQRK+wgNajJ7qJMDmGLvhAazANBgkqhkiG9w0BAQUFADA/\n"
    "MSQwIgYDVQQKExtEaWdpdGFsIFNpZ25hdHVyZSBUcnVzdCBDby4xFzAVBgNVBAMT\n"
    "DkRTVCBSb290IENBIFgzMB4XDTAwMDkzMDIxMTIxOVoXDTIxMDkzMDE0MDExNVow\n"
    "PzEkMCIGA1UEChMbRGlnaXRhbCBTaWduYXR1cmUgVHJ1c3QgQ28uMRcwFQYDVQQD\n"
    "Ew5EU1QgUm9vdCBDQSBYMzCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEB\n"
    "AN+v6ZdQCINXtMxiZfaQguzH0yxrMMpb7NnDfcdAwRgUi+DoM3ZJKuM/IUmTrE4O\n"
    "rz5Iy2Xu/NMhD2XSKtkyj4zl93ewEnu1lcCJo6m67XMuegwGMoOifooUMM0RoOEq\n"
    "OLl5CjH9UL2AZd+3UWODyOKIYepLYYHsUmu5ouJLGiifSKOeDNoJjj4XLh7dIN9b\n"
    "xiqKqy69cK3FCxolkHRyxXtqqzTWMIn/5WgTe1QLyNau7Fqckh49ZLOMxt+/yUFw\n"
    "7BZy1SbsOFU5Q9D8/RhcQPGX69Wam40dutolucbY38EVAjqr2m7xPi71XAicPNaD\n"
    "aeQQmxkqtilX4+U9m5/wAl0CAwEAAaNCMEAwDwYDVR0TAQH/BAUwAwEB/zAOBgNV\n"
    "HQ8BAf8EBAMCAQYwHQYDVR0OBBYEFMSnsaR7LHH62+FLkHX/xBVghYkQMA0GCSqG\n"
    "SIb3DQEBBQUAA4IBAQCjGiybFwBcqR7uKGY3Or+Dxz9LwwmglSBd49lZRNI+DT69\n"
    "ikugdB/OEIKcdBodfpga3csTS7MgROSR6cz8faXbauX+5v3gTt23ADq1cEmv8uXr\n"
    "AvHRAosZy5Q6XkjEGB5YGV8eAlrwDPGxrancWYaLbumR9YbK+rlmM6pZW87ipxZz\n"
    "R8srzJmwN0jP41ZL9c8PDHIyh8bwRLtTcm1D9SZImlJnt1ir/md2cXjbDaJWFBM5\n"
    "JDGFoqgCWjBH4d1QB7wCCZAA62RjYJsWvIjJEubSfZGL+T0yjWW06XyxV3bqxbYo\n"
    "Ob8VZRzI9neWagqNdwvYkQsEjgfbKbYK7p2CNTUQ\n"
    "-----END CERTIFICATE-----\n";

HttpsRequest* post_req = new HttpsRequest(network, DST_ROOT_CA_X3, HTTP_POST, ApiRoute);
post_req->set_header("Content-Type", "application/json");
post_req->set_header("Authorization", token);

char* body = "{\"test\": true}";

HttpResponse* post_res = post_req->send(body, strlen(body));
if (!post_res) {
    printf("HttpRequest failed (error code %d)\n", post_req->get_error());
}    

char* resp = (char*)post_res->get_body_as_string().c_str();

delete post_req;
printf("%s\n", resp);
```

Als nächstes haben wir die anderen Komponenten hinzugefügt und Schlussendlich hatten wir folgendes Programm:

```cpp
#include "mbed.h"
#include "https_request.h"
#include "network-helper.h"
#include "TCPSocket.h"
#include "NTPClient.h"
#include "HTS221Sensor.h"
#include "MbedJSONValue.h"
#include "OLEDDisplay.h"

// Name of the Device shown on Website
char* DeviceName = "[DEVICE_NAME]";

// API Token
char* token = "Bearer [AUTH_TOKEN]";

// Route to submit Temp
char* ApiRoute = "https://alarm.davidlab.ch/api/temperature";

// Trusted Root Certs
const char* DST_ROOT_CA_X3 = "-----BEGIN CERTIFICATE-----\n"
    "MIIDSjCCAjKgAwIBAgIQRK+wgNajJ7qJMDmGLvhAazANBgkqhkiG9w0BAQUFADA/\n"
    "MSQwIgYDVQQKExtEaWdpdGFsIFNpZ25hdHVyZSBUcnVzdCBDby4xFzAVBgNVBAMT\n"
    "DkRTVCBSb290IENBIFgzMB4XDTAwMDkzMDIxMTIxOVoXDTIxMDkzMDE0MDExNVow\n"
    "PzEkMCIGA1UEChMbRGlnaXRhbCBTaWduYXR1cmUgVHJ1c3QgQ28uMRcwFQYDVQQD\n"
    "Ew5EU1QgUm9vdCBDQSBYMzCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEB\n"
    "AN+v6ZdQCINXtMxiZfaQguzH0yxrMMpb7NnDfcdAwRgUi+DoM3ZJKuM/IUmTrE4O\n"
    "rz5Iy2Xu/NMhD2XSKtkyj4zl93ewEnu1lcCJo6m67XMuegwGMoOifooUMM0RoOEq\n"
    "OLl5CjH9UL2AZd+3UWODyOKIYepLYYHsUmu5ouJLGiifSKOeDNoJjj4XLh7dIN9b\n"
    "xiqKqy69cK3FCxolkHRyxXtqqzTWMIn/5WgTe1QLyNau7Fqckh49ZLOMxt+/yUFw\n"
    "7BZy1SbsOFU5Q9D8/RhcQPGX69Wam40dutolucbY38EVAjqr2m7xPi71XAicPNaD\n"
    "aeQQmxkqtilX4+U9m5/wAl0CAwEAAaNCMEAwDwYDVR0TAQH/BAUwAwEB/zAOBgNV\n"
    "HQ8BAf8EBAMCAQYwHQYDVR0OBBYEFMSnsaR7LHH62+FLkHX/xBVghYkQMA0GCSqG\n"
    "SIb3DQEBBQUAA4IBAQCjGiybFwBcqR7uKGY3Or+Dxz9LwwmglSBd49lZRNI+DT69\n"
    "ikugdB/OEIKcdBodfpga3csTS7MgROSR6cz8faXbauX+5v3gTt23ADq1cEmv8uXr\n"
    "AvHRAosZy5Q6XkjEGB5YGV8eAlrwDPGxrancWYaLbumR9YbK+rlmM6pZW87ipxZz\n"
    "R8srzJmwN0jP41ZL9c8PDHIyh8bwRLtTcm1D9SZImlJnt1ir/md2cXjbDaJWFBM5\n"
    "JDGFoqgCWjBH4d1QB7wCCZAA62RjYJsWvIjJEubSfZGL+T0yjWW06XyxV3bqxbYo\n"
    "Ob8VZRzI9neWagqNdwvYkQsEjgfbKbYK7p2CNTUQ\n"
    "-----END CERTIFICATE-----\n";

// Declare LEDs
DigitalOut Led1(MBED_CONF_IOTKIT_LED1);
DigitalOut Led2(MBED_CONF_IOTKIT_LED2);
DigitalOut Led3(MBED_CONF_IOTKIT_LED3);
DigitalOut Led4(MBED_CONF_IOTKIT_LED4);

// OLED Init
OLEDDisplay oled( MBED_CONF_IOTKIT_OLED_RST, MBED_CONF_IOTKIT_OLED_SDA, MBED_CONF_IOTKIT_OLED_SCL );

// Temp Sensor Init
static DevI2C devI2c( MBED_CONF_IOTKIT_I2C_SDA, MBED_CONF_IOTKIT_I2C_SCL );
static HTS221Sensor hum_temp(&devI2c);

// Network
NetworkInterface* network;

//Global temp var
float temp;

bool InitTime() {
    oled.printf("Getting Time...");
    NTPClient ntp(network);
    time_t timestamp = ntp.get_timestamp();
    
    if (timestamp < 0) {
        printf("An error occurred when getting the time. Code: %ld\r\n", timestamp);
        return false;
    }
    else {
        set_time(timestamp);
        return true;
    }
}

bool InitializeNetwork() {
    
    oled.clear();
    oled.printf("Trying to Connect to Network...\n");
    printf("Trying to Connect to Network...\n");
    
    // Network Init
    network = connect_to_default_network_interface();
    oled.clear();

    if (!network) {
        oled.printf("Cannot connect to the network\n");
        printf("Cannot connect to the network\n");
        return false;
    }
    else {
        oled.printf("Connected to Network\n");
        printf("Connected to Network\n");
        return true;
    }
}

void PrintTemp() {
    oled.clear();
    printf("Temp: %3.2f\n", temp);
    oled.printf("Temp: %3.2f\n", temp);
}

char* SendRequest(float temperature) {

    char stringTemp[20];
    sprintf(stringTemp, "%f", temperature);
    char timeBuffer[32];
    
    time_t seconds = time(NULL);
    
    strftime(timeBuffer, 32, "%F %T", localtime(&seconds));

    HttpsRequest* post_req = new HttpsRequest(network, DST_ROOT_CA_X3, HTTP_POST, ApiRoute);
    post_req->set_header("Content-Type", "application/json");
    post_req->set_header("Authorization", token);

    char body[255] = "{\"recorded\":\"";
    strcat(body, stringTemp);
    strcat(body, "\",\"recorded_at\":\"");
    strcat(body, timeBuffer);
    strcat(body, "\",\"device\":\"");
    strcat(body, DeviceName);
    strcat(body, "\"}");
    
    printf("%s\n", body);
    
    HttpResponse* post_res = post_req->send(body, strlen(body));
    if (!post_res) {
        printf("HttpRequest failed (error code %d)\n", post_req->get_error());
    }    
    
    char* resp = (char*)post_res->get_body_as_string().c_str();
    
    delete post_req;
    printf("%s\n", resp);
    return resp;
}

void SetLEDStatusCode(char* response) {
    MbedJSONValue parser;

    parse(parser, response);
    
    int statusCode;
    
    statusCode = parser["code"].get<int>();
    
    if (statusCode == 0) {
        PrintTemp();
        Led1.write(0);
        Led2.write(0);
        Led3.write(0);
        Led4.write(0);
    }
    else if(statusCode == 1) {
        PrintTemp();
        printf("Temperature is above the limits!\n");
        oled.printf("Temperature is above the limits!\n");
        Led1.write(1);
        Led2.write(0);
        Led3.write(0);
        Led4.write(0);
    }
    else if(statusCode == 2) {
        PrintTemp();
        printf("Temperature is below the limits!\n");
        oled.printf("Temperature is below the limits!\n");
        Led1.write(0);
        Led2.write(1);
        Led3.write(0);
        Led4.write(0);
    }
    else if(statusCode == 3) {
        PrintTemp();
        printf("Undefined Error!\n");
        oled.printf("Undefined Error!\n");
        Led1.write(0);
        Led2.write(0);
        Led3.write(1);
        Led4.write(0);
    }
    else if(statusCode == 4) {
        PrintTemp();
        printf("Undefined Error!\n");
        Led1.write(0);
        Led2.write(0);
        Led3.write(0);
        Led4.write(1);
    }
}

void InitializeTempSensor() {
    
    hum_temp.init(NULL);
    hum_temp.enable();
}

int main() {
    
    if (!InitializeNetwork()) {
        return 1;
    }
    
    if (!InitTime()) {
        return 1;
    }
    
    time_t seconds = time(NULL);
    printf("\rDate & Time: \r%s", ctime(&seconds));
    
    InitializeTempSensor();
    
    wait(2);
    
    while (true) {
        oled.clear();
        hum_temp.get_temperature(&temp);
        SetLEDStatusCode(SendRequest(temp));
    }
}
```

Dieses Programm ist nun auch in einer nicht gelisteten repository vorhanden, welche man [hier](https://os.mbed.com/users/sayhey/code/TemperatureAlarm/) finden kann.

### Error-States

Unten sind die möglichen Error-States gelistet.

**Zur erklärung:**
* Die Nummern der LEDs sind von Links nach rechts durchnummeriert.
* Die Spalte Nachricht zeigt, welche Nachricht auf dem OLED display ausgegeben wird.

### No Error (Normal Operation)

| Errorcode | LED | Nachricht |
| --------- | --- | --------- |
| 0         | -   | -         |

Die Observierte Temperatur liegt im festgelegten Normalbereich.

### Temperature too high

| Errorcode | LED | Nachricht                        |
| --------- | --- | -------------------------------- |
| 1         | 1   | Temperature is above the limits! |

Die Observierte Temperatur liegt oberhalb des festgelegten Normalbereichs.

### Temperature too low

| Errorcode | LED | Nachricht                        |
| --------- | --- | -------------------------------- |
| 2         | 2   | Temperature is below the limits! |

Die Observierte Temperatur liegt unterhalb des festgelegten Normalbereichs.