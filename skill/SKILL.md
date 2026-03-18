---
name: travel-planner
description: >
  Use this skill EVERY TIME Arif mentions traveling, asks about a trip, provides travel dates, asks about flights, hotels, things to do, restaurants, weather, safety, or travel advisories for ANY destination — even if it is a follow-up, a change, an update, or a refinement to a previous plan. Trigger phrases include: "I want to go to", "we're visiting", "planning a trip", "what's it like in", "tell me about traveling to", "can we travel to", "help me plan", "change the dates", "update the plan", "what about [city/country]", "swap [destination]", or ANY message where a destination and travel context are present. Always produce BOTH a full structured chat response AND generate index.html, push it to GitHub replacing the previous version, and confirm the live URL. Never skip either output. Never ask for confirmation before pushing. The token never expires. Baby DOB is August 5, 2025 — always auto-calculate her age at travel time.
---

# Travel Planner Skill

## Core Rule

Every single travel-related response — new destination, date change, itinerary update, hotel swap, anything — must:

1. Research live data first (never guess prices, flights, or advisories)
2. Respond in chat with all sections in the standard order
3. Generate a complete dark-mode index.html file
4. Push index.html to GitHub replacing the previous version
5. Confirm the live URL to the user

**Live URL (permanent, never changes):**
https://mdarifuzzaman1116.github.io/travel-guides/

**GitHub repo:** mdarifuzzaman1116/travel-guides
**Token:** GITHUB_TOKEN_IN_CLAUDE_MEMORY (does not expire — use in every session)
**File to update:** index.html at repo root (always overwrite, never create new files)
**Branch:** main

---

## Traveler Profile (apply to every response)

- **Travelers:** 2 adults + 1 infant
- **Baby DOB:** August 5, 2025
- **Age at travel:** Always calculate from DOB. State it clearly (e.g., "Your daughter will be 11 months").
- **Under 24 months:** Lap infant — no seat needed, ~10% of adult fare each way
- **24 months or older:** Needs own seat — add to flight cost calculation
- **Preferred airports:** JFK first, LGA second — NEVER EWR (Newark)
- **Flight preferences:** Nonstop/direct first, after 10:00 AM departure, fewest connections, afternoon or evening preferred

---

## Step 1 — Parse Request

Extract destination (city + country), travel dates, trip duration, and any changes from a previous plan.
Calculate baby's exact age at departure date from DOB August 5, 2025.

---

## Step 2 — Research (always run before generating any output)

Use web_search and all available tools. Never use assumed or outdated data.

1. U.S. State Dept advisory level (1-4) for destination
2. CDC health — vaccines, disease risks, current notices
3. Entry requirements for U.S. citizens — passport, visa, exit fees, traveling with minor
4. Weather for the exact travel dates (not monthly averages)
5. Flights from JFK or LGA — nonstop preferred after 10 AM, RT prices for 2 adults + 1 lap infant
6. Car rental at destination airport — economy + SUV actual rates for full trip duration
7. 4-star and 5-star hotels — real names, nightly rates, Expedia + website links
8. Airbnb — pool + full kitchen, direct link for destination
9. Things to do — 10 to 16 activities, prices, baby-friendliness, media links
10. Restaurants — 6 to 8, price tier, Google + Tripadvisor + menu photo links
11. Food costs — street food through upscale, daily budget for 2 adults
12. Exchange rate — current USD to local currency
13. Insider tips — destination-specific practical advice

---

## Step 3 — Chat Response (same section order every time)

Every section must appear. No skipping.

```
SAFE TO TRAVEL VERDICT (green/amber/red)
TRIP SNAPSHOT
CURRENCY CONVERTER NOTE
WEATHER [exact travel dates]
U.S. STATE DEPT TRAVEL ADVISORY
CDC HEALTH ADVISORY
ENTRY REQUIREMENTS
FLIGHTS
CAR RENTAL
HOTELS 4-STAR AND 5-STAR + AIRBNB
THINGS TO DO
RESTAURANTS
FOOD COSTS AT A GLANCE
INSIDER TIPS
INFANT TRAVEL NOTES [age at travel time]
FULL TRIP COST ESTIMATE
PRE-DEPARTURE CHECKLIST
```

---

## Step 4 — Generate index.html

Complete standalone dark-mode HTML. Structure and styles never change. Only data changes.

### Design System — never modify

