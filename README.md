# Der Fluch der Sonnenfratze

Ein 3D-Rollenspiel im PS1-Look, das komplett im Browser läuft – eine einzige Datei (`index.html`).

**Spielen:** https://yarak5113-alt.github.io/The-Witcher-3/

## Features
- Riesige offene Welt mit der gemütlichen Hügelstadt Butterbühl, Höfen, Häfen, Minen und Bossen
- 5 Klassen: Sternenmagier, Feuerhexer, Sonnenritter, Mooshüter, Glücksstrolch
- Quests, Talente, Beute, Reisebesen, Weltkarte
- Online-Mehrspieler über Firebase: Alle, die das Spiel öffnen, treffen sich in derselben Welt
- Mitspieler auf der Karte, eigene Namen, Tag/Nacht per Abstimmung (Klick auf die Uhr)

## Steuerung
`WASD` laufen · Maus ziehen Kamera · `Leertaste` springen · Klick/`F` zaubern · `E` sprechen ·
`H` Heiltrank · `L` Questbuch (Quest verfolgen) · `I` Inventar · `K` Skilltree · `O` Klasse wechseln · `M` Karte ·
`B` Reisebesen · `Enter` Chat · `1`–`6` Gesten (`5` Caramell-Tanz, `6` UwU) · `G` Freunde heilen · `V` Kameraansicht · `P` Einstellungen

Der Spielstand wird automatisch im Browser gespeichert.

## Konten (Online-Spielstand)
Spieler können ein Konto erstellen (Name oder E-Mail + Passwort) und sich auf jedem Gerät anmelden.
Der Spielstand wird dann zusätzlich in der Firebase Realtime Database unter `sonne/konten/<uid>` gespeichert.

Einmalig in der [Firebase-Konsole](https://console.firebase.google.com/) einrichten:
1. **Authentication → Anmeldemethode → E-Mail/Passwort** aktivieren.
2. **Projekteinstellungen → Allgemein → Web-API-Schlüssel** kopieren und in `index.html` bei `const FB_KEY=''` eintragen.
3. **Realtime Database → Regeln** so setzen (die Welt bleibt offen, Spielstände kann nur ihr Besitzer lesen und schreiben):

```json
{
  "rules": {
    "sonne": {
      "welt": { ".read": true, ".write": true },
      "konten": {
        "$uid": {
          ".read": "auth != null && auth.uid === $uid",
          ".write": "auth != null && auth.uid === $uid"
        }
      }
    }
  }
}
```

Ohne Schlüssel bleibt der Konto-Bereich auf dem Titelbildschirm ausgeblendet, das Spiel speichert dann wie bisher nur im Browser.
