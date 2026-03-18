---
name: travel-planner
description: >
  Use this skill EVERY TIME Arif mentions traveling to a destination, asks about a trip, provides travel dates, or asks about flights, hotels, things to do, restaurants, weather, or travel advisories for any destination. Trigger phrases include: "I want to go to", "we're visiting", "planning a trip to", "what's it like in [place]", "tell me about [destination]", "can we travel to", "what should I know about [country]", "help me plan [trip]", or any message where a destination + travel dates are provided. Always produce both a full chat response AND a downloadable dark-mode HTML travel guide. Never skip the HTML file generation. This skill handles all research, flight search (JFK/LGA preferred, no EWR, after 10 AM departures, direct flights first), hotels (4-star and 5-star + Airbnb), restaurants, things to do with media links, safety, CDC health, weather, car rental, cost estimates, and infant-specific notes. Baby DOB: August 5, 2025 — always calculate her age at travel time automatically.
---

# Travel Planner Skill

## Purpose

When Arif provides a destination and travel dates, this skill:
1. Researches all relevant travel data using web search and available tools
2. Responds in chat with a structured summary (same format every time)
3. Generates a complete dark-mode HTML travel guide and presents it as a downloadable file

Both outputs are always produced. Never skip one.

---

## Traveler Profile (always apply)

- **Travelers:** 2 adults + 1 infant
- **Baby DOB:** August 5, 2025 — always calculate exact age at the travel dates provided
- **Age matters for:** lap infant status (under 2 = lap infant, ~10% of adult fare, no seat), car seat needs, activity eligibility, health recommendations
- **Preferred departure airports:** JFK (first choice), LGA (second choice). **Never EWR.**
- **Flight preferences:**
  - Direct/nonstop flights always first
  - Fewest connections possible
  - Preferred departure time: after 10:00 AM (10 AM or later is fine, nothing before 10 AM)
  - Afternoon and evening departures preferred
  - If no direct flight exists, show the best 1-stop option from JFK or LGA only

---

## Step-by-Step Execution

### Step 1 — Parse the request

Extract:
- Destination (city, country)
- Travel dates (depart / return)
- Any special context (type of trip, preferences, questions)

Calculate baby's age at travel dates:
- DOB: August 5, 2025
- If age < 24 months at departure date = lap infant on all flights
- State age clearly in the response (e.g., "Your daughter will be 14 months at travel time")

### Step 2 — Research (run all searches in parallel where possible)

Use web_search and any available tools to gather:

1. **Safety** — U.S. State Dept travel advisory level (Level 1–4) for destination
2. **CDC health** — Vaccinations required/recommended, disease risks, current health notices
3. **Entry requirements** — Passport validity, visa requirements for U.S. citizens, exit fees, customs notes
4. **Weather** — For the specific travel dates provided, not general averages
5. **Flights** — Search for real flights from JFK (preferred) or LGA to destination for the exact dates. Use lastminute.com tools, web search, or any flight tools available. Look for:
   - Nonstop flights departing 10 AM or later from JFK or LGA
   - If no nonstop: best 1-stop option from JFK or LGA
   - Round-trip prices for 2 adults + 1 lap infant
6. **Car rental** — BZE or destination airport rates for the full trip duration. Search for actual prices.
7. **4-star and 5-star hotels** — Real property names with nightly rates. Include Expedia links or website links where available.
8. **Airbnb options** — With pool + full kitchen if possible. Provide direct Airbnb search link for destination.
9. **Things to do** — Most popular + unique/once-in-a-lifetime experiences. For each activity: price, baby-friendliness rating, Google Images link, YouTube link, TikTok link.
10. **Restaurants** — Best restaurants with prices per person, Google Maps/Tripadvisor link, menu link or photo link.
11. **Food prices** — Street food through upscale, daily budget for 2 adults.
12. **Insider tips** — Practical tips specific to this destination (currency, SIM card, driving rules, local customs, things to avoid).

### Step 3 — Chat response

Respond in chat using this exact structure every time. Use clean prose sections separated by headers. No bullet lists at top level — use the card/row format implied by the headers. Keep it informative and direct.

