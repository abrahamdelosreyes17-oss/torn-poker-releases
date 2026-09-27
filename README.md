# Torn TableMind: a Hold'em advisor for Torn's poker table

A Tampermonkey overlay for Torn's poker page. It reads the table you are looking at. On your turn it shows the best
action and size, with the EV of every action. **It never clicks, types or sends anything: you make every move.**

## Install

Install [Tampermonkey](https://www.tampermonkey.net/) in Chrome. In Chrome 138 and later, open `chrome://extensions`,
open Tampermonkey's **Details**, and turn on **Allow User Scripts**. Then open:

**https://raw.githubusercontent.com/abrahamdelosreyes17-oss/torn-poker-releases/main/torn-tablemind.user.js**

Tampermonkey offers to install it, and keeps it updated from that same URL.

Then open `https://www.torn.com/page.php?sid=holdem` and click once on the page. The panel appears on the right.

## What it shows

- **The exact price to call.** It comes from the game log and is checked against Torn's Call button, which rounds
  amounts over $1k. The footer says "price matches Torn ✓" when both agree.
- **Your equity against each opponent's estimated range.** The range updates with every action, bet size and board card.
- **The EV of every legal action** (fold, call, each raise size, all-in) and the best one. When two options are
  within the noise, it says so.
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
  - hand history export and import
  - safety

## Rules it keeps (Torn's terms)

- It reads **only the poker page you are actively viewing**. A hidden tab always pauses it.
  - **Seated and playing:** it keeps reading, even if you switch windows. It pauses after 5 minutes with no action from you.
  - **Just watching a table:** it pauses when the window loses focus, or after 60 seconds without a click or key.
- It **never clicks, types into, or intercepts** Torn's controls. It only notes the time of your own clicks and keys,
  so it can pause when you are idle.
- **It never touches the network:** no API key, no requests, no `@connect`. Your hand history stays in your browser
  until you export it yourself.
- Every build is scanned for these rules before release.

## Good to know

- Advice is a guide, and it is only as good as the reads behind it. With few hands on a player it leans on the
  Torn pool average, and says so.
- Clearing torn.com site data deletes your hand history. Export a backup from Settings › Hand history now and then.
- If something looks wrong, open Settings › About › **Calibration**. It shows exactly what the script reads from the page.

## Version

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
