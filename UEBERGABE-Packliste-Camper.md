# Übergabedokumentation: Packlisten- & Camper-App (Ulrich)

Diese Doku fasst den Stand von zwei zusammengehörigen Tools zusammen, damit die Arbeit in einem neuen Chat oder in Cowork nahtlos weitergeht.

---

## 1. Überblick & Ziel

Ulrich baut zwei standalone HTML-Apps (PWA-fähig, self-hosted über GitHub Pages) rund um Reisen mit einem Camper:

1. **Packlisten-App** (`packliste.html`) – filtert Items beim Packen nach Reisetyp, Ort, Person; hakt Gepacktes ab.
2. **Camper-Grundriss-App** (`vanderer-grundriss.html`) – interaktiver Grundriss des Campers mit Inventar pro Zone ("Wo ist was?").

Beide sind bewusst als **einzelne HTML-Dateien** gebaut (kein Build-Prozess, kein Framework), damit sie leicht auf GitHub Pages laufen und auf dem iPhone als PWA installierbar sind (via Safari → "Zum Home-Bildschirm").

**Wichtig zum Arbeitsstil von Ulrich:**
- Iterativer Ansatz: einfach starten, dann anhand echter Nutzung erweitern.
- Eingaben oft per Sprache → gelegentliche Transkriptionsfehler, im Zweifel nachfragen statt raten.
- Bevorzugt visuelle/grafische Darstellung, weil reiner Text für Raum-Layouts schwierig ist.
- Grundriss-Genauigkeit kam **nur über hochgeladene Fotos + Handskizze** zustande – Web-Recherche allein reichte nicht.

---

## 2. Fahrzeug: VANDerer Citroën Berlingo

Ausbau der Firma VANDerer mit **Livingroom-Modul + Aufstelldach**.

**Maße:**
- Fahrzeug: 4,75 m lang, 1,85 m breit, 1,89 m hoch
- Aufstelldach-Bett: 1,14 × 1,90 m
- Liegefläche unten: 1,20 × 2,00 m
- Frisch-/Abwasser: je 12 L
- Kühlbox (VANDerer, in Mittelkonsole): 14 L

**Grundriss-Layout (Draufsicht, Fahrtrichtung = oben, Heckklappe = unten):**

Vorne: Fahrersitz (links, Ulrich) · Kühlbox (Mitte) · Beifahrersitz (rechts, Kristin)
Dann: Rücksitzbank (3 Sitze, klappt nach hinten → wird zur Lounge)

Laderaum, von vorne nach hinten:
- **Lounge** – volle Breite, Sitzpolster oben, = umgeklappte Rückbank
- **Gang** – läuft LÄNGS mittig von Lounge bis Heckklappe (durchgehend)
- **Fahrerseite (links), von vorne nach hinten:** Bank (aufklappbar) → Spüle → Kanister (Abw./Frisch, ganz hinten)
- **Beifahrerseite (rechts), von vorne nach hinten:** Küchenzeile (mit integrierter Schublade + 2 Ceranplatten ganz rechts) → Auszug (ganz hinten)
- **Seitentaschen** außen an beiden Wänden, auf mittlerer Höhe
- **Heckklappe** unten über volle Breite

Wichtige Korrekturen, die schon eingearbeitet sind (nicht rückgängig machen):
- Küche ist RECHTS (Beifahrerseite), nicht links.
- Kochplatten liegen HINTEREINANDER (längs), ganz rechts hinten.
- Nur EINE Schublade, in die Küchenzeile integriert.
- Gang ist LÄNGS in der Mitte, nicht quer.
- Kühlbox ist vorhanden (VANDerer-Option in der Mittelkonsole).
- Kanister sitzen auf gleicher Höhe wie der Auszug (ganz hinten).

---

## 3. Camper-Inventar (Ist-Zustand pro Zone)