```
[SAFE TO TRAVEL banner — green/amber/red based on advisory level]

TRIP SNAPSHOT
-- Quick stats: visa, flight time, currency, language, time zone, power plugs, baby age, hurricane/storm season

WEATHER — [dates]
-- Daily forecast for travel dates
-- Overall verdict (good/average/avoid) with clear label

U.S. STATE DEPT TRAVEL ADVISORY
-- Level + date + link
-- What to avoid, what's safe, medical situation, embassy contact

CDC HEALTH ADVISORY
-- Disease risks for destination
-- Required vs recommended vaccines
-- Infant-specific health notes

ENTRY REQUIREMENTS
-- Passport requirements
-- Visa status
-- Exit fees
-- Traveling with minor notes
-- Infant on flights (lap infant or not)

FLIGHTS — JFK/LGA to [DESTINATION CODE]
-- Best nonstop option (if available, after 10 AM): airline, flight number, duration, RT price for 2 adults, infant lap cost
-- Backup 1-stop option from JFK/LGA: airline, route, price
-- Booking links

CAR RENTAL
-- Economy rate (daily + 7-day total)
-- SUV rate (daily + 7-day total) — always recommend SUV for family
-- Best providers with links
-- Key notes: deposit, car seat, border rules

HOTELS — 4-STAR & 5-STAR + AIRBNB
-- 5+ hotel options with names, location, nightly rate, brief description
-- Direct links (hotel website, Expedia, Google)
-- Airbnb search link

THINGS TO DO
-- 12–16 activities organized by category
-- For each: price, baby-friendly note, Google Images link, YouTube link, TikTok link
-- Flag "Once in a Lifetime" experiences prominently

RESTAURANTS
-- 6–8 restaurants with price tier, location, what to order
-- Google/Tripadvisor/menu links for each

FOOD COSTS AT A GLANCE
-- Price table: street food through upscale, daily budget

INSIDER TIPS
-- 10–12 practical tips specific to this destination

INFANT TRAVEL NOTES
-- Age at travel time (calculated from DOB Aug 5, 2025)
-- Flight, car seat, sun, bugs, stroller, formula, medical

FULL TRIP COST ESTIMATE
-- Itemized: flights, car rental, hotels (4-star / 5-star / Airbnb), food, activities, extras
-- Total range for 4-star and 5-star scenarios

PRE-DEPARTURE CHECKLIST
-- 12–15 items, interactive (clicks to check off in HTML version)
```

### Step 4 — Generate the HTML file

After the chat response, generate a complete standalone dark-mode HTML file using this design system:

**Design System — Dark Mode:**
```css
--bg: #0f1117
--bg2: #161b27
--bg3: #1e2535
--bg4: #252d3d
--border: #2a3347
--text: #e8eaf0
--text2: #a8b0c8
--text3: #6b7596
--teal: #1fb89a (primary accent)
--amber: #f59e0b
--green: #22c55e
--red: #ef4444
--blue: #60a5fa
--purple: #a78bfa
```

**Fonts:** Playfair Display (headings/hero) + DM Sans (body) — load from Google Fonts

**Required sections in HTML (in this order):**
1. Hero banner with destination flag, title, dates, traveler tags
2. Safe-to-travel banner (green/amber/red)
3. Trip snapshot grid (8 metric cards)
4. Weather section with daily forecast strips
5. U.S. State Dept advisory card
6. CDC health card
7. Entry requirements card
8. Flights section — 2 cards side by side
9. Car rental — 3-column grid (Economy | SUV recommended | Key notes) — all columns equal width, no text overflow
10. Hotels grid — cards with name, badge, price, description, Expedia/website/Google links
11. Things to do grid — cards with image or emoji placeholder, activity name, note, price, Google Images + YouTube + TikTok links
12. Restaurants grid — cards with emoji placeholder, name, location, price tier, description, Google + Tripadvisor + menu links
13. Food costs table
14. Insider tips grid
15. Infant travel notes card
16. Cost estimate grid + highlighted total card
17. Interactive checklist (click to check off)
18. Footer with data sources and baby DOB note

**Car rental section — critical layout rules:**
- Use a 3-column CSS grid: `grid-template-columns: repeat(3, minmax(0, 1fr))`
- Each column is a card with equal width
- Inside each card, use flex rows: label left, value right — no overflow
- SUV card gets teal border highlight
- Never use table layout for car rental — causes the overflow bug seen in the screenshot

**Image handling:**
- For famous landmarks (Great Blue Hole, etc.), use the NASA Earth Observatory public domain image where available:
  `https://eoimages.gsfc.nasa.gov/images/imagerecords/147000/147158/belize_oli_2020064_lrg.jpg`
- For other activities: use emoji placeholder div with the activity icon
- All `<img>` tags must have `onerror` fallback to show the emoji placeholder
- Activity cards have 130px height image area

**Links to always include:**
- For each activity: Google Images search link, YouTube search link, TikTok search link, Wikipedia if relevant
- For each hotel: hotel website, Expedia link, Google search link
- For each restaurant: Google Maps/search link, Tripadvisor link, menu photo link
- For flights: airline booking page, Google Flights link, KAYAK link
- For car rental: agency websites + KAYAK comparison link
- For Airbnb: direct search link with destination + adults=2 + infants=1 + pool and kitchen filters

**Link construction rules:**
- Google Images: `https://www.google.com/search?q=[query]&tbm=isch`
- YouTube: `https://www.youtube.com/results?search_query=[query]`
- TikTok: `https://www.tiktok.com/search?q=[query]`
- Google Search: `https://www.google.com/search?q=[query]`
- Tripadvisor: search tripadvisor.com for the specific restaurant/hotel first, use direct URL if found
- Expedia: search expedia.com for the specific hotel, use direct URL if found
- Airbnb: `https://www.airbnb.com/s/[destination]/homes?amenities[]=7&amenities[]=8&adults=2&infants=1`
- Google Flights: `https://www.google.com/travel/flights/search?...` (construct with destination codes and dates)

