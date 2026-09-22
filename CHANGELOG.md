# Änderungen

Die Versionsnummer je Skill steht im Feld `version` der jeweiligen `SKILL.md`
und ist nach dem Entpacken auch beim Anwender ablesbar.

## 1.4 — 2026-09-22

Zwei Untersuchungen desselben Tages sind eingearbeitet: eine Benchmark des Kartei-Chats über
**356 protokollierte Läufe** an zwei synthetischen Akten, und die Frage, wo tomedo die
KI-Herkunft eines Karteieintrags speichert.

**Der Kartei-Chat hat zwei Betriebsarten, und bisher war nur eine beschrieben.** Alle
bisherigen Aussagen messen den Betrieb, in dem das Modell die Akte durchsucht. Fügt man
stattdessen ein Briefkommando in das Chat-Eingabefeld ein, löst tomedo es in einer halben
Sekunde auf — der Prompt trägt die Daten dann selbst hinein, und es gibt keinen Abruf, der
fehlschlagen kann. Das beseitigt die Fehlerklasse „findet nicht" vollständig. Es beseitigt
nicht, dass der Chat nicht nachrechnet: dazu braucht es zusätzlich einen ausdrücklichen
Prüfauftrag. Erst beide zusammen heben das Erkennen von Widersprüchen in der Akte von 0 aus
118 auf 18 aus 20.

**Zwei Aussagen waren falsch und sind widerrufen.** `karteichat-prompting` behauptete,
PDF-Anlagen seien für den Chat nicht lesbar. Er liest sie in **54 von 65** Läufen — was er
nicht zuverlässig tut, ist den Leser überhaupt zu starten. Und was die Anleitung als
„korrekte Abstinenz" führte (der Chat meldet „nicht dokumentiert", obwohl der Suchbegriff in
der Kartei vorkommt), ist gegen den Volltextexport gemessen der gefährlichste Fehlschlag des
Bestands: Der Wert stand in der Akte, nur nicht dort, wo der Chat gesucht hat — 20 von 20 Mal,
ohne Warnung.

**Die Herkunft eines KI-Eintrags ist abfragbar.** `karteieintrag.autoquelle` trägt das
✦-Symbol der Karteiliste, `llmcall` protokolliert jeden Modellaufruf mit Token und Kosten.
Damit werden Testläufe per Abfrage prüfbar statt per Hinsehen. Der Schalter „✦ anzeigen" ist
dabei **kein Filter** — er blendet nur das Symbol aus; und die Kennzeichnung verlässt die
Kartei nicht: nicht im Eintragseditor, nicht in der Kommandoausgabe, nicht im Ausdruck.

**Praxisinterna:** `karteichat-prompting` bekommt wie zuvor schon `tomedo-statistik-hql` eine
eigene `references/praxis-interna.md`. Dort gehört die Kommando-Inventur hin — welche
Briefkommandos in Ihrer Installation überhaupt einen Wert liefern. Das ist von Praxis zu
Praxis verschieden und entscheidet, welche Anker Sie bauen können.

| Anleitung | Version | geändert |
|---|---|---|
| karteichat-prompting | 1.2 | Benchmark über 356 Läufe, zweite Betriebsart, KI-Flag, zwei Widerrufe, `praxis-interna.md`, Checkliste von 19 auf 26 Punkte |
| tomedo-statistik-hql | 1.4 | `autoquelle`, Kapitel 18 „KI- und LLM-Subsystem", Regel 4 um die Metabase-Ausnahme ergänzt, Query-Baustein 38, Datumsausreißer |
| tomedo-llm-endpunkt | 1.2 | `llmcall` als serverseitige Telemetrie aller KI-Pfade; Kostenkontrolle wird messbar statt geschätzt |
| tomedo-metabase-migration | 1.2 | Die Replika ist der Katalogzugang — Schemafragen gehören dorthin, nicht in den tomedo-Client |
| übrige fünf | unverändert | — |

## 1.3 — 2026-09-19

Praxisinterna sind ab jetzt eine eigene Datei je Anleitung. In `tomedo-statistik-hql`
heißt sie `references/praxis-interna.md`. Dort — und nur dort — stehen Kürzel, Namen und
gemessene Werte einer konkreten Installation. Der übrige Text nennt dieselben
Sachverhalte generisch: mit Platzhalter (`<TYP-A>`) und einer Query, mit der Sie den
eigenen Wert ermitteln.

Ausgeliefert wird eine leere, erklärte Vorlage dieser Datei. Sie ist zum Ausfüllen
gedacht — tragen Sie Ihre Kürzel und Messwerte ein, dann arbeitet die KI mit Ihren
Werten statt mit Vermutungen. Beim Aktualisieren auf eine neue Fassung die eigene
Fassung dieser Datei stehen lassen, sonst ist die Erhebung weg.

Bisher hieß die Datei `instanz.md` und wurde beim Packen ersatzlos entfernt; das
Lese-Routing zeigte damit im ausgelieferten Stand ins Leere.

