---
layout: default
title: Fazit
---




# Fazit

In Versuch 3 wurden die Grundprinzipien eines automatisieren Transcoding-Workflows erläutert und ein solcher in AWS erstellt. Sobald Rohmaterial ingestiert wurde, wurde dieses in HLS transcodiert und zum Origin-Server des CDNs hochgeladen.

## Abgabe

In der Abgabe sollen mindestens zwei Transcodierungsergebnisse enthalten sein:

- ein Beispiel, das mithilfe der vorgegebenen Transcodiervorlage erzeugt wurde
- ein Beispiel, das mithilfe einer selbst erstellten Transcodiervorlage erzeugt wurde

Dabei ist es nicht erforderlich, alle generierten Dateien einzureichen.
Ausreichend sind jeweils:

- die HLS-Indexdateien (.m3u8)
- ein Segment pro Qualitätsstufe (.ts)

**Zusätzlich können die verwendeten Lambda-Funktionen (CreateJob, UploadFile) als .py-Dateien abgegeben werden, sofern sie nicht bereits vollständig als Screenshot oder Quelltext im Bericht dokumentiert sind.**

Eine mögliche Ordnerstruktur der Abgabe ist nachfolgend beispielhaft dargestellt:

```
📁 Versuch 3
    📁 Standard Vorlage
        📄 Clip1.m3u8
        📄 Clip1_480p.m3u8
        📄 Clip1_720p.m3u8
        📄 Clip1_1080p.m3u8
        📄 Clip1_480p_00001.ts
        📄 Clip1_720p_00001.ts
        📄 Clip1_1080p_00001.ts
    📁 Eigene Vorlage
        📄 Clip2.m3u8
        📄 Clip2_low.m3u8
        📄 Clip2_high.m3u8
        📄 Clip2_low_00001.ts
        📄 Clip2_high_00001.ts
    📁 Templates
        📄 musterstudent_template2.json


        ---

⬅️ **Vorheriges Kapitel:**  
[Testen](05-testen.md)

➡️ **Nächstes Kapitel:**  
[Troubleshooting](07-troubleshooting.md)
    ...

```
