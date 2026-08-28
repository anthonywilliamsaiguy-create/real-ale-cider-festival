# Real Ale & Cider Festival — project notes

Companion app for the 29th Clacton-on-Sea Real Ale & Cider Festival.
Anthony built this for himself, his wife Kirstie and his mates.

## Where everything lives

- **This folder** is the working copy. Edit `index.html` here.
- **Repo:** https://github.com/anthonywilliamsaiguy-create/real-ale-cider-festival
- **Live app:** https://anthonywilliamsaiguy-create.github.io/real-ale-cider-festival/
- Deployment is GitHub Pages from `main` / root. Push to main and it goes live
  in roughly 60-90 seconds. Nothing else to run.

## The whole app is one file

`index.html` — the drinks data, the UI, the sync, everything. No build step,
no dependencies, no server. Open it locally in a browser to test.

## How the live sync works

Phones share their state through **ntfy.sh**, a free anonymous pub/sub relay,
on a private random topic (`SYNC_TOPIC` near the top of the script).
No accounts and nothing to host. Each phone publishes its own rider record and
subscribes with EventSource for live updates; messages are cached ~16h.

**To wipe the board for everyone** (clears every rider and crew): change
`SYNC_TOPIC` to a new random string and push. That is the only reset that
reaches other people's phones.

`BUILD` is a version number - bump it on each change so it is obvious which
version a phone is running.

## Things that bit us (don't regress these)

- **Never style an element that uses the `hidden` attribute with `display:...`**
  A `display:block` rule beats the browser's `[hidden]` rule and the element
  stays visible. There is a global `[hidden]{display:none !important}` guard.
  When testing visibility, check `getComputedStyle(el).display`, NOT `el.hidden`.
- **Don't commit state on every keystroke.** An `input` handler on the crew box
  once saved a crew per letter typed ("t", "th", "the"...). Commit on the button.
- **Test against a returning user's saved state**, not just a fresh install.
  Several bugs only appeared for someone with older localStorage.
- Riders are keyed by a per-device id, deduped by name (newest wins).
- Crew matching is loose (`crewKey`) so "The Lads" and "the lads " are one crew.

## Ground rules Anthony cares about

- Anyone with the link can use it: no sign-in, no app store, no accounts.
- It must work with no signal (everything saves locally, syncs when back).
- Keep it simple and quick to use in a busy hall, one-handed.