| Anleitung | Version | geändert |
|---|---|---|
| tomedo-statistik-hql | 1.3 | `praxis-interna.md` als Überlagerung, Lese-Routing angepasst |
| übrige acht | unverändert | — |

## 1.2 — 2026-09-19

Korrektur in `tomedo-statistik-hql`: Die Anleitung beschrieb den DATE-Filter als
`DATE;Offset;Label;Spalte;Operator`. Gemessen an vier Varianten in zwei Läufen expandiert
das Tag zu `( Komponente3 'Datum' ) Komponente4` — die dritte Komponente trägt Spalte **und**
Operator. Wer der bisherigen Beschreibung folgte, erzeugte immer ungültiges SQL. Die erste
Komponente ist zudem kein Tagesoffset: sie schaltet nur die Uhrzeit, das Datum steht immer
auf heute. Eine relative Vorbelegung wie „Monatsanfang" ist mit DATE deshalb nicht möglich;
dafür trägt `SELECTION` vollwertiges SQL einschließlich `date_trunc`.

Neu beschrieben sind außerdem der Ausführungsweg — ZS-Tags lösen ausschließlich in einer
gespeicherten Statistik vom Typ SQL auf, im Datenbank-Connector und in Metabase gehen sie
roh an PostgreSQL — und die Opt-in-Falle: ein nicht angehakter Parameter expandiert zu
Leerstring und lässt das vorangestellte `AND` stehen.

| Anleitung | Version | geändert |
|---|---|---|
| tomedo-statistik-hql | 1.2 | DATE-Komponentensemantik korrigiert, Ausführungsweg, Opt-in-Falle, Referenzmuster, SELECTION verifiziert |
| übrige acht | unverändert | — |

## 1.1 — 2026-09-15

Einarbeitung der angestauten Erkenntnisse aus 41 Arbeitssitzungen. 94 Änderungsblöcke
appliziert, verteilt auf sieben der neun Anleitungen.

Inhaltlich am wichtigsten: `tomedo-statistik-hql` empfahl bisher `karteieintrag.termin_ident`
als Abkürzung zwischen Karteieintrag und Termin. Das Feld ist in geprüften Beständen leer —
wer der Empfehlung folgte, bekam eine Abfrage, die stumm null Zeilen lieferte. Ebenso
aufgelöst: ein Widerspruch zwischen `tomedo-navigation` und `tomedo-llm-endpunkt` zur
Kostenrechnung, bei dem beide Anleitungen das Gegenteil voneinander behaupteten.

Neu hinzugekommen sind zwei Referenzdateien zur Werkzeugebene des Kartei-Chats und, in
`karteichat-prompting`, die Werkzeuganweisung als vierter Prompt-Baustein neben Anker,
Guard und Typdefinition.

| Anleitung | Version | geändert |
|---|---|---|
| karteichat-prompting | 1.1 | Werkzeuginventar als neue Referenz, vierter Prompt-Baustein, Modellwechsel, Transportkanal |
| tomedo-aktionsketten | 1.1 | `ACTION_ANSWER<n>` als Startpfad, Terminbestätigung als Kaskade |
| tomedo-kommandos | 1.1 | Zeitkappen-Semantik, Anzahl-Deckel, zwei Lücken im Diagnose-Kommando |
| tomedo-llm-endpunkt | 1.1 | Kontextreichweite aufgelöster Briefkommandos |
| tomedo-metabase-migration | 1.1 | Spaltennamen als Vertrag bei API-Zugriff |
| tomedo-navigation | 1.1 | Kostenabschnitt delegiert statt dupliziert, Werkzeugebene als dritter Automatisierungskanal |
| tomedo-statistik-hql | 1.1 | Direkt-Pfad-Korrektur, Statistikverwaltung, Diagnosetypen, Zuweiser, Terminerinnerung und weitere |
| tomedo-patientenformulare | 1.0 | unverändert |
| tomedo-textbausteine | 1.0 | unverändert |

Technisch: Die Fassung steht jetzt unter `metadata.version` im Frontmatter statt als eigenes
Feld — als eigenes Feld lehnt der offizielle Skill-Validator sie ab. Bei
`tomedo-textbausteine` war das Frontmatter zudem nicht als YAML lesbar, weil ein Doppelpunkt
in der Beschreibung es zerlegte; das ist behoben.

## 1.0 — 2026-09-15

Erste Veröffentlichung: Handbuch als Seite und PDF, neun Skills einzeln und als
Sammelpaket.

| Skill | Version |
|---|---|
| karteichat-prompting | 1.0 |
| tomedo-aktionsketten | 1.0 |
| tomedo-kommandos | 1.0 |
| tomedo-llm-endpunkt | 1.0 |
| tomedo-metabase-migration | 1.0 |
| tomedo-navigation | 1.0 |
| tomedo-patientenformulare | 1.0 |
| tomedo-statistik-hql | 1.0 |
| tomedo-textbausteine | 1.0 |
