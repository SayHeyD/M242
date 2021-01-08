# M242

Hier ist die Dokumentation zum Modul 242 der Gruppe 3 (Zellweger, Docampo, Lippuner)

# Inhalt

* [Lernumgebung](https://github.com/SayHeyD/M242/tree/main/Lernumgebung)
* [IoTKitV3](https://github.com/SayHeyD/M242/tree/main/IoTKitv3)
* [Load Balancer](https://github.com/SayHeyD/M242/tree/main/Load%20Balancer)
* [Cloud](https://github.com/SayHeyD/M242/tree/main/Cloud)

# Unser Wissenstand (K2)

## Moritz

## David

Wir hatten in der Schule bereits ein Modul in welchen wir einen Raspberry Pi verwendet haben. Dort haben wir bereits ein kleines Projekt mit einem RFID-Scanner umgesetzt, jedoch war dies im ersten Lehrjahr und ich habe nicht mehr sehr viel davon in Erinnerung behalten. Ansosntne habe ich was IoT, Sensoren udn Aktoren angeht wenig erfahrung. Allerdings bin ich wenn es um Services geht besser aufgestellt, da ich in der Freizeit selber gerne Web-Services entwickle und mich mit dem Thema beschäftige. Ebenfalls habe ich auch schon öfters in Modulen in denen es Möglichg war Web-services entwickelt und, oder konzipiert da mich diese Thema sehr stark interessiert.

## Andi

# Unser Service (K5)

Wir haben uns dafür entschieden einen Temperatur-Alarm mit dem IoT-Board umzusetzen. Das bedeutet, dass wir die Temperatur mittels des Sensors auf dem Board lesen und dann per REST API an eine Web-App übermitteln wollen.

Die Web-App ist für das logging der Einträge verantwortlich. Ausserdem kann über die Web-App ein Temperaturberich festgelegt werden. Bei allen Temperaturen, die ausserhalb dieses Bereiches liegen, wird "Alarm" geschalgen.

Der Alarm, zeigt sich ersten indem die Temperaturanzeige in der Web-App Rot angezegit wird und zweitens auf dem Board ein entsprechendes LED eingeschaltet wird. Ebenfalls wird auch auf dem OLED-Display eine NAchricht ausgegeben die zeigt, was der Feherl nun genau bedeutet.

Weitere Informationen zu den Error-States finden Sie [hier](https://github.com/SayHeyD/M242/tree/main/IoTKitv3#Error-States).