- **Fahrersitz / Vorderraum:** USB-C Ladekabel lang; Box für Items (hängt am Fahrersitz, Türfach unten)
- **Rücksitzbank (Fahrposition):** nur Funktionshinweis, klappt zur Lounge
- **Lounge (= Rückbank quer, umgeklappt):** Diverse Aufbewahrung; Zurrgurte/Spanngurte/Expander; schwarze Techniktasche (Ladekabel, Karabiner); Laken; 5× Verdunkelung; Hängematte; 2 Picknickdecken (gelb); große Plane (grau); Heringe (rot); Camping-Hammer; Campinglampe; Mehrfachsteckdose
- **Bank (links):** [noch Platz frei!] Kehrblech & Feger; Markisenset; Ladekabel Auto; 2 Helinox Stühle, 1 Helinox Tisch, 1 Hocker
- **Spüle (links):** Edelstahlspüle m. Seifenspender, elektr. Wasserpumpe
- **Kanister (links unten):** Abwasser 12 L, Frisch 12 L
- **Seitentasche Fahrerseite (links):** Geschirrtuch/Lappen/Schwamm; Gefriertüten; Toilettenpapier; Mülltüten; Gummis; gelber Keramikschwimmer; Wäscheklammern; Waschmittel; Stirnlampe; Textilerfrischer; Spüli
- **Seitentasche Beifahrerseite (rechts):** Tee (Pfefferminz/Guten-Morgen); Medizinbeutel/Verbandtasche (Pflaster, Erkältung, Salbe, Zecken, Karte, Mückenspray, Wundverband, Natron, Gallseife)
- **Küchenzeile (rechts):** Schublade mit Besteck für vier, Schere, Taschenmesser, Küchenmesser; 2 Ceranplatten
- **Auszug (rechts, hinten):** 4 Becher; 4 Müslischalen, 3 Klappschalen; 3 große + 4 kleine Teller; kleines Schneidebrett; Cafetera; Gaskocher (Außenkochen); Geschirr, Töpfe in offenen Fächern
- **Gang (Mitte):** 2× Schlafsäcke

---

## 4. Technischer Stand der Camper-App (`vanderer-grundriss.html`)

- Standalone HTML mit Inline-SVG-Grundriss (Draufsicht). Zonen sind `<g class="zone" onclick="showInfo('key')">`.
- **Inventar editierbar:** Tap auf Zone → "✏️ Bearbeiten" → Einträge ändern/löschen/hinzufügen → "✓ Fertig".
- **Persistenz:** `localStorage` unter Key `vanderer_inventory_v1`. `DEFAULT_INFO` hält Standardzustand; `loadInventory()` merged gespeicherte Items über Defaults.
- **Export/Import:** Toolbar oben. Export nutzt Web Share API (`navigator.share` mit File) → iOS-Teilen-Menü, Fallback = klassischer Download (Desktop). Import über File-Input. Reset stellt `DEFAULT_INFO` wieder her.
- Datei-Export heißt `vanderer-inventar.json`.
- **Frontbereich entfernt (Commit `5ef7da1`):** Motorhaube/Windschutzscheibe/Armaturenbrett-Zeichnung rausgenommen, Fahrer-/Beifahrersitz + Kühlbox rücken direkt an den oberen Rand. SVG-`viewBox` von `0 0 280 680` auf `0 0 280 572` verkleinert (alle Y-Koordinaten unterhalb der Sitzreihe um 108px nach oben verschoben), dazu Body-Padding/Überschriften-Abstände gestrafft – Skizze passt jetzt komplett ohne Scrollen auf einen iPhone-Screen. Kein Firebase-Sync für diese App, nur lokal.
- **Auspacken-Checkliste (Commit `77fd5cc`, 2026-08-09):** Neuer Abschnitt "🧺 Auspacken-Checkliste" unterhalb von "Maße", nach dem gleichen Muster wie das Camper-Inventar editierbar. Standard-Items: "Wasserbehälter ausgeleert?", "Gasherd draußen?", "Batterie Creabest aufgeladen?" – abhakbar (Checkbox, durchgestrichen wenn erledigt), löschbar, neue eigene Punkte über Eingabefeld + "+"-Button hinzufügbar. Link "Häkchen zurücksetzen" setzt alle Haken auf unchecked zurück (praktisch fürs nächste Mal), ohne die Liste selbst zu leeren. Persistenz: `localStorage` unter `vanderer_auspack_v1` (`DEFAULT_AUSPACK_ITEMS` als Fallback), analog zum bestehenden Inventar-Muster – kein Firebase-Sync. Live über Claude-in-Chrome verifiziert: Abhaken, neuer Punkt, Löschen, Reset – alles fehlerfrei.

