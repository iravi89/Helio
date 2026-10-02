# New solar portal: plan

Status: draft, 2 Oct 2026. The existing portal (solar.ionest.cloud) was reviewed read-only on 2 Oct 2026; see section 6.

Prototype: https://claude.ai/artifact/REq5Ni562DSKLNpHxRTzbh (sample data, 360 sites, 64 users)

## 1. Access model

Two independent questions decide what a person sees:

| Question | Answered by | Example |
|---|---|---|
| What can they do? | **Role** (one per person) | O&M Engineer can acknowledge alarms but not see revenue |
| Which sites can they see? | **User groups** → **site groups**, plus optional direct site grants | "VoltCare O&M South" sees the "South region" site group |

- **Site groups** come in three kinds: Customer (automatic), Region (automatic), Custom (hand-picked or rule-based, e.g. "over 1 MWp").
- **Organisations** (HQ, each customer, each O&M contractor) bound what an admin can manage. A Customer Admin only sees and manages people in their own organisation and can only grant roles below their own.
- Data is filtered on the server by both role and site scope. Revenue/tariff fields are stripped from API responses for roles without "See revenue", including in exports and scheduled emails.

### Starting roles

| Permission | Platform Admin | Customer Admin | Portfolio Mgr | O&M Engineer | Finance | Viewer |
|---|---|---|---|---|---|---|
| View dashboards and sites | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Inverter / device detail | ✓ | ✓ | ✓ | ✓ | | |
| Revenue and tariffs | ✓ | ✓ | ✓ | | ✓ | |
| Export and schedule reports | ✓ | ✓ | ✓ | ✓ | ✓ | |
| Acknowledge alarms | ✓ | ✓ | ✓ | ✓ | | |
| Edit site settings | ✓ | | ✓ | | | |
| Manage users and groups | ✓ | ✓ (own org) | | | | |
| Approve registrations | ✓ | ✓ (own org) | | | | |
| Edit roles | ✓ | | | | | |
| Audit log | ✓ | ✓ (own org) | | | | |

Roles are editable (custom roles allowed); Platform Admin is fixed.

## 2. User registry

- Invite by email (admin), or self-service "Request access" from the sign-in page → lands in a review queue. Nothing is visible until an admin approves and assigns a role plus at least one group.
- Statuses: Invited, Active, Suspended. Suspending ends sessions immediately.
- Two-factor sign-in, enforceable per role (on by default for admins).
- Optional SSO (Google / Microsoft) per organisation later.
- Every change to people, groups, roles and sites goes to an append-only audit log with actor, time and IP.

## 3. Screens

Site setup (Platform Admin and Portfolio Manager, via "Edit site settings"): Add site wizard in seven steps (basics, plant, data connection, devices, targets and alarms, access, review) with validation such as DC/AC ratio and module count vs kWp; "Test connection" pulls the inverter list from the data source; CSV import for many sites at once; Site settings with the same sections as tabs plus Archive (keeps history, stops collection).

Monitor: Overview (portfolio KPIs, daily generation vs expected, status breakdown, worst/best PR, site map) · Sites (search, filter by status/region/type/group, sort, paging for hundreds of sites) · Site detail (live power curve vs expected, 30-day energy, inverters, alarms, details, who has access) · Reports (scope by site group, 7/14/30 days or custom range, group by customer/region/type/site, CSV/PDF export, scheduled emails) · Alarms (open/acknowledged/cleared, acknowledge).

Administration: Users (people + registration requests) · Groups and access (user groups, site groups) · Roles (permission matrix) · Audit log.

## 4. Reporting metrics

Energy (kWh), expected energy (weather-adjusted at target PR), variance, performance ratio, specific yield (kWh/kWp/day), availability, CUF, revenue at contracted tariff, CO₂ avoided (CEA grid factor 0.716 kg/kWh). Later: irradiance-based PR from on-site pyranometers, soiling loss, degradation trend, budget vs actual per month.

## 5. Suggested production stack (to confirm)

- Frontend: React + TypeScript (Next.js), charts with ECharts, Tailwind.
- API: Node (NestJS) or Python (FastAPI); Postgres for users/sites/groups; TimescaleDB for time-series readings.
- Auth: Keycloak or Auth0/Clerk for login, 2FA and SSO; permissions checked in the API, not just hidden in the UI.
- Ingestion: MQTT/Modbus gateway or the existing data source's API → queue → time-series DB; 5–15 min resolution.
- Hosting: containers on AWS/Azure/GCP, nightly report jobs.

## 6. Existing portal inventory

Full walkthrough: `existing-portal.md` (screenshots in the project folder `solar-portal/existing-portal/`).

io.Nest is a single-customer view: Plants, plant detail, Reports, Support, Alerts, Change password, plus staff-only Plant setup, Renewals, Parameter report and Gateways. Its strengths, now carried into the prototype:

- Plain-language assistant briefing on the home and plant screens.
- Multi-source sites: solar with grid import/export and diesel sets, site load and solar share.
- Six live views of a plant: equipment mimic, energy flow, gauges, single-line, contribution, weather.
- Site conditions from a weather forecast and "versus forecast" check.
- ₹ saved (grid and diesel rates), CO₂ and trees equivalent.
- Day/Week/Month/Year chart by source, with a data table.
- Raw logger registers per device.
- Report types: daily, monthly, interval readings, any-parameter report; PDF/Excel/CSV.
- Support tickets answered first by automatic checks, then by staff.
- Subscriptions with automatic downtime credit, fees and tariffs.
- Password rules and a theme that follows sunrise and sunset.

What it lacks, and the prototype already covers: organisations, user groups, site groups, editable roles, invitations and registration approval, 2FA, audit log, a portfolio view for hundreds of sites, PR and availability, alarm acknowledgement and scheduled report emails.

New permissions added for this: "Answer and resolve support tickets" (Platform Admin, Portfolio Manager, O&M Engineer) and "Manage subscriptions and renewals" (Platform Admin, Finance Analyst).

## 7. Open questions

1. Do you want a GitHub repo for the production build, and is the stack above OK?
2. Where does the site data come from today (inverter vendor cloud, own dataloggers, the existing portal's API)?
3. Which customers/orgs and roles do you actually need on day one?
