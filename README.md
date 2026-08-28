# Real Ale & Cider Festival

A phone-friendly drinks menu and live drinking companion for the 29th Clacton-on-Sea
Real Ale & Cider Festival (St James' Hall, 26-29 August 2026).

**Open it:** https://anthonywilliamsaiguy-create.github.io/real-ale-cider-festival/

No sign-in, no app store. Open the link, put a name in, start logging drinks.

## What it does

- **Menu** - all 32 ciders, perries and fruit ciders plus 54 cask ales, with brewery,
  ABV, county and tasting notes. Search, filter by style, sort by ABV, and add drinks
  that aren't on the list. Mark casks sold out and tick which ciders are actually
  on the bar - one person marks it and everyone's phones update.
- **Track** - log halves and pints, rate drinks with a thumbs up or down, and jot
  tasting notes. Everything saves to your own phone.
- **The Half-Pint Handicap** - every half pint moves your glass a furlong along a race
  track. Live standings, race commentary and a crew-vs-crew scoreboard.
- **Bored?** - beer mats for the table: pub trivia, proper riddles, "true or myth?"
  phrase origins, certified groaners and questions about this year's actual list.
  Deal one, read it out, let the table argue, flip it over. The deck never repeats
  until you've been through the lot.
- **Thinking of driving?** - a deliberately cautious sober-clock on the My Night tab:
  from your units and last drink it shows the earliest you should even consider
  driving. A rough guide, never a green light.
- **Trophy Cabinet** - eleven trophies for exploring: perries, stouts, counties,
  tasting notes, going the distance, dealing beer mats.
- **Crews** - create a crew or join one that's already going. Notes and Best in Show
  stay within your crew.

## How the live sync works

Each phone keeps its own copy of your drinks in browser storage, so the app keeps
working with no signal. When there is signal, phones share their tally through a
public relay (ntfy.sh) on a private random topic, so everyone's race board updates
by itself. No accounts and no server to run.

Drinks data taken from the festival's published cask and cider lists.
