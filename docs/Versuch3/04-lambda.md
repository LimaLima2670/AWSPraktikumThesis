# Lambda

## Funktion 1: CreateJob

Funktion 1: CreateJob

Die erste Lambda-Funktion wird ausgeführt, sobald eine neue Videodatei in den Ordner ingest/ des eigenen S3-Buckets hochgeladen wird.
Sie erstellt automatisch einen Transcodierauftrag in AWS MediaConvert und versendet anschließend eine Statusmeldung über AWS SNS.

Dazu wird in der AWS Management Console über das linke Menü der Punkt „Funktionen“ ausgewählt und anschließend auf „Funktion erstellen“ geklickt.

Der Funktionsname soll folgendermaßen aufgebaut sein:

```bash
[HDS-Nutzername]-CreateJob
```
Als Laufzeit wird Python gewählt.
Im Abschnitt „Standard-Ausführungsrolle ändern“ wird die Option
**„Vorhandene Rolle verwenden“** ausgewählt und die Rolle MVS_Lambda_Role festgelegt.

![Lambda Funktion erstellen](../../assets/Versuch3/lambda_createFunction.png)

**Sind die Infos eingetragen, kann auf "Funktion erstellen" geklickt werden, um die Funktion zu erstellen.**

![Lambda Funktion erstellt](../../assets/Versuch3/lambda_createFunctionSuccess.png)

### Auslöser hinzufügen

Damit die Funktion beim Hinzufügen einer neuen Datei aufgerufen wird, muss ein Auslöser hinzugefügt werden. Dies kann über die Schaltfläche "Auslöser hinzufügen" oder im Reiter "Konfiguration -> Auslöser" geschehen.

![Lambda Funktion Auslöser](../../assets/Versuch3/lambda_ausloeser.png)

Als Quelle soll S3 gewählt werden. Im Feld "Bucket" soll der eigene Bucket ausgewählt werden. Im Feld "Prefix" soll der Ingest-Ordner angegeben werden (also `ingest/`) und im Suffix "mp4". So wird die Funktion nur für hinzugefügte MP4-Dateien im Ordner "Ingest" aufgerufen. 

<div style="border:2px solid #ff5555; padding:12px; border-radius:6px; margin:14px 0;"> <b>Wichtig:</b><br> Eingabe- (<code>ingest/</code>) und Ausgabeordner (<code>export/</code>) müssen strikt getrennt sein. Andernfalls kann es zu rekursiven Funktionsaufrufen und unnötigen Kosten kommen. </div>

![Lambda Funktion Auslöser Bucket](../../assets/Versuch3/lambda_ausloeser_bucket.png)

Ist der Auslöser erstellt, wird er oben in der Übersicht angezeigt.

![Lambda Funktion Auslöser Bucket](../../assets/Versuch3/lambda_ausloeser_bucket_success.png)



### Code der Lambda-Funktion

!!! question "Frage 2"
    Versehen Sie den Code mit Kommentaren, die die einzelnen Abschnitte und Befehle beschreiben. Es muss nicht zu jedem Befehl ein Kommentar geschrieben werden, dokumentieren Sie den Code aber so, dass die Funktionsweise ersichtlich wird. 
    
    Fügen Sie den kommentierten Code entweder als .py-Datei der Abgabe hinzu oder integrieren Sie den Code als Bild oder Text in den Bericht.

Im Nachfolgenden soll der Python-Code in die automatisch erstellte Datei "lambda_function.py" kopiert werden.

```
import boto3
import json

def lambda_handler(event, context):

    s3 = boto3.client('s3')
    sns = boto3.client('sns')

    topic_arn = 'arn:aws:sns:xxxxxxxxx'
    queue = 'arn:aws:mediaconvert:eu-central-1:757773874047:queues/Default'

    bucket = event['Records'][0]['s3']['bucket']['name']
    media_key = event['Records'][0]['s3']['object']['key']
    media_path = f"s3://{bucket}/{media_key}"

    filename = media_key.rsplit('/', 1)[1].rsplit('.', 1)[0]
    export_path = f"s3://{bucket}/export/{filename}/"

    try:
        template = s3.get_object(
            Bucket=bucket,
            Key="templates/hls_template.json"
        )
        template_json = json.loads(template['Body'].read())
    except:
        sns.publish(
            TopicArn=topic_arn,
            Message="Fehler beim Laden der Transcoding-Vorlage"
        )
        return

    template_json['Settings']['Inputs'][0]['FileInput'] = media_path
    template_json['Settings']['OutputGroups'][0]['OutputGroupSettings'] \
        ['HlsGroupSettings']['Destination'] = export_path

    mediaconvert = boto3.client(
        'mediaconvert',
        region_name='eu-central-1',
        endpoint_url='https://yk2lhke4b.mediaconvert.eu-central-1.amazonaws.com'
    )

    try:
        response = mediaconvert.create_job(
            Role="arn:aws:iam::757773874047:role/MediaConvert_Default_Role",
            Settings=template_json['Settings'],
            Queue=queue
        )

        sns.publish(
            TopicArn=topic_arn,
            Message=f"Transcoding-Job erstellt. Job-ID: {response['Job']['Id']}"
        )
    except:
        sns.publish(
            TopicArn=topic_arn,
            Message="Fehler beim Erstellen des Transcoding-Jobs"
        )

```

