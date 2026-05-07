# Skill: SaaS UI Redesign — Vertical Sidebar Layout

## Goal

Transform the current horizontal top-nav layout into a modern SaaS-style **vertical collapsible sidebar** layout:
- Sidebar: 240px wide, collapses to 56px icon-only rail (state persisted in `localStorage`)
- Filter bar moves **inside** the sidebar (below nav links)
- No horizontal top nav remains
- Main content fills remaining width with no max-width constraint

## MANDATORY RULE

**ANY time you need to create or significantly modify a `.vue` file, you MUST delegate to the `vue-expert` subagent.**

Do NOT directly edit `.vue` files yourself. Pass the full change specification below to `vue-expert`.

---

## Files to Modify

| File | Nature of change |
|------|-----------------|
| `client/src/App.vue` | Full layout restructure — template + script + styles |
| `client/src/components/FilterBar.vue` | Horizontal sticky strip → vertical stacked layout |

**No changes needed:** views, composables, router, backend, locales, utils.

---

## Change Specification

### 1. `App.vue` — Template

Replace the current structure:
```html
<div class="app">
  <header class="top-nav">…logo, nav-tabs, LanguageSwitcher, ProfileMenu…</header>
  <FilterBar />
  <main class="main-content"><router-view /></main>
  …modals…
</div>
```

With this new structure:
```html
<div class="app">
  <aside class="sidebar" :class="{ collapsed: sidebarCollapsed }">

    <!-- Brand -->
    <div class="sidebar-brand">
      <div class="brand-mark">
        <svg width="28" height="28" viewBox="0 0 28 28" fill="none">
          <rect width="28" height="28" rx="6" fill="#2563eb"/>
          <path d="M7 14h14M14 7v14" stroke="white" stroke-width="2" stroke-linecap="round"/>
        </svg>
      </div>
      <span class="brand-name" v-show="!sidebarCollapsed">{{ t('nav.companyName') }}</span>
      <button class="collapse-btn" @click="sidebarCollapsed = !sidebarCollapsed" :title="sidebarCollapsed ? 'Expand sidebar' : 'Collapse sidebar'">
        <svg width="16" height="16" viewBox="0 0 16 16" fill="none">
          <path
            :d="sidebarCollapsed ? 'M6 3l5 5-5 5' : 'M10 3L5 8l5 5'"
            stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"
          />
        </svg>
      </button>
    </div>

    <!-- Nav links -->
    <nav class="sidebar-nav">
      <router-link to="/">
        <!-- Overview / Dashboard icon: grid of squares -->
        <svg width="18" height="18" viewBox="0 0 18 18" fill="none">
          <rect x="1.5" y="1.5" width="6" height="6" rx="1" stroke="currentColor" stroke-width="1.5"/>
          <rect x="10.5" y="1.5" width="6" height="6" rx="1" stroke="currentColor" stroke-width="1.5"/>
          <rect x="1.5" y="10.5" width="6" height="6" rx="1" stroke="currentColor" stroke-width="1.5"/>
          <rect x="10.5" y="10.5" width="6" height="6" rx="1" stroke="currentColor" stroke-width="1.5"/>
        </svg>
        <span v-show="!sidebarCollapsed">{{ t('nav.overview') }}</span>
      </router-link>
      <router-link to="/inventory">
        <!-- Inventory: archive-box / cube -->
        <svg width="18" height="18" viewBox="0 0 18 18" fill="none">
          <path d="M2.25 5.25h13.5v10.5a1.5 1.5 0 01-1.5 1.5H3.75a1.5 1.5 0 01-1.5-1.5V5.25z" stroke="currentColor" stroke-width="1.5"/>
          <path d="M1.5 2.25h15a.75.75 0 01.75.75v2.25H.75V3a.75.75 0 01.75-.75z" stroke="currentColor" stroke-width="1.5"/>
          <path d="M7.5 9h3" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
        </svg>
        <span v-show="!sidebarCollapsed">{{ t('nav.inventory') }}</span>
      </router-link>
      <router-link to="/orders">
        <!-- Orders: clipboard-list -->
        <svg width="18" height="18" viewBox="0 0 18 18" fill="none">
          <path d="M6 2.25h6a.75.75 0 01.75.75v.75H5.25V3A.75.75 0 016 2.25z" stroke="currentColor" stroke-width="1.5"/>
          <path d="M3.75 3.75H2.25A1.5 1.5 0 00.75 5.25v10.5A1.5 1.5 0 002.25 17.25h13.5A1.5 1.5 0 0017.25 15.75V5.25a1.5 1.5 0 00-1.5-1.5H14.25" stroke="currentColor" stroke-width="1.5"/>
          <path d="M5.25 9h7.5M5.25 12h5.25" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
        </svg>
        <span v-show="!sidebarCollapsed">{{ t('nav.orders') }}</span>
      </router-link>
      <router-link to="/spending">
        <!-- Finance: banknotes -->
        <svg width="18" height="18" viewBox="0 0 18 18" fill="none">
          <rect x="0.75" y="3.75" width="16.5" height="10.5" rx="1.5" stroke="currentColor" stroke-width="1.5"/>
          <circle cx="9" cy="9" r="2.25" stroke="currentColor" stroke-width="1.5"/>
          <path d="M3.75 6.75v4.5M14.25 6.75v4.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
        </svg>
        <span v-show="!sidebarCollapsed">{{ t('nav.finance') }}</span>
      </router-link>
      <router-link to="/demand">
        <!-- Demand Forecast: chart-bar trending up -->
        <svg width="18" height="18" viewBox="0 0 18 18" fill="none">
          <path d="M1.5 13.5l4.5-4.5 3 3 5.25-6 2.25 2.25" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
          <path d="M1.5 16.5h15" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
        </svg>
        <span v-show="!sidebarCollapsed">{{ t('nav.demandForecast') }}</span>
      </router-link>
      <router-link to="/reports">
        <!-- Reports: document-chart-bar -->
        <svg width="18" height="18" viewBox="0 0 18 18" fill="none">
          <path d="M10.5 1.5H4.5A1.5 1.5 0 003 3v12a1.5 1.5 0 001.5 1.5h9A1.5 1.5 0 0015 15V6l-4.5-4.5z" stroke="currentColor" stroke-width="1.5"/>
          <path d="M10.5 1.5V6H15" stroke="currentColor" stroke-width="1.5"/>
          <path d="M6 12.75v-2.25M9 12.75V9M12 12.75v-1.5" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/>
        </svg>
        <span v-show="!sidebarCollapsed">Reports</span>
      </router-link>
    </nav>

    <!-- Filters (hidden when collapsed) -->
    <div class="sidebar-filters" v-show="!sidebarCollapsed">
      <FilterBar />
    </div>

    <!-- Footer: language + profile -->
    <div class="sidebar-footer">
      <LanguageSwitcher v-show="!sidebarCollapsed" />
      <ProfileMenu
        @show-profile-details="showProfileDetails = true"
        @show-tasks="showTasks = true"
      />
    </div>

  </aside>

  <main class="main-content">
    <router-view />
  </main>

  <!-- Modals — keep exactly as they are now -->
  <ProfileDetailsModal
    v-if="showProfileDetails"
    @close="showProfileDetails = false"
  />
  <TasksModal
    v-if="showTasks"
    @close="showTasks = false"
  />
</div>
```

