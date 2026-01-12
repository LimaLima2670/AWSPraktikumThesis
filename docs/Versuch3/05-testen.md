# Testen

## Test mit der erzeugten Transcodiervorlage
Um die automatische Transcodierung zu testen, soll eine mp4-Datei aus dem
S3-Bucket a--sourcefiles mithilfe der S3-Weboberfläche in den
Ingest-Ordner (ingest/) des eigenen Buckets kopiert werden.
Danach sollte automatisch ein neuer Transcodierauftrag bei
AWS MediaConvert gestartet werden.

Nach Abschluss der Transcodierung werden die erzeugten HLS-Dateien automatisch
im Ordner export/ des eigenen S3-Buckets abgelegt.
Dieser Ordner dient als Origin für das Fastly CDN, welches die Inhalte
im Pull-Verfahren aus S3 abruft.

Das Abspielen der erzeugten HLS-Streams kann – wie in Versuch 2 –
mithilfe des HLS-Players erfolgen.


## Status der Bearbeitung überprüfen
Dazu über die Suchleiste den Service CloudWatch auswählen und unter
„Protokolle → Protokollgruppen“ die Übersicht öffnen.
In der Liste werden die eigenen Lambda-Funktionen angezeigt.
Durch Klick auf eine Funktion können die Protokolle der letzten Aufrufe
eingesehen werden.

## Eigene Transcodier-Vorlage

Eigene Transcodiervorlagen können in der AWS MediaConvert GUI
erstellt werden. Dazu wird in MediaConvert der Punkt „Aufgabenvorlagen“
gewählt. Über „Vorlage erstellen“ kann eine neue Vorlage angelegt werden.


![MediaConvert Vorlagen](mediaconvert_templates.png)

Zuerst muss der Name der Vorlage gewählt werden.
Hierbei soll wieder der eigene HDS-Nutzername als Präfix verwendet werden,
sodass der Name der Vorlage folgendem Schema folgt:

```bash
musterstudent_template2
```

Die Felder „Kategorie“ und „Beschreibung“ können leer gelassen werden.

![MediaConvert Vorlage Name](mediaconvert_template_name.png)

Nun können unter „Eingaben“ und „Ausgabegruppen“ wie gewohnt
die gewünschten Transcodierungseinstellungen festgelegt werden.

<div style="border: 2px solid #ffffff; padding: 14px; border-radius: 6px; margin: 14px 0;"> <span style="color:cyan; font-weight:bold; font-size:1.2em;">Frage 4</span><br><br>

Wählen Sie eigene Transcodierungs-Einstellungen.
Probieren Sie andere Codecs, Bitraten, Seitenverhältnisse oder
Farbkorrekturen aus.
Die gewählten Parameter sollen sich beispielsweise an bestimmten Endgeräten
(z. B. Smartphone) orientieren oder sich anderweitig von den bisherigen
Transcodierungseinstellungen unterscheiden.

Dokumentieren Sie Ihre Wahl im Bericht.

</div> <div style="border: 2px solid #ffffff; padding: 14px; border-radius: 6px; margin: 14px 0;"> <b>Hinweis</b><br><br> Das CDN ist auf die Wiedergabe von HLS-Streams konfiguriert. Daher sollte auch bei eigenen Vorlagen HLS als Ausgabeformat gewählt werden. </div>

![MediaConvert Vorlage Einstellungen](mediaconvert_template_settings.png)

Sind alle Parameter festgelegt, kann die Vorlage über den Button
„Erstellen“ angelegt werden.
Anschließend kann über „JSON exportieren“ die entsprechende
JSON-Datei heruntergeladen werden.



![MediaConvert Vorlage Export](mediaconvert_template_export.png)

Damit die neu erstellte Vorlage vom automatisierten Workflow verwendet werden
kann, muss die JSON-Datei in den Ordner templates/ des eigenen
S3-Buckets hochgeladen werden.

In der Lambda-Funktion, die für das Erstellen des Transcodierauftrags zuständig
ist, muss anschließend lediglich der Pfad zur Vorlagendatei auf die neue
JSON-Datei angepasst werden.

Neu in den Ingest-Ordner hochgeladene Mediendateien werden danach automatisch
mithilfe der neuen Vorlage transcodiert.