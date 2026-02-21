# DT426B Labeldrucker (4x6) auf Raspberry Pi OS – Komplett-Setup

Getestet mit:
- Raspberry Pi 5 (aarch64)
- Raspberry Pi OS mit Desktop
- DT426B / MFLABEL 4x6 Thermodrucker
- CUPS

Ziel:
PDF direkt aus Chrome / iPhone / PDF-Viewer drucken → Label kommt raus (TSPL)

---

## 1. CUPS installieren

sudo apt update  
sudo apt install cups cups-filters git poppler-utils imagemagick

Benutzer zu Druckergruppen hinzufügen:

sudo usermod -aG lp kevin  
sudo usermod -aG lpadmin kevin  

(Reboot oder ab-/anmelden)

---

## 2. CUPS Weboberfläche aktivieren

sudo cupsctl --remote-any  
sudo systemctl restart cups  

Browser:
http://localhost:631

---

## 3. TSPL Treiber installieren

cd /tmp  
git clone https://github.com/thorrak/rpi-tspl-cups-driver.git  
cd rpi-tspl-cups-driver  

Architektur prüfen:

uname -m  

TSPL Filter installieren:

sudo cp filters/aarch64/cups_2.4.0/raster-tspl /usr/lib/cups/filter/  
sudo chmod 755 /usr/lib/cups/filter/raster-tspl  
sudo chown root:root /usr/lib/cups/filter/raster-tspl  
sudo systemctl restart cups  

---

## 4. Alte / automatische Drucker löschen

sudo lpadmin -x DT426B  
sudo lpadmin -x DT426B_rpi5moonlight  
sudo lpadmin -x Labeldrucker_4x6_DT426B_rpi3cups_2  

lpstat -p  

---

## 5. Drucker mit TSPL PPD neu anlegen

sudo lpadmin -p DT426B -E \
 -v "usb://MFLABEL/DT426B?serial=264A211160135" \
 -P "/tmp/rpi-tspl-cups-driver/ppd/sp420.tspl.ppd"

sudo cupsenable DT426B  
sudo cupsaccept DT426B  

lpstat -p DT426B -l  

---

## 6. Funktionstest

echo -e "SIZE 100 mm,150 mm
GAP 2 mm,0 mm
CLS
TEXT 100,100,\"3\",0,1,1,\"HELLO\"
PRINT 1
" | lp -d DT426B

---

## 7. USB Autosuspend deaktivieren

sudo mousepad /boot/firmware/cmdline.txt  

Am Ende der Zeile hinzufügen:

usbcore.autosuspend=-1

Reboot.

---

## 8. CUPS Auto-Restart

sudo mkdir -p /etc/systemd/system/cups.service.d  
sudo mousepad /etc/systemd/system/cups.service.d/override.conf  

Inhalt:

[Service]
Restart=always
RestartSec=5

sudo systemctl daemon-reexec  
sudo systemctl restart cups  

---

## 9. Linken Rand korrigieren

sudo mousepad /etc/cups/ppd/DT426B.ppd  

Suchen:

*ImageableArea w100h150/4"x6": "0 0 283 425"

Ändern zu:

*ImageableArea w100h150/4"x6": "0 0 267 425"

sudo systemctl restart cups  

---

## 10. Nur 4x6 / optional 4x8 anzeigen

Im selben PPD:

*OpenUI *PageSize

Nur behalten:

*PageSize w100h150/4"x6": "<</PageSize[288 432]>>setpagedevice"

Optional:

*PageSize w100h200/4"x8": "<</PageSize[288 576]>>setpagedevice"

sudo systemctl restart cups  

---

## 11. Drucken

4x6 PDF → Skalierung 100 %  
4x8 PDF → An Seite anpassen  

---

Ergebnis:
TSPL Treiber, stabile USB Verbindung, nur relevante Formate sichtbar.