## 5. Technischer Stand der Packlisten-App (`packliste.html`)

- Setup-Screen (Reisetyp + Personen + optional Ort) → Pack-Screen (Filter nach Ort/Kategorie/offen/bei-Abreise, Fortschrittsbalken, Suche) → Status-Screen (Fortschritt pro Ort/Person) → Liste-Screen (Items verwalten, Export/Import).
- Personen: Ulrich (default), Kristin, Paul, Moritz. Jede Person hat eine eigene abhakbare Kopie eines Items.
- Tag-Dimensionen: Ort (Keller, EG, OG1, OG2, Auto/Camper), Kategorie inkl. **Workation** und **Necessaire** (Küche, Kleidung, Technik, Sport, MTB, Outdoor, Camping, Baden, Spiele, Dokumente, Orga, Essen, Workation, Necessaire), Reisetyp inkl. **Workation** (Camping, MTB-Trip, Badeurlaub, Winterurlaub, Städtetrip, Alpin, Workation, Immer). Plus separates Boolean-Feld `beiAbreise` (kein Tag-Array, einzelner Toggle im Bearbeiten-Modal unter "Zeitpunkt") – markiert Items, die erst kurz vor Abreise packbar sind (z.B. Handy, Kühlschrank-Essen); Filter-Chip "🧳 Schon packbar" blendet sie aus.
- Persistenz: **Firebase Realtime Database ist die eigentliche Quelle der Wahrheit** (siehe Punkt 7.1), `localStorage` (Key `packliste_v2`) nur noch Offline-Fallback/Cache. `DEFAULT_ITEMS` im Code ist NUR der Startzustand für einen echten Reset/Neuinstallation – die tatsächlichen ~162 Items leben in Firebase und weichen inzwischen von `DEFAULT_ITEMS` ab. **Wichtig für künftige Änderungen an Items:** Neue Items oder Tag-Änderungen an bestehenden Items gehören direkt in die Live-Firebase-Daten (über die laufende App per `window.__packlisteCloud.getSnapshot().items`, dann `saveState()` aufrufen, siehe Workflow unten) – nicht (nur) in `DEFAULT_ITEMS` im Quellcode, sonst sieht Ulrich die Änderung nie.
- **Workflow, um Items in großer Zahl hinzuzufügen/zu taggen:** Über Claude-in-Chrome die Live-App öffnen (`packliste.html` direkt, nicht die Shell, damit `window.__packlisteCloud` im Top-Frame liegt), per `javascript_tool` `items.push(...)`/Tag-Änderungen machen, danach `saveState(); renderManageList(); renderPackList(); renderStats();` aufrufen (pusht automatisch in die Cloud). So wurden am 2026-07-12 auf einen Schlag 35 neue Items für "Workation" und "Bergtour/Hüttentour" (Reisetyp Alpin) ergänzt, plus 20+2 Items mit `beiAbreise:true` markiert. Dabei NICHT vorhandene Items duplizieren, sondern bestehende um fehlende Tags ergänzen (z.B. Wanderschuhe/Wanderstöcke/Äpfel etc. haben jetzt zusätzlich `Alpin` im `reise`-Array).
- Achtung: `confirm()`/`alert()` funktionieren in eingebetteten Umgebungen nicht (die App läuft jetzt sogar in einem iframe innerhalb der Shell!) → wurden entfernt/durch `toast()` ersetzt. Nicht wieder einbauen.
- **Cloud-Sync-Bugfix (Commits `5172aa2`, `a522f78`):** Details siehe Punkt 7.1 – wichtig: `normalizeItems()` sorgt dafür, dass jedes Item beim Laden/Import/Sync alle erwarteten Felder hat (auch `beiAbreise: false` als Default), das nicht wieder entfernen.
- **Filter "Nur Reisetyp" (Commit `c4db723`, 2026-08-09):** Löst folgendes Problem: Items mit `reise:['Immer']` (z.B. Ausweis, Ladekabel, Geldbeutel) erscheinen bisher IMMER in der Packliste, egal welcher Reisetyp gewählt ist – bei einem kurzen Ausflug (z.B. nur "Badeurlaub" für einen Nachmittag am See/Freibad) sind das aber unnötige Einträge. Neuer Filter-Chip "✅ Nur Reisetyp" im Pack-Screen (`state.hideImmerItems`, Funktion `toggleImmerFilter()`) blendet bei Aktivierung alle `Immer`-Items aus der Session aus – wirkt direkt in `getSessionItems()` (nicht nur als nachträglicher Sichtbarkeits-Filter wie "bei Abreise"), damit auch Fortschrittszähler und Ort-/Kategorie-Chips korrekt neu berechnet werden. Jederzeit an-/abschaltbar, nicht Teil der Session-Persistenz (wie `hideAbreiseItems` auch nicht). Live über Claude-in-Chrome verifiziert: bei reinem Reisetyp "Badeurlaub" sank die Artikelzahl von 40 auf 34 nach Aktivieren des Filters, keine Konsolenfehler.

