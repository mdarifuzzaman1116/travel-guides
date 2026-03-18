---
name: travel-planner
description: >
  Use this skill EVERY TIME Arif mentions traveling to a destination, asks about a trip, provides travel dates, or asks about flights, hotels, things to do, restaurants, weather, or travel advisories for any destination. Trigger phrases: "I want to go to", "we're visiting", "planning a trip to", "tell me about [destination]", "help me plan [trip]", or any message where a destination and travel dates are provided. Always produce both a full chat response AND generate index.html, push it to GitHub, and confirm the live URL. Never skip the HTML generation and GitHub push. Handles all research: flights (JFK/LGA only, no EWR, after 10 AM, direct first), hotels (4-star and 5-star plus Airbnb), restaurants with photo sliders, things to do with image sliders, safety advisory, CDC health, weather, car rental, cost estimates, currency converter, infant notes. Baby DOB: August 5, 2025 — always calculate age at travel time automatically.
---

# Travel Planner Skill

## Purpose

When Arif provides a destination and travel dates, this skill:
1. Researches all relevant travel data using web search
2. Responds in chat with a structured summary (same format every time)
3. Generates a complete dark-mode HTML file saved as index.html
4. Pushes index.html to GitHub replacing the previous destination completely
5. Confirms the live URL

The same URL always shows the most recent destination:
https://mdarifuzzaman1116.github.io/travel-guides/

---

## Traveler Profile

- Travelers: 2 adults + 1 infant
- Baby DOB: August 5, 2025 — calculate exact age at travel dates every time
- Under 24 months = lap infant (~10% of adult fare, no seat needed)
- 24 months or older = needs own seat — factor into flight costs
- Preferred airports: JFK first, LGA second. Never EWR.
- Flight preferences: direct/nonstop first, after 10 AM, fewest connections

---

## Step 1 — Parse

Extract destination, travel dates, any special context.
Calculate baby's age and state it clearly in response.

---

## Step 2 — Research (always search before writing)

1. U.S. State Dept travel advisory level for destination
2. CDC health — vaccines, disease risks, current notices
3. Entry requirements for U.S. citizens
4. Weather for exact travel dates
5. Flights from JFK or LGA — real prices for 2 adults + lap infant
6. Car rental at destination airport — actual rates
7. 4-star and 5-star hotels — real names, prices, links
8. Airbnb — pool + full kitchen, direct link
9. Things to do — 10 to 16 activities with prices and media links
10. Restaurants — 6 to 8 with prices and links
11. Food cost breakdown
12. Exchange rate USD to local currency
13. Insider tips specific to destination

---

## Step 3 — Chat Response (same section order every time)

SAFE TO TRAVEL VERDICT
TRIP SNAPSHOT
CURRENCY CONVERTER
WEATHER
STATE DEPT ADVISORY
CDC HEALTH
ENTRY REQUIREMENTS
FLIGHTS
CAR RENTAL
HOTELS
THINGS TO DO
RESTAURANTS
FOOD COSTS
INSIDER TIPS
INFANT TRAVEL NOTES
COST ESTIMATE
PRE-DEPARTURE CHECKLIST

---

## Step 4 — Generate index.html

Build complete standalone dark-mode HTML. Structure and styles never change — only data changes per destination.

### Design System (never change):
--bg: #0f1117 | --bg2: #161b27 | --bg3: #1e2535 | --bg4: #252d3d
--border: #2a3347 | --text: #e8eaf0 | --text2: #a8b0c8 | --text3: #6b7596
--teal: #1fb89a | --amber: #f59e0b | --green: #22c55e | --red: #ef4444
--blue: #60a5fa | --purple: #a78bfa
Fonts: Playfair Display (hero) + DM Sans (body) from Google Fonts

### Sections in HTML (same order every time):
1. Hero — flag, "DESTINATION Travel Guide", dates, traveler tags
2. Safe banner — green (Level 1), amber (Level 2), red (Level 3/4)
3. Currency converter — USD input, local output, quick buttons, rate shown
4. Trip snapshot — 8 metric cards
5. Weather — daily forecast strips per travel date
6. State Dept advisory card
7. CDC health card
8. Entry requirements card
9. Flights — 2 cards side by side
10. Car rental — 3-column equal-width grid (Economy | SUV | Key Notes)
11. Hotels grid — photo + name + badge + price + note + links
12. Things to do grid — image slider + name + note + price + media links
13. Restaurants grid — image slider + name + price + note + links
14. Food costs table
15. Insider tips grid
16. Infant notes card
17. Cost estimate grid + total card
18. Interactive checklist
19. Footer

### Images:
- Use Unsplash: https://images.unsplash.com/photo-PHOTOID?w=600&q=80
- Test photo IDs with curl before using
- Every img tag needs onerror fallback:
  onerror="this.parentElement.innerHTML='<div class=slide-fallback><span>EMOJI</span></div>'"

