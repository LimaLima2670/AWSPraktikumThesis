## Grundbegriffe

Als CDN-Anbieter wird **Fastly** genutzt. Fastly ist ein international eingesetzter
Content-Delivery-Network-Anbieter, der insbesondere für geringe Latenzen und eine
hohe Flexibilität in der Konfiguration bekannt ist. Fastly wird von zahlreichen
Medien-, Streaming- und Webplattformen eingesetzt.

Ebenso wie AWS lässt sich Fastly sowohl über eine **Weboberfläche** als auch über
eine API bedienen. In diesem Versuch wird ausschließlich die Weboberfläche genutzt.


### Fastly CDN Service

Zur Auslieferung von statischen sowie streambaren Mediendateien nutzt Fastly
sogenannte **Services**.  
Ein Service stellt die zentrale Konfigurationseinheit des CDNs dar und definiert,
wie Fastly auf eingehende Anfragen reagieren soll.

Innerhalb eines Services werden unter anderem folgende Aspekte konfiguriert:

- der Origin-Server
- das Caching-Verhalten
- die Hostnamen, unter denen Inhalte erreichbar sind

### Origin

Die im CDN zu verteilenden Mediendateien werden von einem sogenannten
**Origin-Server** bezogen.  
Als Origin kann sowohl ein externer Speicheranbieter wie **AWS S3** als auch ein
eigener Webserver dienen.

In diesem Versuch wird ein AWS-S3-Bucket als Origin verwendet.  
Fastly greift bei der ersten Anfrage auf den Origin zu und speichert die
angeforderten Inhalte anschließend im Cache, sodass sie bei weiteren Zugriffen
direkt über das CDN ausgeliefert werden können.

### Service Domain

Damit ein Mediaplayer auf die im CDN gespeicherten Medien zugreifen kann, stellt
Fastly für jeden Service eine sogenannte **Service Domain** zur Verfügung.  

![Fastly CDN Prinzip](../../assets/Versuch2/fastly.jpg)

Im oberen Teil der Abbildung ist ein Legacy-CDN dargestellt. Änderungen an den Inhalten erfolgen am Origin-Server, werden jedoch nur zeitbasiert (z. B. über feste Cache-Invalidierungsintervalle) an die CDN-Knoten weitergegeben. Dadurch kann es zu Verzögerungen kommen, bis aktualisierte Inhalte bei den Endnutzern verfügbar sind.

Der untere Teil der Abbildung zeigt den Fastly-CDN-Ansatz. Hier werden Änderungen am Origin unmittelbar erkannt und ereignisbasiert („update on change“) an das CDN übertragen. Aktualisierte Inhalte stehen dadurch nahezu in Echtzeit an den Edge-Servern zur Verfügung und können ohne lange Wartezeiten an die Endnutzer ausgeliefert werden
