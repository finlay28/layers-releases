# Layers für macOS

### Eine App. Mehrere Identitäten.

Layers verwaltet getrennte lokale Profile für unterstützte Mac-Apps. So kannst du persönliche, geschäftliche und kundenspezifische Konten auseinanderhalten und mehrere passende Profile gleichzeitig öffnen.

**[Layers 0.1.4 herunterladen](https://github.com/finlay28/layers-releases/releases/download/v0.1.4-5/Layers-0.1.4-5.dmg)** · [Alle Versionen](https://github.com/finlay28/layers-releases/releases) · **[English](README.md)**

**Ab macOS 14.0 · Apple Silicon und Intel · Mit Developer ID signiert und von Apple notarisiert**

Dieses Repository enthält die offiziellen Downloads und die Dokumentation. Der Quellcode von Layers ist proprietär und wird hier nicht veröffentlicht.

## Inhalt

- [Was ist Layers?](#was-ist-layers)
- [Für wen ist Layers gedacht?](#für-wen-ist-layers-gedacht)
- [Was ein Layer tatsächlich trennt](#was-ein-layer-tatsächlich-trennt)
- [Unterstützte Anwendungen](#unterstützte-anwendungen)
- [Systemvoraussetzungen](#systemvoraussetzungen)
- [Installation und erster Layer](#installation-und-erster-layer)
- [Layers im Alltag](#layers-im-alltag)
- [Datenschutz, Sicherheit und Speicherorte](#datenschutz-sicherheit-und-speicherorte)
- [Updates und häufige Probleme](#updates-und-häufige-probleme)
- [Fragen und Antworten](#fragen-und-antworten)

## Was ist Layers?

Viele Desktop-Apps öffnen normalerweise einen einzelnen Arbeitsbereich oder ein einzelnes Konto. Wer zwischen privaten Aufgaben, dem Arbeitgeber und mehreren Kunden wechselt, muss sich möglicherweise ständig ab- und anmelden und aufpassen, im richtigen Kontext zu arbeiten.

Layers ergänzt für kompatible Anwendungen eine lokale Profilverwaltung. Du erstellst ein benanntes Profil, gibst ihm ein Symbol und eine Akzentfarbe und öffnest anschließend diese Identität. Unterstützte Profile können parallel laufen.

Ein möglicher Aufbau:

| Profil | Verwendung |
|---|---|
| Claude Privat | Persönliches Konto und private Aufgaben |
| Claude Arbeit | Geschäftlicher Account |
| Browser Kunde A | Web-Accounts eines Kunden in einem eigenen Browserprofil |
| Browser Kunde B | Davon getrennte Konten eines anderen Kunden |
| Editor Projekt A | Eigene Editor-Einstellungen und Erweiterungen für ein Projekt |

Die Namen sind Beispiele. Layers erstellt keine Benutzerkonten und vergibt keine Abonnements. Du meldest dich innerhalb der jeweiligen Anwendung selbst beim gewünschten Konto an.

## Für wen ist Layers gedacht?

- **Freelancer und Agenturen:** Kundenkontexte übersichtlich benennen und getrennt öffnen.
- **Menschen mit privaten und geschäftlichen Accounts:** Die passende Identität starten, ohne ständig das Konto zu wechseln.
- **Entwickler und Tester:** Eigene Browser- oder Editor-Konfigurationen für unterschiedliche Projekte verwenden.
- **Teams mit mehreren Organisationen:** Mehrere passende Arbeitskontexte parallel nutzen, sofern die Anwendung getrennte Profile unterstützt.

Layers organisiert den lokalen Anwendungskontext. Anmeldungen, Zwei-Faktor-Prüfungen, Abonnements, Kontolimits und Nutzungsbedingungen des jeweiligen Anbieters gelten weiterhin.

## Was ein Layer tatsächlich trennt

Jeder Layer erhält eine dauerhafte UUID und einen eigenen Ordner:

```text
~/Library/Application Support/Layers/Profiles/<Profil-UUID>/
```

Ein anwendungsspezifischer Adapter weist die App an, ihren jeweiligen Datenpfad zu verwenden. Je nach Anwendung geschieht das über eine passende Umgebungsvariable, einen Startparameter für den Benutzerdatenordner oder getrennte Verzeichnisse für Editor-Einstellungen und Erweiterungen.

Die Original-App behält ihren normalen Speicherort. Neue Layer übernehmen die bestehende Anmeldung nicht automatisch. Sie beginnen üblicherweise mit einem frischen lokalen Kontext, in dem du dich beim gewünschten Konto anmeldest.

### Die Profile bleiben ohne laufenden Manager bestehen

Wenn du Layers beendest, bleiben die Profilordner erhalten. Bereits gestartete Anwendungen werden dadurch nicht automatisch geschlossen. Aktuelle benannte Launcher enthalten ihre Startkonfiguration selbst und können den passenden Layer direkt aus Finder, Dock oder Spotlight öffnen.

Layers dient zum Erstellen, Anzeigen und Verwalten der Profile. Es muss keinen Hintergrunddienst ausführen, um deren Ordner am Leben zu halten.

### Anwendungsprofile innerhalb deines macOS-Benutzers

Ein Layer ist ein separates Anwendungsprofil unter demselben macOS-Benutzer. Es entsteht keine virtuelle Maschine, kein eigenes Betriebssystem und keine Sicherheitsgrenze gegenüber anderer Software mit deinen Benutzerrechten. Layers verspricht keine eigene IP-Adresse, keine getrennte Geräteidentität und keine vollständige Trennung sämtlicher Systemressourcen eines Drittanbieters. Eine Anwendung kann weiterhin beispielsweise macOS Keychain verwenden.

## Unterstützte Anwendungen

Eine gefundene App ist nicht automatisch kompatibel. Für die Profiltrennung braucht Layers einen passenden Adapter.

| Anwendung oder Familie | Verfahren | Geltungsbereich |
|---|---|---|
| Claude Desktop | `CLAUDE_USER_DATA_DIR` und eine eigenständig identifizierte Profil-App | Eigener Anwendungsdatenpfad; Freigaben der App oder des Schlüsselbunds können nötig sein |
| Kompatible ChatGPT-Desktop-Installationen | Eigener Benutzerdatenordner und `CODEX_HOME` | Nur die vom ausgelieferten Adapter erkannten Desktop-Builds |
| Google Chrome, Chromium, Brave | Eigener `--user-data-dir` | Getrennte Browserprofildaten |
| Visual Studio Code, Cursor | Eigene Benutzerdaten- und Erweiterungsordner | Getrennte Editor-Einstellungen und Erweiterungsdaten |
| Kompatible Codex-Installationen | `CODEX_HOME` | Abhängig davon, ob die installierte Version diese Variable berücksichtigt |

Der aktuelle ChatGPT-spezifische Adapter erkennt die Bundle-ID `com.openai.codex` zusammen mit der ausführbaren Datei `ChatGPT`. Das bedeutet ausdrücklich nicht, dass jede unter dem Namen ChatGPT veröffentlichte App kompatibel ist. Entscheidend ist die installierte Variante.

Anwendungen ohne passenden Adapter werden als nicht unterstützt angezeigt. Für die native WhatsApp-App gibt es derzeit beispielsweise keine unterstützte Layer-Integration. Ein gemeinsam verwendeter Account-Kontext wird nicht stillschweigend kopiert und als getrennt bezeichnet.

Updates eines Drittanbieters können das Profilverhalten ändern. Die Kompatibilitätsanzeige in Layers und die konkrete App-Version sind deshalb wichtiger als die Annahme, dass jede Version einer Produktfamilie identisch funktioniert.

## Systemvoraussetzungen

| Voraussetzung | Aktuelle Version |
|---|---|
| macOS | **14.0 oder neuer**; macOS 14 heißt Sonoma |
| Prozessor | Universal-App für Apple Silicon und Intel |
| Verteilung | Mit Developer ID signiert und von Apple notarisiert |
| Drittanbieter-App | Muss in einer kompatiblen Version installiert sein und ihre eigenen Systemvoraussetzungen erfüllen |
| Konto | Dein bestehender Account und gegebenenfalls ein erforderliches Abonnement beim Drittanbieter |

Die Mindestversion von Layers senkt nicht die Anforderungen anderer Anwendungen. Eine Drittanbieter-App kann ein neueres macOS oder einen bestimmten Prozessor voraussetzen, obwohl Layers selbst auf deinem Mac läuft.

Das macOS-14-Ziel wird für beide Architekturen durch Kompilierung und API-Verfügbarkeitsprüfung kontrolliert. Die automatisierten Laufzeittests laufen auf dem Entwicklungs-Mac; das ist keine vollständige Prüfung jeder älteren macOS- und App-Kombination. macOS 13 und älter werden derzeit nicht unterstützt. Dafür wären Änderungen an Zustandsverwaltung und Oberfläche nötig, nicht nur eine niedrigere Versionsnummer.

Neuere visuelle Effekte werden nur auf dafür geeigneten macOS-Versionen aktiviert. Auf älteren unterstützten Systemen verwendet Layers die vorhandene materialbasierte Darstellung.

## Installation und erster Layer

1. Lade die aktuelle **DMG-Datei** über den Link oben oder die [Releases-Seite](https://github.com/finlay28/layers-releases/releases) herunter.
2. Öffne den Installer und ziehe **Layers** nach **Programme / Applications**.
3. Starte Layers aus Programme. macOS kann weiterhin die normale Bestätigung beim ersten Öffnen einer heruntergeladenen App anzeigen.
4. Wähle, ob Layers im Dock oder in der Menüleiste erscheinen soll.
5. Öffne **Apps** und wähle eine kompatible installierte Anwendung.
6. Klicke auf **Add layer**. Lege Name, Symbol und Akzentfarbe fest und erstelle bei Bedarf eine separat benannte Dock-App.
7. Erstelle und öffne den Layer. Melde dich darin beim gewünschten Drittanbieter-Konto an.

Für einen weiteren Account erstellst du einen weiteren Layer mit einem anderen Namen. Du musst dich dafür nicht aus der Original-App abmelden.

## Layers im Alltag

### Home, Layers und Apps

- **Home** zeigt aktive verwaltete Profile und zuletzt verwendete Identitäten.
- **Layers** gruppiert die gespeicherten Profile je Anwendung und enthält auch einen Original/Main-Eintrag.
- **Apps** findet installierte Programme und zeigt ihre Kompatibilität mit den vorhandenen Adaptern.
- **Settings** enthält Darstellung, Start bei Anmeldung, Updates, App-Erkennung und Launcher-Einstellungen.

Ein Fenster der gewöhnlichen App ist nicht automatisch ein laufender erzeugter Layer. Claude Main kann beispielsweise geöffnet sein, während Claude Arbeit geschlossen ist. Layers unterscheidet die Original-App von benannten Profilen und zählt nicht jedes Fenster als eigene Identität.

### Benannte Dock-Apps

Die erzeugten Apps liegen unter `~/Applications/Layers`. Du kannst sie ins Dock ziehen oder über Spotlight finden. Ihre eingebettete Startkonfiguration wählt denselben Datenordner, unabhängig davon, ob du sie über Layers oder direkt öffnest.

Für Claude und die unterstützte ChatGPT-Variante werden lokale Profil-App-Bundles abgeleitet. Andere Adapter können einen schlanken Launcher verwenden, der die weiterhin installierte Original-App benötigt. Veränderte Drittanbieter-Profil-Apps werden lokal neu signiert. Die Notarisierung des Layers-Managers notarisiert diese veränderten App-Bundles nicht automatisch mit.

### Laufstatus, Beenden und Neubauen

Layers gleicht die tatsächlichen macOS-Prozesse und dauerhaften Launcher-Einträge ab. Dazu gehören auch direkt gestartete Profile. Ein Neustart des Managers soll einen laufenden Layer nicht aus der Übersicht verschwinden lassen.

Beim Beenden eines Profils wird die Anwendung zum Schließen aufgefordert. Ungespeicherte Arbeit kann weiterhin deine Aufmerksamkeit erfordern. Das Schließen von Layers selbst lässt die Drittanbieter-Anwendungen weiterlaufen.

Beim Neubauen wird der Launcher ersetzt, während der Profilordner erhalten bleibt. Laufende ältere Launcher werden erst nach ihrem Ende umgestellt. Schlägt der Neubau fehl, bleibt der bisherige Launcher verfügbar.

### Löschen und Sicherungen

Das Löschen eines Layers kann dessen lokale Sitzungen, Einstellungen und Dateien dauerhaft entfernen. Beende ihn vorher und lies die Bestätigung sorgfältig. Die Original-App kann nicht über die Layer-Löschfunktion entfernt werden.

Sichere wichtige Profildaten. Nur die Layers-App zu entfernen ist etwas anderes als die Profilordner zu löschen. Das Entfernen von `~/Library/Application Support/Layers` löscht die dort gespeicherten Metadaten und verwalteten Profildaten. Dieser Ordner muss für ein normales Update nicht entfernt werden.

## Datenschutz, Sicherheit und Speicherorte

| Ort | Inhalt |
|---|---|
| `~/Library/Application Support/Layers/profiles.json` | Namen, IDs, zugehörige Apps, Darstellung und Launcher-Verweise |
| `~/Library/Application Support/Layers/profiles.backup.json` | Wiederherstellungskopie der Profilmetadaten |
| `~/Library/Application Support/Layers/Profiles/` | Dauerhafte Anwendungsdaten je Profil |
| `~/Applications/Layers/` | Erzeugte Profil-Apps und benannte Launcher |

Layers untersucht und migriert keine Passwörter, Account-Cookies, Authentifizierungstoken oder Schlüsselbundinhalte. Drittanbieter-Apps können ihre eigenen Kontodaten in den zugewiesenen Profilordnern speichern und ihre eigene Keychain-Anbindung verwenden. Ihre Netzwerkaktivität unterliegt weiterhin den Regeln ihrer Anbieter.

Layers enthält keine eingebaute Analyseplattform, kein Werbe-SDK und kein Layers-Cloud-Konto. Update-Prüfungen und Downloads kontaktieren GitHub über normale Netzwerkanfragen. Dabei erhält die Hosting-Infrastruktur übliche Verbindungsmetadaten. Automatische Update-Prüfungen lassen sich in Settings ausschalten; eine manuelle Prüfung stellt weiterhin eine Netzwerkverbindung her.

Die offizielle Layers-App und der Installer sind signiert und notarisiert. Update-Archive tragen zusätzlich eine Sparkle-EdDSA-Signatur. Die Notarisierung ist Apples Schadsoftwareprüfung und keine Garantie für Fehlerfreiheit oder eine automatische Freigabe jeder erzeugten Drittanbieter-App.

## Updates und häufige Probleme

### Aktualisieren

Verwende **Settings → Updates → Check for updates**. Der Update-Kanal und die Downloads liegen auf GitHub. Ein Update ersetzt den Manager und behält die Profilordner bei.

Version 0.1.0 hatte noch keinen konfigurierten Update-Kanal und braucht einmalig eine manuelle Installation einer aktuellen Version. Builds mit eingerichtetem Kanal können spätere kompatible Releases finden. Der Feed enthält die Mindestversion des Betriebssystems, damit einem älteren Mac nicht gezielt ein inkompatibler Build angeboten wird.

### Typische Situationen

| Situation | Was du prüfen kannst |
|---|---|
| Eine App erscheint, aber Add layer ist nicht verfügbar | App-Erkennung und Kompatibilität sind verschieden; möglicherweise fehlt ein Adapter für diese Variante |
| Ein neuer Layer verlangt eine Anmeldung | Frische Profile kopieren die Anmeldung der Original-App nicht |
| macOS fragt nach Schlüsselbundzugriff | Prüfe die Anfrage der Drittanbieter-App; veränderte Profil-Apps können eine Freigabe benötigen |
| Ein benannter Layer wird nicht als aktiv angezeigt | Prüfe, ob du tatsächlich diesen Layer oder die Original-App geöffnet hast |
| Ein Launcher startet nach dem Verschieben der Original-App nicht | Stelle die passende Original-App wieder her oder baue den Launcher über Layers neu |
| Eine Profil-App muss neu gebaut werden | Beende sie zuerst und behalte ihren Profilordner |
| Ein Update erscheint nicht sofort | Prüfe deine macOS-Version und gib GitHubs öffentlichem Feed-Cache Zeit zur Aktualisierung |
| Nach einem Drittanbieter-Update ändert sich das Verhalten | Notiere beide App-Versionen und die genauen Schritte für einen Fehlerbericht |

## Fragen und Antworten

**Kann ich mehrere unterschiedliche Accounts gleichzeitig nutzen?**  
Das ist der Zweck kompatibler Layer-Profile: getrennte lokale Kontexte, die parallel laufen können. Anmeldung, Abonnements und Kontobeschränkungen kontrolliert weiterhin der jeweilige Dienst. Nicht jede Drittanbieter-Version wurde mit zwei tatsächlich angemeldeten Accounts getestet.

**Verschwinden meine Profile, wenn Layers geschlossen ist?**  
Nein. Die Ordner bleiben erhalten, und aktuelle benannte Launcher enthalten ihre Startkonfiguration selbst. Du kannst den Manager später wieder öffnen, um die Profile zu verwalten.

**Wird mein bereits angemeldeter Account in jeden neuen Layer kopiert?**  
Nein. Bestehende Zugangsdaten werden nicht migriert. Melde dich im neuen Profil beim gewünschten Account an.

**Ist ein Layer eine virtuelle Maschine oder ein eigener macOS-Benutzer?**  
Nein. Es handelt sich um getrennte Anwendungsprofile innerhalb deines bestehenden macOS-Benutzers.

**Kann ich jede beliebige App hinzufügen?**  
Nur Apps mit einem geeigneten Profilverfahren können als Layer unterstützt werden. Anwendungen ohne ausgelieferten Adapter bleiben als nicht unterstützt gekennzeichnet.

**Funktioniert Layers offline?**  
Die Profilverwaltung speichert ihre Konfiguration lokal. Ob eine geöffnete App offline funktioniert, entscheidet deren Anbieter. Update-Prüfungen und Downloads benötigen eine Verbindung.

**Erhalte ich damit zusätzliche Abonnements oder umgehe Dienstlimits?**  
Nein. Du benötigst die passenden Konten und Abonnements und musst die Bedingungen des jeweiligen Dienstes einhalten.

**Ist Layers Open Source, weil es auf GitHub liegt?**  
Nein. Dieses Repository enthält Downloads und Dokumentation. Layers bleibt proprietär; siehe [LICENSE](LICENSE).

## Probleme melden

Nutze die [Issues-Seite](https://github.com/finlay28/layers-releases/issues). Nenne macOS-Version, Mac-Prozessor, Layers-Version, Drittanbieter-App samt Version, genaue Schritte und erwartetes sowie tatsächliches Verhalten.

Hänge keine Profilordner, Cookies, Passwörter, Token oder privaten Account-Screenshots an. Ein Prozess- oder Launcher-Problem lässt sich beschreiben, ohne den Inhalt eines Accounts offenzulegen.

## Produkt und Lizenz

Layers ist ein Produkt von **Falah Agency, Finlay Sippel**. Drittanbieter-Anwendungen und ihre Namen gehören ihren jeweiligen Rechteinhabern. Kompatibilität bedeutet keine offizielle Partnerschaft oder Empfehlung.

Aktuelle Version: **0.1.4 (5)** · [Release-Hinweise](https://github.com/finlay28/layers-releases/releases/tag/v0.1.4-5) · [Update-Feed](https://raw.githubusercontent.com/finlay28/layers-releases/main/releases/appcast.xml)

© 2026 Layers. Alle Rechte vorbehalten.
