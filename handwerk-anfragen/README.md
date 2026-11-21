# Kundenanfragen im Handwerk automatisch sortieren (n8n)

Für Handwerksbetriebe aus Sanitär, Heizung, Klima und Elektro, bei denen täglich Anfragen per E-Mail eingehen. Oft fehlt die Telefonnummer oder die Adresse, Notfälle gehen zwischen Angebotsanfragen unter, und den Auftrag bekommt der Betrieb, der zuerst antwortet.

## Was der Workflow macht
1. **Liest jede neue E-Mail** im Anfragen-Postfach (IMAP, funktioniert mit jedem Anbieter, auch IONOS, Strato oder Microsoft 365).
2. **Eine KI liest die Anfrage aus:** Name, Telefon, Einsatzort, Gewerk, Art des Auftrags, Gerät, Objekt, Wunschtermin und Dringlichkeit. Sie erfindet nichts: Was der Kunde nicht schreibt, bleibt leer.
3. **Notfälle** (Wasser läuft aus, Heizung oder Warmwasser ausgefallen, Gasgeruch) gehen sofort per E-Mail an den Notdienst, mit Telefonnummer und Adresse vorn.
4. **Fehlen Angaben**, bekommt der Kunde eine freundliche E-Mail, die genau nach dem Fehlenden fragt: Telefonnummer, Adresse oder bei Reparatur und Wartung Gerät und Hersteller.
5. **Jede Anfrage landet als Zeile** in einer Google-Tabelle mit Spalte „Status“, damit das Büro den Überblick behält.
6. **Einzugsgebiet:** Über Postleitzahl-Anfänge erkennt der Workflow, ob der Einsatzort im eigenen Gebiet liegt. Außerhalb wird nichts nachgefragt, nur eingetragen.

## Einrichtung
1. `workflow.json` in n8n einfügen.
2. Zugangsdaten anlegen: IMAP und SMTP des Anfragen-Postfachs, OpenAI (oder ein anderes Sprachmodell anschließen), Google Sheets.
3. Google-Tabelle mit dem Tabellenblatt `Anfragen` anlegen, in Zeile 1 diese Spalten:
   `Eingang, Dringlichkeit, Kunde, Telefon, E-Mail, Einsatzort, Im Einzugsgebiet, Gewerk, Art, Gerät, Objekt, Wunschtermin, Zusammenfassung, Fehlende Angaben, Status`
   Die Adresse der Tabelle im Knoten **Save to inquiry sheet** eintragen.
4. Im Knoten **Settings** anpassen: Firmenname, Postleitzahl-Anfänge des Einzugsgebiets (z. B. `40, 41, 42`), Absenderadresse, Adresse des Notdiensts.
5. Mit einer Test-Mail an das Postfach prüfen, dann aktivieren.

## Anpassen
- **Gewerke und Notfall-Regeln:** im Knoten **Extract inquiry details** (Schema und Anweisung).
- **Was als fehlend gilt:** im Code-Knoten **Prepare inquiry**.
- **Benachrichtigung:** Statt E-Mail an den Notdienst geht auch SMS, Telegram oder Signal.

## Gut zu wissen
- Antwortet ein Kunde auf die Nachfrage, kommt seine Antwort als neue Anfrage herein und wird wieder ausgelesen, dann meist vollständig. Wer das zusammenführen will, gleicht in der Tabelle über die E-Mail-Adresse ab.
- Der Workflow liest jede Mail im Postfach. Am besten läuft er auf einer eigenen Adresse nur für Anfragen, zum Beispiel `anfragen@ihrbetrieb.de`, sonst bitte vorher einen Filter auf Werbung setzen.
- Getestet mit n8n 2.48. Die Auswertung der Felder, die Notfall-Weiche, die Nachfrage und der Tabelleneintrag sind mit Beispielanfragen geprüft. Wie gut die KI Ihre echten Anfragen liest, prüfen Sie mit eigenen Test-Mails.
- Personenbezogene Daten: Kundendaten gehen an das gewählte Sprachmodell. Für den Einsatz in Deutschland einen Anbieter mit Auftragsverarbeitungsvertrag wählen oder ein selbst gehostetes Modell anschließen.

## English
Reads customer inquiries from an IMAP mailbox, extracts contact and job details with an AI model, alerts the on-call team for emergencies, asks the customer for missing details in German and logs every inquiry to Google Sheets. Built for plumbing, heating and HVAC businesses in Germany, Austria and Switzerland.

---
Erstellt von [azurai](https://azurai.de), KI-Automatisierung und [n8n-Workflows](https://azurai.de/loesungen/n8n-agentur/) für den Mittelstand. Sie möchten so einen Ablauf in Ihrem Betrieb, angepasst an Ihre Programme? [Kostenloser Prozess-Check](https://azurai.de/prozess-check/).
