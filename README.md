# Torn TableMind: a Hold'em advisor for Torn's poker table

A Tampermonkey overlay for Torn's poker page. It reads the table you are looking at. When you face an all-in, it tells
you whether calling gains, and why. When you're short-stacked and first in, it tells you whether to shove or fold, from a
solved chart. Everywhere else it shows the exact price and your made hand, but no verdict yet.
**It never clicks, types or sends anything: you make every move.**

## Install

Install [Tampermonkey](https://www.tampermonkey.net/) in Chrome. In Chrome 138 and later, open `chrome://extensions`,
open Tampermonkey's **Details**, and turn on **Allow User Scripts**. Then open:

**https://raw.githubusercontent.com/abrahamdelosreyes17-oss/torn-poker-releases/main/torn-tablemind.user.js**

Tampermonkey offers to install it, and keeps it updated from that same URL.

Then open `https://www.torn.com/page.php?sid=holdem` and click once on the page. The panel appears on the right.

## What it shows

- **All-in calls (a verdict):** CALL ALL-IN, FOLD or CLOSE: EITHER.
  - The EV is worked out against what the shover is likely holding. That uses a model of for-fun shovers (any two cards)
    versus value shovers, updated from each player's shoves and shown hands at this stake.
  - It states the break-even point: "Call if the chance X is a for-fun shover is 64% or more. Now 90%."
  - A call that only gains on a thin read shows as CLOSE: EITHER.
- **Short stack, first in (a verdict):** PUSH ALL-IN, FOLD or CLOSE: LEAN PUSH/FOLD, for 2–9 players dealt in and up to
  20 big blinds. It comes from a push/fold chart solved for each table size, checked against a public heads-up chart
  with no disagreements. Facing one all-in with everyone between folding, the chart's call or fold sits beside the
  all-in verdict.
  - Not covered, and it says so: limps, raises that are not all-in, a caller before you, straddles and dead blinds.
- **Learn mode explains in plain words:** every verdict has a short "why" with no poker-math jargon, and the same words
  are saved with each decision in your hand history.
- **Everywhere else, exact prices, no verdict:**
  - "Need 28.6% to call $1,250", priced against the pot you can actually win;
  - the stack-to-pot ratio;
  - when you're short (≤ 25 bb), the fold rate your shove needs.

  The price comes from the game log and is checked against Torn's Call button.
- **Your made hand**, e.g. "Straight, 5-high (wheel)".
- **The old 0.6 engine** is still in Settings › Advice as "legacy, unrated", off by default. It gives an action for every
  spot, but it lost the raise wars at $500/$1k.
- **A read tile under each player:** their type (Nit, TAG, Fish, Station, Maniac…), VPIP/PFR, how many hands it rests on,
  and how sure the read is.
  - Reads lean on each player's **recent form**. In 8.8 million play-money hands, a player's last ~25 hands predicted
    their next moves better than their lifetime numbers.
  - When someone is playing clearly looser or tighter than usual, for example after a big loss, the tile says so:
    "Looser after −60 bb / last 35h 51% · usual 26%". It only appears when the change is statistically real.
- **The same note for your own play** on your turn: "Last 35 hands you played 51%. Usual: 31%."
- **Results (▤):**
  - your real profit next to all-in EV, each with a 95% range, so you can tell skill from luck
  - results by stake, seat and opponent
  - leaks: where going against the advice cost you EV
  - your recent hands
- **Settings (⚙):**
  - Quick or Learn mode, $ or big blinds, and how strongly reads move the advice
  - tiles and notes
  - **automatic backup** to a folder you choose once (see below)
  - hand history export and import
  - safety

## Rules it keeps (Torn's terms)

- It reads **only the poker page you are actively viewing**. A hidden tab always pauses it.
  - **Seated and playing:** it keeps reading, even if you switch windows. It pauses after 5 minutes with no action from you.
  - **Just watching a table:** it pauses when the window loses focus, or after 60 seconds without a click or key.
- It **never clicks, types into, or intercepts** Torn's controls. It only notes the time of your own clicks and keys,
  so it can pause when you are idle.
- **It never touches the network:** no API key, no requests, no `@connect`. Your hand history stays on your computer:
  in your browser, and in the backup folder if you choose one. It writes no other files.
- Every build is scanned for these rules before release.

## Good to know

- Advice is a guide, and it is only as good as the reads behind it. With few hands on a player it leans on the
  Torn pool average, and says so.
- Clearing torn.com site data deletes the hand history in your browser. **Turn on automatic backup** in Settings ›
  Hand history: make a new, empty folder (for example on D:, not in OneDrive, Google Drive or Dropbox) and choose it.
  Your hands then go there as you play, one file per day, and Import reads them back. After a Chrome restart the panel
  may ask you to allow the folder again (one click). Chrome gives the folder to torn.com, so use it for nothing else.
- If something looks wrong, open Settings › About › **Calibration**. It shows exactly what the script reads from the page.

## Version

**0.7.2** "auto-backup":
- Hands and your decisions are written to a folder you choose once, so nothing depends on remembering Export. Nothing
  about a hand in progress is written, and nothing is lost while the backup waits for you to allow the folder.
- Import takes several files at once.

**0.7.1** "push/fold":
- A push/fold chart for 2–9 players, 1–20 big blinds, solved offline (about 94 billion simulated deals). Heads-up it
  matches HoldemResources' Nash chart on every hand that isn't a coin flip between the two actions.
- Learn mode's "In plain words" box on every verdict; each decision in the journal keeps its why.

**0.7.0** "all-in advisor":
- A verdict only where it can price the spot exactly: all-in calls. The all-in EV was checked against an independent
  evaluator (pokerkit), and it matched exactly on every recorded all-in.
- The verdict is ready the moment your turn starts: it is worked out when the shove appears in the log.
- **Fixes:**
  - no advice after you fold;
  - "need %" against the pot you can win;
  - tiles never cover Torn's buttons or seats;
  - "< 1%" instead of "0% of hands";
  - your starting stack after a bust or rebuy.
- **Hand history:** it records timestamps and the stake. Older exports still import.

**0.4.0** fixes the advice mistakes the first real-stakes session showed:
- **No more bluff escalation into an all-in.** It no longer shoves weak hands into a player who just raised, and it
  won't go all-in without real equity against the hands that call.
- **Bluffs need evidence:** fold chances are estimated on the cautious side until the players show otherwise.
- **Multiway pots are priced with everyone who might call,** not just one opponent.
- **It learns the table:** reads lean on how this table has played before falling back to the general pool.
- **"The money"** in Learn mode explains each decision in dollars: what you put in, how often they fold, what you
  average when called.
- **Results** show your poker profit in Torn dollars, including today.
- **Fixes:**
  - old advice no longer looks live
  - no advice glitch right after you act
  - the panel stays on screen at half-width (plus Reset panel position)
  - the ` key collapses the panel

**0.3.0** fixes what the first live session showed:
- **Advice mid-hand:** it now works after you sit down or come back to the window. Before, it waited for the next hand.
- **Seated reading:** while you're seated and dealt in, it keeps reading when you switch windows. It pauses only when
  the tab is hidden, or after 5 minutes with no action from you.
- **Torn's buttons stay clickable.** The panel ends above them, or fades and lets clicks through, and its title bar
  stays usable.
- **Your position is known from the first decision of a hand.**
- **Fixes:**
  - a backup reminder that stays until you export
  - faster Results
  - clearer settings
  - safer imports
  - a stricter safety audit

**0.2.0**:
- reads the seated table
- recency-weighted reads, with tilt notes for opponents and for you
- fold odds that follow bet size
- a bet-size tell
