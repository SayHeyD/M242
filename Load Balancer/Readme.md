# Load Balancer

## Aufsetzen (K4)

Der Load-Balancer ist ein CX11 Cloud-Server, der bei Hetzner gehostet wird. Als Betriebssystem läuft Ubuntu 20.04. Da die CLoud-Server automatisch provisioniert werden mussten wir hier keine Schritte unternehmen um den Server selbst aufzusetzen.

## Dienst Installieren (K4)

Als software für den Load Balancer haben wir uns für [nginx](https://www.nginx.com/) entschieden. Nginx kann auf Ubuntu über apt Installiert werden:

```apt install nginx -y```
 
Nach der Installation können wir mit folgendem Befehl überprüfen, ob der Dienst korrekt isntalleirt ist:

```nginx -v```

Ausserdem sollte man nun auf der IP des Servers eine Seite sehen die einem Willkommen heisst.

## Konfiguration (K4)

Um den Load Balncer zu konfigurieren, bearbeiten wir das File ```/etc/nginx/sites-available/default```. In diesem File steht die KOnfiguration für die Seite "default". Da wir diese Installation jedoch nicht für eine Website sondern als Load Balancer benutzen möchten, werden wir nginx hier zu einen Reverse-Proxy umkonfigurieren, der Load Balancing nach dem Round-Robin prinzip betriebt. Dies beduetet, dass bei jeder neuen Verbindung der Request an den Server weitergeleitet wird der als nächstes drankommt. Heisst es wird reih-um gegangen und falls man bei dem letzten Server in der liste ankommt wird einfach wieder der erste genommen.

Nun aktivieren wir noch IP Hash. IP Hash ist dafür verantwortlich, dass wenn einmal ein Request von einer IP an den Server gesendet wird, dass der Load-Balancer alle Requests dieser IP an den gleichen Server weiterleitet. Dies ist, was das Load-Balancing an sich angeht nicht die optimalste Lösung, da man so in einer Produktionsumgebung einen Server überalsten könnte. Jedoch hatten wir hier nicht die Zeit die Applikation auf Load-Balancing ohne IP-Hash vorzuberieten, weshalb wir dies heir aktiviert haben.

Unser ```/etc/nginx/sites-available/default``` sieht nach der Umkonfiguration nun so aus:

```nginx

```

Danach kann man die Änderungen mit ```nginx -t``` überprüfen und falls der Test erfolgreich war den Service neu starten ```service nginx restart```.

Nun sollte die Applikation über die definierte Domain "alarm.davidlab.ch" erreichbar sein. Auf welchem Server man dann schlussendlich landet, sieht man standardmässig nicht. Bei dieser Appliaktion sieht man jedoch unten auf jeder Seite (ausser login) auf welchem Server man sich befindet. Dies wird gemacht um zu testen ob das Load Balancing korrekt funktioniert.

### Verbindungsinformationen

Die Verbindung zwischen Client udn Load-Balacner wird per HTTPS aufgebaut. Ebenfalls sind die Appliaktionsinstanzen über HTTPS abgesichert und der Traffic zwischen Load Balancer und Instanzen läuft auch über HTTPS.

Das "Netzwerk" kann man sich bei [Cloud](https://github.com/SayHeyD/M242/tree/main/Cloud) anschauen.