---

## 6. Deployment (GitHub Pages)

- Repo: `ulricherlenmaier-ux` → Ordner/Repo `Packliste`.
- Live-URL: `https://ulricherlenmaier-ux.github.io/Packliste/` → liefert jetzt die **Shell** (`index.html`), die per iframe zwischen den drei Apps umschaltet (siehe Punkt 2 unten). Direkter Zugriff auf einzelne Apps auch möglich: `.../packliste.html`, `.../vanderer-grundriss.html`, `.../taschen.html`.
- **Git-Push funktioniert direkt aus der Cowork-Sandbox**, kein manueller Upload mehr nötig: Ulrich hat ein GitHub Personal Access Token (classic, `repo`-Scope, kurze Ablaufzeit) im Chat geteilt, damit klont/committet/pusht Claude direkt über `git` in der Sandbox (Repo lässt sich nicht in den gemounteten Cowork-Ordner klonen – dortiges Dateisystem blockiert `.git`-Lockfiles; Workaround: Klon nach `/tmp` in der Sandbox, von dort pushen). Token ist zeitlich befristet und müsste bei Bedarf neu erstellt/geteilt werden.
- Nach jedem Push kurz warten (~30–40s) bis GitHub Pages das Deployment gebaut hat (Tab "Actions" im Repo zeigt "pages build and deployment", sollte grün sein), dann erst live testen.
- PWA-Installation nur über **Safari** (nicht Chrome) auf iOS: Teilen → "Zum Home-Bildschirm". Wichtig: Das Home-Bildschirm-Icon hält eine eigene, von Safaris "Verlauf löschen" unabhängige Cache-Kopie – bei größeren Updates ggf. App-Wechsler → Icon wegwischen → neu öffnen, im Zweifel auch Icon neu anlegen.
- **Live-Verifikation**: Claude kann über die Claude-in-Chrome-Erweiterung (Chrome-Seitenleiste, muss vom User verbunden sein) die Live-Seite direkt öffnen, Konsole auf Fehler prüfen und Klicks simulieren – das ist der Standard-Workflow nach jeder Änderung, bevor sie als erledigt gilt. Sandbox-eigenes `curl` funktioniert NICHT für github.io/firebasedatabase.app (Netzwerk-Allowlist blockiert diese Domains), nur `github.com` direkt ist erreichbar (git-Operationen gehen darüber).

---

## 7. Offene Punkte / nächste Schritte

