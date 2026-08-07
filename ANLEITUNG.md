# Dienstplan — Überblick

Stand: 6. August 2026

Die Anwendung ist **kostenlos**. Damit ist der Weg kurz: kein Zahlungsanbieter, keine
Umsatzsteuerabwicklung, keine AGB, kein Widerrufsrecht für einen Kauf, der nicht
stattfindet. Wer die Weiterentwicklung freiwillig unterstützen will, bekommt eine
Rechnung — als Kleinunternehmer nach § 19 UStG, ohne ausgewiesene Umsatzsteuer.

**Die drei Schritte zum Veröffentlichen stehen in `KLICK-FUER-KLICK.md`.**

---

## Was in den Ordnern liegt

| Ordner | Inhalt |
|---|---|
| **Zum Hochladen** | die fertige Webseite, 15 Dateien. Wird von `bauen.py` erzeugt — hier nichts von Hand ändern |
| **GitHub-Ordner** | dieselben Dateien plus README, als Git-Repository. Das ist der Ordner, den du in GitHub Desktop hinzufügst |
| **Fuers GitHub-Release** | die drei Installer, 1,1 GB. Kommen als Release ins Repository, nicht in den Ordner |
| **Fuer-Kunden** | Installationsanleitung, Hausnetz-Anleitung, Datenschutz-Datenblatt — je als Markdown und PDF |
| **Marke** | Logos und Bildschirmaufnahmen für Mails an Träger |
| **Dienstplan-Sicherung** | Sicherungskopien |

Der Quelltext der Seite liegt **nicht** hier, sondern bei der App unter
`~/Developer/dienstplan-app/hochladen/`. Dort läuft `python3 bauen.py`, das den
Hochlade-Ordner neu erzeugt, und `./pruefen.sh`, das die Seite abklopft.

---

## Was schon erledigt ist

- **Domain** `dienstplan-jugendhilfe.de` gekauft, liegt bei INWX
- **E-Mail** `kontakt@dienstplan-jugendhilfe.de` — steht im Impressum, muss ankommen
- **Gewerbe angemeldet**, Einzelunternehmen ohne Handelsregistereintrag
- **Impressum vollständig**: Mike Weinert, Tumlinger Weg 32, 72178 Waldachtal.
  Keine Umsatzsteuer-Angabe nötig, weil keine USt-IdNr. vorhanden ist (§ 5 DDG
  verlangt sie nur „soweit vorhanden")
- **Webseite fertig** im Gewand der Anwendung, Schriften liegen lokal statt bei Google
- **Drei Installer gebaut** und geprüft, alle auf dem aktuellen Stand
- **Kundenunterlagen** als PDF: Installation, Hausnetz, Datenschutz-Datenblatt

---

## Was noch offen ist

### Veröffentlichen — drei Schritte, siehe `KLICK-FUER-KLICK.md`

Repository hochladen, Release mit den Installern anlegen, DNS bei INWX umstellen.
In dieser Reihenfolge.

### Der Handy-Test fürs Hausnetz

Die Seite verspricht den Zugang über das Hausnetz. Geprüft wurde er über die
LAN-Adresse von diesem Mac aus — aber noch nicht von einem zweiten Gerät. In der
Verwaltung unter *System* einschalten, App neu starten, dann die angezeigte Adresse
im Handy-Browser aufrufen. Die Zertifikatswarnung beim ersten Mal ist normal.

### Apple- und Windows-Signierung

Ohne Zertifikat meldet sich beim ersten Start das Betriebssystem. Das ist erklärt —
auf der Seite und in der Installationsanleitung — und bei Auslieferung per USB-Stick
tritt es auf dem Mac gar nicht auf.

Wenn du es lösen willst: `APPLE-SIGNIERUNG.md`, rund 100 € im Jahr. Danach läuft
`~/Developer/dienstplan-desktop/signieren.sh` und die Warnung verschwindet. Windows
wäre der zweite Schritt; dort ist die Hürde nur „zwei Klicks weiter".

---

## Was in der App offen bleibt

**Nachtbereitschaft** steht auf 0 %. Jedes Haus stellt seinen Tarifwert selbst ein —
auch du für deine eigene Gruppe.

**Der Name.** „Dienstplan" ist beschreibend und als Marke nicht schützbar. Solange die
Anwendung kostenlos ist, ist das folgenlos.

**Updates** kommen nicht von selbst. Eine neue Fassung heißt: neuer Build, neues
Release, und die Häuser laden sie herunter. Die Datenbank wird beim Start automatisch
mitgezogen, die Daten bleiben erhalten.
