# 📋 Dokumentation: Planungs- & Analysedokumentation zur iPhone 11 Pro Wiederherstellung

**Dokumententyp:** System- & Prozessdokumentation  
**Ziel-Hardware:** iPhone 11 Pro  
**Host-Plattform:** Windows 11 PC / Laptop  
**Software-Komponenten:** Apple Devices App (Apple-Geräte-App), Windows Device Manager  

---

## 📑 1. Einleitung & Zielsetzung

Dieses Dokument beschreibt die strukturiere Planungs- und Vorbereitungsphase für die Wiederherstellung (Factory Restore) eines iPhone 11 Pro auf die Werkseinstellungen unter Windows 11. 

Ziel der Planungsphase war es, potenzielle Fehlerquellen auf Betriebssystem- und Hardwareebene zu identifizieren, Treiberkonflikte präventiv zu beheben und die optimale Verbindungsinfrastruktur vor dem Flashen der Firmware festzulegen.

---

## 🔬 2. Systemanalyse & Systemanforderungen

### 2.1 Hardware-Konfiguration
* **Smartphone:** iPhone 11 Pro (Lightning-Schnittstelle)
* **Host-System:** Windows 11 
* **Schnittstellen:** USB-C (Power Delivery / UCSI Controller) sowie USB-A Ports

### 2.2 Identifizierte Fehlerquellen & Risiken (Risk Assessment)
Vor Beginn der Firmware-Wiederherstellung wurden folgende kritische Schwachstellen analysiert:
1. **Treiber-Blockade durch ACPI (UCM-UCSI Controller):**
   * *Risiko:* Code 10 / Gelbes Ausrufezeichen im Windows Geräte-Manager führt zu Verbindungsabbrüchen während der Recovery-Phase.
   * *Auswirkung:* Abbruch des Schreibvorgangs der Firmware (Error 1109).
2. **Kabel- und Port-Inkompatibilität:**
   * *Risiko:* Verwendung reiner Ladekabel (ohne Datenleitungen) oder instabiler USB-Hubs.
3. **Power-Management-Interrupts:**
   * *Risiko:* Automatische Energieeinsparung der USB-Root-Hubs durch Windows während langer Lade- und Flashzyklen.

---

## 🛠️ 3. Technische Vorbereitung & Treiber-Hardening

Vor Initiierung des Recovery-Modus wurden am Host-System folgende Präventivmaßnahmen durchgeführt:

```
[Windows 11 Host] 
       │
       ├──> 1. Geräte-Manager: UCM-UCSI ACPI-Gerät zurücksetzen
       ├──> 2. USB-Root-Hubs: Treiber-Reset & Re-Initialisierung
       └──> 3. Kaltstart (10s Stromlos) ──> Treiber-Entladung
```

### 3.1 Entkopplung des USB-Connector-Managers (UCM-UCSI)
* **Maßnahme:** Manuelles Deinstallieren des fehlerhaften Treiber-Eintrags unter *USB-Connector-Manager*.
* **Ziel:** Erzwingen einer sauberen Neu-Initialisierung der USB-C Power-Delivery-Steuerung.

### 3.2 Kaltstart-Prozedur (Power Cycle)
* **Maßnahme:** Vollständiges Herunterfahren des Laptops (kein Warmstart) für mindestens 10 Sekunden.
* **Ergebnis:** Vollständige Entladung der Mainboard-Kondensatoren und Bereinigung blockierter Register der USB-Controller.

---

## 📱 4. Hardware-Spezifikation & Modus-Vorbereitung

### 4.1 Kabel-Spezifikation
* **Pflicht-Komponente:** Original USB-C auf Lightning-Kabel (Data-Sync-fähig).
* **Ausschluss:** Drittanbieter-Ladekabel ohne Datenübertragungs-Pins.

### 4.2 Befehlssequenz für den Recovery-Modus (DFU / Recovery Vorbereitung)
Zur sicheren Initialisierung der Wiederherstellungsschnittstelle wurde folgender Ablauf festgelegt:
1. Verbinden des iPhones per USB-C an den Host-PC.
2. Tastenkombination zur Auslösung des Hardware-Bootloaders:
   * Kurz **Lauter-Taste**
   * Kurz **Leiser-Taste**
   * Lang gedrückt halten der **Seitentaste (Power-Button)** bis zum Erscheinen des Wiederherstellungs-Bildschirms (Support-URL + Kabel/PC-Symbol).

---

## 📋 5. Checkliste für die Ausführungsphase

- [x] Windows 11 Treiber für Apple Mobile Devices im Geräte-Manager überprüft.
- [x] UCM-UCSI ACPI Controller entladen und neu gestartet.
- [x] Apple-Geräte-App auf den neuesten Stand aktualisiert.
- [x] Batteriestand des Host-Laptops / Netzteil angeschlossen.
- [x] Datenkabel auf physische Beschädigung und Datentauglichkeit geprüft.

---
> **Hinweis:** Die eigentliche Übertragung von Nutzerdaten (Fotos/Videos) sowie der Flash-Durchlauf selbst sind Gegenstand der nachgelagerten Ausführungsdokumentation.