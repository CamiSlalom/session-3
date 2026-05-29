# Product Requirements Document (PRD) - Todo App: Due Dates, Priority, and Filters (MVP)

## 1. Overview

We are upgrading the basic Todo app to help users organize tasks by due date and priority. The MVP adds optional due dates, a three-level priority enum, and simple date-based filters (All, Today, Overdue). Storage stays local — no backend changes.

---

## 2. MVP Scope

- Add `dueDate` (optional). Format: ISO `YYYY-MM-DD`. Invalid values are ignored and treated as absent.
- Add `priority` enum: `P1 | P2 | P3` with default `P3`.
- Add filters/tabs: `All`, `Today`, `Overdue` (UI controls to switch views).
- Data model & validation:
  - `title`: required.
  - `completed`: boolean.
  - `priority`: one of `P1`, `P2`, `P3`; default `P3`.
  - `dueDate`: optional ISO `YYYY-MM-DD`; invalid values are ignored.
- Local persistence only (e.g., `localStorage`). No backend or external storage changes.
- Filtering behavior for MVP:
  - `All`: show completed and incomplete tasks.
  - `Today` and `Overdue`: show only incomplete tasks.

---

## 3. Post-MVP Scope

- Visual overdue highlighting (e.g., red styling/badge for overdue tasks).
- Color-coded priority badges: `P1` red, `P2` orange, `P3` gray.
- Sorting improvements: `overdue` first → by `priority` (P1→P3) → by `dueDate` ascending → undated last.
- Additional convenient filters (e.g., `Upcoming`, custom date ranges) and UX polish.

---

## 4. Out of Scope

- Notifications and reminders.
- Recurring tasks and scheduling.
- Multi-user sync or backend persistence.
- Keyboard navigation / advanced accessibility features (deferred).

---

## Data Model (example)

{
  "title": "string",         // required
  "completed": false,        // boolean
  "priority": "P1|P2|P3",  // default "P3"
  "dueDate": "YYYY-MM-DD"  // optional, ISO date or absent
}

---

## Notes & Acceptance Criteria

- Users can add or edit a `dueDate` in ISO `YYYY-MM-DD` format; invalid dates are ignored.
- Users can set task `priority`; default is `P3` when not specified.
- Tabs switch correctly among `All`, `Today`, and `Overdue` and follow the filtering rules above.
- Data persists locally across reloads.
