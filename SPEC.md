

# SPEC.md
## Wolff's Naturkosmetik

**Version:** 2  
**Grundlage:** Meeting 1 und Meeting 2  
**Kundin:** Nina Wolf  
**Projekt:** Smart Applications

## 1. Ziel der Anwendung

Nina Wolf verkauft selbst hergestellte Naturkosmetik über Instagram, auf Märkten und an einige Wiederverkäufer. Viele Abläufe werden derzeit manuell über Excel, Instagram-Nachrichten und E-Mails organisiert.

Die Anwendung soll diese Abläufe vereinfachen und die wichtigsten Informationen zentral speichern. Dazu gehören Kunden, Bestellungen, Zahlungen, Produkte und Lagerbestände. Besonders wichtig sind eine bessere Übersicht über offene Zahlungen und die Vermeidung von Fehlern beim Verkauf und Versand.

Zusätzlich soll die Anwendung die monatliche Self-Care-Box verwalten. Dabei sollen Abonnements, die Zusammenstellung der Boxen und die benötigten Lagerbestände berücksichtigt werden.

## 2. Anforderungen

Die Anwendung soll folgende Funktionen unterstützen:

- Kunden und deren Kontaktdaten verwalten.
- Allergien und Unverträglichkeiten der Kunden speichern und sichtbar anzeigen.
- Produkte mit Zutaten, Allergenen, Preisen und weiteren Informationen verwalten.
- Produktionschargen mit Herstellungsdatum, Menge und Mindesthaltbarkeitsdatum erfassen.
- Lagerbestände für Online-Bestellungen und Marktverkäufe gemeinsam verwalten.
- Bestellungen anlegen und ihren Bearbeitungsstatus anzeigen.
- Zahlungen einzelnen Bestellungen zuordnen und offene Zahlungen kenntlich machen.
- Lieferungen und Sendungsnummern speichern.
- Verschiedene Preise für Privatkunden und Wiederverkäufer berücksichtigen.
- Rabattaktionen und Gutscheincodes verwalten.

### Neue Anforderung aus Meeting 2: Self-Care-Monthly-Box

Die Anwendung soll ein monatliches Abonnement für die Self-Care-Box verwalten. Die Box kostet 39 Euro und enthält normalerweise vier ausgewählte Produkte.

Dabei sollen folgende Abläufe unterstützt werden:

- Abonnements mit Startdatum und aktuellem Status verwalten.
- Für jeden Monat die vier ausgewählten Produkte speichern.
- Die Allergien und veganen Anforderungen der Kunden bei der Zusammenstellung berücksichtigen.
- Den Lagerbestand prüfen und den Bedarf für aktive Abonnements frühzeitig berechnen.
- Kunden über die monatliche Zusammenstellung und den voraussichtlichen Liefertermin informieren.
- Abonnements pausieren und wieder aktivieren können.
- Einzelne Monatsboxen überspringen und Abonnements kündigen können.

Eine Pause soll höchstens zwei Monate hintereinander dauern. Danach wird das Abonnement automatisch fortgesetzt. Eine Kündigung soll mit einer Vorlaufzeit von zwei bis drei Wochen vor der nächsten Box möglich sein.

Nina möchte außerdem für jeden Monat vier Hauptprodukte und zwei bis drei Ersatzprodukte festlegen. Wenn kein passendes Ersatzprodukt für einen Kunden verfügbar ist, soll Nina informiert werden. Sie entscheidet dann, ob sie ein anderes Produkt auswählt oder eine kleinere Box mit entsprechender Gutschrift verschickt.

## 3. Entitäten

Für die Anwendung werden acht Entitäten vorgesehen.

