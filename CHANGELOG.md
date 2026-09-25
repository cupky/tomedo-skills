# Änderungen

Die Versionsnummer je Skill steht im Feld `version` der jeweiligen `SKILL.md`
und ist nach dem Entpacken auch beim Anwender ablesbar.

## 1.8 — 2026-09-23

**Die Metabase-Anleitung bringt ihr Import-Skript jetzt selbst mit.** Bisher verwies sie auf ein
Skript, das nicht beilag; jedes Paket musste seinen Import neu erfinden, und jede Neufassung war
ungetestet. Jetzt liegt ein fertiges Skript im Skill, das Sammlung, Fragen und Dashboard samt
Texten und Layout anlegt und nach dem Import die Ergebnisse gegen Prüfwerte aus dem
tomedo-Export abgleicht. Getestet gegen Metabase v0.63.3: sieben Fragen und 13 Kacheln, alle
vier Prüfwerte gleich, ein zweiter Lauf aktualisiert statt zu verdoppeln.

**Neu ist `--ersetzen`.** Das Skript erkennt Vorhandenes am Namen. Wurde eine Frage zwischen zwei
Paketen umbenannt, stand die alte bisher weiter neben der neuen. Mit `--ersetzen` wandert
alles aus der Sammlung, was nicht mehr in der Konfiguration steht, in den Papierkorb von
Metabase — wiederherstellbar. Endgültig löscht erst, wer den Papierkorb leert.

Außerdem beschrieben: das Format der `dashboard.json` und welche Rechte der API-Schlüssel
braucht. Ein Schlüssel, der nur Ergebnisse lesen darf, reicht für den Import nicht.

**Alle neun Anleitungen beginnen jetzt mit `tomedo-`.** Die Anleitung für den Kartei-Chat hieß
bisher als einzige nur `karteichat-prompting`; sie heißt jetzt `tomedo-karteichat-prompting`.
Am Inhalt ändert das nichts. **Wer die alte Fassung installiert hat, entfernt sie**, bevor er die
neue einspielt — sonst liegen zwei Anleitungen mit denselben Auslösern nebeneinander, und es ist
Zufall, welche die KI liest.

**Eine neue Fassung einspielen heißt: einfach hochladen.** Claude ersetzt den Skill gleichen
Namens und führt die Fassungen; vorher löschen ist nicht nötig. Das steht jetzt im Handbuch
und auf der Downloadseite, zusammen mit den zwei Ausnahmen: ein umbenannter Skill, und eine
selbst ausgefüllte `praxis-interna.md`, die vor dem Hochladen ins neue ZIP gehört.

**Im Handbuch heißt es jetzt durchgehend Skill statt Anleitung.** Teil 3 erklärt den Begriff
einmal: Ein Skill gibt der KI eine Fähigkeit samt Regeln und Nachschlagewerk, nicht nur eine
Gebrauchsanweisung.

| Anleitung | Version | geändert |
|---|---|---|
| tomedo-metabase-migration | 1.3 | Import-Skript im Skill, `--ersetzen`, Format der `dashboard.json`, Schlüsselrechte, getestet gegen v0.63.3 |
| tomedo-karteichat-prompting | 1.4 | umbenannt, vorher `karteichat-prompting`; Inhalt unverändert |
| Handbuch | — | Begriff Skill statt Anleitung, Hinweis zum Einspielen neuer Fassungen, Verweise auf den neuen Namen |
| fünf weitere Skills | unverändert | Verweise auf den neuen Namen nachgezogen |
| übrige zwei | unverändert | — |

**Nachtrag, 25.09.:** Die leere Vorlage der `praxis-interna.md` in `tomedo-statistik-hql` und
`tomedo-karteichat-prompting` sagte noch, eine neue Fassung überschreibe die eigene Erhebung
nicht, solange man die Datei stehen lasse. Das stimmt nicht: Beim Hochladen wird der ganze Skill
ersetzt. Die Vorlage sagt jetzt, dass die eigene Fassung vor dem Hochladen ins neue ZIP gehört.

## 1.7 — 2026-09-22

Die Kommando-Inventur des Kartei-Chats wurde ein zweites Mal gefahren — einmal vor und einmal
nach einer Nachpflege derselben Akte. Das Ergebnispaar ist der eigentliche Befund:

