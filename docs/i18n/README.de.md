# Aethmere · 识海

> Öffentliches Distributionsrepository — **dies ist kein Open-Source-Repository**.

[简体中文](../../README.md) | [English](README.en.md) | [日本語](README.ja.md) | [한국어](README.ko.md) | [ไทย](README.th.md) | [Tiếng Việt](README.vi.md) | [Bahasa Indonesia](README.id.md) | [Bahasa Melayu](README.ms.md) | [Filipino](README.fil.md) | [Español](README.es.md) | [Português](README.pt.md) | [Français](README.fr.md) | **Deutsch** | [Русский](README.ru.md) | [العربية](README.ar.md)

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

**Was gemessen wurde:** der kontrollierte Gedächtnisvertrag von Aethmere — dessen
explizite Befehlsgrammatik und acht Abfrage-Aufgabenfamilien (8 × 300 Cluster) —
durchgehend über die realen Aufnahme- und Freigabedienste. Kontrollierte Antworten
werden von deterministischen Diensten erzeugt, **nicht von einem improvisierenden
großen Sprachmodell**, weshalb die untenstehenden Zahlen nicht davon abhängen,
welches Anbietermodell Sie mitbringen.

**Wie gemessen wurde:** Das Kandidatensystem wurde zuerst per Hash eingefroren, und
erst danach wurde ein vorab festgelegter Zufallsstartwert (Seed) gezogen; die Fälle
wurden deterministisch erzeugt, jede Antwort wurde von einem zum Erzeugungszeitpunkt
fixierten maschinellen Orakel bewertet, und sämtliche Belege wurden aufbewahrt. Die
Bewertung verlangt exakte Antworten bei beantwortbaren Fragen, Verweigerung bei
unbeantwortbaren und Durchleitung bei gewöhnlichen — jede der drei Richtungen fällt
gesondert durch, sodass sich Korrektheit niemals durch Verweigerungen gewinnen lässt.

**Womit verglichen wurde:** „vorher“ = dieselben Konversationen, direkt an ein lokales
qwen2.5:7b gegeben (Ollama, Temperatur 0, ohne Governance); „nachher“ = die
kontrollierte Gedächtnisspur. Die Bewertung der Baseline ist bewusst großzügig (eine
Antwort, die den korrekten Wert enthält, zählt als korrekt, chinesische Zahlformen
eingeschlossen), sodass die Heilungszahlen konservativ sind. Der Vorschlagsgeber der
Freitext-Erfassungsspur ist dasselbe lokale 7B-Modell, bei null Abfluss Ihres
Originaltexts.

| Aufgabenfamilie | Vorher (7B, ungesteuert) | Nachher (kontrollierte Spur) |
|---|---|---|
| Direkte Erinnerung | 41 / 300 (13.7%) | **300 / 300** |
| Mengen und Zählen | 98 / 300 (32.7%) | **300 / 300** |
| Zeitlich eingegrenzte Erinnerung | 63 / 300 (21.0%) | **300 / 300** |
| Aktualisierungen und Konflikte | 41 / 300 (13.7%) | **300 / 300** |
| Mehrschritt-Verknüpfungen | 65 / 300 (21.7%) | **300 / 300** |
| Falscherinnerungs-Druck | 45 / 300 (15.0%) | **300 / 300** |
| Offene Schlüssel-Wert-Notizen | 34 / 300 (11.3%) | **300 / 300** |
| Grenzfall-Druck * | 213 / 300 (71.0%) | **300 / 300** |
| **Gesamt** | **600 / 2,400 (25.0%)** | **2,400 / 2,400 (100%, 95% einseitige untere Schranke ≥ 99.87%)** |

\* Gewöhnliche Fragen in der Grenzfall-Familie werden der Baseline automatisch
gutgeschrieben (das Modell soll sie ja beantworten), weshalb ihr Baseline-Anteil
höher ausfällt.

Die acht Aufgabenfamilien decken ab: direkte Erinnerung, Mengen und Zählen,
zeitlich eingegrenzte Erinnerung, Aktualisierungen und Konflikte, Mehrschritt-Verknüpfungen,
Falscherinnerungs-Druck (wo jeder herausgegebene Wert eine Erfindung wäre), offene
Schlüssel-Wert-Notizen sowie Grenzfall-Druck (erzählende Sätze, die nicht aufgenommen
werden dürfen, und gewöhnliche Fragen, die nicht geschluckt werden dürfen).
Heilungsbilanz: Alle 1,800 Cluster, bei denen die Baseline erfand oder irrte, wurden
von der kontrollierten Spur **behoben**, bei **null Regressionen** unter den 600, die
die Baseline richtig hatte — begrenzte Heilung 100% (95% einseitige untere Schranke
≥ 99.83%).

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
- Jede Erinnerung ist prüfbar und bis auf Ihre eigenen Worte zurückverfolgbar;
  zurückgezogene Werte tauchen über keine Abfrage je wieder auf.
- Bestätigung vor dem Schreiben: Neue Gedächtnisbefehle erfordern Ihre ausdrückliche
  Bestätigung im Produkt, bevor irgendetwas gespeichert wird.
- Auch natürliche Sätze können zu Erinnerungen werden: Bevor irgendetwas gespeichert
  wird, prüft das System es eigenständig und nimmt nur Inhalte an, die Ihrem
  ursprünglichen Wortlaut entsprechen — bei null Abfluss Ihres Originaltexts.

**Skills-Hub und Wissensbasis**

- Serverseitiger Skills-Hub: ab dem Login verfügbar — eine wachsende Bibliothek von
  fachlichen Fähigkeitskarten wird automatisch auf Ihre Frage geroutet, ganz ohne
  manuelle Verdrahtung.
- Persönliche Wissensbasis: Ihre hochgeladenen Dokumente werden zu einem kontoisolierten,
  durchsuchbaren privaten Korpus, der bei Bedarf zur Antwortzeit herangezogen wird.
- Persönliche Cloud-Gedächtnis-Erinnerung: über Sitzungen und Geräte hinweg, wobei nur
  begrenzte, für die jeweilige Frage relevante Fragmente eingebunden werden.

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
gewährt. Siehe [NOTICE.md](../../NOTICE.md).

## Support

Verwenden Sie [GitHub Issues](https://github.com/kzkz137806/aethmere-os/issues) für
öffentliche Fehlerberichte und Funktionswünsche. Fügen Sie keine Passwörter,
API-Schlüssel, privaten Erinnerungen, personenbezogenen Daten oder vertraulichen
Projektinhalte bei.

Bei Sicherheitsproblemen folgen Sie [SECURITY.md](../../SECURITY.md).