| Entität | Attribute |
|---|---|
| **Kunde** | Kunden-ID, Name, Adresse, E-Mail, Telefonnummer, Allergien, Kundentyp |
| **Produkt** | Produkt-ID, Name, Beschreibung, Zutaten, Allergene, vegan, Verkaufspreis, Wiederverkäuferpreis, Herstellungsdauer |
| **Charge** | Chargen-ID, Produkt-ID, Herstellungsdatum, produzierte Menge, Restmenge, Mindesthaltbarkeitsdatum |
| **Bestellung** | Bestell-ID, Kunden-ID, Bestelldatum, Bestellkanal, Bestellart, Gutscheincode, Status, Gesamtbetrag |
| **Bestellposition** | Positions-ID, Bestell-ID, Produkt-ID, Menge, Einzelpreis |
| **Zahlung** | Zahlungs-ID, Zahlungsdatum, Betrag, Zahlungsart, Zahlungsstatus, Verwendungszweck |
| **Lieferung** | Lieferungs-ID, Bestell-ID, Versanddatum, Versandart, Sendungsnummer, Lieferstatus |
| **Abonnement** | Abonnement-ID, Kunden-ID, Startdatum, Status, monatlicher Preis, Pausenbeginn, Pausenende, nächster Liefertermin |

Marketingaktionen und Gutscheincodes werden über die Bestelldaten erfasst. So können Bestellungen später einer Aktion zugeordnet werden.

## 4. Beziehungen

Die Entitäten stehen in folgenden Beziehungen zueinander:

1. Ein Kunde kann mehrere Bestellungen aufgeben. Eine Bestellung gehört jeweils zu einem Kunden (1:n).
2. Eine Bestellung kann mehrere Bestellpositionen enthalten. Jede Bestellposition gehört zu einer Bestellung (1:n).
3. Ein Produkt kann in mehreren Bestellpositionen vorkommen. Eine Bestellposition bezieht sich auf ein Produkt (1:n).
4. Ein Produkt kann in mehreren Chargen hergestellt werden. Jede Charge gehört zu einem Produkt (1:n).
5. Eine Bestellposition kann Produkte aus mehreren Chargen enthalten. Eine Charge kann wiederum für mehrere Bestellpositionen verwendet werden (n:m).
6. Eine Bestellung kann durch mehrere Zahlungen bezahlt werden. Eine Zahlung kann sich auch auf mehrere Bestellungen beziehen, zum Beispiel wenn ein Kunde mehrere Bestellungen gemeinsam überweist (n:m).
7. Eine Bestellung kann mehrere Lieferungen haben. Jede Lieferung gehört zu einer Bestellung (1:n).
8. Ein Kunde kann mehrere Abonnements haben. Jedes Abonnement gehört zu einem Kunden (1:n).
9. Ein Abonnement kann mehrere monatliche Bestellungen erzeugen. Jede dieser Bestellungen gehört zu einem Abonnement (1:n).

## 5. Geschäftsregeln (Business Rules)

**BR1: Verwendung älterer Chargen**

Bei der Zusammenstellung einer Bestellung soll zuerst die älteste geeignete Charge verwendet werden. Dadurch wird verhindert, dass ältere Produkte unnötig lange im Lager bleiben. Produkte dürfen außerdem erst verkauft werden, wenn die erforderliche Reifezeit abgeschlossen ist. Abgelaufene Produkte dürfen nicht mehr verkauft werden.

Quelle: M1-05

**BR2: Berücksichtigung von Allergien**

Allergien und Unverträglichkeiten müssen beim Kundenprofil gespeichert werden. Bei einer Bestellung und beim Verpacken muss ein deutlicher Hinweis erscheinen, wenn ein Produkt einen Stoff enthält, auf den der Kunde reagiert.

Quelle: M1-03, M1-04

**BR3: Gemeinsamer Lagerbestand**

Der Lagerbestand muss für alle Verkaufskanäle gemeinsam geführt werden. Verkäufe auf Märkten und über Instagram müssen berücksichtigt werden, damit ein Produkt nicht gleichzeitig mehrfach verkauft oder reserviert wird, obwohl es nicht mehr verfügbar ist.

Quelle: M1-05, M1-06, M1-09

**BR4: Zahlungsstatus und Versand**

Bei Online-Bestellungen muss vor dem Versand geprüft werden, ob die Zahlung eingegangen ist. Eine Bestellung darf erst als bezahlt markiert werden, wenn der Zahlungseingang bestätigt wurde. Für Wiederverkäufer gelten die vereinbarten Zahlungsbedingungen. Sie können ihre Bestellung auf Rechnung erhalten.

