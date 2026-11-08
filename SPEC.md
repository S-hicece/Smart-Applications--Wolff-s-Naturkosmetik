# SPEC.md – Wolff's Naturkosmetik

**Version:** 1.0  
**Stand:** Nach Meeting 1  
**Projekt:** Smart Applications – Anforderungen und App für Wolff's Naturkosmetik

## 1. Ziel der App

Die App soll Nina bei der Verwaltung ihres Naturkosmetikgeschäfts unterstützen. Bisher erledigt sie viele Aufgaben über Instagram-Nachrichten und Excel-Tabellen. Dadurch entstehen Fehler, zum Beispiel bei Zahlungseingängen, Lagerbeständen und Allergieangaben.

Die wichtigsten Ziele sind eine bessere Übersicht über Bestellungen und Zahlungen, eine gemeinsame Lagerverwaltung und eine einfachere Verwaltung der Kundendaten.

## 2. Anforderungen

Die App soll folgende Funktionen ermöglichen:

- Kunden und ihre Kontaktdaten, Lieferadressen und Allergien speichern.
- Bestellungen aus verschiedenen Verkaufskanälen verwalten.
- Zahlungen und offene Beträge übersichtlich anzeigen.
- Produkte und deren Inhaltsstoffe verwalten.
- Chargen mit Herstellungsdatum, Menge und Mindesthaltbarkeitsdatum erfassen.
- Lagerbestände aktuell halten, damit Produkte nicht doppelt verkauft werden.
- Versand, Abholung und Lieferungen dokumentieren.
- Stammkunden erkennen und mögliche Treuerabatte berücksichtigen.
- Rabattaktionen und deren Bestellungen nachvollziehen.
- Bei Allergien automatisch warnen, sowohl bei der Bestellung als auch beim Packen.

## 3. Datenmodell – Entitäten und Attribute

### 3.1 Kunde
- Kundennummer
- Vorname und Nachname
- E-Mail-Adresse
- Telefonnummer
- Lieferadresse
- Kundentyp (Neu- oder Stammkunde)
- Allergien und Unverträglichkeiten
- Besondere Wünsche

### 3.2 Produkt
- Produktnummer
- Name
- Beschreibung
- Zutaten
- Enthaltene Allergene
- Vegan (Ja/Nein)
- B2C-Preis
- B2B-Preis
- Herstellungs- bzw. Aushärtezeit

### 3.3 Charge
- Chargennummer
- Produktnummer
- Herstellungsdatum
- Produzierte Menge
- Aktuelle Restmenge
- Mindesthaltbarkeitsdatum
- Verfügbar ab (nach der Aushärtezeit)

### 3.4 Bestellung
- Bestellnummer
- Kundennummer
- Bestelldatum
- Verkaufskanal (Instagram, Markt oder B2B)
- Bestellstatus
- Zahlungsstatus
- Versand- bzw. Abholart
- Gesamtbetrag
- Verwendeter Rabattcode

### 3.5 Bestellposition
- Positionsnummer
- Bestellnummer
- Produktnummer
- Bestellte Menge
- Einzelpreis
- Gesamtpreis

### 3.6 Zahlung
- Zahlungsnummer
- Zahlungsdatum
- Betrag
- Zahlungsart (Überweisung, Barzahlung oder SumUp)
- Verwendungszweck
- Zahlungsstatus

### 3.7 Lieferung
- Lieferungsnummer
- Bestellnummer
- Lieferart (DHL, Abholung oder eigene Lieferung)
- Lieferadresse
- Sendungsnummer
- Lieferstatus

### 3.8 Marketingaktion
- Aktionsnummer
- Name der Aktion
- Rabattcode
- Rabattart und Rabatthöhe
- Gültigkeitszeitraum
- Verkaufskanal

## 4. Beziehungen zwischen den Entitäten

1. **Kunde – Bestellung (1:n):** Ein Kunde kann mehrere Bestellungen aufgeben. Jede Bestellung gehört zu einem Kunden.

2. **Bestellung – Bestellposition (1:n):** Eine Bestellung enthält eine oder mehrere Bestellpositionen. Jede Position gehört zu einer Bestellung.

3. **Produkt – Bestellposition (1:n):** Ein Produkt kann in vielen Bestellpositionen vorkommen. Eine Bestellposition bezieht sich auf ein Produkt.

4. **Produkt – Charge (1:n):** Ein Produkt kann in mehreren Chargen hergestellt werden. Jede Charge gehört zu einem Produkt.

5. **Charge – Bestellposition (n:m):** Eine Bestellposition kann bei Bedarf aus mehreren Chargen beliefert werden. Eine Charge kann wiederum für mehrere Bestellpositionen verwendet werden.

6. **Bestellung – Zahlung (n:m):** Eine Bestellung kann mehrere Zahlungen haben. Eine Zahlung kann auch mehreren Bestellungen zugeordnet werden, wenn eine Kundin mehrere Bestellungen zusammen überweist.