**Vorher konnte der Chat mit zwei Datenklassen arbeiten, nachher mit fünf.** Verordnungen kamen
danach mit PZN, Dosis und Einnahmeschema zurück, die Arbeitsunfähigkeit mit Zeitraum und ICD,
die Freitextdiagnosen ebenfalls. Am Chat hatte sich nichts geändert, nur an der Dokumentation.
**Briefkommandos koppeln den KI-Nutzen unmittelbar an die Dokumentationsdisziplin** — kein
Prompt-Kniff ersetzt einen gepflegten Datensatz.

Damit wird ein Beispiel der Fassung 1.2 hinfällig: Dort stand, der gefährlichste Fall sei ein
Kommando, das einen gültigen Wert 0 liefert. Das war an der ungepflegten Akte gemessen. Der
bleibende Befund ist schärfer — `medikamentenplan` und `med` bleiben leer, **obwohl die
Prüfabfrage `medikamenteVorhanden` den Wert 1 meldet**, weil tabellenerzeugende Kommandos im
Chat-Eingabefeld nicht auflösen. Der Umweg führt über den Karteieintragstyp.

**Neu ist der stärkste bisher gemessene Baustein: der Medikationsabgleich.** Verordnung gegen
Karteitext, zwei Anker plus Prüfauftrag, 20 Läufe. Die Dosisabweichung fand er 20 von 20 Mal,
Handelsname und Wirkstoffname setzte er in 18 von 20 Fällen gleich — das leistet kein
Zeichenvergleich. Kein erfundenes Präparat. Die Grenze gehört dazu: In 4 von 20 Läufen führte
er ein abgesetztes Präparat als aktuelle Medikation mit auf, weil der Absetz-Eintrag noch im
Ankerfenster lag.

Ebenfalls neu: **`invTime` wählt vom Aktenanfang, nicht vom Ende.** Bei kleinen Anzahlen
verankert der Flag damit die ältesten statt der neuesten Einträge — und die Antwort sieht in
beiden Fällen gleich souverän aus.

| Anleitung | Version | geändert |
|---|---|---|
| karteichat-prompting | 1.3 | zweite Inventur, Medikationsabgleich, `invTime`-Falle, Tabellenkommando-Grenze, Belegbasis auf 376 Läufe, Checkliste von 26 auf 28 Punkte |
| Handbuch | — | Teil 1 Kapitel 5 um den Pflege-Befund ergänzt |
| übrige acht | unverändert | — |

## 1.6 — 2026-09-22

Der Rückstand aus Fassung 1.1 ist abgearbeitet. Damals wurden Erkenntnisse aus 41
Arbeitssitzungen eingearbeitet, ohne die Angaben der eigenen Installation herauszutrennen; seither
stand in `tomedo-statistik-hql` Vokabular und Zahlenmaterial einer konkreten Praxis.

**Der wichtigste Teil betrifft nicht Kürzel, sondern Personen.** Eine Tabelle listete, wie viele
Abwesenheitstage auf welchen Sperrgrund entfallen und wie viele Mitarbeiter betroffen sind — bei
22 Beschäftigten sind „86 Tage bei einem Mitarbeiter" und „Elternzeit, zwei Mitarbeiter" Aussagen
über identifizierbare Personen, im ersten Fall eine über ihren Gesundheitszustand. Solche
Zählungen stehen jetzt ausschließlich in der nicht ausgelieferten Praxis-Datei. Im Text steht
stattdessen die Abfrage, mit der jede Praxis den eigenen Wert ermittelt — mit dem ausdrücklichen
Hinweis, dass das Ergebnis personenbezogen ist und lokal bleibt.

Ebenso ausgelagert: der Terminartkatalog mit Bezeichnungen und Fallzahlen sowie die Auflösung
praxiseigener Abkürzungen. Die **Lehren** bleiben vollständig im Text und sind teilweise besser
als vorher — etwa, dass ein Terminart-Kürzel doppelt existiert (einmal als Kapazitätsblock,
einmal als buchbarer Termin) und ein Filter ohne `infotermin` die Fallzahl vervielfacht. Das gilt
überall; nur die konkreten Kürzel und Zahlen waren praxisspezifisch.