### 2. `App.vue` — Script (`setup()`)

Add the following to the `setup()` function:

```js
import { ref, watch } from 'vue'

const sidebarCollapsed = ref(
  localStorage.getItem('sidebar-collapsed') === 'true'
)
watch(sidebarCollapsed, val => localStorage.setItem('sidebar-collapsed', String(val)))
```

Make sure `sidebarCollapsed` is returned from `setup()`.

Remove the existing `activeTab` / navigation tracking logic that was used by the old `.nav-tabs` — it is no longer needed since `router-link` handles active state automatically via the `.active` class.

### 3. `App.vue` — Styles

**Replace all existing styles** with the following (keep any global body/reset rules, remove all `.top-nav`, `.nav-container`, `.nav-tabs` rules):

```css
/* ── Layout shell ───────────────────────────────────────── */
.app {
  display: flex;
  flex-direction: row;
  min-height: 100vh;
  background: #f8fafc;
}

/* ── Sidebar ────────────────────────────────────────────── */
.sidebar {
  width: 240px;
  min-height: 100vh;
  height: 100vh;
  background: #ffffff;
  border-right: 1px solid #e2e8f0;
  display: flex;
  flex-direction: column;
  position: sticky;
  top: 0;
  overflow-y: auto;
  overflow-x: hidden;
  flex-shrink: 0;
  transition: width 0.2s ease;
  z-index: 100;
}

.sidebar.collapsed {
  width: 56px;
}

/* Brand row */
.sidebar-brand {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 1.25rem 1rem;
  border-bottom: 1px solid #f1f5f9;
  min-height: 64px;
}

.sidebar.collapsed .sidebar-brand {
  justify-content: center;
  padding: 1.25rem 0;
}

.brand-mark {
  flex-shrink: 0;
  display: flex;
  align-items: center;
}

.brand-name {
  font-size: 0.9375rem;
  font-weight: 700;
  color: #0f172a;
  white-space: nowrap;
  overflow: hidden;
  flex: 1;
}

.collapse-btn {
  flex-shrink: 0;
  width: 24px;
  height: 24px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: none;
  border: 1px solid #e2e8f0;
  border-radius: 4px;
  color: #64748b;
  cursor: pointer;
  padding: 0;
  transition: all 0.15s ease;
  margin-left: auto;
}

.collapse-btn:hover {
  background: #f1f5f9;
  color: #0f172a;
}

.sidebar.collapsed .collapse-btn {
  margin-left: 0;
}

/* Nav links */
.sidebar-nav {
  padding: 0.75rem 0.5rem;
  flex: 0 0 auto;
}

.sidebar-nav a {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.625rem 0.75rem;
  border-radius: 6px;
  color: #64748b;
  text-decoration: none;
  font-size: 0.875rem;
  font-weight: 500;
  transition: all 0.15s ease;
  white-space: nowrap;
  margin-bottom: 2px;
}

.sidebar-nav a:hover {
  background: #f1f5f9;
  color: #0f172a;
}

.sidebar-nav a.router-link-active,
.sidebar-nav a.active {
  background: #eff6ff;
  color: #2563eb;
}

.sidebar-nav a svg {
  flex-shrink: 0;
}

.sidebar.collapsed .sidebar-nav a {
  justify-content: center;
  padding: 0.625rem;
  gap: 0;
}

/* Filters section */
.sidebar-filters {
  padding: 0.75rem 0.5rem 0;
  border-top: 1px solid #f1f5f9;
  flex: 1 1 auto;
  overflow-y: auto;
}

/* Footer */
.sidebar-footer {
  margin-top: auto;
  padding: 0.75rem 0.5rem;
  border-top: 1px solid #f1f5f9;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

/* ── Main content ───────────────────────────────────────── */
.main-content {
  flex: 1;
  min-width: 0;
  padding: 1.5rem 2rem;
  overflow: auto;
}
```

