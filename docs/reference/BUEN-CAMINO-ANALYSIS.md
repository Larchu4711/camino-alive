# Buen Camino 4.10.0: Referenzanalyse für Camino Alive

Stand: 12. Oktober 2026. Dies ist eine statische Analyse der vom Nutzer bereitgestellten XAPK, keine fertig implementierte App und kein Nachweis vollständiger Funktionsgleichheit.

## Nutzerziel

Camino Alive soll zunächst das Grundgerüst, die Bedienabläufe und die Inhalte der bereitgestellten Buen-Camino-Vorlage möglichst exakt nachbilden. Anschließend sollen zusätzliche Features und Ideen darauf aufbauen. Die zusätzlichen Features sind bislang nicht konkret benannt. Ruflo und Superpowers wurden ausdrücklich angefordert.

## Methode und Nachweise

- XAPK und enthaltene ZIP-Archive lokal gelesen; Originaldatei unverändert.
- Paketmanifest, Asset-Inventar, Flutter-Bibliotheken, öffentlich erkennbare Funktionsnamen und Inhaltsdateistrukturen geprüft.
- Unverschlüsseltes SQLite-Lesen versucht: `sqlite3.DatabaseError: file is not a database`.
- Keine Entschlüsselung, Anmeldung, Backend-Anfrage oder Verwendung eingebetteter API-Schlüssel durchgeführt.
- ZIP-Integritätsprüfung der XAPK erfolgreich.
- Keine Installation und keine Laufzeitprüfung der Original-App durchgeführt.
- Maschinenlesbare Zusammenfassung: [buen-camino-4.10.0-inventory.json](buen-camino-4.10.0-inventory.json).

## Identität des Pakets

| Merkmal | Befund |
|---|---|
| App | Buen Camino |
| Paket | `com.editorialbuencamino.buencamino` |
| Version | 4.10.0, Versionscode 508 |
| XAPK-Größe | 60.790.143 Bytes |
| SHA-256 | `5fdfe23eca4302d252b96b66841cd5cbe8fc5c03f7111e8ed4c706d158ca5c6f` |
| Basis-APK | 25.603.153 Bytes |
| APKs im Paket | 20: Basis plus 19 Split-APKs |
| CPU-Architektur | arm64-v8a |
| Android SDK | Minimum 24, Target 36 laut Paketmanifest |
| Technologie | Flutter, mit `libflutter.so` und `libapp.so` |
| Dart-Quelldateien | Keine `.dart`-Dateien in der Basis-APK |

