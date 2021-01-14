# Cloud (K5)

Der Cloud-Service läuft auf 2 Applikationsinstanzen, einem Load-Balancer und 2 Applikationsinstanzen. Der [Source Code](https://github.com/SayHeyD/alarm-setter) ist in einer eigenen Repository aug GitHub.

## Dienst aus Beispielen (K5)

## Eigener Dienst (K5)

### Netzwerk

<img src="https://github.com/SayHeyD/M242/raw/main/Cloud/Network.png" alt="Netzwerkdiagramm">

Alle Server sind öffentlich über die gezeigten Domains erreichbar. Dies ist hier für die Bewertung so eingerichtet. In einem Production envoirenment, würden die Apllikationsinstanzen nicht öffentliche erreichbar sein.

### Load-Balancer

Die Dokumentation der [Load Balancer](https://github.com/SayHeyD/M242/tree/main/Load%20Balancer) ist in ihrem eigenen Teil.

### Applikationsinstanzen

Die Appliaktionsisntanzen sind CX11 Cloud-Server, die bei Hetzner gehostet werden. Somit mussten wir das Betriebssystem nicht aufsetzen sondern konntes es aus einer Liste auswählen, hier haben wir uns für Ubuntu 20.04 entschieden.

Nun mussten wir nur noch die benötiget Software Installieren:

#### Web-Server (Nginx)

#### Certbot (SSL Zertifikat)

#### PHP7.4

#### Composer

#### Nodejs

### Datenbank-Server