Damit die Transcoding-Aufträge in der richtigen Warteschlange landen und die Ergebnisse in das richtige SNS Thema veröffentlicht werden, müssen die folgenden Punkte im Code modifiziert werden:

In Zeile 9 muss die topic_arn durch das eigene Thema aus SNS ersetzt werden. Die topic_arn findet man in der Themenübersicht in der Liste in der Spalte "ARN". 

In Zeile 10 muss außerdem die Warteschlange eingetragen werden. Dabei kann einfach "Default" durch die entsprechende Warteschlange ersetzt werden. Es soll die gleiche Warteschlange wie in Versuch 1 genutzt werden, die auch in der Mail mit den Zugangsdaten zu finden ist.

Ist der Code hinzugefügt und modifiziert, kann die Lambdafunktion mithilfe des Buttons "Deploy" aktualisiert werden.

![Lambda Funktion Code](../../assets/Versuch3/lambda_code.png)

### Weitere Einstellungen

Damit die Funktion reibungslos ablaufen kann, muss noch eine weitere Option geändert werden. Da die Erstellung des Auftrages und das Abrufen der Transcodiereinstellungen verhältnismäßig lang dauert, muss das Timeout der Funktion vergrößert werden. Das Timeout dient dazu, dass Funktionen sich nicht in Endlosschleifen verfangen und damit viel Geld kosten.

Im Reiter `Konfiguration -> Allgemeine Konfiguration` kann das Timeout geändert werden. Dazu klickt man auf "Bearbeiten" und wählt ein Timeout von 10 Sekunden. Dies sollte ausreichend sein, um die Funktion auszuführen (Die durchschnittliche Ausführungszeit liegt bei ca. 2,5 Sekunden).

![Lambda Funktion Timeout](../../assets/Versuch3/lambda_timeout.png)

## Weiterverarbeitung der Transcoding-Ergebnisse

Nach Abschluss des Transcodierauftrags legt AWS MediaConvert die erzeugten HLS-Dateien automatisch im Ordner export/ des eigenen S3-Buckets ab.

Dieser Ordner dient direkt als Origin für das Fastly CDN.
Fastly ruft die Inhalte im Pull-Verfahren per HTTPS aus dem S3-Bucket ab und verteilt sie über seine Edge-Server.

export/

des eigenen S3-Buckets ab.

Dieser Ordner dient direkt als Origin für das Fastly CDN.
Fastly greift im Pull-Verfahren per HTTPS auf die Dateien zu und verteilt sie
über seine Edge-Server.


### Anbindung an das Fastly CDN

Der Ordner export/ des eigenen S3-Buckets ist so strukturiert, dass er direkt als Origin für ein Content Delivery Network (CDN) verwendet werden kann.
In den folgenden Versuchen wird dieser Ordner an ein Fastly CDN angebunden.

Fastly greift dabei im Pull-Verfahren über HTTPS auf die im export/-Ordner abgelegten HLS-Dateien zu.
Die vom CDN bereitgestellte Fastly-URL ersetzt anschließend die direkte S3-URL bei der Wiedergabe der Streams.

Die konkrete Einrichtung des Fastly-CDNs (Service, Origin, Hostname) erfolgt in Versuch 4 und ist nicht Bestandteil dieses Versuchs.


<div style="
  border: 2px solid #ffffff;
  padding: 14px;
  border-radius: 6px;
  margin: 14px 0;
">
  <span style="color:cyan; font-weight:bold; font-size:1.2em;">
    Frage 3
  </span><br><br>

  Erläutern Sie, warum für die Anbindung an <b>Fastly</b> keine zweite
  <b>Lambda-Funktion</b> zur Übertragung der Dateien erforderlich ist.
  <br><br>
  Gehen Sie dabei insbesondere auf das <b>Pull-Prinzip von CDNs</b> und die
  Rolle von <b>AWS S3 als Origin</b> ein.
</div>

