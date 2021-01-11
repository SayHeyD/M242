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

## Eigener Service

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