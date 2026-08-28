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

## Beer mats (the "Bored?" tab) and the roasts

- **Bored?** is the 5th tab: a deck of flip-over beer mats (trivia, riddles,
  "true or myth?" origins, jokes, plus cards computed live from the drinks
  data). Internally it is still called `snug` everywhere (panel id, state key,
  function names) — only the labels changed when Anthony vetoed the name.
  The deck is `MATS_SRC` / `festMats()` in the script; card ids are hashes of
  the front text, so editing or adding cards never breaks anyone's "seen"
  list. Per-phone no-repeat state lives in `state.snug`.
- **`ROASTS`** (right under `BUILD`) is targeted banter for specific mates:
  Jack (lightweight, tight with rounds), Ross (very tall), Luke (runner;
  after a few he wants a booth at the Loft nightclub), Chris (tech guy, only
  drinks Disaronno & coke) and Tony/Anthony himself (unemployed, built this
  app with the free time). Every phone watches the live board and fires the
  same gags locally as tallies move: pre-arrival watch lines, a first-drink
  toast (real hours-late maths from the `fs` field riders publish), second
  drink, occasional digs, plus race-commentary lines. Name match is
  whole-word against `alts` ("Jack"/"jacko"/"Big Jack", not "Jackie"). Add a
  victim: copy a block. Disarm: delete it. A phone that joins late adopts
  current state silently so nobody gets stale news. **Keep the roasts out of
  README.md** — the victims read that.

## The shared bar map (sold out / what's on)

The cider bar only has a subset out at once. `state.bar` is `{id:{s,t}}`
(s: 0 on, 1 sold out, 2 not out yet) - it rides in every rider payload
(seconds timestamps, capped to the 60 newest) and merges newest-wins in
`ingest()`, so one person marking a cask sold or ticking the cider sheet
updates every phone, including late joiners via the ntfy cache. The tick
sheet is the apple button at the top of the Menu. Old per-phone
`state.soldOut` marks migrate into the map on first load of build 33.

## Driving guide

"🚗 Thinking of driving?" on My Night: earliest-drive estimate from
`state.lastLogAt` + 1h + 1h/unit. Deliberately cautious wording — it must
never read as permission to drive. Hidden at 0 units.

## Things that bit us (don't regress these)

- **Never style an element that uses the `hidden` attribute with `display:...`**
  A `display:block` rule beats the browser's `[hidden]` rule and the element
  stays visible. There is a global `[hidden]{display:none !important}` guard.
  When testing visibility, check `getComputedStyle(el).display`, NOT `el.hidden`.
- **Don't commit state on every keystroke.** An `input` handler on the crew box
  once saved a crew per letter typed ("t", "th", "the"...). Commit on the button.
- **Test against a returning user's saved state**, not just a fresh install.
  Several bugs only appeared for someone with older localStorage.
- **Race boards split silently by session date.** Riders only race together
  when `dayDate(sessionDay)` matches, and phones used to keep yesterday's
  date until someone found New session (bit us Sat morning at the hall -
  fresh installs raced alone while older phones sat on Friday's board).
  Since build 36 `maybeRollDay()` rolls to today on open/foreground unless
  the last drink was under 6h ago (a past-midnight session keeps running -
  that part is still deliberate).
- Riders are keyed by a per-device id, deduped by name (newest wins).
- Crew matching is loose (`crewKey`) so "The Lads" and "the lads " are one crew.

## Ground rules Anthony cares about

- Anyone with the link can use it: no sign-in, no app store, no accounts.
- It must work with no signal (everything saves locally, syncs when back).
- Keep it simple and quick to use in a busy hall, one-handed.
