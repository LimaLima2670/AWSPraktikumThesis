---
layout: default
title: S3-Vorbereitung
---




## S3-Vorbereitungen

Für diesen Versuch werden mehrere Unterordner im bereits vorhandenen
AWS-S3-Bucket benötigt. Diese dienen der strukturierten Abbildung des
automatisierten Transcoding-Workflows.

Es werden folgende Ordner angelegt:

- **ingest/**  
  Ablageort für neu hochgeladene Quelldateien.  
  Das Ablegen einer Datei in diesem Ordner löst die erste Lambda-Funktion aus.

- **export/**  
  Zielordner für die von AWS MediaConvert erzeugten HLS-Streaming-Dateien.  
  Dieser Ordner dient anschließend als **Origin für das Fastly CDN**.

- **templates/**  
  Enthält JSON-Vorlagen mit Transcodier-Parametern für MediaConvert.

## Unterordner erstellen

**Um den Unterordner zu erstellen, muss auf der AWS Weboberfläche der entsprechende Bucket geöffnet werden und über den Button "Ordner erstellen" erstellt werden.**

![S3 Bucket](../assets/unterordner.png)

**Außer dem Ordnername müssen keine weiteren Einstellungen bei der Erstellung festgelegt werden.**

![S3 Bucket](../assets/ordner.png)

**Wiederholen Sie den Vorgang, bis alle drei Ordner erstellt sind.**

![S3 Bucket](../assets/exportetc.png)

## Transcodiervorlage hinzufügen

Für die automatische Transcodierung muss MediaConvert mitgeteilt werden, welche Transcodierparameter gewählt werden sollen. Dies geschieht über eine Vorlage im JSON-Format. Ein Beispiel für diese Vorlage steht in `a--sourcefiles` zur Verfügung und soll in den Ordner `templates` kopiert werden.

<div style="border:2px solid #d1d5db; background:#ffffff; padding:18px; border-radius:8px; margin:25px 0;">

<div style="color:#06b6d4; font-weight:600; font-size:18px; margin-bottom:10px;">
Frage 5
</div>

Laden Sie die JSON-Vorlage herunter und öffnen Sie diese in einem Texteditor.  
Welche **Auflösungen** werden durch diese Vorlage erzeugt?  
Welche **durchschnittlichen und maximalen Video-Bitraten** besitzen die Auflösungen jeweils?

</div>

![S3 Quellpfad](../assets/vorlage.png)

![S3 Kopie Zielpfad](../assets/copy.png)

Kontrollieren Sie, dass die Datei in das richtige Verzeichnis kopiert wurde.

---

⬅️ **Vorheriges Kapitel:**  
[Einführung](01-einfuehrung.md)

➡️ **Nächstes Kapitel:**  
[SNS](03-sns.md)