### Currency converter — update these 3 values per destination:
const RATE = 2.00;
const SYMBOL = 'BZ$';
const CURRENCY_NAME = 'Belize Dollar (BZD)';

### Car rental layout — NEVER break this:
.car-grid { display: grid; grid-template-columns: repeat(3, minmax(0, 1fr)); gap: 12px; }
.car-row { display: flex; justify-content: space-between; gap: 12px; padding: 7px 0; }

### Save to:
/home/claude/index.html
/mnt/user-data/outputs/index.html

---

## Step 5 — Push to GitHub (always, no confirmation needed)

TOKEN: YOUR_GITHUB_TOKEN
REPO: mdarifuzzaman1116/travel-guides
FILE: index.html at repo root
BRANCH: main

### Push script (use bash_tool):

```bash
TOKEN="YOUR_GITHUB_TOKEN"
REPO="mdarifuzzaman1116/travel-guides"

# Step 1: Get current SHA (required for update, empty string for first push)
SHA=$(curl -s \
  -H "Authorization: token $TOKEN" \
  https://api.github.com/repos/$REPO/contents/index.html \
  | python3 -c "import sys,json; d=json.load(sys.stdin); print(d.get('sha',''))" 2>/dev/null || echo "")

# Step 2: Base64 encode the file
CONTENT=$(base64 -w 0 /home/claude/index.html)

# Step 3: Build JSON payload
if [ -n "$SHA" ]; then
  PAYLOAD="{\"message\":\"Update: DESTINATION DATES\",\"content\":\"$CONTENT\",\"sha\":\"$SHA\"}"
else
  PAYLOAD="{\"message\":\"Add: DESTINATION DATES\",\"content\":\"$CONTENT\"}"
fi

# Step 4: Push
RESULT=$(curl -s -X PUT \
  -H "Authorization: token $TOKEN" \
  -H "Accept: application/vnd.github.v3+json" \
  https://api.github.com/repos/$REPO/contents/index.html \
  -d "$PAYLOAD")

echo "$RESULT" | python3 -c "
import sys,json
d=json.load(sys.stdin)
c=d.get('content',{})
if c.get('sha'):
    print('SUCCESS | sha:', c['sha'][:7])
else:
    print('ERROR:', d.get('message','unknown error'))
"
```

### After successful push, tell the user:
✅ Pushed to GitHub Pages → https://mdarifuzzaman1116.github.io/travel-guides/
(Live in ~30 seconds. Same URL every time — bookmark it.)

### If push fails:
Push failed: [error]. The HTML is still available to download above.

---

## What Changes vs What Stays the Same

### NEVER change (same for every destination):
- CSS variables and color system
- Font choices
- Section order and layout
- Slider JS, checklist JS, currency converter JS structure
- Card component styles
- Car rental 3-column grid

### ALWAYS replace with new destination data:
- Hero: flag, destination name, dates, tags
- Safe banner: level + color
- Currency: RATE, SYMBOL, CURRENCY_NAME
- All 8 trip snapshot metrics
- Weather: dates + forecast per day
- Advisory: level, zones, embassy
- CDC: disease risks, vaccines for this country
- Entry: visa, passport, fees for this country
- Flights: airlines, routes, real prices, booking links
- Car rental: rates, providers, local rules
- Hotels: real properties with prices and links
- Things to do: destination activities with media links
- Restaurants: destination restaurants with links
- Food costs: local prices
- Insider tips: country-specific
- Infant age: recalculated from DOB Aug 5 2025
- Cost estimate: all figures for this destination
- Checklist: destination-specific items
- Footer: destination, dates

---

## GitHub Reference

Repo: github.com/mdarifuzzaman1116/travel-guides
Live URL: https://mdarifuzzaman1116.github.io/travel-guides/
Branch: main
File: index.html (repo root)
Token: YOUR_GITHUB_TOKEN (does not expire)
Visibility: public (required for GitHub Pages on free plan)

---

## Trigger Examples

"I want to go to Puerto Plata in June"
"Plan a trip to Dominican Republic"
"Cancun July 4–11"
"Tell me about traveling to Jamaica"
"Italy with a baby, August"
"Paris in October"
Any destination + dates combination

---

## Pre-Push Quality Checklist

- Baby age calculated from DOB Aug 5 2025 for travel dates
- Flights from JFK or LGA only, 10 AM or later, direct first
- Currency converter has correct RATE, SYMBOL, CURRENCY_NAME
- Car rental is 3-column grid, no text overflow
- Dark mode applied throughout
- Activity cards have slider + Google Images + YouTube + TikTok
- Hotel cards have Expedia + website + Google links
- Restaurant cards have Google + Tripadvisor + menu links
- All img tags have onerror fallback
- Checklist is interactive
- index.html saved locally
- GitHub push ran, SHA fetched first, result confirmed
- Live URL shown in chat
