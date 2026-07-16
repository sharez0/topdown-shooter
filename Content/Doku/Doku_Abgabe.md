# Dokumentation — OneShot

**Projekt:** Top-Down Wave-Shooter (Einzelarbeit, Hochschule Fulda)
**Engine:** Unreal Engine 5 (Blueprints only)
**Zielplattform:** Windows-Build für Arcade-Automat
**Stand:** 13.07.2026 — *bei jedem Meilenstein aktualisieren!*

---

## 1. Bedienungsanleitung

Das Spiel ist vollständig per Gamepad spielbar (Primäreingabe am Arcade-Automaten).
Tastatur/Maus wird zusätzlich unterstützt.

| Aktion | Gamepad | Tastatur / Maus |
|---|---|---|
| Bewegen | Linker Stick | W / A / S / D |
| Zielen | Rechter Stick | Maus |
| Schießen | RT/R2 | Linke Maustaste |
| Nachladen | LB/L1 | R |
| Dash / Ausweichrolle | A/X | Leertaste (Space) |
| Pause | *[noch nicht implementiert — nachtragen]* | *[noch nicht implementiert]* |
| Menü-Navigation | *[noch nicht implementiert — nachtragen]* | *[noch nicht implementiert]* |

**Ziel-Verhalten:** Das Fadenkreuz zeigt die aktuelle Schussrichtung an.
- **Maus:** Fadenkreuz folgt frei der Cursor-Position.
- **Gamepad:** Fadenkreuz kreist in festem Abstand um die Spielfigur in Stick-Richtung.
- Es gewinnt automatisch das zuletzt bewegte Eingabegerät — kein manuelles Umschalten nötig.

**Spielprinzip:** One-Shot-One-Kill in beide Richtungen — jeder Treffer tötet,
sowohl Gegner als auch Spieler. Pro Level spawnen 100 Gegner in Etappen
(Batches von 5–10). Sind alle 100 besiegt, ist das Level geschafft.
*[Später ergänzen: Boss, weitere Level/Modi, Game Over/Restart-Ablauf]*

---

## 2. Asset- & Quellenliste

### Extern bezogene Assets

| Asset | Verwendung | Quelle | Lizenz/Anmerkung |
|---|---|---|---|
| Crosshair-Texturen (5 Stück: Center, Top, Bottom, Left, Right) | Fadenkreuz (Maus- & Gamepad-Aiming) | UE5-Kurs: Code Your First Four Game Projects in Unreal Engine 5 with Blueprint Visual Scripting - From Beginner to Advanced! | Erlaubt |

### Selbst erstellte Inhalte

| Asset | Verwendung | Erstellt mit und von |
|---|---|---|
| In-Game-Musik (Loop) | Hintergrundmusik während des Spiels | FL-Studio/prodz.Otosan |

### Engine-Inhalte (Unreal Engine Standard)

| Asset | Verwendung |
|---|---|
| Plane (Static Mesh, Engine Content) | Crosshair-Elemente (5 Planes) |
| Niagara-Partikelsystem aus UE5 Kurs | Treffer-Effekt (Burst) beim Projektil-Einschlag |

### Noch einzutragen, sobald verwendet

- 3D-Modelle für Spieler / Gegner / Umgebung (aktuell Platzhalter? → bei Ersatz Quelle eintragen)
- Sound-Effekte (Schuss, Treffer, Tod, Dash, UI-Sounds)
- Fonts für die Menüs (auch Google Fonts o. Ä. gehören in die Liste!)
- UI-Grafiken (Buttons, Logos, Icons)
- Weitere Musik (Menü-Musik, Boss-Musik)

---

## 3. Technischer Überblick (intern, hilfreich für Abgabe-Fragen)

**Blueprint-Struktur (Stand jetzt):**
- `BP_GameMode` — Default Pawn/Controller-Zuweisung, In-Game-Musik (Loop via Spawn Sound 2D)
- `BP_TopDownPlayerController` + `BP_TopDownCameraManager` — Kamera-Kette
- `BP_PlayerPawn` — Bewegung (Floating Pawn Movement), Aim-System beide Geräte, Dash, Schießen
- `BP_EnemyBase` + `BP_EnemyAIController` — Pathfinding (NavMesh, timerbasiert), State-Machine (Chasing/Attacking), Schuss-Logik
- `BP_BlasterBeam` / `BP_EnemyBlasterBeam` — Projektile mit eigenen Collision-Channels
- `BP_EnemySpawner` + `BP_SpawnPoint` — Etappen-Spawning (100/Level, Batches, MaxAlive-Limit)
- `BP_Crosshair` — Fadenkreuz-Actor (5 Planes, Unlit-Material-Instances)
- `BPI_Damageable` — Interface für One-Shot-Kill in beide Richtungen

**Eingabesystem:** Enhanced Input, ein IMC (`IMC_Player`) mit Gamepad- und
Tastatur/Maus-Mappings. Maus-Aiming per Tick-Polling (Get Mouse Position),
nicht event-basiert.

**Fix was noch gemacht werden muss**
- gegner hinter wänden erkennen mich trotzdem und schießen
- Dash auf tastatur zu weit wenn A+W oder D+W

**Noch offen (Fahrplan):**
- [ ] Game Over / Restart-System
- [ ] Menüs: Main Menu, Pause, Settings, Credits (Pflicht)
- [ ] In-Game-Tutorial (selbsterklärend, eingeblendet)
- [ ] Boss-Fight
- [ ] Score / HUD
- [ ] Sound-Effekte & Polish
- [ ] Windows-Build + Test auf Zielhardware
- [ ] Lokaler Multiplayer prüfen: *Achtung — Aufgabenstellung erwähnt zwei
  Controller am Test-PC. Klären, ob 2-Spieler-Pflicht besteht oder ob
  Singleplayer (wie geplant) ausreicht!*
