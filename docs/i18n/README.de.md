# Aethmere · 识海

> Öffentliches Distributionsrepository — **dies ist kein Open-Source-Repository**.

[English](../../README.md) | [简体中文](README.zh-CN.md) | [日本語](README.ja.md) | [한국어](README.ko.md) | [ไทย](README.th.md) | [Tiếng Việt](README.vi.md) | [Bahasa Indonesia](README.id.md) | [Bahasa Melayu](README.ms.md) | [Filipino](README.fil.md) | [Español](README.es.md) | [Português](README.pt.md) | [Français](README.fr.md) | **Deutsch** | [Русский](README.ru.md) | [العربية](README.ar.md)

Aethmere ist eine Gedächtnisschicht für KI-gestütztes Arbeiten, die **das Nicht-Erfinden**
als technische Anforderung behandelt, nicht als Werbeversprechen. Sie gibt unterstützten
KI-Clients ein dauerhaftes, nutzerkontrolliertes Gedächtnis mit sichtbaren Antwortgrenzen:
Was Sie ihm ausdrücklich zu merken aufgetragen haben, wird exakt beantwortet; was nie erfasst
oder wieder zurückgezogen wurde, wird verweigert statt geraten; gewöhnliche Fragen gehen
unverändert an Ihr Modell durch.

