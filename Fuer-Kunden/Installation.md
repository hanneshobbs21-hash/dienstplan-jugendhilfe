# Dienstplan installieren

Diese Anleitung führt in wenigen Minuten zur laufenden Anwendung. Es wird nichts aus dem
Internet nachgeladen — auch während der Installation nicht.

Stand: August 2026

---

## Welche Fassung brauchen wir?

**Auf dem Mac:** Apfel-Menü oben links → *Über diesen Mac*.

| Dort steht | Nehmt |
|---|---|
| Chip: Apple M1, M2, M3, M4 … | **Dienstplan-Mac-AppleChip.dmg** |
| Prozessor: Intel … | **Dienstplan-Mac-Intel.dmg** |

**Auf Windows:** **Dienstplan-Windows-Setup.exe** — passt auf Windows 10 und 11 (64 Bit).

Alle drei Fassungen liegen auf **dienstplan-jugendhilfe.de** zum Herunterladen. Ihr
nehmt die, die zu eurem Rechner passt.

---

## Weg 1 · Heruntergeladen

Beim ersten Start meldet sich das Betriebssystem einmalig. Das ist kein Fehler und kein
Hinweis auf Schadsoftware — die Meldung sagt nur, dass der Hersteller nicht bei Apple
beziehungsweise Microsoft hinterlegt ist. Warum das so ist, steht ganz unten.

### Mac

1. Die geladene **.dmg**-Datei doppelklicken
2. Im Fenster, das aufgeht, **Dienstplan** auf **Programme** ziehen
3. Dienstplan aus dem Programme-Ordner starten

Beim ersten Start erscheint: *„Dienstplan" kann nicht geöffnet werden, da der Entwickler
nicht überprüft werden kann.*

1. Auf **Fertig** klicken (nicht „In den Papierkorb")
2. **Systemeinstellungen** öffnen → **Datenschutz & Sicherheit**
3. Nach unten scrollen. Dort steht: *„Dienstplan" wurde blockiert…* → auf
   **Dennoch öffnen** klicken
4. Rückfrage mit **Öffnen** bestätigen, Passwort oder Touch ID

Danach nie wieder. Ab dem zweiten Start geht die App ganz normal auf.

> Bei älteren Systemen (macOS 14 und davor) genügt: **Rechtsklick** auf die App →
> **Öffnen** → im Fenster nochmal **Öffnen**.

### Windows

Beim Start des Installationsprogramms erscheint ein blaues Fenster:
*Der Computer wurde durch Windows geschützt.*

1. Auf **Weitere Informationen** klicken (der kleine Text, leicht zu übersehen)
2. Auf **Trotzdem ausführen**

Danach läuft die Installation normal.

---

## Weg 2 · Vom USB-Stick

Habt ihr die Anwendung auf einem Stick bekommen, entfällt die Meldung auf dem Mac
vollständig — sie hängt an der Herkunft „aus dem Internet geladen", nicht an der Datei.

**Mac:** Die **.dmg**-Datei vom Stick doppelklicken, **Dienstplan** auf **Programme**
ziehen, Stick auswerfen, aus dem Programme-Ordner starten.

**Windows:** **Dienstplan-Windows-Setup.exe** doppelklicken. Meldet sich SmartScreen
trotzdem, gilt derselbe Weg wie oben: *Weitere Informationen → Trotzdem ausführen*.

---

## Beim ersten Start

Ein kurzer Assistent führt durch zwei Schritte:

1. **Passwort für die Verwaltung festlegen.** Damit sind Verwaltung und Mitarbeitenden-
   Bereich getrennt. Notiert es — es gibt keine Wiederherstellung über uns, weil wir
   keinen Zugriff auf eure Installation haben.
2. **Erste Gruppe anlegen.** Sie startet mit Früh-, Spät- und Nachtdienst zu üblichen
   Zeiten. Alles davon lässt sich in der Verwaltung ändern, umbenennen oder ergänzen.

Danach steht der Monatsplan. Als Nächstes tragt ihr unter *Verwaltung → Team* eure
Mitarbeitenden ein.

---

## Wo die Daten liegen

| System | Ordner |
|---|---|
| Mac | `~/Library/Application Support/Dienstplan/` |
| Windows | `%APPDATA%\Dienstplan\` |

Darin:

- **dienstplan.db** — die eigentliche Datenbank. Eine einzige Datei.
- **Sicherungen/** — automatisch beim Start, höchstens einmal am Tag, die letzten 30
  Stände. Zusätzlich einmal pro Woche der Plan als PDF, damit die Daten auch ohne diese
  Anwendung lesbar bleiben.

**Was ihr selbst tun solltet:** Die Festplatte des Rechners verschlüsseln — FileVault auf
dem Mac, BitLocker unter Windows. Die Datenbank liegt unverschlüsselt darin, und ein
Dienstplan enthält Personaldaten. Zusätzlich in der Verwaltung einen zweiten
Sicherungsort einrichten (USB-Stick oder Netzlaufwerk), damit die Sicherungen nicht auf
demselben Rechner liegen wie das Original.

---

## Mehrere Arbeitsplätze

Ein Rechner trägt die Daten, alle anderen rufen den Plan über euer Hausnetz auf — ohne
Installation, im Browser. Wie das eingerichtet wird, steht in
**Hausnetz-einrichten.md**.

---

## Wenn etwas nicht geht

**Die App startet nicht, es passiert gar nichts.**
Auf dem Mac einmal aus dem Programme-Ordner starten, nicht vom eingehängten
Installationsabbild. Unter Windows: über das Startmenü, nicht über die Setup-Datei.

**„Die Datenbank ist gesperrt" oder Ähnliches.**
Läuft die App vielleicht zweimal? Ein Fenster reicht. Unter Windows im Task-Manager
prüfen, ob ein zweiter Dienstplan-Prozess läuft.

**Wir haben das Verwaltungs-Passwort vergessen.**
Wir können es nicht zurücksetzen — wir haben keinen Zugriff auf eure Installation, das
ist Absicht. Schreibt uns trotzdem: Es gibt einen Weg über die Datenbank direkt, und der
kostet nur ein paar Minuten.

**Der Plan sieht in der Wochenansicht leer aus.**
Dann hat die Gruppe noch keine Dienstarten. Die Wochenansicht sagt das und verlinkt die
Stelle in der Verwaltung.

Fragen: **kontakt@dienstplan-jugendhilfe.de**

---

## Warum kommt überhaupt eine Warnung?

Apple und Microsoft verlangen für eine warnungsfreie Installation ein kostenpflichtiges
Entwicklerzertifikat, das jährlich verlängert wird. Wir sind gerade dabei, das
einzurichten; bis dahin erscheint die Meldung.

Was die Meldung **nicht** bedeutet: dass die Anwendung geprüft und für schädlich befunden
wurde. Sie bedeutet: „Der Hersteller ist uns nicht hinterlegt."

Wer sichergehen will, kann die Datei vor der Installation bei **virustotal.com**
hochladen — dort prüfen rund siebzig Virenscanner gleichzeitig. Und wer die Anwendung
vom Stick installiert statt aus dem Internet, sieht auf dem Mac gar keine Meldung: Die
Warnung hängt an der Herkunft „aus dem Internet geladen", nicht an der Datei selbst.
