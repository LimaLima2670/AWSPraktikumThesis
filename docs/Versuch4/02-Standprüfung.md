# Versuch 4 – Überprüfung der Ausgangsbasis

## Ordnerstrukturüberprüfung

- Die HLS-Streaming-Dateien befinden sich im Ordner

```bash
export/<clipname>/
```
- Der Fastly Service wurde bereits in einem vorherigen Versuch angelegt.
- Die grundlegende Funktionsweise des CDNs sowie die Wiedergabe im HLS-Player sind bekannt.

**In diesem Versuch werden keine neuen CDN-Konfigurationen erstellt, sondern ausschließlich der Origin-Pfad angepasst und getestet.**





## 4.2 Ausgangsbasis

- Die HLS-Streaming-Dateien befinden sich im Ordner

```bash
export/<clipname>/
```
- Der Fastly Service wurde bereits in einem vorherigen Versuch angelegt.
- Die grundlegende Funktionsweise des CDNs sowie die Wiedergabe im HLS-Player sind bekannt.

**In diesem Versuch werden keine neuen CDN-Konfigurationen erstellt, sondern ausschließlich der Origin-Pfad angepasst und getestet.**



## Kontrolle der Transcoding-Ergebnisse im S3-Bucket

**Öffnen Sie in der AWS Management Console den eigenen S3-Bucket und navigieren Sie in den Ordner export/.**

**Dort sollte für jede transcodierte Videodatei ein eigener Unterordner existieren, der unter anderem folgende Dateien enthält:**

- eine Master-Playlist (*.m3u8)
- mehrere Variant-Playlists für unterschiedliche Qualitätsstufen
- zugehörige Segment-Dateien (*.ts)


![exportfolder](../../assets/Versuch4/dolbycount.jpg)

