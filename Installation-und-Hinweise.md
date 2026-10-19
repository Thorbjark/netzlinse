# NetzLinse 1.0.0

Entwicklung und Umsetzung: NordPing  
Entwickler: Thørbjærk  
Feedback: Thorbjark@proton.me

## Installation

Die fertige, signierte APK liegt in `dist/NetzLinse-1.0.0.apk`.
Auf das Android-Gerät kopieren, öffnen und die Installation durch die verwendete Datei-App erlauben.
Mindestens Android 8.0 (API 26). Kein Root erforderlich. Paketname: `de.netzlinse.app`.
Die APK ist eine eigenständig installierbare Universal-APK für die unterstützten Gerätearchitekturen.
Die ältere Entwicklungsversion mit dem Paketnamen `de.zenmap.android` und die neue Debug-Version sind getrennte Apps.

## Funktionen

Neun Seiten per horizontalem Swipe oder Registerauswahl: Port Scan, Ping, Datenverkehr,
Netzwerkschnittstellen, WLAN-Radar, DNS, Geräte im Netzwerk, TXT/CSV-Export und Über.

- Schnellscan: 30 häufige TCP-Ports.
- Standardscan: TCP-Ports 1–1024.
- Vollständiger Scan: TCP-Ports 1–65535.
- Benutzerdefiniert: einzelne Ports und Portbereiche, Zeitlimit 100–5000 ms.
- Einzelziele: IPv4, IPv6 oder Hostname; Netzbereiche: IPv4 /24 bis /32.
- Maximal 32 parallele TCP-Verbindungen; maximal 2 Millionen Verbindungsversuche pro Scan.
- Kein vorgelagerter Ping-Zwang: Ein blockierter Ping verhindert keinen Port-Scan.
- Abbrechen erhält Teilergebnisse; Scan-Zustand bleibt bei Bildschirm-Neuerstellung erhalten.
- Ein Verlust der aktiven Netzwerkverbindung beendet den Port-Scan mit Teilergebnis.
- Offene Ports und antwortende Geräte werden in die Geräteübersicht übernommen.
- Abgeschlossene Ergebnisse bleiben lokal gespeichert. Export bietet TXT und korrekt maskiertes UTF-8-CSV.
- CSV-Zellen werden gegen Formelausführung durch Tabellenprogramme abgesichert.
- Feedback öffnet eine Mail-App; kein automatischer Versand.

## Berechtigungen und praktische Grenzen

Android 17 und neuer: lokalen Netzwerkzugriff in der App freigeben.
WLAN-Radar: WLAN einschalten sowie genauen Standortzugriff und Android-Standortdienste erlauben.
Android begrenzt die Häufigkeit von WLAN-Scans. Gespeicherte Messwerte werden als solche gekennzeichnet.
Ein Emulator zeigt keine realen WLANs in der Umgebung des PCs.
Geräte, die sämtliche Erreichbarkeitstests blockieren, können in der Gerätesuche fehlen.
Port-Scans unterscheiden offene, ausdrücklich geschlossene, unbeantwortete und fehlerhafte Verbindungen.
Ohne Antwort bedeutet nicht automatisch, dass ein Port geschlossen ist.
Die Verkehrsanzeige misst ausschließlich den Datenverkehr der App, nicht den Gesamtverkehr anderer Apps.
Es gibt keine UDP-/SYN-Scans, Betriebssystemerkennung oder Nmap-Scripting-Engine.
Es wird kein Nmap-/Zenmap-Programmcode eingebunden.

## Signatur und spätere Updates

`signing/netzlinse-release.jks` ist der dauerhafte Release-Signaturschlüssel.
`signing/keystore.properties` und `signing/store-password.txt` enthalten die Zugangsdaten.
Den gesamten Ordner `signing` privat und getrennt vom Computer sichern; nicht mit der APK verteilen.
Für Updates denselben Schlüssel verwenden und `versionCode` in `app/build.gradle.kts` erhöhen.
Release- und Debug-Version verwenden unterschiedliche Signaturen und Paketnamen.

## Erneut bauen

Android Studio mit installiertem SDK 37 und JDK (mitgelieferte JBR) verwenden.

```powershell
$env:JAVA_HOME = 'C:\Program Files\Android\Android Studio\jbr'
.\gradlew.bat :app:assembleRelease :app:testDebugUnitTest :app:lintRelease --no-configuration-cache
```

Ergebnis: `app/build/outputs/apk/release/app-release.apk`.
Build-Datum (Europe/Berlin), Version und Buildnummer werden in der Über-Seite aus BuildConfig angezeigt.

## Verifikation

Unit-Tests prüfen Portbereiche, Subnetze, echte offene/geschlossene lokale TCP-Sockets,
Scan-Fortschritt, Abbrechen, DNS-Zeitlimits und CSV-Maskierung.
Instrumentierung prüft neun Swipe-Seiten, DNS, Geräteerkennung, WLAN-Scan-Ablauf,
Port-Scan gegen einen lokalen Testserver und Zustandserhalt bei Activity-Neuerstellung.
TXT und CSV werden im Emulator tatsächlich über den ContentResolver in Dokumente geschrieben
und einschließlich Unicode-Inhalt wieder eingelesen; der Speicherort-Dialog wird dafür im Test simuliert.
Signatur und Paketdaten der ausgelieferten Release-APK werden separat geprüft.
Physische Android-Geräte und reale WLAN-Messungen wurden nicht getestet.
