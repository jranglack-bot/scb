# SCB Erfolgs-DNA Workbook

Interaktives Pre-Workshop-Workbook für **Jacky & Julez / Secret Creators Club**.
Eine einzelne Seite: 3 Übungen, 11 Fragen, Fortschritts-Häkchen, PDF-Export und anonyme
Übermittlung der Antworten an einen Make-Webhook.

## Struktur

```
.
├─ index.html              # komplette Seite (HTML + CSS + JS)
└─ assets/
   ├─ logo.png             # SCB-Logo (transparent, Gold)
   └─ jacky-julez.jpg      # Foto für den Statement-Bereich
```

- **jsPDF** wird per CDN geladen (`cdnjs`), ebenso die Schriften (Google Fonts).
- Kein Build-Schritt nötig – `index.html` einfach im Browser öffnen.

## Hosten (GitHub Pages)

1. Repo auf GitHub anlegen, diese Dateien pushen.
2. **Settings → Pages → Branch: `main`, Ordner: `/root`** → speichern.
3. Nach kurzer Zeit ist die Seite unter `https://<user>.github.io/<repo>/` erreichbar.

## Make-Webhook (anonyme Auswertung)

Beim Klick auf **„PDF speichern"** (wenn alle Felder ausgefüllt sind) wird **einmal pro
Browser-Sitzung** ein POST an die Webhook-URL in `index.html` geschickt – **anonym, ohne Namen**.

Gesendet werden zwei Felder (als `application/x-www-form-urlencoded`, damit Make sie sauber trennt):

| Feld       | Inhalt                                                        |
|------------|---------------------------------------------------------------|
| `titel`    | Zeitstempel `JJJJ-MM-TT SS-MM-SS` (= Name des Google-Docs)    |
| `dokument` | Kompletter Text als HTML (alle Fragen + Antworten)            |

**Make-Setup:** Webhook → Google Docs „Create a Document": *Name* = `titel`, *Content* = `dokument`.
Nach Änderungen am Payload in Make einmal **„Redetermine data structure"** ausführen.

Die Webhook-URL steht in `index.html` in der Konstante `WEBHOOK_URL` (anpassbar).

## Teilbare Einzeldatei (optional)

Für den Versand per WhatsApp o. ä. lässt sich eine **einzelne, eigenständige HTML-Datei**
erzeugen, in der Logo, Foto und jsPDF als Base64/Data-URI eingebettet sind
(funktioniert offline, keine Begleitdateien). Diese Variante wird separat gebaut und
ist nicht Teil dieses Repos.

## Anpassen

- **Farben/Branding:** CSS-Variablen in `:root` (Beige-Theme, Gold-Akzente).
- **Fragen:** im HTML (Cards `q1`–`q11`) **und** im JS-Array `PDF_SECTIONS` (für PDF + Doc-Text) synchron halten.
- **Texte/Spruch:** direkt im HTML bzw. in `buildPdf()` / `buildDocHtml()`.
