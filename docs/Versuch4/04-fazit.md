# Versuch 4 – Überprüfung der Ausgangsbasis

## Fazit zu dem Versuch

In Versuch 4 wurde die Auslieferung der in Versuch 3 automatisch erzeugten
HLS-Streaming-Inhalte über ein Fastly Content Delivery Network (CDN)
praktisch umgesetzt und getestet.
Im Gegensatz zu Versuch 2, in dem vorbereitete Beispielinhalte verwendet wurden,
erfolgte die Auslieferung in diesem Versuch erstmals direkt aus dem
export/-Ordner, der von AWS MediaConvert im Rahmen des automatisierten
Transcoding-Workflows befüllt wird.

Dabei konnte gezeigt werden, dass für die CDN-Anbindung keine Anpassung der
Fastly-Service-Konfiguration erforderlich ist. Der bestehende Service verweist
weiterhin auf den gesamten AWS-S3-Bucket als Origin.
Welche Inhalte ausgeliefert werden, entscheidet ausschließlich der
Pfad der Abruf-URL, in diesem Fall der Zugriff auf
export/<clipname>/.
Dies verdeutlicht die inhaltsagnostische Arbeitsweise eines CDNs:
Fastly agiert als reine Auslieferungsschicht zwischen Client und Origin und ist
unabhängig davon, ob Inhalte manuell oder automatisiert erzeugt wurden.

Ein wesentlicher Bestandteil dieses Versuchs war die Berechtigungslogik des
Origins. Da AWS MediaConvert Ausgabedateien standardmäßig privat ablegt, musste
der Lesezugriff auf den export/-Ordner explizit freigegeben werden, damit Fastly
als externer Dienst auf die HLS-Dateien zugreifen kann. Erst durch diese gezielte
Freigabe konnte der Stream erfolgreich über das CDN ausgeliefert werden.

Die abschließende Wiedergabe der Streams im hls.js-Player bestätigte die korrekte
Funktion des gesamten Workflows – von der automatisierten Transcodierung über die
strukturierte Ablage im Objektspeicher bis hin zur performanten,
CDN-gestützten Auslieferung an den Client.



<div style="border: 2px solid #ffffff; padding: 14px; border-radius: 6px; margin: 14px 0;"> <span style="color:cyan; font-weight:bold; font-size:1.2em;"> Abschließende Frage </span><br><br>

Erläutern Sie, warum für die Auslieferung der Inhalte über <b>Fastly</b>
keine Anpassung der CDN-Konfiguration erforderlich war, obwohl sich der
Speicherort der Inhalte von vorbereiteten Testdaten zu automatisch
erzeugten MediaConvert-Outputs im <code>export/</code>-Ordner geändert hat.
<br><br>
Gehen Sie dabei insbesondere auf die Rolle des
<b>Origin-Prinzips</b> und die Bedeutung des
<b>URL-Pfads</b> bei der CDN-Auslieferung ein.

</div>