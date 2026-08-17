# gym

Mobile training log for the John Reed 3×/week hypertrophy plan.
Single self-contained `index.html` — no build step, no dependencies, no backend.

## Publishing it

1. In this repo: **Add file → Upload files**, drop in `index.html` (and `README.md`), commit to `main`.
2. **Settings → Pages** → Source: *Deploy from a branch* → Branch: `main`, folder: `/ (root)` → Save.
3. Wait ~1 minute. The page appears at:

   **https://chrisrepo-gh.github.io/gym/**

4. On the phone: open that URL in Chrome → menu → **Add to Home screen**. It then opens
   full-screen like an app and works without network once loaded.

## Images

The exercise figures are already embedded in `index.html` as data URIs — cropped from your
own app screenshots, resized to 190 px, ~77 KB in total. There is no `images/` folder and
nothing else to upload. One file is the whole site.

All twenty exercises have an image.

**Note:** the figures are RSG Group's artwork from the John Reed app, and this repo is
public. If that matters to you, replace them with your own photos of the machines using the
same `IMG` keys, or make the repo private.

## How it works

- **Set logging** — weight and reps per set. Your numbers from last time are already in the
  fields when you open a day, so you start from where you finished. Adjust and go.
- **Accidental-save guard** — if you press Finish without changing anything, the first tap
  only warns you; a second tap saves the carried-over numbers as today.
- **`↑ add weight next time`** — appears when every set of an exercise hits the top of its
  rep range. That is the double-progression rule from the plan, automated.
- **Finish session** — moves the current day into history and re-seeds the fields.
- **One entry per day, per day-type** — saving Day A twice on the same date *replaces* the
  earlier entry instead of adding a duplicate; the dialog says "Updated" rather than "Saved".
  Day A and Day B on the same date stay separate, as they should. Any duplicates already in
  storage are merged the next time the page loads, keeping the most recent of each.
- **Data** — cloud sync setup, plus copy / download / restore of the whole log as JSON.

## Cloud sync (optional)

Off by default. With it on, the whole log is written to a **private** GitHub repo after every
finished session, so it can be reviewed without you exporting anything.

Setup, once:

1. Create a **new private repo**, e.g. `chrisrepo-gh/gym-log`. It must NOT be this repo —
   this one is public and your training data does not belong in it.
2. GitHub → Settings → Developer settings → **Fine-grained personal access tokens** →
   Generate new token. Repository access: **Only select repositories** → `gym-log`.
   Permissions: **Contents → Read and write**. Nothing else. Set an expiry you'll remember.
3. On the phone: **Data → Cloud sync** → enter owner, repo name and the token → *Save & sync now*.

The token is stored in that phone browser's `localStorage` only. It is not in this page's
source and is not in this repo. Scope it to the one private repo so that even if it leaked,
nothing else is reachable.

The pill under the title shows the state at a glance: green with a timestamp = your log is
safely up; amber = saved on the phone but not uploaded yet, tap to retry; red = something
is wrong and the message says what. A failed upload never loses data — it stays on the
phone and retries when you next open the page or come back online.

## Where the data lives — and why there is no file

In the browser's `localStorage`, on that phone only. That is **not a file you can find with a
file manager**: it lives inside Chrome's own profile database, and a web page cannot choose
where it is stored. There is no path to pick. That is a browser rule, not a design choice.

If you want a file you can see, use a download:

- **Data → Download** writes `training-log-YYYY-MM-DD.json` into the phone's Downloads folder.
- **Download this log** appears on the confirmation after every finished session.
- **Data → "Also download a JSON copy after every session"** does it automatically. One file
  per session in Downloads, named `training-YYYY-MM-DD-dayA.json`.

`localStorage` survives closing the browser and restarting the phone. It is lost if you clear
site data for the domain, use a private tab, or switch phones — so keep a download or turn on
cloud sync.

## Knowing your session was saved

Two independent confirmations, neither of which is just a message:

1. **After Finish session** a dialog appears that you have to dismiss. It reports the day, the
   timestamp, how many sets were written and how many sessions are now stored. Those numbers
   come from *reading the storage back after writing* — if the write silently failed, the box
   turns red and says NOT saved, with the reason and a Download button so the session is not
   lost.
2. **The line under the title**, always visible: `N sessions saved on this phone · last: Day A,
   17/08 19:40`. Open the page any time and you can see whether your last session is there.

## Changing the plan

Everything is in the `PLAN` object at the top of the `<script>` block in `index.html`:
exercise name, `sets`, rep range (`lo`/`hi`), rest, the setup cue, and the `id` that
determines the image filename. Edit it directly in GitHub's web editor and commit — Pages
redeploys in about a minute.

If you change an exercise `id`, its logged history stays under the old id and will no longer
be shown. Rename the image file to match, and keep ids stable once you are training on them.