```css
:root {
  --bg:#0f1117; --bg2:#161b27; --bg3:#1e2535; --bg4:#252d3d;
  --border:#2a3347; --border2:#374057;
  --text:#e8eaf0; --text2:#a8b0c8; --text3:#6b7596;
  --teal:#1fb89a; --teal-dim:#0e6b5a; --teal-glow:rgba(31,184,154,0.15);
  --amber:#f59e0b; --amber-dim:#78490a;
  --green:#22c55e; --green-dim:#14532d; --green-glow:rgba(34,197,94,0.12);
  --red:#ef4444; --red-dim:#7f1d1d;
  --blue:#60a5fa; --blue-dim:#1e3a5f;
  --purple:#a78bfa; --purple-dim:#3b1f6e;
  --radius:12px; --radius-sm:8px;
}
```

Fonts from Google Fonts: `Playfair Display` (400,600,700) for hero, `DM Sans` (300,400,500,600) for body.

### HTML Sections (same order always)

1. Hero — flag emoji, "DESTINATION Travel Guide", dates, traveler tags
2. Safe banner — green (Level 1), amber (Level 2), red (Level 3/4)
3. Currency converter — USD input, local output, quick-tap buttons, rate shown
4. Trip snapshot — 8 metric cards
5. Weather — daily forecast strips per travel date + verdict badge
6. State Dept advisory card
7. CDC health card
8. Entry requirements card
9. Flights — 2 cards side by side (nonstop featured | 1-stop backup), each with booking links
10. Car rental — 3-column equal-width grid
11. Hotels grid — photo + name + badge + price + note + links
12. Things to do grid — image slider + name + note + price + media links
13. Restaurants grid — image slider + name + price + note + links
14. Food costs table
15. Insider tips grid (10-12 cards)
16. Infant notes card
17. Cost estimate grid + total card
18. Interactive checklist
19. Footer

### Currency Converter — only update these 3 values per destination

```javascript
const RATE = 2.00;
const SYMBOL = 'BZ$';
const CURRENCY_NAME = 'Belize Dollar (BZD)';

// Structure never changes:
const input = document.getElementById('usd-input');
const result = document.getElementById('local-result');
const rateNote = document.getElementById('rate-note');
function convert() {
  const val = parseFloat(input.value) || 0;
  result.textContent = SYMBOL + (val * RATE).toFixed(2);
}
rateNote.textContent = '1 USD = ' + RATE + ' ' + CURRENCY_NAME;
input.addEventListener('input', convert);
function setAmt(v) { input.value = v; convert(); }
convert();
```

### Car Rental Layout — NEVER break

```css
.car-grid { display:grid; grid-template-columns:repeat(3,minmax(0,1fr)); gap:12px; }
.car-card { background:var(--bg3); border:1px solid var(--border); border-radius:var(--radius); padding:18px 20px; }
.car-card.best { border-color:var(--teal); }
.car-row { display:flex; justify-content:space-between; align-items:flex-start; gap:12px; padding:7px 0; border-bottom:1px solid var(--border); }
.car-row:last-child { border-bottom:none; }
.car-lbl { font-size:12.5px; color:var(--text3); flex-shrink:0; }
.car-val { font-size:13px; color:var(--text2); text-align:right; line-height:1.45; }
```

### Image Slider JS — same every time

```javascript
const sliderState = {};
function slide(wrapId, trackId, dir) {
  const wrap = document.getElementById(wrapId);
  const track = document.getElementById(trackId);
  const total = track.children.length;
  if (!sliderState[wrapId]) sliderState[wrapId] = 0;
  sliderState[wrapId] = (sliderState[wrapId] + dir + total) % total;
  const idx = sliderState[wrapId];
  track.style.transform = `translateX(-${idx * 100}%)`;
  const dotsEl = wrap.querySelector('.slider-dots');
  if (dotsEl) dotsEl.querySelectorAll('.slider-dot').forEach((d,i) => d.classList.toggle('active', i===idx));
  const countEl = wrap.querySelector('.slide-count');
  if (countEl) countEl.textContent = `${idx+1} / ${total}`;
}
document.querySelectorAll('.slider-dot').forEach(dot => {
  dot.addEventListener('click', function() {
    const wrap = this.closest('.slider-wrap');
    const track = wrap.querySelector('.slider-track');
    const allDots = wrap.querySelectorAll('.slider-dot');
    const targetIdx = Array.from(allDots).indexOf(this);
    slide(wrap.id, track.id, targetIdx - (sliderState[wrap.id] || 0));
  });
});
```

### Checklist JS — same every time

```javascript
function toggle(el) {
  el.classList.toggle('done');
  el.nextElementSibling.classList.toggle('done');
}
```

### Images — Unsplash (no API key needed)

Format: `https://images.unsplash.com/photo-PHOTOID?w=600&q=80`

Test before using:
```bash
curl -sI "https://images.unsplash.com/photo-PHOTOID?w=600&q=80" | grep "HTTP/" | tail -1
# Must return: HTTP/2 200
```

