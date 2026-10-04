# Screen Generation Prompt Template

> **How to use:**  
> 1. Copy `system.md` content as your system prompt (or paste at the top)  
> 2. Fill in the `[ ]` fields below  
> 3. Paste the filled prompt into Claude (or send via API)  
> 4. Claude returns a self-contained HTML file

---

## Prompt

```
[PASTE system.md CONTENT HERE — or send as the system message via API]

---

## Screen to generate

**Screen type:** [e.g. dashboard/overview]  
**Page title:** [e.g. "Sales Dashboard"]  
**Description:** [1–2 sentences on what this screen is for and who uses it]

---

## Layout shell

Use: [dashboardShell | authShell | settingsShell | listDetailShell]

Shell specs:
- dashboardShell → Sidebar 280px (left, persistent) + AppBar 64px (top) + main content area 40px padding, max-width 1200px
- authShell      → Centered card 480px wide, gradient or solid color background
- settingsShell  → Vertical nav tabs 240px (left) + content panel max-width 800px
- listDetailShell → List panel 360px (left, scrollable) + detail panel flex-1

---

## Building blocks to include

[List the blocks this screen uses, in top-to-bottom order. Remove ones you don't need.]

- [ ] sidebar-nav      — persistent left nav with logo, grouped items, user account
- [ ] page-header      — breadcrumb + h4 title + action buttons
- [ ] filter-bar       — search input + status/category selects + date range
- [ ] stats-row        — 4 KPI cards (number + trend chip + period label)
- [ ] chart-card       — area/bar/donut chart with period toggle
- [ ] data-table       — toolbar + sortable thead + tbody rows + pagination
- [ ] card-grid        — responsive 3-col card grid with image, label, title, actions
- [ ] form-section     — 2-col labeled field groups in a card with save/cancel
- [ ] kanban-column    — status column with draggable task cards
- [ ] empty-state      — centered icon/illustration + heading + CTA
- [ ] notification-item — avatar + action text + timestamp + unread dot
- [ ] detail-header    — cover image + overlapping avatar + name + stats + buttons

---

## Content specification

**Domain:** [e.g. SaaS analytics / e-commerce / project management / HR platform]

**Data to show** (be specific — AI uses this to write realistic copy):
- Page title: [e.g. "Analytics Overview"]
- KPI metrics: [e.g. "Total Revenue $38.5k (+12%), Active Users 8,249 (+1.2%), Churn Rate 2.1% (-0.3%), Open Tickets 142 (+18)"]
- Table columns: [e.g. "User name+email, Role, Status (Active/Pending/Inactive), Joined date, Revenue"]
- Table rows: [e.g. "4–6 realistic rows with SaaS company names/people"]
- Chart: [e.g. "Monthly revenue line chart, Jan–Oct 2026, upward trend, two series: Revenue vs Target"]
- Card titles: [e.g. "3 article cards: design system article, accessibility guide, animation best practices"]

---

## Component variants to use

[Override defaults if needed — otherwise leave blank and AI picks from variants.json]

- Buttons: [e.g. "primary actions = contained primary, secondary = outlined inherit"]
- Status labels: [e.g. "Active = success soft, Pending = warning soft, Inactive = error soft"]
- Table: [e.g. "medium density, with row selection checkboxes"]
- Charts: [e.g. "area chart with gradient fill, primary color"]

---

## Constraints

- Dark mode: [required | optional | light-only]
- Responsive: [full (desktop+mobile) | desktop-only]  
- Interactive: [yes — include hover states, active nav, working toggles | static only]
- Sidebar state: [expanded 280px | collapsed 88px]

---

## Output

Return a single self-contained HTML file.  
Filename suggestion: `[screen-type]-screen.html`  
(e.g. `dashboard-overview-screen.html`)
```

---

## Quick-fill examples

### Example A — Dashboard Overview

```
Screen type: dashboard/overview
Page title: "Sales Dashboard"
Description: Main dashboard for a SaaS sales team to monitor revenue, user growth, and recent activity.

Layout shell: dashboardShell

Blocks: sidebar-nav, page-header, stats-row, chart-card (×2), data-table

Domain: SaaS / CRM

Data:
- KPIs: Total Revenue $38.5k (+12.4%), New Users 1,240 (+8.1%), Conversion 3.6% (-0.8%), Open Deals 47 (+3)
- Chart 1: Monthly revenue area chart Jan–Oct 2026, two series Revenue (green) vs Target (orange dashed)
- Chart 2: Deal stage donut — Prospecting 35%, Negotiation 28%, Closed Won 22%, Lost 15%
- Table: Recent deals — Company name, Contact, Stage, Value, Closing date (5 rows, realistic SaaS companies)

Variants: contained primary buttons, soft labels for stages, medium table
Dark mode: required
Interactive: yes
```

### Example B — User Management Table

```
Screen type: data/table
Page title: "User Management"
Description: Admin page for managing team members, roles, and permissions.

Layout shell: dashboardShell

Blocks: sidebar-nav, page-header, filter-bar, data-table, empty-state

Domain: SaaS admin panel

Data:
- Page header: "User Management", subtitle "48 members", buttons: "Import CSV" (outlined) + "Add user" (contained primary)
- Filters: search by name/email, Status select (All/Active/Inactive/Pending), Role select (All/Admin/Editor/Viewer)
- Table: Name+email (with avatar), Role badge, Status label, Last active, Actions (more-vert menu)
- 5 rows: realistic people, mixed statuses

Variants: dense table, soft status labels, no row selection
Dark mode: required
Interactive: yes (filter inputs work, hover rows)
```

### Example C — Login Screen

```
Screen type: auth/login
Page title: "Sign In"
Description: Login screen for a SaaS analytics product called "Insightful".

Layout shell: authShell

Blocks: form-section

Domain: Data analytics SaaS

Data:
- Logo: "Insightful" wordmark with a small chart icon
- Headline: "Welcome back"
- Subline: "Sign in to your Insightful workspace"
- Fields: Email, Password (with show/hide toggle)
- Extras: "Remember me" checkbox, "Forgot password?" link
- Social: "Sign in with Google" button
- Footer: "Don't have an account? Start free trial"

Dark mode: required
Interactive: yes (password visibility toggle, form validation state on submit)
```
