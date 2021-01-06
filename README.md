# M242

Hier ist die Dokumentation zum Modul 242 der Gruppe 3 (Zellweger, Docampo, Lippuner)

# Inhalt

* [IoTKitV3](https://github.com/SayHeyD/M242/tree/main/IoTKitv3)
* [Load Balancer](https://github.com/SayHeyD/M242/tree/main/Load%20Balancer)
* [Cloud](https://github.com/SayHeyD/M242/tree/main/Cloud)

# Unser Service

Wir haben uns dafür entschieden einen Temperatur-Alarm mit dem IoT-Board umzusetzen. Das bedeutet, dass wir die Temperatur mittels des Sensors auf dem Board lesen und dann per REST API an eine Web-App übermitteln wollen.

Die Web-App ist für das logging der Einträge verantwortlich. Ausserdem kann über die Web-App ein Temperaturberich festgelegt werden. Bei allen Temperaturen, die ausserhalb dieses Bereiches liegen, wird "Alarm" geschalgen.

Der Alarm, zeigt sich ersten indem die Temperaturanzeige in der Web-App Rot angezegit wird und zweitens auf dem Board ein entsprechendes LED eingeschaltet wird. Ebenfalls wird auch auf dem OLED-Display eine NAchricht ausgegeben die zeigt, was der Feherl nun genau bedeutet.

Weitere Informationen zu den Error-States finden Sie [hier](https://github.com/SayHeyD/M242/tree/main/IoTKitv3#Error-States).