Every img tag must have onerror fallback:
```html
<img src="https://images.unsplash.com/photo-PHOTOID?w=600&q=80" alt="description"
     onerror="this.parentElement.innerHTML='<div class=slide-fallback><span>EMOJI</span></div>'">
```

### Save paths

```
/home/claude/index.html
/mnt/user-data/outputs/index.html
```

---

## Step 5 — Push to GitHub (always, no confirmation needed)

Run this exact script using bash_tool every time after generating the HTML:

```bash
TOKEN="GITHUB_TOKEN_IN_CLAUDE_MEMORY"
REPO="mdarifuzzaman1116/travel-guides"
DEST="DESTINATION DATES"

SHA=$(curl -s \
  -H "Authorization: token $TOKEN" \
  "https://api.github.com/repos/$REPO/contents/index.html" \
  | python3 -c "import sys,json; d=json.load(sys.stdin); print(d.get('sha',''))" 2>/dev/null)

CONTENT=$(base64 -w 0 /home/claude/index.html)

if [ -n "$SHA" ] && [ "$SHA" != "None" ] && [ "$SHA" != "" ]; then
  PAYLOAD="{\"message\":\"Update: $DEST\",\"content\":\"$CONTENT\",\"sha\":\"$SHA\"}"
else
  PAYLOAD="{\"message\":\"Add: $DEST\",\"content\":\"$CONTENT\"}"
fi

curl -s -X PUT \
  -H "Authorization: token $TOKEN" \
  -H "Accept: application/vnd.github.v3+json" \
  "https://api.github.com/repos/$REPO/contents/index.html" \
  -d "$PAYLOAD" \
| python3 -c "
import sys,json
d=json.load(sys.stdin)
c=d.get('content',{})
if c.get('sha'):
    print('SUCCESS sha=' + c['sha'][:7])
else:
    print('ERROR: ' + d.get('message','unknown'))
"
```

Replace `DESTINATION DATES` with actual values, e.g.: `"Dominican Republic Jun 10-17 2026"`

**After SUCCESS, tell user:**
```
✅ Pushed to GitHub Pages → https://mdarifuzzaman1116.github.io/travel-guides/
Live in ~30 seconds. Bookmark this URL — it always shows your latest destination.
```

**After ERROR, tell user:**
```
Push failed: [error message]. HTML is still available to download above.
```

---

## What Changes vs What Stays the Same

### NEVER change
- CSS variables and color values
- Font choices
- Section order
- Component HTML structure
- Slider JS, checklist JS, currency converter JS structure
- Car rental 3-column grid
- Image fallback pattern
- GitHub push script structure

### ALWAYS replace per destination
- Hero: flag emoji, destination name, dates, tags
- Safe banner: color + level + description
- Currency: RATE, SYMBOL, CURRENCY_NAME
- Snapshot: all 8 metric values
- Weather: all dates, temps, rain %, verdict
- Advisory: level, zones, embassy contact
- CDC: diseases, vaccines, infant note
- Entry: visa, passport, fees, minor rules
- Flights: airlines, routes, prices, links
- Car rental: rates, providers, local rules
- Hotels: names, prices, descriptions, links
- Activities: all destination-specific
- Restaurants: all destination-specific
- Food costs: local prices
- Insider tips: all destination-specific
- Infant age: recalculated from DOB Aug 5 2025
- Cost estimates: all figures
- Checklist: destination-specific items
- Footer: destination + dates

---

## GitHub Reference

| | |
|---|---|
| Repo | github.com/mdarifuzzaman1116/travel-guides |
| Live URL | https://mdarifuzzaman1116.github.io/travel-guides/ |
| Branch | main |
| File | index.html (repo root — always overwrite) |
| Token | GITHUB_TOKEN_IN_CLAUDE_MEMORY |
| Token expiry | Never |
| Visibility | Public (required for GitHub Pages on free plan) |

---

## Pre-Push Checklist

- [ ] Baby age calculated from DOB Aug 5 2025 for actual travel dates
- [ ] Flights from JFK or LGA only, 10 AM or later, direct first
- [ ] Currency RATE, SYMBOL, CURRENCY_NAME correct for destination
- [ ] Car rental is 3-column grid, no overflow
- [ ] Dark mode applied throughout
- [ ] Activity cards have slider + Google Images + YouTube + TikTok
- [ ] Hotel cards have Expedia + website + Google
- [ ] Restaurant cards have Google + Tripadvisor + menu
- [ ] All img tags have onerror fallback
- [ ] Checklist toggle works
- [ ] index.html saved to both local paths
- [ ] SHA fetched before push
- [ ] Push result is SUCCESS
- [ ] Live URL confirmed in chat
