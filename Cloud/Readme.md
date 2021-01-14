# Cloud (K5)

Der Cloud-Service läuft auf 2 Applikationsinstanzen, einem Load-Balancer und 2 Applikationsinstanzen. Der [Source Code](https://github.com/SayHeyD/alarm-setter) ist in einer eigenen Repository aug GitHub.

## Dienst aus Beispielen (K5)

## Eigener Dienst (K5)

Der eigene Dienst läuft auf CLoud-Sevrer, das deployment ist jedoch nicht komplett automatisiert, da uns im Rahmen des Moduls die Zeit dafür fehlte. In einer Produktionsumgebung würden wir hier eine automatisierung erstellen um den Ausbau des Dienstes zu erelichtern.

### Netzwerk

<img src="https://github.com/SayHeyD/M242/raw/main/Cloud/Network.png" alt="Netzwerkdiagramm">

Alle Server sind öffentlich über die gezeigten Domains erreichbar. Dies ist hier für die Bewertung so eingerichtet. In einem Production envoirenment, würden die Apllikationsinstanzen nicht öffentliche erreichbar sein.

### Load-Balancer

Die Dokumentation der [Load Balancer](https://github.com/SayHeyD/M242/tree/main/Load%20Balancer) ist in ihrem eigenen Teil.

### Applikationsinstanzen

Die Appliaktionsisntanzen sind CX11 Cloud-Server, die bei Hetzner gehostet werden. Somit mussten wir das Betriebssystem nicht aufsetzen sondern konntes es aus einer Liste auswählen, hier haben wir uns für Ubuntu 20.04 entschieden.

Nun mussten wir nur noch die benötiget Software Installieren:

#### Web-Server (Nginx)

Als Web-Server haben wir [nginx](https://www.nginx.com/) verwendet, diesen installiert man auf Ubuntu folgendermassen:

```apt install nginx -y```

Nun mussten wir die Instanzen noch konfigurieren. Hierfür konnten wir das [Nginx Template](https://laravel.com/docs/8.x/deployment#nginx) welches von Laravel bereitgestellt wird verwenden.

Die Konfiguration ist im File ```/etc/nginx/sites-available/default``` zu finden.

Hier mussten wir nur noch die Angabe des ```server_name``` den jeweiligen Instanzen anpassen. Also ```alarm1.davidlab.ch``` und ```alarm2.davidlab.ch```.

Auch hier überprüfen wir wieder die konfiguration mit ```nginx -t``` und starten den Service neu:

```service nginx restart```

#### Certbot (SSL Zertifikat)

Da die ganze Verbindung verschlüsselt ablaufen soll, mussten wir uns hier SSL Zertifikate beschaffen. Da wir mit Linux und nginx arbeiten, können wir dies mit dem Tool [Certbot](https://certbot.eff.org/) machen, das uns eigentlich die ganze Arbeit abnimmt.

Certbot können wir folgendermassen installeiren:

```apt install certbot-python3-nginx -y```

Nach der Installation und der konfiguration des Web-Servers müssen wir nur noch certbot ausführen und den Anweisungen folgen:

```certbot```

Hier haben wir den automatischen redirect von HTTP auf HTTPS aktiviert um eine unverschlüsselte Verbindung zu vermeiden, auch wenn auf diese Applikationsinstanzen im normalfall nur durch interne Systeme ei9n zugriff vorgenommen wird. Dies ist also ein Punkt der Fehlkonfigurationen vermeidet.

#### PHP 7.4

Um PHP 7.4 auf Ubuntu 20.04 zu installieren haben wir apt verwendet:

```apt install php7.4 php7.4-{fpm,mbstring,mysql,zip,mysql}```

Der Bereich ```php7.4-{fpm,mbstring,mysql,zip,mysql}``` installiert alle benötigten Module für Composer und Laravel. Ohne diese Module würde unser Dienst nicht funktionieren.

Mit dem aufrufen der PHP Version können wir prüfen ob die installation erfolgreich war:

```php -v```

Nun müssen wir noch den das "fpm" Modul starten:

```service php7.4-fpm start```

PHP ist jetzt ready-to-use und wir können zum nächsten Schritt weiter gehen.

#### Composer

[Composer](https://getcomposer.org/) ist eine Paketverwaltungs-Software für PHP. Um die ganzen Abhängigkeiten der Laravel-Applikation zu isntallieren benötigen wir dieses Tool.

Um Composer isntallieren zu könenn muss PHP installiert sein.

Als erstes laden wir das setup script herunter und überprüfen den hash. Der hash dient zur validierung und generell würden wir empfehlen immer der Installationsanleitung auf der [Website von Composer](https://getcomposer.org/download/) zu folgen, da dort auch immer der aktuellste Hash ist. Hier wird das Hash-Checking jedoch mit dokumentiert.

Herunterladen des setup scripts:

```php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"```

Überprüfen des Hash:

```php -r "if (hash_file('sha384', 'composer-setup.php') === '756890a4488ce9024fc62c56153228907f1545c228516cbf63f885e036d37e9a59d27d63f46af1d4d07ee0f76181c7d3') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"```

Nun führen wir das Setup aus. Normalerweise würde composer nun die datei "composer.phar" direkt in dieses Verzeichnis ablegen, jedoch möchten wir das wir composer global über dem Befehl ````composer``` ausführen können, deshalb geben wir dem Installationscript noch 2 Parameter mit:

```php composer-setup.php --install-dir=/usr/local/bin --filname=/composer```

Nun ist composer unter folgendem Pfad installiert: ```/usr/local/bin/composer```

Da der Pfad ```/usr/local/bin``` Standardmässig in der ```$PATH``` variable steht, sollten wir composer nun über folgenden Befehl ausführen können. Um die Installation zu testen führen wir also folgendes aus:

```composer```

Nun können wir die setup datei löschen:

```rm composer-setup.php```

#### Nodejs

Node.js an sich wird für die Laravel-Appliaktion nicht benötigt, jedoch brauchen wir einen Package-Manger für javascript Pakete, um das Front-End builden zu können. Hier verwenden wir npm welcher nur mit Node.js zusammen installiert wird. In unserem Fall haben wir Node 14.x LTS verwendet.

Um die Installationsdatei für Node.js herunterladen zu können, führen wir folgenden Befehl aus:

```curl -sL https://deb.nodesource.com/setup_14.x -o nodesource_setup.sh```

Nun führen wir das script aus um die nötigen PPAs zu installieren:

```bash nodesource_setup.sh```

Danch updaten wir die repositories:

```apt update```

und dann installeiren wir nodejs:

```apt install nodejs -y```

Nun können wir mit ```npm -v``` überprüfen ob npm installiert wurde.

#### Laravel application deploy

### Datenbank-Server
