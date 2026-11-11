# 🏓 Tischtennis Liga

Eine einfache Web-App für Tischtennis mit Freunden: Spieler eintragen, Saisons anlegen, Spiele erfassen und die Tabelle automatisch berechnen lassen. Läuft komplett kostenlos auf GitHub Pages mit Supabase als Datenbank und ist mit einem gemeinsamen Passwort geschützt.

*created by XiN@NBeY*

## Funktionen

- **Spieler** hinzufügen und löschen
- **Saisons** anlegen, jeweils mit eigenem Spielmodus und Satzformat
- **Spiele** mit Satzergebnis eintragen, einzelne Spiele wieder löschen
- **Tabelle** mit Spielen, Siegen, Niederlagen, Sätzen und Punkten
- **Gemeinsame Daten:** Alle sehen denselben Stand, die Seite aktualisiert sich alle 15 Sekunden
- **Passwortschutz:** Ohne Login sind keine Daten lesbar oder änderbar
- Für das Handy optimiert, mit hellem und dunklem Modus

## Spielmodi

| Einstellung | Auswahl |
| --- | --- |
| Spielmodus | Hin- und Rückrunde (2× gegen jeden), nur Hinrunde (1× gegen jeden), freies Spielen |
| Satzformat | Best of 3, Best of 5, Best of 7 oder beliebig |

Die App prüft beim Eintragen, ob ein Duell nicht öfter gespielt wurde als erlaubt und ob das Satzergebnis zum Format passt (bei Best of 5 z. B. nur 3:0, 3:1 oder 3:2).

## Wertung

- Sieg = **2 Punkte**, Niederlage = 0 Punkte
- Bei Punktgleichheit entscheidet die **Satzdifferenz**, danach die Zahl der gewonnenen Sätze

## Technik

- Eine einzelne Datei: `index.html` (HTML, CSS und JavaScript, keine Bibliotheken)
- Hosting: GitHub Pages
- Datenbank und Login: Supabase (Free-Plan)

## Einrichtung

### 1. Supabase

1. Auf supabase.com ein neues Projekt anlegen.
2. Im **SQL Editor** diesen Code ausführen:

```sql
create table state (
  id text primary key,
  json text not null,
  updated_at timestamptz default now()
);
alter table state enable row level security;
create policy "login lesen" on state for select to authenticated using (true);
create policy "login aendern" on state for update to authenticated using (true) with check (true);
insert into state (id, json)
values ('liga', '{"players":[],"seasons":[],"matches":[],"cur":null}');
```

3. Unter **Authentication → Users → Add user** einen Benutzer mit der E-Mail `liga@tischtennis.app` und dem gewünschten Gruppenpasswort anlegen, mit gesetztem Häkchen bei **Auto Confirm User**.
4. Unter **Authentication → Sign In / Providers** die Option „Allow new users to sign up" ausschalten.
5. Unter **Project Settings → API** die **Project URL** und den **publishable (anon) Key** kopieren.

### 2. Zugangsdaten in der Seite

In `index.html` ganz oben im Skript diese beiden Zeilen anpassen:

```js
const SB_URL = "https://DEIN-PROJEKT.supabase.co";
const SB_KEY = "DEIN-PUBLISHABLE-KEY";
```

Der publishable Key ist für die Nutzung im Browser gedacht. Den **service_role** Key niemals in die Seite oder ins Repository schreiben.

### 3. GitHub Pages

1. `index.html` in ein öffentliches Repository hochladen.
2. Unter **Settings → Pages** den Branch `main` mit dem Ordner `/ (root)` wählen und speichern.
3. Nach 1 bis 2 Minuten ist die Seite unter `https://BENUTZERNAME.github.io/REPOSITORY/` erreichbar.

## Benutzung

1. Link öffnen und das Gruppenpasswort eingeben.
2. Spieler hinzufügen.
3. **Neue Saison** antippen, Namen, Spielmodus und Satzformat wählen.
4. Spiele eintragen, die Tabelle aktualisiert sich automatisch.
5. Falsche Einträge mit dem ✕ löschen (zweimal tippen zum Bestätigen).

Beim Löschen eines Spielers werden auch alle seine Spiele entfernt. Das lässt sich nicht rückgängig machen.

## Hinweise

- Das Passwort gilt für die ganze Gruppe. Wer es kennt, kann Spieler und Spiele ändern.
- Das Passwort bleibt auf dem Gerät gespeichert, bis jemand auf **Abmelden** tippt.
- Gratis-Projekte bei Supabase werden nach längerer Inaktivität pausiert und lassen sich im Dashboard mit einem Klick wieder starten.