**Interactive checklist:**
```javascript
function toggle(el) {
  el.classList.toggle('done');
  const lbl = el.nextElementSibling;
  lbl.classList.toggle('done');
}
```

**File naming:** `[destination]_travel_guide_[dates].html`
- Example: `belize_travel_guide_apr2026.html`
- Example: `punta_cana_travel_guide_jun2026.html`

**After generating the file:**
1. Save to `/home/claude/[filename].html`
2. Copy to `/mnt/user-data/outputs/[filename].html`
3. Call `present_files` with the output path

---

## Output Format Rules (apply to chat response)

1. **No em dashes** — use "--" or commas instead
2. **Copy-paste friendly** — all hotel names, restaurant names, flight numbers, prices should be in plain text easy to copy
3. **Both chat + HTML every time** — never skip either
4. **Consistent structure** — same section order every time regardless of destination
5. **Real data** — always search before responding, never use estimated or assumed data for flights, hotels, or prices
6. **Flag infant-specific items** clearly in every relevant section
7. **Always note departure airport** used (JFK or LGA) and why in flight section
8. **Safety verdict first** -- always open with a clear "safe / use caution / do not travel" statement before anything else

---

## Special Notes

- If no direct flight exists from JFK/LGA, clearly state this and show the best 1-stop option
- If departure is before 10 AM, note it and suggest alternatives
- If the destination has a Level 3 or 4 advisory, state this prominently at the top and provide specific safe zones if any exist
- If the baby will be 24+ months at travel time, she needs her own seat — factor this into flight costs automatically
- Always recalculate baby's age from DOB August 5, 2025 against the actual travel dates — never hardcode an age
- Weather data should be for the actual travel dates, not general monthly averages
- Run all searches before generating HTML -- the HTML is the final assembled product, not a draft

---

## Step 5 — Auto-Push to GitHub

After the HTML file is generated and presented to the user, always push it to GitHub automatically. No confirmation needed — just do it silently as part of the workflow.

**Repository:** `mdarifuzzaman1116/travel-guides` (private)
**Branch:** `main`
**Folder:** `guides/`
**Token:** Store in session only — do not log or print the token value in any response

### Push workflow (bash_tool):

```bash
cd /home/claude

# 1. Clone or pull the repo
git clone https://TOKEN@github.com/mdarifuzzaman1116/travel-guides.git gh-travel 2>/dev/null || (cd gh-travel && git pull)

cd gh-travel

# 2. Copy the new HTML file into guides/
cp /home/claude/FILENAME.html guides/FILENAME.html

# 3. Update README index table — add a new row for this destination

# 4. Stage and commit
git config user.email "arif@travel-guides"
git config user.name "Arif Travel Bot"
git add guides/FILENAME.html README.md
git commit -m "Add DESTINATION DATES travel guide

- Flights, hotels, car rental, activities, restaurants
- Currency converter, CDC advisory, State Dept advisory
- Infant notes (baby age auto-calculated from DOB Aug 5 2025)
- Interactive checklist"

# 5. Push
git push origin main
```

### README index update rule:
Every push must also update the `| Destination | Dates | File | Travelers |` table in README.md.
Add a new row like:
```
| 🇩🇴 Dominican Republic | Jun 10–17, 2026 | [punta_cana_jun2026.html](guides/punta_cana_jun2026.html) | 2 adults + infant (10 mo) |
```
Calculate infant age at travel time from DOB August 5, 2025.

### After push completes:
Tell the user in one line:
```
Pushed to GitHub → github.com/mdarifuzzaman1116/travel-guides/guides/FILENAME.html
```

If the push fails for any reason, report the error clearly so the user can troubleshoot.

---

## Trigger Examples

These phrases should always trigger this skill:

- "I want to go to Puerto Plata in June, what do you think?"
- "Plan a trip to the Dominican Republic for me"
- "We're thinking about Cancun in July, what should I know?"
- "Tell me about traveling to Jamaica"
- "Can we travel to Italy with a baby?"
- "What's Paris like in October?"
- "Help me plan a trip to Greece, dates June 20–28"
- Any message containing a destination + travel dates
- Any message asking about safety, flights, hotels, or things to do for a specific place

---

## Quality Checklist Before Responding

Before presenting the final output, verify:
- [ ] Baby's age at travel time is calculated correctly from DOB Aug 5, 2025
- [ ] Flights are from JFK or LGA only (not EWR)
- [ ] Departure times are 10 AM or later (flag if earlier options are the only ones)
- [ ] Car rental section uses 3-column equal-width grid (no text overflow)
- [ ] Dark mode theme applied throughout HTML
- [ ] All activity cards have Google Images + YouTube + TikTok links
- [ ] All hotel cards have website + Expedia + Google links
- [ ] All restaurant cards have Google + Tripadvisor + menu links
- [ ] Checklist is interactive (click to check off)
- [ ] HTML file is saved and presented with present_files
- [ ] Chat response covers all required sections in order
- [ ] Safety advisory is current (pulled from live search)
- [ ] CDC health data is current (pulled from live search)
