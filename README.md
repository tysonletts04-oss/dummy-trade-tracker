# Hit Rate Calculator

A simple paper-trading tracker. You log each trade idea your strategy produces, record a pretend entry and exit, and the dashboard tells you whether the ideas are actually working — without risking any real money.

Trades are saved to the cloud (Firebase), so your history sticks around between sessions and can be opened from any device.

Think of it as a scorecard for your trading rubric. If a certain type of signal keeps winning, you'll see it. If it keeps losing, you'll see that too.

---

## What it does

- Lets you **manually log dummy (paper) trades** against any ticker.
- **Saves every trade to Firebase**, so nothing is lost when you refresh or close the browser.
- Calculates your **hit rate** — the percentage of trades that won.
- Breaks the hit rate down **by signal type**, so you can see which parts of your strategy work and which don't.
- Shows a few other plain numbers: win/loss count, average size of wins vs losses, and a running total of paper profit.

That's it. It's meant to be easy to read at a glance, not a full trading platform.

---

## What it does NOT do

So there are no surprises:

- **No real money and no real orders.** Everything is pretend.
- **No live prices.** It does not connect to IRESS, a broker, or any data feed. You type the entry and exit prices in yourself.
- **No automatic exits.** You decide when a trade is "closed" and enter the exit price by hand.
- **No login (for now).** Anyone who has the link and config can view or add trades. For dummy paper-trading data that's usually fine — but see the "Who can access it" note below so it's a deliberate choice.

---

## How to use it (day to day)

1. **Open the file** in any web browser.
2. **Log a trade** by filling in the form. A trade needs:
   - **Ticker** — e.g. `BHP`
   - **Date** — when you took the trade
   - **Signal type** — what kind of idea it was (see list below)
   - **Direction** — Long (betting it goes up) or Short (betting it goes down)
   - **Entry price** — the price you "bought" or "shorted" at
   - **Exit price** — the price you "closed" at
   - **Notes** *(optional)* — anything useful, e.g. "high conviction" or "ASX down big that day"
3. **The trade saves to Firebase** and the dashboard updates instantly — your hit rate and other numbers recalculate every time you add a trade.
4. **Review the breakdown** to see which signal types are pulling their weight.

Because the data lives in the cloud, you can close everything and come back later — or open it on another device — and your full history will still be there.

---

## One-time setup (connecting to Firebase)

You only do this once. It takes a few steps, but none of them are hard, and you don't need to have used Firebase before.

1. **Create a free Firebase project**
   - Go to the Firebase website and sign in with a Google account.
   - Create a new project (you can name it anything, e.g. `hit-rate-calculator`).
   - The free plan is more than enough for this — see "Cost" below.

2. **Turn on Firestore**
   - In your project, enable **Firestore Database** (this is the database that stores your trades).
   - When asked, start it in a locked/test mode for now — you'll set the proper rules in step 4.

3. **Get your config and paste it into the file**
   - In your project settings, register a web app and copy the **config** block (a small set of values like `apiKey`, `projectId`, etc.).
   - Paste those values into the marked spot near the top of the HTML file (look for the `<paste your Firebase config here>` placeholder).
   - Note: these config values are *meant* to be public — they are not a secret password. What actually protects your data is the security rules in the next step.

4. **Set your security rules**
   - In Firestore's **Rules** section, set rules that control who can read and write your trades.
   - For a personal, no-login tool, the simplest safe-ish approach is to limit access to your one trades collection and nothing else. (If you later want to lock it down to specific people, that's when you'd add login — see below.)

That's the whole setup. After this, just open the file and start logging.

---

## Signal types

These match the kinds of ideas your strategy generates. Use whichever fits each trade:

- **Cap Raise – Long**
- **Cap Raise – Short**
- **Results – Long**
- **Results – Short**
- **Other**

Logging the signal type is the most important part — it's what lets the dashboard tell you *which* ideas are working, not just whether you're winning overall.

---

## How to read the numbers

| Number | What it means | Why it matters |
|---|---|---|
| **Hit Rate** | % of your trades that made a profit | The headline "are these ideas working?" number |
| **Hit Rate by Signal Type** | Same %, but split by idea type | Shows which parts of the strategy have an edge |
| **Wins / Losses** | Simple count of each | Quick sense of your sample size |
| **Avg Win % vs Avg Loss %** | How big your wins are vs your losses | A 50% hit rate is still great if wins are bigger than losses |
| **Running P&L** | Total pretend profit/loss so far | The bottom-line scoreboard |

**One thing to keep in mind:** hit rate alone doesn't tell the whole story. You can win less than half your trades and still come out ahead if your winners are bigger than your losers. That's why the "Avg Win vs Avg Loss" numbers sit right next to the hit rate — read them together.

---

## A note on sample size

A handful of trades won't tell you much — a couple of lucky wins can make a bad signal look great. Give each signal type a decent number of trades (a few dozen is far more trustworthy than five) before you draw any conclusions about whether it actually works.

---

## Who can access it

Right now there is **no login**, so access is "whoever has the link and the config." For pretend trading data this is usually a fine trade-off and keeps things simple.

If you later want it restricted to specific people (for example, just you and one other person), that's done by adding **Firebase login** and updating the security rules to allow only those accounts. It's a sensible future upgrade rather than something you need on day one.

---

## Cost

At this scale, expect to pay **nothing**. Firebase's free tier allows tens of thousands of database operations per day, and a couple of people logging paper trades won't come close to that. You'd only start paying at very high, constant volumes — which a personal trade log isn't. There's also no AI/token cost from using the dashboard; it's just a web page talking to your database.

---

## Why this exists

The whole point is to test whether your trading ideas have a real edge **before** putting money behind them. Log trades honestly (including the losers), let the numbers build up in Firebase over time, and let the hit rate tell you the truth about your strategy.