### 4. `FilterBar.vue` — Vertical layout

Change the filter bar from a horizontal sticky strip to a vertical stacked layout:

- **Remove** `position: sticky; top: 70px` and any horizontal container/flex rules from the outer wrapper
- **`.filters-container`**: `display: flex; flex-direction: column; gap: 0.75rem; padding: 0; background: none; border: none; box-shadow: none;`
- **`.filters-grid`** (or equivalent row): `display: flex; flex-direction: column; gap: 0.5rem;`
- **`.filter-group`**: `display: flex; flex-direction: column; align-items: flex-start; gap: 0.25rem;`
- **`.filter-label`** / `label`: `font-size: 0.75rem; font-weight: 600; color: #64748b; text-transform: uppercase; letter-spacing: 0.05em;`
- **`.filter-select`**: `width: 100%; font-size: 0.8125rem;`
- **Reset button**: place below the last filter, full-width or auto-width, aligned left

---

## SVG Icon Reference

All icons use: `stroke="currentColor"`, `stroke-width="1.5"`, `fill="none"`, size `18×18`.

| Route | Icon style |
|-------|-----------|
| `/` Overview | 4-square grid (rect×4) |
| `/inventory` | Archive box with center line |
| `/orders` | Clipboard with two lines |
| `/spending` Finance | Rectangle with circle center, vertical end-lines |
| `/demand` Demand Forecast | Trending line chart with baseline |
| `/reports` | Document with folded corner + 3 bar chart lines |

---

## Verification Checklist

After executing this skill, verify:

- [ ] Sidebar appears on the left at 240px
- [ ] Collapse toggle shrinks sidebar to 56px icon-only rail
- [ ] Collapsed state persists on page reload (via `localStorage`)
- [ ] All 6 nav links visible with correct icons; active route highlighted in blue
- [ ] Clicking a nav link navigates correctly and highlight updates
- [ ] 4 filter selects render vertically inside sidebar; filter state still works
- [ ] Reset button still clears all filters
- [ ] LanguageSwitcher and ProfileMenu visible at sidebar bottom
- [ ] Language switch works; ProfileMenu dropdown opens
- [ ] ProfileDetailsModal and TasksModal open from ProfileMenu
- [ ] Main content area fills remaining width with no max-width constraint
- [ ] No horizontal top nav bar visible anywhere
- [ ] App tested at `http://localhost:3000`