1. **iCloud-/Cloud-Speicherung (Hauptfrage von Ulrich) – ERLEDIGT (voll automatisch):**
   - `packliste.html` hat Export/Import-Buttons (Screen "Liste" oben rechts) als manuelles Backup, analog zur Camper-App (Commit `e7207b4`).
   - **Zusätzlich: echte Live-Synchronisierung über Firebase Realtime Database** (Commit `ce0d469`). Projekt `packliste-f1334`, anonyme Authentifizierung (kein Login-Screen), Daten liegen unter Pfad `packliste` als ein JSON-Blob (`{items, session}`). App liest beim Öffnen automatisch den aktuellen Cloud-Stand (Auto-Import) und hält ihn per Live-Listener synchron, wenn sich auf einem anderen Gerät etwas ändert. `localStorage` bleibt als Offline-Fallback aktiv, `saveState()` pusht bei jeder Änderung automatisch in die Cloud (Guard gegen Rückkopplungsschleife über `window.__packlisteCloud.applyingRemote`).
   - Security Rules (`auth != null` für read/write unter `packliste`) sind gesetzt und per Test (`.json`-Endpunkt liefert "Permission denied" ohne Auth) verifiziert.
   - **Bugfix (Commits `5172aa2`, `a522f78`):** Nach dem ersten Cloud-Sync stürzte `renderPackList()` ab, weil alte lokale Daten kein `orte`-Feld in `session` hatten bzw. manche Items Felder wie `ort`/`typ` nicht gesetzt hatten (undefined → `.length`/`.forEach` warf). Dadurch blieben "Packen" und "Status" leer und Cloud-Sync blockierte sich selbst (`applyingRemote` blieb `true` hängen). Behoben durch: defensives Merge von `session` (Object-Spread mit Defaults statt hartem Ersetzen) + neue `normalizeItems()`-Funktion, die jedes Item beim Laden/Import/Cloud-Sync auf vollständige Feldstruktur bringt + `try/finally` um `applyRemote()`. Live über Claude-in-Chrome verifiziert: Liste (57 Items unter Keller sichtbar, Export/Import-Buttons da), Packen (116/123 gepackt), Status (94 % Fortschritt, korrekte Aufschlüsselung nach Ort/Person) – alles fehlerfrei, echte Daten intakt.
   - Camper-App (`vanderer-grundriss.html`) hat den Firebase-Sync NICHT bekommen – nur die Packliste. Falls gewünscht, gleiches Muster übertragbar.
   - **iOS-Kurzbefehl (Level 2, optional, nicht mehr nötig für Auto-Sync):** Anleitung für einen Kurzbefehl, der den Export-Share ohne Speicherort-Auswahl in eine feste iCloud-Datei schreibt, wurde im Chat gegeben – rein als zusätzliches manuelles Backup interessant, nicht mehr das Hauptmittel.
   - Out-of-the-box-Alternativen (Notion/Apple Erinnerungen/Google Sheet) wurden mit Ulrich durchgesprochen – er hat sich bewusst für die Custom-App + Firebase entschieden, nicht für einen Wechsel des Tools.
2. **Packliste + Camper verbinden – ERLEDIGT (Commit `e94a754`):** Statt eines echten Zusammenführens in eine Datei (Kollisionsrisiko: beide Apps hatten gleichnamige globale Funktionen wie `toast()`/`exportJSON()`/`importJSON()` und eigene `:root`-Farbvariablen) gibt es jetzt eine schlanke Iframe-Shell.
   - Repo-/Ordnerstruktur: `index.html` = Shell mit Umschalter oben, inzwischen **drei** Tabs ("📦 Packliste" / "🚐 Camper" / "🎒 Taschen", siehe Punkt 3), bindet die einzelnen HTML-Dateien unverändert per iframe ein. Merkt sich die zuletzt genutzte App in `localStorage` (`shell_last_app`).
   - `packliste.html` ist jetzt der stabile Dateiname sowohl lokal als auch im Repo (vorher lief die Packliste im Repo unter `index.html` direkt – dieser Sonderfall ist damit aufgelöst, lokal und Repo haben ab jetzt identische Dateinamen).
   - `vanderer-grundriss.html` (Camper-App) wurde damit zum ersten Mal überhaupt ins GitHub-Repo gepusht/deployt – vorher existierte sie nur lokal.
   - Live über Claude-in-Chrome verifiziert: Umschalter funktioniert, beide Apps laden fehlerfrei in ihren iframes, keine Konsolenfehler.
   - Firebase-Sync bleibt auf die Packliste beschränkt; die Camper-App hat weiterhin nur lokale Speicherung (`localStorage` + manueller Export/Import). Falls gewünscht, gleiches Sync-Muster übertragbar.
