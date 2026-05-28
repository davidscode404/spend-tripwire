# Spend Tripwire

Spend Tripwire leverages agentic AI to bid on ad placements within LLM conversations. On the left, there are (simulated) examples of user input. The agent bids only when target categories are identified. There are three different zones for budget governance. In the green zone, the agent fires a bid and commits the spend immediately upon recognition of a target category. Once the total surpasses £500, the agent enters the amber zone. Here, every bid is held in the override queue for 30 seconds before execution, allowing human-in-the-loop to stop a bid if necessary. And once the total surpasses £800, the agent stops entirely, and can only be restarted manually.

---

## Hackathon track

**Buy-Side Agents** — agents that plan, bid, and buy ads inside LLM channels. Built for the advertiser: brands, agencies, DSPs.

---

## Setup

No installation required. Spend Tripwire is a single self-contained HTML file with no external dependencies and no API keys.

**Run locally:**

```bash
python3 -m http.server 8080
```

Then open `http://localhost:8080/spend-tripwire.html` in your browser.

Alternatively, open `spend-tripwire.html` directly in any modern browser.

---

## Files

```
spend-tripwire.html   — the full application (HTML, CSS, JS in one file)
README.md             — this file
```

---

## How to demo

1. Press **▶ Start agent** — the intent feed begins scanning simulated user queries.
2. Watch **BID FIRED** badges appear when a query matches a target category (car insurance, home insurance). Misses are greyed out.
3. Let the agent run on **Normal** or **Fast** speed until spend crosses **£500** — it enters the amber zone and actions begin queuing with a 30-second countdown.
4. Hit **Block** on a queued action before the countdown expires to tombstone it.
5. Hit **⚡ Inject spike** at any point to force a large spend jump and trigger the red zone hard pause.
6. After the hard pause alert fires, choose **Acknowledge only** or **Reset & restart**.
7. Paste ad copy into the **Creative guardrail validator** (bottom strip) and press **▶ Check copy** to see it pass or fail against the brand envelope. Use **Load example** for a copy that fails on multiple rules.

---

## Budget zones

| Zone | Spend range | Agent behaviour |
|------|-------------|-----------------|
| Green | £0 – £500 | Bids commit immediately, no human involvement |
| Amber | £500 – £800 | Every bid queued for 30s — human can block before execution |
| Red | £800+ | Agent halts entirely — manual restart required |

---

## Creative guardrail rules

Copy is checked against five rules before it is allowed to serve:

- **No forbidden words** — `guaranteed`, `cheapest`, `no questions asked`, `free`, `unlimited`, etc.
- **No ALL CAPS words** — shouts erode brand trust
- **No unqualified superlatives** — `best`, `#1`, `leading`, `unbeatable`, etc.
- **Disclaimer present** — must contain `T&Cs apply` or `terms apply`
- **Length ≤ 200 characters** — fits conversational placement units

A single failure blocks the creative. All checks run client-side with no external calls.

---

## Controls

| Control | Action |
|---------|--------|
| ▶ Start agent | Begin scanning intents and firing bids |
| ⏸ Pause / ▶ Resume | Suspend and resume the agent |
| ↺ Reset | Clear all state and return to idle |
| ⚡ Inject spike | Force a large spend jump (£100–£180) to demonstrate zone breach |
| Speed selector | Slow / Normal / Fast — controls intent feed interval |