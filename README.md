<h1 align="center">🏓 Tischtennis Liga</h1>

<p align="center">
  <b>A table tennis league tracker for friends</b><br>
  Players, seasons, matches and a live standings table, free to host and password protected.
</p>

<p align="center"><i>created by XiN@NBeY</i></p>

---

## Features

- **Players:** add and remove players, with total matches, wins and losses shown under each name
- **Seasons:** create and delete seasons, each with its own play mode and set format
- **Matches:** record results with set scores and delete single matches
- **Schedule:** single-round seasons list all open pairings with result fields, and saving a result removes the pairing from the list. Other seasons with a fixed set format (best of 3, 5 or 7) show the next open pairings and fill them into the entry form
- **Standings:** matches played, wins, losses, sets and points, calculated automatically
- **Overall ranking:** a team counter across all seasons with the total number of matches, points per player and the best player overall
- **Shared data:** everyone sees the same state, and the page refreshes every 15 seconds
- **Password protection:** without logging in, no data can be read or changed
- Mobile friendly, with light and dark mode

> The app interface is in German.

## Play modes

| Setting | Options |
| --- | --- |
| Play mode | Home and away (everyone plays everyone twice), single round (once), free play |
| Set format | Best of 3, best of 5, best of 7, or any |

When a match is saved, the app checks that a pairing is not played more often than the mode allows and that the set score fits the format (for best of 5, only 3:0, 3:1 or 3:2).

## Scoring

- Win = **2 points**, loss = 0 points
- Ties on points are broken by **set difference**, then by sets won

## Tech

- A single file: `index.html` (HTML, CSS and JavaScript, no libraries)
- Hosting: GitHub Pages
- Database and login: Supabase (free plan)

## Setup

### 1. Supabase

1. Create a new project at supabase.com.
2. Run this in the **SQL Editor**:

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

3. Go to **Authentication → Users → Add user** and create a user with the email `liga@tischtennis.app` and your group password. Tick **Auto Confirm User**.
4. Under **Authentication → Sign In / Providers**, turn off "Allow new users to sign up".
5. Under **Project Settings → API**, copy the **Project URL** and the **publishable (anon) key**.

### 2. Credentials in the page

At the top of the script in `index.html`, set these two lines:

```js
const SB_URL = "https://YOUR-PROJECT.supabase.co";
const SB_KEY = "YOUR-PUBLISHABLE-KEY";
```

The publishable key is meant to be used in the browser. Never put the **service_role** key into the page or the repository.

### 3. GitHub Pages

1. Upload `index.html` to a public repository.
2. Go to **Settings → Pages**, choose the branch `main` with the folder `/ (root)` and save.
3. After one or two minutes the site is live at `https://USERNAME.github.io/REPOSITORY/`.

## Usage

1. Open the link and enter the group password.
2. Add players.
3. Tap **Neue Saison** (new season) and choose a name, play mode and set format.
4. Enter matches, and the standings update automatically.
5. Delete wrong entries with the ✕ (tap twice to confirm).

Deleting a player also removes all of that player's matches, and deleting a season removes all matches of that season. This cannot be undone.

## Notes

- The password is shared by the whole group. Anyone who knows it can change players and matches.
- The login stays saved on the device until someone taps **Abmelden** (log out).
- Free Supabase projects are paused after a longer period of inactivity and can be restarted with one click in the dashboard.