7. **Bestellung – Lieferung (1:n):** Eine Bestellung kann mehrere Lieferungen haben, zum Beispiel wenn eine Ersatzsendung notwendig wird. Jede Lieferung gehört zu einer Bestellung.

8. **Marketingaktion – Bestellung (1:n):** Eine Marketingaktion kann für mehrere Bestellungen verwendet werden. Eine Bestellung kann einer Marketingaktion zugeordnet werden.

## 5. Geschäftsregeln

**BR-01 – Reihenfolge der Chargen (M1-01, M1-05)**  
Bei Bestellungen soll grundsätzlich die älteste noch verwendbare Charge zuerst verwendet werden (FIFO). Produkte dürfen erst nach Ablauf ihrer Aushärtezeit verkauft werden. Abgelaufene Chargen dürfen nicht mehr verkauft werden.

**BR-02 – Allergien und Unverträglichkeiten (M1-03, M1-04)**  
Die Allergien eines Kunden müssen im Kundenprofil gespeichert werden. Enthält ein bestelltes Produkt einen Stoff, auf den der Kunde reagiert, muss die App beim Bestellen und beim Packen eine deutliche Warnung anzeigen.

**BR-03 – Lagerbestand (M1-05, M1-06, M1-09)**  
Der Lagerbestand muss für alle Verkaufskanäle gemeinsam geführt werden. Verkäufe auf Märkten, Online-Bestellungen und Abholungen müssen berücksichtigt werden, damit ein Produkt nicht mehrfach verkauft wird.

**BR-04 – Zahlung vor Versand bei Online-Bestellungen (M1-01, M1-02, M1-07, M1-11)**  
Bei normalen Online-Bestellungen muss der Zahlungseingang vor dem Versand nachvollziehbar geprüft werden. Für B2B-Kunden gilt die vereinbarte Zahlung auf Rechnung mit einem Zahlungsziel von 30 Tagen als Ausnahme.

**BR-05 – B2B-Bestellungen (M1-11)**  
Bei Bestellungen von Wiederverkäufern müssen die gesonderten B2B-Preise und die vereinbarte Mindestbestellmenge berücksichtigt werden. Die Zahlung erfolgt auf Rechnung mit einem Zahlungsziel von 30 Tagen.

## 6. Widersprüche und ihre Auflösung

### Widerspruch 1: Zahlungsarten

Einerseits werden Online-Bestellungen ausschließlich per Überweisung bezahlt. Andererseits akzeptiert Nina auf Märkten Barzahlung und SumUp.

**Auflösung:** Die Zahlungsart hängt vom Verkaufskanal ab. Bei Online-Bestellungen wird die Überweisung verwendet, bei Marktverkäufen sind Barzahlung und SumUp möglich.

### Widerspruch 2: Verkauf und Bestandsverwaltung

Der Lagerbestand wird bei Marktverkäufen teilweise erst am Ende des Tages in Excel aktualisiert. Gleichzeitig nimmt Nina weiterhin Online-Bestellungen an. Dadurch kann ein Produkt online noch als verfügbar angezeigt werden, obwohl es auf dem Markt bereits verkauft wurde.

**Auflösung:** Alle Verkaufskanäle müssen auf denselben Lagerbestand zugreifen. Verkäufe und Reservierungen müssen zeitnah berücksichtigt werden.

## 7. Prioritäten

### Hohe Priorität
- Übersicht über bezahlte und offene Bestellungen
- Zuordnung von Zahlungseingängen zu Bestellungen
- Gemeinsame Lagerverwaltung für alle Verkaufskanäle
- Verwaltung der Chargen und Mindesthaltbarkeitsdaten
- Allergiewarnungen bei Bestellung und Verpackung

### Mittlere Priorität
- Verwaltung von Versand, Abholung und eigener Lieferung
- Kundendaten und Bestellhistorie
- Verwaltung von B2B-Bestellungen und unterschiedlichen Preisen

### Niedrigere Priorität
- Treuerabatte für Stammkunden
- Informationen über neue Chargen an ausgewählte Kunden
- Auswertung von Rabattaktionen und deren Erfolg

## 8. Offene Fragen

- Ab wie vielen Bestellungen soll ein Kunde einen Treuerabatt erhalten und wie hoch soll dieser sein?
- Wie genau wird die Mindestbestellmenge für B2B-Kunden festgelegt?
- Sollen B2B-Kunden ihre Bestellungen später selbst über die App aufgeben können?
- Wie soll die App Banküberweisungen automatisch erkennen und zuordnen?
- Sollen Kunden ihre Bestellungen und den Lieferstatus selbst einsehen können?
- Wie sollen Rabattaktionen und deren Wirtschaftlichkeit genau ausgewertet werden?
- Soll die App Kunden automatisch über neue Chargen informieren können?