**Neu ist eine Prüfregel, die diesen Rückstand künftig verhindert.** Sie schlägt bei
praxiseigenen Kalender- und Terminartbezeichnungen an. Die verbliebenen zehn Stellen sind einzeln
begründet freigegeben — dort trägt das Kürzel eine Lehre, die mit einem Platzhalter nicht mehr
nachvollziehbar wäre. Jede **neue** Stelle bricht den Bau ab. Damit ist der Rückstand keine Liste
in einer Datei mehr, die niemand liest, sondern eine Prüfung.

Nicht erfasst wird die bloße Nennung des Fremdprodukts ArZeKo — ein Produktname ist kein
Praxisbezug.

| Anleitung | Version | geändert |
|---|---|---|
| tomedo-statistik-hql | 1.6 | Abwesenheits- und Terminartzahlen ausgelagert, Ermittlungsabfragen ergänzt, Prüfregel für Organisationsvokabular |
| tomedo-kommandos · tomedo-metabase-migration | unverändert | je ein Beispielwert generisch gefasst |
| übrige sechs | unverändert | — |

## 1.5 — 2026-09-22

Nachtrag desselben Tages. Die Prüfung, die vor jeder Auslieferung läuft, hat einen
Praxisbezug in `tomedo-statistik-hql` durchgelassen — die Domäne des Fragebogen-Alt-Systems
stand in `datenmodell.md` und `query-bausteine.md` im Klartext und ist damit auch in Fassung
1.4 ausgeliefert worden. Wer 1.4 heruntergeladen hat, kann die beiden Dateien ersetzen oder
gleich die neue Fassung ziehen; ein Handeln ist nicht nötig, es handelt sich um eine Adresse
der herausgebenden Praxis, nicht um Patientendaten.

**Ursache war die Schreibweise.** Die Prüfregel für den Praxisnamen suchte nach
Großbuchstaben. Die Domäne ist klein geschrieben und fiel deshalb durch. Die Regel läuft
jetzt unabhängig von Groß- und Kleinschreibung, ebenso die Regeln für Ortsangaben,
Personennamen und praxiseigene Typkürzel. Bei derselben Gelegenheit ist eine zweite Lücke
derselben Regel geschlossen worden: Sie grenzte den Namen mit einer Wortgrenze ab und übersah
deshalb `SZDD_Praxiskontext.md`, weil ein Unterstrich für den regulären Ausdruck als
Wortzeichen zählt.

Zwei Regeln bleiben bewusst auf Großschreibung beschränkt: „AGILE" und der Verdacht auf
unbekannte Dreibuchstaben-Kürzel — klein geschrieben sind das gewöhnliche Wörter, und die
Prüfung würde in Fehltreffern ersticken. Am geprüften Bestand erzeugen die Verschärfungen
zusammen **keinen einzigen zusätzlichen Fehltreffer**.

**Im Text steht jetzt ein Platzhalter.** `<FRAGEBOGEN-DOMAENE>` ersetzt die Adresse an allen acht
Stellen in zwei Dateien, auch in den SQL-Beispielen. Dort ist zu beachten, dass die Domäne im Regex
mit escapten Punkten eingesetzt wird — `fragebogen\.example\.de`. Wo der Wert einzutragen
ist, steht in `references/praxis-interna.md` Abschnitt 1; die ausgelieferte Vorlage führt die
Zeile jetzt mit.

**Das Handbuch spricht in Teil 1 jetzt Klartext.** Der Abschnitt zum Kartei-Chat war stellenweise
für Entwickler geschrieben — „Betriebsart", „Verankerung", „Fehlerklasse", dazu eine rohe
Kommandozeile. Teil 1 lesen aber Ärzte und Praxispersonal. Der Abschnitt sagt jetzt zuerst, was
zu tun und zu lassen ist, und das Warum in einem Satz. Die Messzahlen bleiben in Teil 2, wo sie
hingehören.

| Anleitung | Version | geändert |
|---|---|---|
| tomedo-statistik-hql | 1.5 | Domäne des Fragebogen-Alt-Systems durch `<FRAGEBOGEN-DOMAENE>` ersetzt, Auflösung in `praxis-interna.md` §1 |
| Handbuch | — | Teil 1 Kapitel 5 in Klartext neu gefasst |
| übrige acht | unverändert | — |

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
