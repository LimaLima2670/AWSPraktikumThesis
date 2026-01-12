# Abruf der HLS-Streams über CDN 

Die Auslieferung erfolgt über den bereits vorhandenen Fastly Edge Hostnamen.

**URL-Struktur**

```bash
https://<FASTLY-EDGE-HOSTNAME>/export/<clipname>/<master-playlist>.m3u8
```

**Beispiel**

```bash
https://lelugoue.global.ssl.fastly.net/export/clip1/master.m3u8
```

![lelugoueservice](lelugoueservice.jpg)




## Ordnerstrukturüberprüfung

- Die HLS-Streaming-Dateien befinden sich im Ordner

```bash
export/<clipname>/
```
- Der Fastly Service wurde bereits in einem vorherigen Versuch angelegt.
- Die grundlegende Funktionsweise des CDNs sowie die Wiedergabe im HLS-Player sind bekannt.

**In diesem Versuch werden keine neuen CDN-Konfigurationen erstellt, sondern ausschließlich der Origin-Pfad angepasst und getestet.**


## Test der Wiedergabe im HLS-Player

 **Zum Testen der Wiedergabe wird erneut der hls.js-Demo-Player verwendet:**

**Hinweis zu Berechtigungen (zwingend erforderlich)**

Die von AWS MediaConvert erzeugten HLS-Dateien werden standardmäßig privat
im S3-Bucket abgelegt.
Da Fastly als externer CDN-Anbieter auf die Dateien zugreift, muss dem
Bucket bzw. den relevanten Ordnern Lesezugriff (GetObject) gewährt werden.

Ohne diese Berechtigung schlägt der Abruf der Master-Playlist mit folgendem
Fehler fehl: **403 Forbidden (manifestloaderror)**

Stellen Sie daher sicher, dass in der Bucket-Richtlinie mindestens die
folgenden Ordner für den öffentlichen Lesezugriff freigegeben sind:

- versuch2-hls-test/

- export/

**Navigieren SIe dafür zu folgendem Reiter in ihrem dafür vorgesehenen Bucket**

![berechtigung](berechtigung.jpg)

**Dort müssen noch weitere Bucketrichtlinien angepasst werden. Navigieren sie hierfür etwas weiter runter bis Sie folgendes Feld sehen**

![Bucket](Bucketrichtlinien.jpg)


**Dort muss folgende Anpassung noch entstehen:**


```bash
        {
            "Sid": "PublicReadExport",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::<mvs-IhrBucketname>/export/*"
        }
```

**Navigieren Sie nun wieder zum Webplayer:**

 https://hlsjs.video-dev.org/demo/

 **Vorgehen:**

 1. Öffnen die die Demo-Seite
 2. Ersetzen Sie die voreingestellte Beispiel-URL durch die Fastly-URL der Master-Playlist aus dem export/-Ordner
```bash
https://<FASTLY-EDGE-HOSTNAME>/export/<CLIPNAME>/<CLIPNAME>.m3u8
```
 3. Starten Sie die Wiedergabe

**Sie sollten folgendes nun auf ihrem Bildschirm sehen:**

![SuccesfulCountdown](countdownsuc.jpg)


**Hinweis zu CORS (zwingend erforderlich)**

Der hls.js-Demo-Player wird von einer anderen Domain geladen als der
HLS-Stream, der über das Fastly CDN ausgeliefert wird.
Dadurch greift die browserseitige Same-Origin-Policy.

Wie bereits in einem vorherigen Versuch ist daher für diesen Test
eine CORS-Browser-Extension erforderlich.