[Website](https://aethmere.com) ·
[Web-App](https://app.aethmere.com) ·
[Neueste Version](https://github.com/kzkz137806/aethmere-os/releases/latest) ·
[Problem melden](https://github.com/kzkz137806/aethmere-os/issues)

## Warum Aethmere

Die meisten KI-Gedächtnissysteme scheitern auf eine von zwei Weisen: Sie halluzinieren
Erinnerungen, die man ihnen nie gegeben hat, oder sie schlucken gewöhnliche Fragen mit
unnötigen Verweigerungen. Die kontrollierte Gedächtnisspur von Aethmere ist so gebaut,
dass sich keine der beiden Richtungen verstecken kann:

- **Beantwortbare Fragen müssen exakt beantwortet werden.** Die Verweigerung einer
  beantwortbaren Frage zählt in unserer Evaluation als Fehler — Korrektheit lässt sich
  niemals durch Verweigerungen erkaufen.
- **Unbeantwortbare Fragen müssen verweigert werden.** Wenn ein Wert nie erfasst wurde,
  zurückgezogen wurde oder mehrdeutig ist, wäre die Herausgabe *irgendeines* Wertes eine
  Erfindung. Die kontrollierte Spur verweigert deterministisch.
- **Gewöhnliche Fragen müssen durchgelassen werden.** Eine Frage, die lediglich
  Gedächtnis-Stichwörter enthält, wird an Ihr Modell geleitet und nicht geschluckt.
- **Schreibvorgänge werden bestätigt.** Eine Nachricht, die wie ein Gedächtnisbefehl
  aussieht, wird erst nach Ihrer ausdrücklichen Bestätigung geschrieben; lehnen Sie ab,
  bleibt sie gewöhnlicher Chatverlauf.

## Gemessene Ergebnisse (versiegelte, begrenzte Evaluation)

In einer versiegelten internen Evaluation des kontrollierten Gedächtnisvertrags — der
Kandidat per Hash eingefroren, bevor ein vorab festgelegter Zufallsstartwert (Seed) gezogen wurde,
die Fälle deterministisch erzeugt, jede Antwort von einem zum Erzeugungszeitpunkt
fixierten maschinellen Orakel bewertet, sämtliche Belege aufbewahrt:

| Kennzahl | Ergebnis | 95% untere Schranke |
|---|---|---|
| Begrenzte Korrektheit | **2,400 / 2,400 Cluster korrekt** (8 Aufgabenfamilien × 300, Null-Toleranz je Familie) | ≥ 99.87% |
| Begrenzte Halluzinationsheilung | **1,800 / 1,800 Baseline-Fehler behoben, 0 / 600 Regressionen** gegenüber einem lokalen 7B-Modell mit denselben Konversationen ohne Governance | ≥ 99.83% |

Die acht Aufgabenfamilien decken ab: direkte Erinnerung, Mengen und Zählen,
zeitlich eingegrenzte Erinnerung, Aktualisierungen und Konflikte, Mehrschritt-Verknüpfungen,
Falscherinnerungs-Druck (wo jeder herausgegebene Wert eine Erfindung wäre), offene
Schlüssel-Wert-Notizen sowie Grenzfall-Druck (erzählende Sätze, die nicht aufgenommen
werden dürfen, und gewöhnliche Fragen, die nicht geschluckt werden dürfen). Bei denselben
Konversationen erfand oder irrte die ungesteuerte lokale 7B-Baseline bei 75% der Cluster;
die kontrollierte Spur behob sie alle, ohne Regressionen bei den Clustern, die die
Baseline richtig hatte.

**Geltungsbereich, klar gesagt:** Dies sind begrenzte Ergebnisse zum kontrollierten
Gedächtnisvertrag von Aethmere — dessen expliziter Befehlsgrammatik und Abfragefamilien —
durchgehend über die realen Aufnahme- und Freigabedienste gemessen. Sie sind keine
Aussage über die offene Welt, keine Aussage über die Korrektheit des Gesamtprodukts und
keine Aussage über die allgemeinen Antworten Ihres Modells. Außerhalb des kontrollierten
Vertrags antwortet Ihr Modell wie gewohnt, und die üblichen Modellgrenzen gelten
unverändert.

## Was Aethmere leistet

**Kontrolliertes Gedächtnis (der Kern)**

- Explizite Gedächtnisbefehle mit exakter, prüfbarer Semantik: erfassen, aktualisieren,
  zurückziehen, auffinden und offene Schlüssel-Wert-Notizen; Mehrwert-Mengen; zeitlich
  eingegrenzte Erinnerung.
- Signierte Gedächtnisherkunft: Jeder akzeptierte Fakt trägt eine überprüfbare Kette
  zurück bis zur ursprünglichen Nachricht; zurückgezogene Werte tauchen über keine
  Abfrage je wieder auf.
- Bestätigung vor dem Schreiben: Neue Gedächtnisbefehle erfordern Ihre ausdrückliche
  Bestätigung im Produkt, bevor irgendetwas gespeichert wird.
- Freitext-Erfassung mit lokaler Prüfung: Natürliche Sätze können über ein lokales
  Modell Gedächtniskandidaten vorschlagen und werden vor der Annahme deterministisch
  erneut geprüft — bei null Abfluss Ihres Originaltexts.

**Persönliches Cloud-Gedächtnis**

- Kontoisolierter Cloud-Speicher (etwa 100M geschätzte Token, verteilt auf bis zu
  200 Konversationen)
  mit geräteübergreifender Wiederherstellung; Upload-Schalter je Gerät; Antworten
  binden nur begrenzte, relevante Historie ein — niemals das gesamte Archiv.
- Anbieter-API-Schlüssel werden als AES-GCM-Chiffrat kontogebunden gespeichert;
  gewöhnliche APIs sehen stets nur die letzten vier Zeichen.

**Dokumente und Bilder**

- Dokumenten-Wissensbasis: TXT, Markdown, CSV, JSON, HTML und PDF; der Text wird in
  Ihrem Browser extrahiert, und gespeichert werden nur kontoisolierte Retrieval-Fragmente
  sowie ein hybrider Vektorindex — die Originaldateien werden nicht aufbewahrt.
- Bild-OCR: Der erkannte Text wird mit Quellenpräfix und einer Zusammenfassung
  „prüfungsbedürftig“ eingefügt; die Erkennung läuft über den von Ihnen konfigurierten
  Anbieter.

**Echtzeitsuche**

- Echtzeit-Websuche über mehrere Suchmaschinen mit Aktualitätsfenstern (Tag / Tage /
  Woche / Monat), automatischer Abfrageplanung und Wiederholungen sowie Ergebnisgrenzen,
  die auf die Fundierung der Antwort abgestimmt sind.
- Sprachübergreifendes Retrieval: Chinesische Fragen werden automatisch auf fokussierte
  internationale Suchthemen abgebildet (Märkte, Rohstoffe, Währungen und mehr).
- Live-Momentaufnahmen chinesischer Futures für unterstützte Symbole, zum Antwortzeitpunkt
  abgerufen und in der Antwort als Datenquellen ausgewiesen.

**Überall, wo Sie arbeiten**

- Installierbare Web-App für Mobilgeräte und Desktop (PWA) mit Streaming-Antworten,
  Codeblöcken, Tabellen und dem Kopieren von Nachrichten mit einem Fingertipp.
- Desktop-CLI (`aethmere-cli`) mit einmaliger Gerätekopplung: `aethmere sync`
  spiegelt Ihr Cloud-Gedächtnis lokal; Claude Code, Codex und andere MCP-Clients können
  es über `cloud_memory_recall` nutzen. Standardmäßig schreibgeschützt; der Upload
  erfordert ein ausdrückliches doppeltes Opt-in.
- Chat-Kanäle: Telegram (Bot-Direktnachricht) oder Discord (`/aethmere ask`, nur für
  den Fragenden sichtbare Antworten) mit Einmalcodes an Ihr Konto binden; die Aufhebung
  der Bindung entzieht den Zugriff sofort.
- Serverseitiger Skills-Hub: Kuratierte Fähigkeitskarten werden nach dem Login
  automatisch geroutet — keine manuelle Skill-Verdrahtung.

## Aethmere CLI installieren

Voraussetzung: Node.js 22 LTS (`>=22.13.0 <23`).

```bash
npm install -g https://github.com/kzkz137806/aethmere-os/releases/download/v0.7.0/aethmere-cli-0.7.0.tgz
aethmere --version
aethmere connect
aethmere doctor --profile package
```

Erwartete Version:

```text
Aethmere CLI 0.7.0
```

`aethmere connect` erstellt eine Verbindung auf Benutzerebene für unterstützte
KI-Clients. Sie müssen sich nicht jedes Mal neu verbinden, wenn Sie den Projektordner
wechseln. Die lokale Nutzung erfordert keine Web-Einladung. Cloud-Login und
Synchronisierung sind optional, und der Desktop-Upload bleibt deaktiviert, bis der
Benutzer ihn einschaltet.

Eine Schritt-für-Schritt-Anleitung auf Chinesisch finden Sie unter
[aethmere.com](https://aethmere.com/#install).

## Download überprüfen

SHA-256 für `aethmere-cli-0.7.0.tgz`:

```text
964903d1f5787e6fb58dfe37a762d29c966971abd20e06a2b22cdcfe9954a2a6
```

PowerShell:

```powershell
Get-FileHash .\aethmere-cli-0.7.0.tgz -Algorithm SHA256
```

macOS/Linux:

```bash
shasum -a 256 aethmere-cli-0.7.0.tgz
```

Die CLI überprüft vor der Installation eines Updates außerdem die signierten
Update-Metadaten, die Paketgröße und den SHA-256-Wert. Updates werden nie ohne
Bestätigung installiert.

## Was dieses Repository enthält

Dieses öffentliche Repository ist der offizielle Ort für:

- Release-Downloads und Prüfsummen;
- Installations- und Update-Anleitungen;
- öffentliche Änderungsprotokolle;
- Issue-Tracking und Sicherheitsmeldungen.

Der proprietäre Aethmere-Kern, private Wissenssysteme, Evaluationsmaterial, die
Dienstimplementierung und die interne Entwicklungshistorie sind **nicht enthalten**.

## Produktmodell

Aethmere verwendet ein Modell aus öffentlichem Client und privatem Kern:

- öffentliche Distributions- und Integrationszugänge;
- proprietäre gehostete Kerndienste;
- herunterladbarer Endkunden-Client;
- keine öffentliche Offenlegung des Kernquellcodes.

Die Inhalte dieses Repositorys und seiner Release-Assets sind proprietär, sofern eine
Datei nicht ausdrücklich etwas anderes bestimmt. Es wird keine Open-Source-Lizenz
gewährt. Siehe [NOTICE.md](NOTICE.md).

## Support

Verwenden Sie [GitHub Issues](https://github.com/kzkz137806/aethmere-os/issues) für
öffentliche Fehlerberichte und Funktionswünsche. Fügen Sie keine Passwörter,
API-Schlüssel, privaten Erinnerungen, personenbezogenen Daten oder vertraulichen
Projektinhalte bei.

Bei Sicherheitsproblemen folgen Sie [SECURITY.md](SECURITY.md).
