# AWS CloudShell

Die AWS CloudShell kann auf der Web-Konsole durch die Suchfunktion gefunden werden oder direkt mit dem Kommandozeilen-Symbol oben rechts gestartet werden.

![CloudShell](cloudshell_button.png)

## Kommando-Aufbau[^1]

Das AWS Kommandozeilen-Tool ist nach den einzelnen Produkten gegliedert. Der erste Befehl an die Kommandozeile gibt dabei das Produkt / den Service an. Nachfolgende Befehle werden auf diesem Service ausgeführt. Anschaulicher lässt sich dies mit Beispielen erklären:

```
aws s3 ls
```

<div style="
  border: 2px solid #ffffff;
  padding: 14px;
  border-radius: 6px;
  margin: 14px 0;
">
  <span style="color:cyan; font-weight:bold; font-size:1.2em;">
    Info: Aufbau von AWS-CLI-Befehlen
  </span><br><br>

  Eine detaillierte Beschreibung des Aufbaus und der Syntax von
  AWS-CLI-Befehlen finden Sie in der offiziellen AWS-Dokumentation.
  <br><br>

  <b>Dokumentation:</b><br>
  <a href="https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-commandstructure.html" target="_blank">
    https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-commandstructure.html
  </a>
</div>


![CloudShell](terminal.png)

Dabei ruft `aws` die AWS CLI auf, `s3` legt den Service, in diesem Fall AWS S3, fest und `ls` ist der auszuführende Befehl. Wie auch in Linux-Systemen steht `ls` für "list" und listet alle enthaltenen Elemente, in diesem Fall S3-Buckets auf.


<div style="
  border: 2px solid #ffffff;
  padding: 14px;
  border-radius: 6px;
  margin: 14px 0;
">
  <span style="color:cyan; font-weight:bold; font-size:1.2em;">
    Frage 1
  </span><br><br>

  Führen Sie den oben angegebenen Befehl aus (<code>aws s3 ls</code>).
  <br><br>

  Dokumentieren Sie:
  <ul>
    <li>welche <b>S3-Buckets</b> in der Ausgabe angezeigt werden</li>
    <li>einen <b>Screenshot der Kommandozeilenausgabe</b></li>
  </ul>
</div>


### Parameter

Um die Parameter eines Befehls anzupassen, wird wie bei anderen Kommandozeilenprogrammen verfahren. Soll eine Datei vom S3-Bucket auf die Cloudshell kopiert werden, kann dieser Befehl ausgeführt werden:

```
aws s3 cp s3://a--sourcefiles/hls_template.json . --dryrun
```

Bei diesem Befehl wird `cp`, also eine Kopieroperation, ausgeführt. Der S3-Pfad danach gibt die Quelldatei an. Danach wird mit "." der Zielpfad angegeben. Der Punkt bedeutet dabei, dass der Zielpfad der Ort ist, von dem der Befehl ausgeführt wird. Die Option `--dryrun` führt den Befehl nur "auf dem Trockenen", also ohne das wirkliche Kopieren der Datei aus. Auf der Kommandozeile werden trotzdem alle Statusmeldungen wie üblich ausgegeben.

Um zu überprüfen, ob eine Datei in die CloudShell kopiert wurde, kann der Befehl `ls` in der CloudShell ausgeführt werden (ohne `aws s3` davor). Dieser Befehl listet die Dateien im aktuellen Pfad der CloudShell auf.

<div style="
  border: 2px solid #ffffff;
  padding: 14px;
  border-radius: 6px;
  margin: 14px 0;
">
  <span style="color:cyan; font-weight:bold; font-size:1.2em;">
    Frage 2
  </span><br><br>

  Führen Sie den Kopierbefehl zunächst mit der Option
  <code>--dryrun</code> aus und anschließend ohne diese Option.
  <br><br>

  Dokumentieren Sie:
  <ul>
    <li>die Ausgabe des Befehls mit <code>--dryrun</code></li>
    <li>die tatsächliche Ausführung des Kopiervorgangs ohne <code>--dryrun</code></li>
    <li>die Kontrolle des Dateisystems mit dem Befehl <code>ls</code></li>
    <li>einen <b>Screenshot der Kommandozeilenausgabe</b></li>
  </ul>
</div>




[^1]: [https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-commandstructure.html](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-commandstructure.html)