Quelle: M1-01, M1-02, M1-07, M1-11

**BR5: Bedingungen für Wiederverkäufer**

Wiederverkäufer erhalten eigene Preise und müssen die festgelegte Mindestbestellmenge erreichen. Die Zahlungsfrist beträgt 30 Tage. Die genauen Mindestmengen müssen noch festgelegt werden.

Quelle: M1-11

## 6. Widersprüche und Entscheidungen

**Widerspruch 1: Unterschiedliche Zahlungsmethoden**

Online-Bestellungen werden derzeit ausschließlich per Banküberweisung bezahlt. Auf Märkten nimmt Nina dagegen Bargeld und Kartenzahlungen über SumUp an.

**Entscheidung:** Die Anwendung muss mehrere Zahlungsarten unterstützen. Die Zahlungsart wird bei jeder Bestellung beziehungsweise Zahlung gespeichert.

Quelle: M1-01, M1-07

**Widerspruch 2: Unterschiedliche Aktualisierung des Lagerbestands**

Bei Online-Bestellungen prüft Nina den Bestand vorab in Excel. Verkäufe auf Märkten werden dagegen häufig erst am Ende des Tages eingetragen. Dadurch kann es passieren, dass ein Produkt online noch als verfügbar angezeigt wird, obwohl es auf dem Markt bereits verkauft wurde.

**Entscheidung:** Alle Verkaufskanäle sollen auf denselben Lagerbestand zugreifen. Verkäufe müssen möglichst direkt erfasst werden, damit die Bestandszahlen aktuell bleiben.

Quelle: M1-05, M1-06

## 7. Prioritäten

| Priorität | Funktion | Begründung |
|---|---|---|
| Hoch | Bestellungen verwalten | Der aktuelle Ablauf ist zeitaufwendig und wird manuell durchgeführt. |
| Hoch | Zahlungen überwachen | Offene und falsch zugeordnete Zahlungen führen zu Problemen beim Versand. |
| Hoch | Lagerbestand verwalten | Doppelverkäufe sollen vermieden werden. |
| Hoch | Allergiehinweise anzeigen | Allergien müssen beim Bestellen und Verpacken berücksichtigt werden. |
| Mittel | Chargen verwalten | Herstellungsdatum, Reifezeit und Mindesthaltbarkeit müssen nachvollziehbar sein. |
| Mittel | Lieferungen verwalten | Sendungsnummern und Lieferstatus sollen zentral gespeichert werden. |
| Mittel | Kundenverwaltung | Wiederkehrende Kunden und ihre Informationen sollen leichter verwaltet werden. |
| Mittel | Abonnements verwalten | Die monatlichen Boxen und ihre Zusammenstellung sollen planbar sein. |
| Niedrig | Marketingaktionen auswerten | Nina möchte besser erkennen können, welche Aktionen zu Bestellungen führen. |

## 8. Offene Fragen

Folgende Punkte müssen noch genauer geklärt werden:

1. Wie hoch soll die Mindestbestellmenge für Wiederverkäufer sein?
2. Sollen Kunden ihre normalen Bestellungen später selbst über eine App oder Website aufgeben können?
3. Welche Möglichkeiten zur automatischen Zahlungszuordnung sollen angeboten werden?
4. Wie sollen persönliche Abholungen und Lieferungen durch Nina selbst genau dokumentiert werden?
5. Wie sollen Rabattaktionen automatisch ausgewertet werden?
6. Wie sollen Bestandskorrekturen durchgeführt werden, wenn Produkte beschädigt werden oder nicht mehr verkauft werden dürfen?
7. Welche Zahlungsmethode soll für die monatlichen Abonnements verwendet werden?
8. Wie genau soll bei fehlgeschlagenen Abo-Zahlungen vorgegangen werden? Die im Meeting besprochenen Erinnerungen und erneuten Zahlungsversuche sind noch nicht endgültig festgelegt.
9. Über welchen Kanal sollen Kunden die monatliche Vorschau und Informationen zu Änderungen ihrer Box erhalten?

