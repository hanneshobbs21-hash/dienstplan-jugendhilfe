# Zugang über das Hausnetz einrichten

Damit mehrere Arbeitsplätze am selben Dienstplan arbeiten, läuft die App auf **einem**
Rechner. Alle anderen öffnen sie im Browser — ohne eigene Installation.

---

## 1. Auf dem Server-Rechner

Verwaltung → Reiter **System** → Haken **„Zugang über das Hausnetz"** setzen.
Dann die App einmal beenden und neu starten.

Danach steht an derselben Stelle, unter welchen Adressen die anderen euch erreichen,
zum Beispiel:

```
https://192.168.2.125:8443/
```

**Der Rechner muss laufen**, solange jemand am Plan arbeitet. Und er sollte nicht in
den Ruhezustand gehen — sonst bricht die Verbindung mitten in der Arbeit ab.

---

## 2. Die Warnung im Browser

Beim ersten Aufruf meldet der Browser: *„Die Verbindung ist nicht privat"* oder
*„Warnung: Mögliches Sicherheitsrisiko"*.

**Was das heißt — und was nicht.** Die Verbindung *ist* verschlüsselt, mit demselben
Verfahren wie beim Online-Banking (TLS 1.3). Der Browser bemängelt etwas anderes: Das
Zertifikat hat euer eigener Rechner ausgestellt, nicht eine der Stellen, denen der
Browser von Haus aus vertraut. Für einen Server im eigenen Haus ist das normal — ein
öffentliches Zertifikat bekommt man nur für eine öffentliche Adresse.

Ihr habt zwei Möglichkeiten.

### Der saubere Weg: das Zertifikat einmalig hinterlegen

Danach ist Ruhe, und — wichtiger — eine Warnung bedeutet später wirklich etwas.

**Die Zertifikatsdatei liegt auf dem Server-Rechner:**

| System | Ort |
|---|---|
| Windows | `%APPDATA%\Dienstplan\Zertifikat\zertifikat.pem` |
| macOS | `~/Library/Application Support/Dienstplan/Zertifikat/zertifikat.pem` |

Kopiert diese Datei auf die anderen Arbeitsplätze. **Nur diese** — die Datei
`schluessel.pem` daneben bleibt, wo sie ist. Sie ist das Geheimnis des Servers und darf
den Rechner nie verlassen.

**Windows**, in der PowerShell als Administrator:

```
Import-Certificate -FilePath "C:\Pfad\zertifikat.pem" -CertStoreLocation Cert:\LocalMachine\Root
```

**macOS**, im Terminal:

```
sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain zertifikat.pem
```

Danach den Browser einmal komplett schließen.

> **Firefox** bringt einen eigenen Zertifikatsspeicher mit und übernimmt die Einstellung
> des Systems nicht. Dort: Einstellungen → Datenschutz & Sicherheit → Zertifikate
> anzeigen → Importieren.

> Sollte der Browser trotz Import weiter warnen, nehmt den zweiten Weg. Manche
> Browser-Versionen akzeptieren ein selbst ausgestelltes Server-Zertifikat nicht als
> vertrauenswürdige Wurzel.

### Der schnelle Weg: Ausnahme bestätigen

„Erweitert" → „Weiter zu … (unsicher)". Funktioniert sofort und gilt für diesen Browser.

**Der Haken daran ist nicht technisch, sondern menschlich:** Wer sich angewöhnt,
Sicherheitswarnungen wegzuklicken, tut es auch dann, wenn sie berechtigt ist. Nehmt
diesen Weg zum Ausprobieren — für den Dauerbetrieb den ersten.

---

## 3. Wenn sich die Adresse ändert

Bekommt der Server-Rechner eine neue Adresse im Netz (üblich bei automatischer
Vergabe), stellt die App beim nächsten Start automatisch ein neues Zertifikat aus. Dann
müsst ihr es erneut hinterlegen.

**Das lässt sich vermeiden:** Vergebt dem Server-Rechner im Router eine feste Adresse.
Jeder handelsübliche Router kann das; die Einstellung heißt meist „DHCP-Reservierung"
oder „Statische Zuordnung".

---

## 4. Drei Dinge, die ihr nicht überspringen solltet

**Festplatte verschlüsseln.** Die Datenbank liegt unverschlüsselt auf dem Server-Rechner.
Wer ihn mitnimmt, hat alle Dienstpläne, Namen und Stundenkonten. Schaltet **BitLocker**
(Windows) beziehungsweise **FileVault** (macOS) ein. Das ist die übliche und erwartete
Maßnahme — ohne sie ist der Rest halb umsonst.

**Nur ein eigenes Netz.** In einem Netz, in das auch Gäste oder Bewohner kommen, gehört
der Plan nicht. Wenn ihr ein WLAN nutzt: ein eigenes, mit WPA2 oder WPA3, ohne
Gastzugang.

**Anständige Passwörter.** Die App verlangt mindestens acht Zeichen und sperrt nach fünf
Fehlversuchen für 15 Minuten. Das hilft nur, wenn die Passwörter nicht „Sommer2026"
lauten. Die App kann sie auf Wunsch selbst erzeugen — Verwaltung → Team → Passwort.

---

## 5. Wenn es klemmt

**„Die Website ist nicht erreichbar"** — Läuft die App auf dem Server-Rechner? Ist der
Haken gesetzt und die App danach neu gestartet wurden? Blockiert die Firewall den Port?
Unter Windows fragt sie beim ersten Start; wurde dort „Abbrechen" geklickt, muss der
Port in der Windows-Firewall freigegeben werden.

**„Port schon belegt"** — Eine andere Anwendung nutzt 8443. Tragt in der Verwaltung eine
andere Zahl ein, etwa 8444, und startet neu.

**Alles ist langsam** — Der Server-Rechner sollte kein Uralt-Gerät sein und nicht
gleichzeitig für anderes benutzt werden.
