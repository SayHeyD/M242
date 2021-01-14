# M242

Hier ist die Dokumentation zum Modul 242 der Gruppe 3 (Zellweger, Docampo, Lippuner)

# Inhalt

* [Lernumgebung](https://github.com/SayHeyD/M242/tree/main/Lernumgebung)
* [Cloud](https://github.com/SayHeyD/M242/tree/main/Cloud)
* [IoTKitV3](https://github.com/SayHeyD/M242/tree/main/IoTKitv3)
* [Load Balancer](https://github.com/SayHeyD/M242/tree/main/Load%20Balancer)
* [Selbst aufsetzen]()

# Unser Wissenstand (K2)

## Moritz

## David

Wir hatten in der Schule bereits ein Modul in welchen wir einen Raspberry Pi verwendet haben. Dort haben wir bereits ein kleines Projekt mit einem RFID-Scanner umgesetzt, jedoch war dies im ersten Lehrjahr und ich habe nicht mehr sehr viel davon in Erinnerung behalten. Ansosntne habe ich was IoT, Sensoren udn Aktoren angeht wenig erfahrung. Allerdings bin ich wenn es um Services geht besser aufgestellt, da ich in der Freizeit selber gerne Web-Services entwickle und mich mit dem Thema beschäftige. Ebenfalls habe ich auch schon öfters in Modulen in denen es Möglichg war Web-services entwickelt und, oder konzipiert da mich diese Thema sehr stark interessiert.

## Andi

# Wichtige Lernschritte (K2)

## Moritz

## David

Ich habe am meisten beim Programmieren der Applikation für den Mikroprozessor gelernt. Da ich nur Erfahrung in C# hatte und mir C++ erst einaml für ein par Stunden angeschaut hatte, konnte ich in diesem Modul mein Wissen was C++ betrifft vertiefen. Das was mich am meisten überrascht hat, aber im Nachinein natürlich völlig logisch ist, ist, dass ich das Zertifikat für einen HTTPS Request dem Mikroprozessor übergeben muss, da dieser wie die meisten Betriebssysteme keinen Vorinstallierten Zertifikatsspeicher hat. Ebenfalls konnte ich noch nie einen Load-Balancer konfigurieren und habe somit auch damit in diesem Modul meine ersten Erfahrungen damit gemacht.

## Andi

# Persönliche Lernentwicklung (K6)

## Moritz

## David

Ich habe in diesem Modul vieles über Mikroprozessoren gelernt, dazu gehören:

- Mikroprozessor vs CPU
- Programmieren eines Mikroprozessors
- Anwendungsgebiete von Mikroprozessoren
- Man kann selbst Mikroprozessoren designen und drucken lassen
- Mikroprozessoren haben keinen Certificate-Store 🙄

Auch habe ich etwas über nginx gelernt, obwohl ich diesen Dienst schon lange benutze:

- Nginx als Load balancer

Auch konnte ich noch einiges über das Prinzip Laod Balancer lernen:

- Prinzipien wie Round-Robin, least connected
- IP Hash bei Laod Balancing
- Sessionhandling einer Applikation muss beim load-balancing angepasst werden

## Andi

# Reflexion (K6)

## Moritz

## David

Ich denke, ich bin allgemein gut mit meinen Arbeiten für das Modul vorangekommen, hätte aber wahrscheinlich etwas früher die Bewertungskriterien anschauen sollen, da mir aufgefallen ist, dass ich vieles nicht dokumentiert hatte was wir dokumentiert hätten haben sollen. Somit mussten wir vieles 2-Mal machen, da wir es nicht dokumentiert gehabt haben. Ansonsten haben wir gut zusammengearbeitet und sind gut vorangekommen.

## Andi

# Unser Service (K5)

Wir haben uns dafür entschieden einen Temperatur-Alarm mit dem IoT-Board umzusetzen. Das bedeutet, dass wir die Temperatur mittels des Sensors auf dem Board lesen und dann per REST API an eine Web-App übermitteln wollen.

Die Web-App ist für das logging der Einträge verantwortlich. Ausserdem kann über die Web-App ein Temperaturberich festgelegt werden. Bei allen Temperaturen, die ausserhalb dieses Bereiches liegen, wird "Alarm" geschalgen.

Der Alarm, zeigt sich ersten indem die Temperaturanzeige in der Web-App Rot angezegit wird und zweitens auf dem Board ein entsprechendes LED eingeschaltet wird. Ebenfalls wird auch auf dem OLED-Display eine NAchricht ausgegeben die zeigt, was der Feherl nun genau bedeutet.

Weitere Informationen zu den Error-States finden Sie [hier](https://github.com/SayHeyD/M242/tree/main/IoTKitv3#Error-States).

