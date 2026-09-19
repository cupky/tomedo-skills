# Änderungen

Die Versionsnummer je Skill steht im Feld `version` der jeweiligen `SKILL.md`
und ist nach dem Entpacken auch beim Anwender ablesbar.

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
