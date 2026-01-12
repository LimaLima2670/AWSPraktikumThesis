# Einführung

In diesem dritten Versuch wird der Transcoding- und CDN-Workflow vollständig automatisiert umgesetzt.
Sobald eine neue Videodatei in einem definierten AWS-S3-Bucket abgelegt wird, startet automatisch ein mehrstufiger Verarbeitungsprozess.

**Die Quelldatei wird dabei:**

- automatisch erkannt,

- in mehrere HLS-Streaming-Varianten transcodiert,

- in einen Auslieferungs-Bucket exportiert

- und anschließend über ein Fastly CDN weltweit bereitgestellt.

 ![V3Flow](Versuch3Schaltbild.jpg)













## Grundbegriffe

### AWS Lambda

![AWS Labda Logo](aws_lambda.svg) 

AWS Lambda ist eine Plattform, auf der anhand eines Auslösers Funktionen in verschiedenen Programmiersprachen ausgeführt werden können. Dafür ist kein Servermanagement nötig und nur die Zeit der Berechnung wird in Rechnung gestellt.

Die in Lambda erstellten Funktionen können anhand eines Auslösers ausgeführt werden und mit den richtigen Berechtigungen auf andere AWS Ressourcen zugreifen. In diesem Versuch bildet Lambda das Rückgrat der Automatisierung des Prozesses. Wird eine neue Datei im S3 Speicher abgelegt, wird dadurch eine Lambda-Funktion ausgelöst, die einen Transcoding-Auftrag erstellt und startet.

!!! info "Kosten"
    Wie bei vielen anderen Services steigen die Kosten, je mehr die Services genutzt werden. Da Funktionen in Lambda im Normalfall keine großen Berechnungen ausführen, werden erst ab mehrerer Millionen Ausführungen pro Monat Kosten signifikant. Achtet man jedoch nicht darauf, wie oft ein Auslöser eine Funktion anstößt, können die Kosten im industriellen Umfeld [schnell steigen](https://asankha.medium.com/lambda-programming-errors-that-could-cost-you-thousands-of-dollars-a-day-265dfac354f).

### AWS SNS

![AWS SNS Logo](aws_sns.svg)

Über den AWS Simple Notification Service, kurz *SNS*, können Nachrichten von Applikation zu Applikation oder von Applikation zu Nutzer verteilt werden. SNS funktioniert über ein Pub/Sub-System, bei dem Nachrichten von einem Ersteller in einem Themen-Kanal veröffentlicht werden können (*Publish*) und Empfänger die Nachrichten empfangen, solange sie den entsprechenden Themen-Kanal abonniert haben (*Subscribe*).

Die Zustellung von einem Service zu Nutzern geschieht wahlweise über SMS- oder Push-Nachrichten, sowie via E-Mail. SNS ist nicht für z.B. Newsletter gedacht, sondern für interne Kommunikation.

!!! info "Kosten"
    Wie auch bei Labda fallen erst Kosten ab 1 Millionen Anfragen an. Diese können in einem industriellen Umfeld anfallen, werden aber in diesem Versuch nicht ansatzweise erreicht.