Die App-Logik liegt als kompilierte native Flutter-Bibliothek vor. Das APK-Archiv ist daher kein ursprüngliches, direkt erweiterbares Flutter-Quellprojekt. Der [offizielle Flutter-Leitfaden](https://docs.flutter.dev/deployment/android) beschreibt diese Release-Paketierung.

## Enthaltene Inhalte

| Bestand | Umfang | Status |
|---|---:|---|
| App-Assets insgesamt | 374 | Inventar erstellt |
| Icons und Bilder | 306: 88 SVG, 207 PNG, 11 JPG | Lokal zur Referenz extrahiert |
| Planer-Tutorials | 54 MP4: sechs pro Sprache | Lokal extrahiert; Videos noch nicht visuell ausgewertet |
| Hilfe und Tipps | neun JSON-Dateien | Struktur gelesen |
| Hilfe-Sprachen | de, en, es, fr, it, ko, pl, pt, zh | Im Paket nachgewiesen |
| Hilfe-Abschnitte | je 21; Chinesisch 22 | Gezählt |
| Neuigkeiten | JSON mit neun Sprachbereichen | Struktur gelesen |
| Datenbank | `BuenCamino_v2.db`, 4.624.384 Bytes | Inhalt bislang nicht lesbar |
| Umgebungskonfiguration | zwei Dateien | Werte nicht veröffentlicht oder übernommen |

Keine separaten GPX-, GeoJSON-, KML- oder MBTiles-Dateien wurden im Basis-APK-Dateiinventar gefunden. Das beweist nicht, dass Routendaten fehlen: Sie können in der Datenbank oder in späteren Downloads liegen.

## Funktionsabdeckung und Evidenz

| Bereich | Lokale Hinweise | Noch zu verifizieren |
|---|---|---|
| Routen und Varianten | `GuiaRutaScreen`, `TrackRuta`, `RouteDetail` | Datensätze, genaue Navigation, Auswahlverhalten |
| Karte und Standort | `MapScreen`, `MapaBloc`, `FiltroMapaDialog` | Layout, Marker, Filter und Positionierung im Betrieb |
| Offline-Karten | `DownloadMapsNotifier`, `NuevosMapasScreen` | Kartendaten, Downloadformat, Anbieter und Offline-Verhalten |
| Höhenprofil | `PerfilScreen`, `TrackPerfil` | Geometrie, Höhenwerte und Interaktion |
| Etappenplaner | `NewPlanificadorRepository`, `PlanificadorPpalScreen`, sechs Tutorialschritte | Neue/alte Planerabläufe und persistierte Daten |
| Orte und Unterkünfte | `LocalidadScreen`, `Alojamiento`, `Alojamientos` sowie Unterkunftsicons | Vollständigkeit, Detailfelder, Fotos und Öffnungszeiten |
| Wetter | `WeatherRepository`, `OpenWeatherMapDatasource` | Eigene Konfiguration und Laufzeitverhalten |
| Benutzer und Sync | `LoginScreen`, `SyncroRepository` | Eigene Identität, Backend-Vertrag und Konfliktverhalten |
| Teilen | `PlanSharingRepository`, `LocationSharingRepository`, `ContentSharingRepository` | Rechte, Empfänger, Laufzeit und Datenfluss |
| Abo | `SubscriptionScreen`, Billing-Berechtigung | Konkreter Funktionsumfang und eigenes Produktmodell |
| Gebetsführer | `GuiasOracionScreen` | Texte und Sprachabdeckung |
| KI-Chat | `goToChatIaScreen`, `CaminoIaApi` | Tatsächliche Funktion und eigene Dienstanbindung |
| Bewertungen | `ValoracionesRepository`, `TopRatedAlojamientosScreen` | Datenquelle und Bedienablauf |

Ein Klassenname ist ein Hinweis auf einen Funktionsbereich. Er beweist weder Erreichbarkeit noch Vollständigkeit der Funktion in Version 4.10.0.

Die [offizielle Produktseite](https://www.editorialbuencamino.com/app-del-camino-de-santiago/) bestätigt Karten, Varianten, Höhenprofile, Etappenplanung, Offline-Nutzung, Orts-/Unterkunftsinformationen und Wetter. Ihre aktuelle Beschreibung nennt 51 Routen. Dieser Wert ist eine Herstellerangabe zur aktuellen App, kein aus der bereitgestellten Datenbank ermittelter Bestand.

## Grenze von „inklusive allen Inhalten“

Die Datenbank besitzt keinen normalen SQLite-Header. Das Paket enthält `libsqlcipher.so` sowie SQLCipher-bezogene Klassen. Zusammen spricht dies deutlich für eine verschlüsselte Datenbank; deren Tabellen, Zeilenzahlen und Inhalte sind noch nicht nachgewiesen.

Es gibt außerdem Hinweise auf Downloads, Remote-Datenquellen und Synchronisierung. Ob alle benötigten Routentexte, Fotos, Geometrien, Unterkünfte und aktuellen Hinweise bereits im Paket enthalten sind, ist daher offen. Eine vollständige Inhaltsübernahme kann erst nach einem lesbaren Export und einem Abgleich der Bestände bestätigt werden. Ein vorhandenes Original-Quellprojekt würde die Übernahme des Grundgerüsts wesentlich vereinfachen.

## Vergleich der Umsetzungswege

1. **Eigenes Flutter-Projekt anhand der Referenz rekonstruieren (Empfehlung).** Gleiche UI-Technologie; Module, Datenimporte und spätere Camino-Alive-Erweiterungen bleiben bearbeitbar. Vollständige Datenbasis und ein visueller Laufzeitvergleich werden weiterhin benötigt.
2. **Eigenes natives Android-Projekt.** Gute Android-Integration, aber die Flutter-Oberfläche müsste in einer anderen Technologie nachgebaut werden. Für die geforderte Referenznähe entsteht zusätzlicher Aufwand.
3. **Vorhandenes Original-Quellprojekt übernehmen, falls verfügbar.** Am nächsten am gewünschten Wiederverwenden des Grundgerüsts. Die XAPK allein liefert dieses Projekt nicht; bloßes Umpacken der Binärdatei schafft keine wartbare Entwicklungsbasis.

## Vorgeschlagene erste Architektur zur Besprechung

Noch nicht freigegeben oder implementiert:

- Eigene Flutter-Anwendung mit eigener Paketidentität für Camino Alive.
- Funktionsmodule für Routen, Karte/Profil, Orte/Unterkünfte, Etappenplanung und allgemeine Inhalte.
- Ein Datenzugriff pro Fachbereich; lokale Speicherung für Offline-Nutzung, optionaler Remote-Zugriff dahinter. Diese Trennung entspricht dem [Flutter-Leitfaden für Offline-Architektur](https://docs.flutter.dev/app-architecture/design-patterns/offline-first).
- Ein Importformat mit Herkunft, Sprache und Version für Routen, Wegpunkte, Unterkünfte, Texte und Medien. Belegte Referenzdaten und noch fehlende Bestände werden getrennt ausgewiesen.
- Externe Dienste über austauschbare Adapter. Neue Zugangsdaten und eigener Backend-Zugriff statt eingebetteter Fremd-Konfiguration.
- Baseline zuerst anhand einer Paritätsliste prüfen; zusätzliche Camino-Alive-Features anschließend als getrennte Ausbauschritte.

## Abnahme der Baseline

- Alle vereinbarten Screens und Bedienabläufe haben einen beobachteten Referenzfall.
- Routen-, Orts-, Unterkunfts-, Text- und Medienbestände werden gegen einen lesbaren Referenzexport gezählt und verglichen.
- Karte, Profil und Planer beziehen sich auf dieselben Route- und Wegpunktdaten.
- Planung und heruntergeladene Inhalte funktionieren ohne Netzwerk.
- Import, Geometrie, Entfernungen, Planerspeicherung und Sync-Verhalten erhalten passende Prüfungen.
- UI-Vergleich auf festgelegter Bildschirmgröße und Sprache; App auf einem Android-Gerät oder Emulator starten und prüfen.
- Erst ein erfolgreicher Build mit erfolgreichem Start bestätigt eine nutzbare APK.

## Werkzeuge und Projektablage

- **Superpowers:** `using-superpowers` und `brainstorming`; als Architekturarbeit klassifiziert. Die verlangte Entwurfs- und Planprüfung ist noch offen.
- **Ruflo:** Aufgabe erfasst; technische Befunde mit Herkunft `tool_result` im Namespace `camino-alive` gespeichert.
- **Context7:** aktuelle offizielle Flutter-Dokumentation zur Offline-Architektur geprüft.
- **GitHub:** Analyse auf dem Branch `codex/buen-camino-reference`.
- Lokale Referenzpakete, extrahierte Originalinhalte, `.swarm/` und `.claude-flow/` sind ausgeschlossen. Es werden nur bereinigte Analyseergebnisse versioniert.

Im aktuellen Terminal wurden keine ausführbaren Flutter-, Dart-, Java-, ADB- oder Gradle-Werkzeuge gefunden. Ein Android-Build wurde nicht versucht. Die Werkzeuginstallation gehört zur späteren Implementierungsphase.
