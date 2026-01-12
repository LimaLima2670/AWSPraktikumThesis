# Versuch 4 – Auslieferung der automatisch erzeugten HLS-Streams über ein CDN

## 4.1 Einführung

In Versuch 3 wurde ein vollständig automatisierter Transcoding-Workflow in AWS aufgebaut.
Neu hochgeladene Videodateien wurden dabei automatisch in das HLS-Streamingformat transcodiert und im Ordner export/ des eigenen AWS-S3-Buckets abgelegt.

In diesem Versuch wird geprüft, ob diese automatisch erzeugten HLS-Streaming-Dateien korrekt über ein Content Delivery Network (CDN) ausgeliefert werden können.
Dazu wird das bereits zuvor eingerichtete CDN auf Basis von Fastly verwendet und gezielt auf den export/-Ordner als Origin angewendet.

Ziel dieses Versuchs ist es, zu überprüfen, ob die Ergebnisse des automatisierten Transcodings unmittelbar für die weltweite Auslieferung geeignet sind und ohne weitere Verarbeitungsschritte über das CDN abgespielt werden können.

## 4.2 Ausgangsbasis

- Die HLS-Streaming-Dateien befinden sich im Ordner

```bash
export/<clipname>/
```
- Der Fastly Service wurde bereits in einem vorherigen Versuch angelegt.
- Die grundlegende Funktionsweise des CDNs sowie die Wiedergabe im HLS-Player sind bekannt.

**In diesem Versuch werden keine neuen CDN-Konfigurationen erstellt, sondern ausschließlich der Origin-Pfad angepasst und getestet.**