# Apple-Signierung: Schritt für Schritt

Ziel: Die App startet auf jedem Mac mit einem Doppelklick — **ohne Warnung**.
Kosten: 99 USD im Jahr. Zeit: 20 Minuten Arbeit, danach 1–2 Tage Warten auf Apple.

Diese Anleitung ist so geschrieben, dass eine neue Sitzung sie ohne Vorwissen
abarbeiten kann.

---

## Was heute schon vorbereitet ist

| | |
|---|---|
| `build/entitlements.mac.plist` | angelegt, mit den vier nötigen Ausnahmen |
| Payload | von `/private/tmp/…` nach `dienstplan-desktop/payload/` verschoben — **lag vorher im Zwischenspeicher einer Sitzung und wäre gelöscht worden** |
| `afterPack.js`, `package.json`, `electron-builder-macx64.json` | zeigen auf den neuen, dauerhaften Ort |
| Payload geprüft | 8132 Dateien in `next/` je Architektur — der Sollwert |

**Noch nicht geändert:** In `package.json` steht weiterhin `"identity": null`. Das
schaltet die Signierung ab. Wird erst umgestellt, wenn das Zertifikat da ist — sonst
bricht jeder Build ab.

---

## Teil 1 · Bei Apple anmelden (heute, 15 Minuten)

### 1.1 Apple-ID vorbereiten

Zwei-Faktor-Bestätigung muss aktiv sein. Prüfen unter
**appleid.apple.com** → Anmelden → *Anmeldung und Sicherheit*.

Nimm die Apple-ID, die du dauerhaft behältst. Sie wird zur Identität deiner Software.

### 1.2 Developer Program

→ **developer.apple.com/programs/enroll**

- Anmeldung mit derselben Apple-ID
- **„Individual / Sole Proprietor"** wählen, nicht „Organization"
  > „Organization" verlangt eine **D-U-N-S-Nummer** und dauert Wochen. Als
  > Einzelperson bist du in ein bis zwei Tagen fertig. Der Unterschied ist sichtbar:
  > In der Signatur steht dann **dein Name** statt eines Firmennamens. Wechseln geht
  > später jederzeit.
- Name und Anschrift müssen **genau** mit deinem Ausweis übereinstimmen
- 99 USD mit Kreditkarte
- Apple kann einen Ausweis zur Prüfung anfordern

**Danach warten.** Die Freischaltung kommt per E-Mail, meist innerhalb von 48 Stunden.

---

## Teil 2 · Zertifikat erzeugen (wenn Apple freigeschaltet hat)

### 2.1 Zertifikatsanfrage auf dem Mac erstellen

Auf diesem Mac ist **kein vollständiges Xcode** installiert, nur die
Command Line Tools. Der Weg führt deshalb über die Schlüsselbundverwaltung:

