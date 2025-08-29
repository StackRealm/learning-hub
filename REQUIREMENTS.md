# REQUIREMENTS — Student Programming Hub (Frontend MVP)

**Project name:** Stack Realm (working title)  
**Goal:** A fast, focused web app that tells student developers **what to do next**, with roadmaps, cheatcards, resources, to-do, and a scheduler.  
**Phase:** Frontend-only (no backend). Future phases add auth, sync, challenges, certificates, and AI.

---

## 1) Scope

### In-Scope (MVP)
- Static + client-side app (React + Tailwind).
- Pages: Home, Learn (Skill Tree/Roadmap), Resources, Cheatcards, Organize (To-Do + Scheduler), About.
- Global search/Command Palette (Ctrl/Cmd+K).
- Local persistence via `localStorage` (to-dos, schedule, pinned items).
- PWA basics: installable + offline cache for key pages/content.
- SEO essentials (meta, sitemap, robots).
- Deploy on Netlify/Vercel (free SSL).

### Out-of-Scope (for MVP)
- User accounts, server-side APIs, DB sync.
- Code judge for challenges, certificates issuance.
- Payments or gated content.

---

## 2) Users & Goals

- **Primary user:** Student/new dev learning programming.
- **Jobs-to-be-done:**
  - “Show me a small, clear next step to learn.”
  - “Let me keep a simple to-do and study schedule.”
  - “Give me quick-reference cheatcards & curated resources.”

---

## 3) Core Features (Functional Requirements)

### 3.1 Home
- Hero with one-liner + CTA (“Open Command Palette”, “Start Learning”).
- Highlights: Learn / Practice / Organize / Resources cards.
- Progress glance (client-only): current streak, weekly minutes (from timer logs), next suggested step.

### 3.2 Learn (Skill Tree / Roadmap)
- Render skill nodes from JSON (`/src/data/roadmap.*.json`).
- Node states: locked → available → completed.
- Node drawer: tabs `Learn` (links), `Practice` (prompts), `Reflect` (notes saved locally).
- XP bar by node; total track progress.

### 3.3 Resources
- Search + filter by tags, difficulty, duration.
- Resource cards: title, minutes, tags, external link, pin.
- Pinning persists in `localStorage`.

### 3.4 Cheatcards
- Tabs for HTML / CSS / JS (extendable).
- Card with snippet title, code block, **Copy** button, and “When to use”.
- Client-side search.

### 3.5 Organize
- **To-Do:** add/edit/delete; reorder; mark done; filter by status; persists locally.
- **Scheduler Generator:** user inputs available hours + topics → generate weekly plan table with export (Print to PDF / CSV download).
- **Focus Timer:** presets (25/5, 45/10), start/pause/reset, auto-log minutes per topic (local).

### 3.6 Command Palette
- Hotkey Ctrl/Cmd+K.
- Search across: pages, roadmap nodes, resources, cheatcards.
- Keyboard navigation + enter to jump.

### 3.7 PWA & Offline
- Install prompt + icons.
- Cache shell + pages: Home, Learn, Cheatcards, Resources list.
- Read-only offline (no external fetches required).

---

## 4) Non-Functional Requirements

- **Performance:** LCP < 2.5s on mid devices; initial JS gzip < 200KB (route-splitting).
- **Accessibility:** WCAG AA contrast; visible focus; ARIA for interactive controls; keyboard-first.
- **Responsiveness:** 360px, 768px, 1280px breakpoints.
- **Reliability:** App works offline for cached pages; local data not lost on refresh.
- **Security (frontend):** No secrets in code; only public URLs; HTTPS enforced by host.

---

## 5) Tech Stack (MVP)

- **Core:** React + Vite, React Router, Tailwind CSS.
- **State:** Context or Zustand (simple global store), `localStorage` for persistence.
- **Motion:** Framer Motion (subtle).
- **Charts (optional):** Recharts for streak/time charts.
- **PWA:** Workbox or Vite PWA plugin.
- **Hosting:** Netlify or Vercel.
- **Lint/Format:** ESLint + Prettier; Husky pre-commit.

---

## 6) Information Architecture

- **Top Nav:** Home, Learn, Resources, Cheatcards, Organize, About.
- **Utilities:** Command Palette, Dark/Light toggle, Install PWA.
- **Footer:** Contribute, License, Contact/Community.

---

## 7) Data Model (Frontend JSON)

```ts
// resource
type Resource = {
  id: string;
  title: string;
  type: "article" | "video" | "tool";
  url: string;
  minutes: number;
  tags: string[];
  rating?: number;
};

// roadmap
type Roadmap = {
  track: "webdev" | "javascript" | "python";
  nodes: {
    id: string;
    title: string;
    xp: number;
    learn: string[];       // resource ids
    practice: string[];    // micro-challenge ids (prompts only in MVP)
    requires?: string[];   // prerequisite node ids
  }[];
};

// cheatcard
type Cheatcard = {
  id: string;
  topic: "html" | "css" | "js";
  title: string;
  code: string;
  whenToUse: string;
};
