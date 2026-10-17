# Sustaincia Smart Mobility 2026 Theme (`sustaincia-mobility2026`)

Theme and asset repository for the **6th China (Macau) New Energy & Smart Transportation Trends Development Forum** (第六屆中國（澳門）新能源與智能交通趨勢發展論壇) and the live demonstration session **Road 03 · Car Show Visitor Sightlines** (道路03 · 車展參觀者眼中的看點，導師：Judy Wong).

- **Event Date**: October 31, 2026 (15:45 — 16:45)
- **Venue**: The Venetian Macao, Halls ABC (澳門威尼斯人展館 ABC)
- **Organizer**: 可持續發展協會 (Sustaincia / sustaincia.org)
- **Reference Webpage**: [https://sustaincia.org/i/e/261031a/](https://sustaincia.org/i/e/261031a/)

## Asset Structure

```
event_ui/
├── go-config.json              # Forum titles, description & photo count config
├── frame_assets/               # 4 event-branded transparent photo frames
│   ├── 1.png                   # Classic Forum Ticket Frame
│   ├── 2.png                   # Road 03 Visitor Insights Frame
│   ├── 3.png                   # Color & Parts Analytics Frame
│   └── 4.png                   # Smart Mobility Future City Frame
├── mobile_assets/
│   ├── app_background.png      # High-res warm paper textured mobile app background
│   └── backside_default.png    # Double-sided photocard default ticket stub backside
└── wall_assets/
    ├── background_event.png    # 1920x1080 Ultra-wide LED/Wall display background
    ├── background_lucky-draw.png
    ├── gift.png
    ├── config-dynamic-img.json
    └── static/                 # Sponsor logos, static wall cards, and metadata
```

---

## Live Demo & Market Research AI Dashboard Plan (`收集 → 拆解 → 重組`)

In this session, each participant is invited to take **5 photos** (`/market-research/sustaincia-mobility2026`) of exhibits on the expo floor that interest them (which may or may not be passenger cars — e.g., EV charging piles, battery packs, autonomous sensors/LiDAR, eVTOLs, smart traffic solutions, or booth displays).

Because the exact mix of exhibits photographed by attendees is only known once photos start arriving, the **Market Research AI Dashboard** uses a **generalized data structure (`MarketResearchInsight`)** where all analytical dimensions are configurable via `event_ui/go-config.json` and executed through the `OnlineService` interface (`APP_MODE=online`).

### Two-Stage Event Workflow

1. **Stage 1: Preliminary Photo Collection & Field Submission (`/market-research/sustaincia-mobility2026`)**
   - **Toggles in `event_ui/go-config.json`**:
     - `"enable_lucky_draw": false`: Hides the gift corner icon on the projection wall.
     - `"enable_market_research": true`: Activates the live research dashboard (`#corner-dashboard`).
     - `"enabled": false`, `"enable_event_form": false`: Bypasses external forms and completes submission immediately in the app.
     - `"capture_instruction"` / `"capture_instruction_en"`: Event-tailored capture instructions displayed to visitors.
     - `"analysis_fields"`: 4 structured dimensions (`interest_topic`, `visitor_role`, `experience_rating`, `observation_notes`) collected directly on the Arrange & Submit page.
   - **Flutter Web App**: [http://localhost:8080/market-research/sustaincia-mobility2026](http://localhost:8080/market-research/sustaincia-mobility2026)
   - Participants capture 5 photos of exhibits that catch their attention, arrange them, fill in their name and research fields, and submit.
   - Photos and participant names are stored in PostgreSQL (`photo_files`) and float live on the projection wall (`/ui/?ui=modern-wall&event=sustaincia-mobility2026`).

2. **Stage 2: Post-Preliminary Parameter Discovery & Batch Re-Analysis**
   - After inspecting the preliminary round of photos to see what exhibits visitors actually photographed (e.g., ratio of vehicles vs. charging infrastructure vs. smart cockpit tech), you can either:
     - Click **"AI Suggest Parameters"** (`POST /api/sustaincia-mobility2026/dashboard/suggest-fields`) on the Dashboard to let `OnlineService` inspect the preliminary photos and populate tailored `fields`, **or**
     - Edit `market_research_config.fields` directly in `event_ui/go-config.json`.
   - Trigger **Batch Re-Analysis** (`POST /api/sustaincia-mobility2026/dashboard/reanalyze`) so the `OnlineService` AI layer extracts the newly refined dimensions across all preliminary photos and updates the live dashboard charts and Q&A context without rebuilding any code.

---

## Direct Links & Access Points

| Component | URL / Route | Description |
| :--- | :--- | :--- |
| **Flutter Market Research App** | [`http://localhost:8080/market-research/sustaincia-mobility2026`](http://localhost:8080/market-research/sustaincia-mobility2026) | Mobile web app for visitors to capture 5 photos and submit analysis insights |
| **Interactive Wall Display** | [`http://localhost:8080/ui/?ui=modern-wall&event=sustaincia-mobility2026`](http://localhost:8080/ui/?ui=modern-wall&event=sustaincia-mobility2026) | Ultra-wide LED projection wall displaying floating attendee photocards |
| **Market Research Dashboard** | Click `#corner-dashboard` on Wall or visit [`http://localhost:8080/dashboard?event=sustaincia-mobility2026`](http://localhost:8080/dashboard?event=sustaincia-mobility2026) | Real-time AI analytics, visual breakdown, and natural-language Q&A |



