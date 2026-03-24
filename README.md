# Phantom Noise for Firefox

> **This is a Firefox port of [Phantom Noise](https://github.com/soufianetahiri/phantom-noise), originally built for Chrome. All core functionality is identical — only the browser API layer and manifest format have been adapted. Credits to **soufianetahiri** for the original idea. Let'em eat spam!**

**Privacy-enhancing decoy traffic generator for Firefox.**

Pollutes your browsing profile with realistic noise — random site visits, natural-sounding searches, and simulated human behavior — making it harder for ISPs, data brokers, surveillance programs, and ad-tech companies to build an accurate picture of who you are and what you care about.

Runs entirely locally. No accounts, no servers, no data sent anywhere.

---

## Differences from the Chrome version

| | Chrome | Firefox |
|---|---|---|
| Manifest | V3 (service worker) | V2 (persistent background script) |
| API namespace | `chrome.*` | `browser.*` (native Promises) |
| Private tabs | Incognito (`chrome://extensions` → Allow in Incognito) | Private windows (`about:addons` → Run in Private Windows) |

No functional changes. The scheduling, behavior simulation, UI, and all settings are identical.

---

## Features

### Decoy Traffic Engine
- Opens random websites across **12 categories**: news, tech, shopping, health, finance, social media, science, entertainment, sports, travel, food, and lifestyle (150+ real URLs)
- Performs realistic searches on **Google, DuckDuckGo, Bing, and Yahoo** with natural-sounding queries (including occasional typos for authenticity)
- Uses **Poisson-process scheduling** so timing looks human, not robotic — events follow exponential inter-arrival times instead of fixed intervals

### Human Behavior Simulation
Each decoy tab simulates realistic browsing:
- **Smooth scrolling** with eased acceleration curves
- **Mouse movement** along Bézier curves (not straight lines)
- **Hovering** over links, images, and headings with natural pauses
- **Clicking** same-domain links (safely avoids logout/delete/form links)
- **Text selection** to mimic reading behavior
- All timing is randomized per intensity level

### Privacy Mode — Fully Invisible Operation
- **Zero visible tabs**: All decoy activity happens in a completely invisible window — force-minimized on creation, and auto-re-minimized if it ever surfaces. You will never see a tab flash, a window appear, or anything in your taskbar.
- **Private window preferred**: When available, decoy tabs run in an invisible private window for full profile isolation — no history, cookies, or cache pollution
- **Hidden window fallback**: If private browsing isn't enabled, a regular window is used with the same invisibility guarantees plus automatic cleanup of cookies, cache, history, localStorage, and service workers after each tab closes
- **No data exfiltration**: Everything runs locally in your browser. Zero network calls to external servers.

### 4 Intensity Levels

| Level | Rate     | Description                                      |
|-------|----------|--------------------------------------------------|
| Low   | ~18/hr   | Light background noise. Minimal resource usage.   |
| Med   | ~60/hr   | Moderate noise. Good default for daily use.       |
| High  | ~150/hr  | Heavy noise. Noticeably more tabs opening/closing.|
| Max   | ~300/hr  | Maximum noise. Uses more bandwidth and CPU.       |

### Status Tab
- Live session counters: searches performed, pages visited, ad sites hit, active tabs
- All-time statistics persisted across sessions
- Bandwidth sparkline chart (last 60 minutes)
- Session uptime tracker

### Log Tab
- Real-time feed of every action the extension takes
- Filter by type: Search, Visit, Ad, Action, System
- Export full logs as JSON

### Settings Tab
- **Search Engines** — Enable/disable Google, DuckDuckGo, Bing, Yahoo and set their relative frequency weights
- **Task Mix** — Adjust the ratio of searches vs. page visits vs. ad-site visits
- **Site Categories** — Toggle entire categories on or off (all 12 independently)
- **Active Hours** — Schedule noise generation only during specific hours
- **Tab Behavior** — Configure min/max tab lifetime
- **Privacy Controls** — Toggle private window mode and post-close cleanup independently

---

## Installation

1. Download or clone this repository
2. Open Firefox and navigate to `about:debugging#/runtime/this-firefox`
3. Click **Load Temporary Add-on...**
4. Select the `manifest.json` file inside the cloned folder
5. **Important**: Go to `about:addons` → find **Phantom Noise** → click it → set **Run in Private Windows** to **Allow**

> **Note**: Loading via `about:debugging` is temporary — the extension is removed when Firefox restarts. For permanent installation, the extension needs to be signed by Mozilla or installed via Firefox's developer edition with signature enforcement disabled (`about:config` → `xpinstall.signatures.required` → `false`).

---

## Hardcoded Sources & Websites

All websites visited by the extension are listed below. No other sites are ever contacted.

### Search Engines

| Engine     | Search URL Pattern |
|------------|-------------------|
| Google     | `google.com/search?q=…` |
| DuckDuckGo | `duckduckgo.com/?q=…` |
| Bing       | `bing.com/search?q=…` |
| Yahoo      | `search.yahoo.com/search?p=…` |

### News & Media (15 sites)
`reuters.com` · `apnews.com` · `bbc.com/news` · `npr.org` · `theguardian.com` · `aljazeera.com` · `france24.com` · `dw.com` · `news.yahoo.com` · `usatoday.com` · `abcnews.go.com` · `cbsnews.com` · `nbcnews.com` · `pbs.org/newshour` · `politico.com`

### Technology (15 sites)
`arstechnica.com` · `theverge.com` · `techcrunch.com` · `wired.com` · `slashdot.org` · `news.ycombinator.com` · `tomshardware.com` · `anandtech.com` · `engadget.com` · `cnet.com` · `zdnet.com` · `techmeme.com` · `stackoverflow.com` · `github.com/trending` · `dev.to`

### Shopping & Retail (15 sites)
`amazon.com` · `ebay.com` · `walmart.com` · `target.com` · `bestbuy.com` · `etsy.com` · `wayfair.com` · `homedepot.com` · `costco.com` · `nordstrom.com` · `zappos.com` · `overstock.com` · `newegg.com` · `ikea.com` · `macys.com`

### Health & Wellness (14 sites)
`webmd.com` · `mayoclinic.org` · `healthline.com` · `nih.gov` · `cdc.gov` · `medicalnewstoday.com` · `everydayhealth.com` · `health.com` · `verywellhealth.com` · `drugs.com` · `clevelandclinic.org` · `medlineplus.gov` · `psychologytoday.com` · `hopkinsmedicine.org`

### Finance & Business (14 sites)
`bloomberg.com` · `cnbc.com` · `finance.yahoo.com` · `marketwatch.com` · `fool.com` · `investopedia.com` · `wsj.com` · `ft.com` · `nerdwallet.com` · `bankrate.com` · `seekingalpha.com` · `barrons.com` · `forbes.com` · `economist.com`

### Social Media (12 sites)
`reddit.com` · `reddit.com/r/popular` · `reddit.com/r/technology` · `reddit.com/r/science` · `reddit.com/r/worldnews` · `reddit.com/r/askscience` · `twitter.com/explore` · `tumblr.com/explore` · `pinterest.com` · `quora.com` · `medium.com` · `linkedin.com/feed`

### Science & Education (14 sites)
`nature.com` · `sciencedaily.com` · `space.com` · `scientificamerican.com` · `phys.org` · `newscientist.com` · `nasa.gov` · `khanacademy.org` · `coursera.org` · `edx.org` · `ocw.mit.edu` · `scholar.google.com` · `arxiv.org` · `britannica.com`

### Entertainment (14 sites)
`imdb.com` · `rottentomatoes.com` · `metacritic.com` · `ign.com` · `gamespot.com` · `polygon.com` · `rollingstone.com` · `pitchfork.com` · `billboard.com` · `vulture.com` · `avclub.com` · `variety.com` · `hollywoodreporter.com` · `ew.com`

### Sports (12 sites)
`espn.com` · `cbssports.com` · `nba.com` · `nfl.com` · `mlb.com` · `nhl.com` · `bbc.com/sport` · `skysports.com` · `bleacherreport.com` · `theathletic.com` · `si.com` · `sbnation.com`

### Travel (12 sites)
`tripadvisor.com` · `lonelyplanet.com` · `booking.com` · `expedia.com` · `kayak.com` · `airbnb.com` · `hotels.com` · `skyscanner.com` · `google.com/travel` · `frommers.com` · `cntraveler.com` · `travelzoo.com`

### Food & Cooking (12 sites)
`allrecipes.com` · `foodnetwork.com` · `epicurious.com` · `bonappetit.com` · `seriouseats.com` · `simplyrecipes.com` · `food.com` · `delish.com` · `eater.com` · `tastingtable.com` · `kingarthurbaking.com` · `cooking.nytimes.com`

### Lifestyle & Home (11 sites)
`architecturaldigest.com` · `apartmenttherapy.com` · `bhg.com` · `hgtv.com` · `dwell.com` · `thespruce.com` · `familyhandyman.com` · `realsimple.com` · `marthastewart.com` · `gardeningknowhow.com` · `thisoldhouse.com`

### Ad / Shopping Sites (6 additional sites)
`groupon.com` · `wish.com` · `alibaba.com` · `aliexpress.com` · `rakuten.com` · `slickdeals.net`

**Total: ~156 unique websites across 12 categories + 4 search engines**

---

## Permissions Explained

| Permission      | Why                                                    |
|-----------------|--------------------------------------------------------|
| `tabs`          | Open and close decoy tabs                              |
| `storage`       | Persist settings and all-time stats                    |
| `browsingData`  | Clean up cookies/cache/history after tab close         |
| `webNavigation` | Track page load completion                             |
| `<all_urls>`    | Visit any website as a decoy target                    |

---

## Privacy & Safety

- **No data leaves your machine.** There are no analytics, no telemetry, no external API calls.
- **Safe clicking**: The content script only clicks links on the same domain and explicitly avoids logout, delete, unsubscribe, mailto, tel, and form-related links.
- **No form interaction**: The extension never fills in forms, enters text, or submits anything.
- **Private window isolation**: When enabled, all decoy traffic is fully isolated from your real browsing session.

## License

MIT
