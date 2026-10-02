# io.Nest Solar portal: feature walkthrough

Read-only walkthrough of https://solar.ionest.cloud on 2 Oct 2026, signed in as a client user with 2 plants. Nothing was changed: no forms submitted, no tickets raised, no downloads. Screenshots are in the project folder solar-portal/existing-portal/ (not in the repo, as they show client data).

## 1. Structure

Single-page app (hash routes), one top bar and four tabs. Installable on phones (web app manifest).

| Area | Route | Who can open it |
|---|---|---|
| Sign in | `#/login` | everyone |
| Plants (home) | `#/fleet` | every user |
| Plant detail | `#/plant/<id>` | every user, own plants only |
| Reports | `#/reports` | every user |
| Support (tickets) | `#/support`, `#/ticket/<id>` | every user (staff see all tickets) |
| Alerts (fault monitor) | `#/alerts` | every user |
| Change password | `#/password` | every user |
| Admin tools | `#/admin` | hub page, links depend on role |
| Plant setup (location, capacity) | `#/siting` | support team only |
| Subscription renewals | `#/renewals` | staff (client sees "Nothing due") |
| Parameter report | `#/param-report` | root (Master) account only |
| Gateways and new clients | `#/gateways` | Master account only |

Roles seen in the code: client user, support team, Master/root. There is no user management screen, no groups, no per-site sharing, no audit log.

Top bar on every screen: back button, logo (to Plants), page title, Change password (key icon), Theme (follows sunrise and sunset, or always light, or always dark), Sign out.

## 2. Screen by screen

### Sign in (00_login.png)
Username and password, Sign in. No "forgot password", no request access, no 2FA.

### Plants, the home screen (01_plants.png)
- **Assistant briefing** in plain language: greeting by time of day, how many plants are reporting, generation now, today's kWh and its ₹ value, and "nothing needs your attention" or what does.
- **KPI tiles**: Generating now (kW, n of n live), Today (kWh), Lifetime (MWh), Attention (offline and delayed counts), Electricity value generated (₹ lifetime and today), CO₂ avoided (t, with trees equivalent).
- **Your plants** cards: name, location, last data age, Live/Offline chip, Now kW, Today kWh, Total MWh. Click opens the plant.
- Map of plant locations ("Where your plants are") appears once locations are set.

### Plant detail (02_plant_detail.png)
- Assistant briefing for the plant, with current weather at the site.
- **Subscription card**: active until date, days to go, days served, extra days credited for periods with no data.
- KPI tiles: Generating now, Today, Lifetime, Status (Live, last update).
- **Six live diagram styles** (10–15): Plant (drawn mimic of panels, inverters, grid tower, diesel set, busbar, building with live kW on each), Energy flow (animated), Gauges (one shared scale for all dials), Single-line (breakers open or closed), Contribution (share of site load from solar, grid, diesel), Weather (irradiance and supply).
- Per-source panel: Solar, Grid (import or export), Diesel (running or standby), Site load (calculated), and per-inverter and per-DG-meter tables. Hints when a meter is missing ("no grid meter, add one to see consumption").
- **Site conditions** from a weather forecast (Open-Meteo): irradiance, ambient temperature, humidity, wind, cloud cover, flagged EST when there are no on-site sensors.
- **Versus weather forecast**: expected kW vs actual, peak today.
- Impact tiles: Running on solar (% of site load), Saved today and lifetime in ₹ at the plant tariff (and DG tariff), Peak today, CO₂ avoided today and lifetime.
- Equipment list: each inverter and meter with kW and energy.
- **Generation and consumption chart**: Day (15-min power by source), Week, Month, Year (energy per day or month); tap a source to isolate it; "Show the numbers" opens the data table; ₹ value of the period.
- **Straight from the logger**: connection state, time recorded and checked, and every raw register per device (voltages and currents per phase, power, PF, frequency, import and export kWh, PV string V and I, inverter state, fault code, load %).
- **Last data**: expandable per-device card with the latest values.

### Reports (03_reports.png, 17_report_monthly.png)
Pick plant, report type (Daily generation, Monthly summary, Interval readings), quick ranges (This month, Last month, Last 30 days, This year) or a From/To range, Generate. Shows totals (solar kWh, peak kW, diesel kWh) and the table. Downloads as PDF, Excel (with chart and live formulas) or CSV. Interval readings are limited to 62 days.

### Support (04_support.png)
Raise a ticket: category (no data, low generation, wrong figure, faulty inverter, reports, login, other), plant, subject, description. The system runs automatic checks on the plant first and often answers straight away. Ticket list and ticket page with conversation, reply, and Mark resolved. Staff see a "What is needed from you" queue.

### Alerts (05_alerts.png)
Fault monitor derived from live data: Faults, Warnings, Plants monitored, last check time, and a list of plants not reporting or inverters producing nothing.

### Change password (06_password.png)
Current password, new password with a live strength checklist (8+ characters, upper and lower case, number, symbol), confirm, show passwords.

### Admin tools (07_admin_tools.png) and staff screens (seen in the app code, not openable with this account)
- Plant setup: location (nearest town) and capacity kWp per plant, so output can be compared with live weather.
- Subscription renewals: who is due within N days, service delivered, days extended by downtime, annual fee, tariff ₹/kWh, DG ₹/kWh, Save, Mark paid, Recalculate.
- Parameter report: pick any parameters across meters (search "freq", "volt", "kWh"), interval (auto, 15 min, hourly, daily), range, preview, PDF or Excel with charts.
- Gateways: create a gateway folder, provision a new client from a folder that is already sending data (plant name, database, optional login; password shown once).
- An "Ask" AI assistant panel exists in the code, off for this account.

## 3. What it does well
Plain-language summaries, money and CO₂ framing, multi-source sites (solar with grid and diesel), six live diagram styles, raw logger data for engineers, downtime credit on subscriptions, auto-triage of support tickets, good mobile layout, automatic dark theme.

## 4. What it lacks (for the new portal)
No multi-user model: no organisations, user groups, site groups, roles you can edit, invitations, registration approval, suspension, 2FA or audit log. No portfolio view for hundreds of plants (no search, filters, sorting, paging, status breakdown, PR ranking). No performance ratio, specific yield, availability or expected-vs-actual by day. No alarm acknowledgement workflow. No scheduled report emails. Reports are one plant at a time.

## 5. Gap against the Helio prototype
| io.Nest feature | In prototype before | Added |
|---|---|---|
| Assistant briefing (plain-language summary) | no | yes, Overview and Site |
| Grid and diesel sources, site load, solar share | no | yes |
| Live diagram styles (plant, flow, gauges, single-line, contribution, weather) | no | yes |
| Site conditions and versus-forecast | no | yes |
| ₹ saved, CO₂, trees equivalent | partly | yes |
| Day/Week/Month/Year chart, source toggles, data table | no | yes |
| Raw logger registers per device | no | yes |
| Report types (daily, monthly, interval), quick ranges, PDF/Excel/CSV | partly | yes |
| Parameter report | no | yes |
| Support tickets with auto-checks and conversation | no | yes |
| Subscriptions and renewals with downtime credit | no | yes |
| Change password with strength rules | no | yes |
| Theme: auto by sunrise/sunset, light, dark | toggle only | yes |
| Fault monitor | yes (Alarms) | kept |
| Gateway provisioning | yes (Add site wizard, data connection) | kept |
