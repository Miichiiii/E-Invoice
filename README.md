# InvoiceAI – Rechnungen erfassen & auswerten

Browser-Webanwendung zur automatischen Erfassung, Prüfung und Weiterverarbeitung von Rechnungen, Tickets und Bestellbestätigungen. Kein Login, keine Installation, läuft vollständig im Browser.

## Kernfunktionen

- **Upload:** PDF per Drag & Drop oder Auswahl, mehrere Dateien gleichzeitig (Warteschlange).
- **E-Rechnung (XRechnung/ZUGFeRD):** XML-Dateien direkt oder als eingebettetes Attachment in einer PDF werden strukturiert ausgelesen (kein KI-Raten, ~99 % Confidence). Unterstützt UBL- und CII-Format.
- **KI-Auswertung (Fallback):** Ohne strukturierte E-Rechnung liest die KI den PDF-Text aus, erkennt Dokumenttyp (Rechnung/Ticket/Quittung), alle relevanten Felder, Rechnungspositionen und einen Confidence-Wert pro Feld. Bei niedriger Confidence erfolgt eine zweite Prüfrunde. Ohne KI-Freigabe greift eine einfache Regex-Heuristik.
- **Duplikaterkennung:** über Datei-Hash, Rechnungsnummer+Lieferant oder Lieferant+Datum+Betrag.
- **Validierung:** Prüft Netto+MwSt.=Brutto und IBAN-Prüfziffer, zeigt Warnungen im Ticket.
- **Workflow-Board (Kanban):** Stufen Erfasst → Fachliche Prüfung → Wartet auf Freigabe → Freigegeben → Zahlungsbereit. Verschieben per Drag & Drop (Desktop) oder Dropdown auf der Karte (Mobil/Touch).
- **Automatisches Routing:** Kategorie bestimmt zuständige Rolle; Rollen und Zuordnungen sind frei konfigurierbar.
- **Fristen & Eskalation:** Fälligkeits-/Veranstaltungsdatum setzt die Frist; kritische Tickets werden rot markiert, mit optionaler Browser-Benachrichtigung.
- **Split-Screen-Detailansicht:** PDF-Vorschau links, editierbare Felder, Kommentare mit @-Erwähnung, verknüpfte Child-Tickets rechts.
- **Makros:** „Fehlende Pflichtangabe“ und „Nachfrage an Besteller“ öffnen einen E-Mail-Entwurf und pausieren das Ticket automatisch.
- **Zahlungs-Export:** SEPA-XML (pain.001) und Buchungssatz (JSON) ab Status „Freigegeben“.
- **Lernfähig:** Korrekturen an Lieferantennamen und Kategorien werden gespeichert und bei künftigen Belegen automatisch angewendet.
- **Verwaltung:** Lieferanten (inkl. Zusammenführen), Kategorien, Team, Rollen – alles editierbar.
- **Archiv:** Bezahlte/abgeschlossene Tickets lassen sich archivieren, mit Wiederherstellen-Funktion.
- **Reports & Audit-Log:** Monatsauswertung, Umsatzsteuer, wiederkehrende Zahlungen, vollständiges Ereignisprotokoll.
- **Admin-Bereich:** Auftraggeberdaten (für SEPA), Standard-Kategorie, Schwellenwerte, Backup-Export/Import (JSON), gestufte Löschoptionen (Tickets/Archiv/alles).
- **Rollen-Filter & Sidebar:** Ansicht nach Rolle filterbar; komplette Navigation in einer einklappbaren Sidebar (mobil als Overlay).
- **CSV-Export** der Rechnungsliste.

## Technik

- Aufgeteilt in `Invoice.html` (Markup), `Invoice.css` (Styling) und `Invoice.js` (Anwendungslogik); kein Build-Prozess, kein Server.
- PDF-Rendering/-Text via **PDF.js** (CDN).
- KI-Aufrufe laufen über die Plattform-eigene `sample`-Capability (fragt bei erstem Gebrauch um Erlaubnis).
- **Speicherung:** ausschließlich lokal im Browser (`localStorage` für Daten/Einstellungen, `IndexedDB` für PDF-Dateien). Kein Server, kein Sync zwischen Geräten, kein Mehrbenutzer-Zugriff.

## Bekannte Grenzen

- Gescannte PDFs ohne Textebene (kein OCR) werden mit Fehlermeldung abgelehnt.
- Kein echter Login/Rechtesystem – „Rollen“ sind nur ein Anzeigefilter, kein Zugriffsschutz.
- E-Mails werden nur als Entwurf im Mailprogramm geöffnet, nicht automatisch versendet.
- Kein Export als eigene XRechnung/ZUGFeRD-Datei (nur Import/Lesen).
- Alle Daten sind an den jeweiligen Browser/Gerät gebunden; Backup nur manuell über Admin-Export.

## Nutzung

1. `Invoice.html` im Browser öffnen (lokal oder gehostet).
2. Beleg hochladen → automatische Auswertung → Ticket im Board prüfen und durch die Stufen bewegen.
3. Bei Bedarf: Admin-Bereich für Auftraggeberdaten, Backup und Grundeinstellungen nutzen.