3. **Taschen-Tab – ERLEDIGT (Commit `9e297bb`), aber anders umgesetzt als ursprünglich gedacht:** Statt einer Dimension INNERHALB der Packliste (mit Fotos) gibt es jetzt eine dritte, eigenständige App `taschen.html` als dritter Shell-Tab ("🎒 Taschen"), Muster wie die Camper-Zonen (Karten-Grid, Tap → editierbare Liste, lokal in `localStorage` unter `taschen_v1`, kein Firebase-Sync). Enthält: T1 Kleidung (North Face, blau), T2 Sport (Ikea, hellblau), T3 Schuhe (Ikea, dunkelblau), T9 Rucksack (erste Übernachtung), plus separater "Auto"-Bereich (Zieharmonika-Tasche, Boxen, Kühlschrank, Packsäcke, Trinkwasser voll machen). Fotos/Bilderkennung wurde nicht umgesetzt (Ulrich hat es nicht mehr angefragt, evtl. später).
4. **Backup-Strategie – ERLEDIGT:** Sofort-Backup am 2026-07-12 manuell über Firebase-Konsole (Export JSON) gezogen. Automatisches Backup bei jedem Cloud-Sync nach `packliste_backups/<Zeitstempel>` (max. 1×/Stunde pro Session, kein Pruning – Commit `5e6beba`) bleibt aktiv. Der separate Scheduled Task `weekly-packliste-backup-reminder` (wöchentliche Erinnerungsnachricht) wurde am 2026-07-26 auf Ulrichs Wunsch wieder gelöscht – die automatische Backup-on-Sync-Funktion reicht ihm, solange die App regelmäßig genutzt wird. SKILL.md-Prompt liegt zur Not noch unter `/Users/ulrich/Claude/Scheduled/weekly-packliste-backup-reminder/SKILL.md`, falls das Thema nochmal aufkommt. Empfehlung an künftige Sessions bleibt bestehen: **vor größeren Änderungen an der Datenstruktur zusätzlich manuell ein frisches Firebase-Export ziehen lassen**, nicht nur auf die automatischen Backups verlassen.
5. **Workation + Bergtour/Hüttentour-Inhalte – ERLEDIGT (2026-07-12):** Siehe Punkt 5 oben (Tag-Dimensionen) für Details. Bekannte Korrektur: "Laptop" (allgemeines Item, `e16`) ist NICHT mit Workation getaggt – das bezog sich auf "KfW Laptop" (`e20`), welches jetzt den Workation-Tag trägt.
6. **Offene Grenzfälle bei "bei Abreise"-Tagging (noch unbeantwortet von Ulrich):** Küchenrolle, Kindle, Marmelade/Pesto, Geschirrtücher/Lappen/Schwamm (EG), Fahrzeugschein, Kulturbeutel – Ulrich sollte gefragt werden, ob diese auch `beiAbreise:true` bekommen sollen, siehe Chatverlauf vom 2026-07-12 für die genaue Fragestellung.
7. **GitHub Personal Access Token läuft ab:** Token wurde mit kurzer Gültigkeit (auf Ulrichs Wunsch) erstellt. Falls Git-Push in einer neuen Session fehlschlägt ("could not read Username"/403), braucht es ein neues Token von Ulrich (github.com/settings/tokens/new, classic, Scope `repo`).
8. **Firebase-API-Key-Warnung von GitHub (Secret Scanning):** Ist ein bekannter Fehlalarm – Firebase-`apiKey` ist bewusst öffentlich, Schutz kommt über die Security Rules (siehe Punkt 7.1), nicht über Geheimhaltung des Keys. Falls der Alert erneut auftaucht: in GitHub unter Security → Secret scanning alerts → "Close as" → "False positive", nicht rotieren.
9. **Camper-Auspacken-Checkliste – ERLEDIGT (2026-08-09):** Siehe Punkt 4 oben für Details.
10. **Packliste-Filter "Nur Reisetyp" für Immer-Artikel – ERLEDIGT (2026-08-09):** Siehe Punkt 5 oben für Details.
11. **Neue Kategorie "Necessaire" – ERLEDIGT (2026-08-09, Commit `0c12016`):** Neue Kategorie (`typ`) für den Kulturbeutel-Inhalt: Zahnpasta/Duschgel (bestehendes Item `custom_1785082844863`, um den Tag `Necessaire` ergänzt statt dupliziert), Zahnbürste, Zahnseide, Haargel, Hautcreme, Deo, Nageletui (6 neue Items, IDs `necessaire_*`). Alle mit `reise:['Immer']`, `persons:['Ulrich']`, `beiAbreise:false`, `ort:['Keller']` (auf Wunsch nachträglich von OG1 auf Keller geändert, Commit direkt in Firebase, kein Code-Push nötig). `TYPEN`-Array im Quellcode um `'Necessaire'` erweitert (nötig, damit der Kategorie-Filter-Chip erscheint und die Kategorie im Bearbeiten-Modal auswählbar ist – Items direkt in Firebase zu taggen reicht dafür allein nicht) und steht dort bewusst an erster Stelle, damit der Chip in der Filter-Zeile direkt hinter den Ort-Chips (Keller/EG/OG1/OG2/Auto-Camper) erscheint, noch vor den übrigen Kategorien. Der Kulturbeutel selbst (`k4`, Kategorie "Orga") wurde NICHT mit umgetaggt, bleibt wie bisher. Bei diesem Schritt außerdem: GitHub-PAT war abgelaufen, neuer Token von Ulrich angefordert und erhalten (erster Versuch hatte falschen Scope/403, zweiter Token mit korrektem "repo"-Scope hat funktioniert).
12. **Filter-Chips beim Scrollen sichtbar (sticky) – ERLEDIGT (2026-08-09, Commit `d158266`):** Die Chip-Zeile (`#filter-row`) im Packen-Screen wurde vom `.header`-Div bisher als eigenständiges Geschwister-Element direkt darunter gerendert und scrollte dadurch mit der Item-Liste weg. Jetzt liegt `#filter-row` HTML-strukturell innerhalb von `.header` (nach der Fortschrittsleiste) und erbt damit dessen bereits vorhandenes `position: sticky; top: 0;` – Titel, Fortschrittsbalken und Filter-Chips bleiben so gemeinsam oben fixiert, während darunter nur noch die Item-Liste scrollt. `.filter-row` hat dafür `margin: 0 -16px; padding-left/right: 16px` bekommen (negatives Margin gleicht das Eltern-Padding des Headers aus, damit die horizontale Scroll-Chip-Leiste weiterhin randlos nutzbar ist). Live über Claude-in-Chrome verifiziert: Chip-Reihe bleibt beim Scrollen durch die Camping-Liste sichtbar, keine Konsolenfehler.

---

## 8. Referenz-Präferenzen (Reise, für Kontext)

- Prioritäten: Natur & Ruhe > Aktivität & Sport > neue Orte entdecken > Erholung & Komfort.
- Reist oft mit Frau Kristin (mag lebendige Städte/Kultur; fährt kein MTB, wandert aber gern). Ideale Trips kombinieren Natur/MTB + Stadt mit Charakter.
- Nächste geplante Reise: Sardinien (Camper). Könnte als eigener Reisetyp in der Packliste sinnvoll sein.
