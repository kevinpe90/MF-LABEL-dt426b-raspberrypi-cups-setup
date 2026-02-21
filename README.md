# DT426B / MFLABEL 4x6 – Raspberry Pi Setup

Dieses Repository enthält eine vollständige Schritt‑für‑Schritt Anleitung, um einen **DT426B / MFLABEL 4x6 Thermo‑Labeldrucker** auf **Raspberry Pi OS** mit **CUPS + TSPL** stabil zum Laufen zu bringen.

Ziel:

➡ PDFs direkt aus **Chrome / iPhone / PDF‑Viewer** drucken  
➡ kein RAW, kein Zebra  
➡ echtes TSPL  
➡ kein USB‑Sleep  
➡ nur 4x6 (optional 4x8) Formate sichtbar  
➡ industrie‑stabiles Setup

---

## 📄 Inhalt

- `DT426B_RaspberryPi_Setup.md`  
  → komplette technische Anleitung

Empfohlen: Diese Datei als Referenz behalten, falls der Pi jemals neu installiert werden muss.

---

## ✅ Getestet mit

- Raspberry Pi 5 (aarch64)
- Raspberry Pi OS Desktop (Bookworm)
- DT426B / MFLABEL USB Labeldrucker
- CUPS 2.4
- TSPL Filter (rpi-tspl-cups-driver)

---

## 🎯 Ergebnis

Nach dem Setup:

- Drucken direkt aus Chrome / iOS AirPrint
- kein Piepen
- kein „Waiting for printer“
- saubere 4x6 Labels
- korrigierter linker Rand
- CUPS startet automatisch neu
- USB schläft nicht ein

Kurz: es funktioniert wie ein professioneller Versandplatz.

---

## 🧠 Warum TSPL?

Diese DT426B Geräte sind TSC‑Klone.

Sie sprechen **TSPL**, nicht PDF, nicht ZPL.

Darum wird:

PDF → CUPS Raster → TSPL → Drucker

verwendet.

---

## ⭐ Empfehlung

Nach erfolgreichem Setup:

- Repo behalten
- bei Änderungen committen
- als persönliches Backup verwenden

---