1. **Schlüsselbundverwaltung** öffnen (Spotlight: „Schlüsselbund")
2. Menü **Schlüsselbundverwaltung → Zertifikatsassistent → Zertifikat einer
   Zertifizierungsinstanz anfordern…**
3. E-Mail-Adresse: deine Apple-ID
   Allgemeiner Name: dein Name
   **„Auf der Festplatte sichern"** ankreuzen und **„Schlüsselpaar selbst angeben"**
4. Schlüsselgröße **2048 Bit**, Algorithmus **RSA**
5. Die Datei `CertificateSigningRequest.certSigningRequest` speichern

### 2.2 Zertifikat bei Apple anfordern

→ **developer.apple.com/account/resources/certificates/add**

- Art: **„Developer ID Application"**
  > Achtung, nicht verwechseln: *Mac App Store* wäre für den Verkauf über den
  > App Store. Für eine App, die du selbst ausliefert, ist **Developer ID** richtig.
- Die `.certSigningRequest` hochladen
- Das erzeugte `.cer` herunterladen und **doppelklicken** — damit landet es im
  Schlüsselbund

### 2.3 Prüfen, dass es angekommen ist

```bash
security find-identity -v -p codesigning
```

Erwartet wird eine Zeile wie:

```
1) A1B2C3… "Developer ID Application: Dein Name (AB12CD34EF)"
```

Der Teil in Klammern ist deine **Team-ID**. Notieren — sie wird gleich gebraucht.

---

## Teil 3 · Passwort für die Notarisierung

Apple prüft jede App vor der Auslieferung automatisch auf Schadsoftware. Dafür braucht
der Rechner ein eigenes Passwort — nicht dein Apple-ID-Passwort.

→ **appleid.apple.com** → *Anmeldung und Sicherheit* → **App-spezifische Passwörter**
→ Neues erzeugen, Bezeichnung z. B. „Dienstplan Notarisierung"

**Das Passwort wird nur einmal angezeigt.** Sofort sichern.

---

## Teil 4 · Was ich dann mache

Sobald du **Team-ID**, **Apple-ID** und **App-Passwort** hast, sag mir Bescheid. Ich
stelle um:

```jsonc
"mac": {
  "identity": "Developer ID Application: DEIN NAME (TEAMID)",
  "hardenedRuntime": true,
  "gatekeeperAssess": false,
  "entitlements": "build/entitlements.mac.plist",
  "entitlementsInherit": "build/entitlements.mac.plist",
  "notarize": { "teamId": "TEAMID" }
}
```

Die Zugangsdaten kommen als Umgebungsvariablen dazu, nicht in die Datei:

```bash
export APPLE_ID="deine@apple-id.de"
export APPLE_APP_SPECIFIC_PASSWORD="xxxx-xxxx-xxxx-xxxx"
export APPLE_TEAM_ID="AB12CD34EF"
```

> **Das App-Passwort gehört nicht ins Projekt.** Wäre es in `package.json`, läge es
> später im öffentlichen GitHub-Repository.

---

## Der Punkt, an dem es bei genau dieser App klemmen kann

Die App bringt **ein eigenes Node mit — 113 MB** — und mehrere native Module
(`better-sqlite3`, `lightningcss`, `tailwindcss-oxide`). Unter *Hardened Runtime*
verlangt macOS, dass **jede ausführbare Datei signiert** ist. electron-builder
übersieht Dateien in `extraResources` gern.

Wenn die Notarisierung mit einer Meldung wie *„The binary is not signed"* oder
*„The signature does not include a secure timestamp"* scheitert, liegt es fast sicher
daran. Die Lösung ist eine `binaries`-Liste in der Mac-Konfiguration, damit
electron-builder diese Dateien mitsigniert.

**Das ist erwartbar und eingeplant** — nur nicht überraschen lassen.

Zur Erinnerung: `disable-library-validation` steht bereits in den Entitlements. Ohne
diese Ausnahme dürfte das mitgelieferte Node die nativen Module gar nicht laden, und
der Server startet nicht.

---

## Wie du merkst, dass es geklappt hat

```bash
codesign -dv --verbose=4 /Applications/Dienstplan.app 2>&1 | grep -E "Authority|TeamIdentifier"
spctl -a -vvv /Applications/Dienstplan.app
xcrun stapler validate /Applications/Dienstplan.app
```

Erwartet:

- `Authority=Developer ID Application: Dein Name (…)` — **nicht** „adhoc"
- `spctl` sagt **accepted**, `source=Notarized Developer ID`
- `stapler` sagt **The validate action worked**

Der echte Beweis ist ein anderer: Die `.dmg` auf einen Mac laden, auf dem sie noch nie
war, und doppelklicken. Kommt keine Warnung, ist es fertig.

---

## Was das nicht löst

**Windows.** Dort gilt ein eigenes Verfahren, es kostet mehr (~250 €/Jahr, OV genügt —
EV bringt seit März 2024 keinen Vorteil mehr) und setzt die **Gewerbeanmeldung**
voraus. Und selbst mit Zertifikat kann SmartScreen weiter warnen, bis genug Downloads
zusammengekommen sind.

Beim Mac ist es eindeutig: Notarisiert heißt keine Warnung. Deshalb zuerst Apple.
