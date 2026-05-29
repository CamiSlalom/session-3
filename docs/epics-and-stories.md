# Epics and User Stories (MVP and Post-MVP)

Based on the PRD: Todo App - Due Dates, Priority, and Filters.

## MVP Epics

- Epic: Due Date Management
  - Story: Add optional `dueDate` field to tasks
    - Acceptance Criteria:
      - Users can enter an optional `dueDate` in ISO `YYYY-MM-DD` when creating a task.
      - If the `dueDate` field is left blank, the task is saved without a `dueDate`.
      - Invalid date formats are ignored and the `dueDate` is not stored.
      - Saved `dueDate` appears in the task list in `YYYY-MM-DD` format.
  - Story: Validate `dueDate` format on create/edit
    - Acceptance Criteria:
      - On create or edit, the app accepts only `YYYY-MM-DD` strings as valid dates.
      - Invalid `dueDate` input is treated as absent (ignored) and does not prevent saving the task.
      - No backend errors occur when an invalid date is entered; stored model remains consistent.
  - Story: Edit and remove task `dueDate`
    - Acceptance Criteria:
      - Users can update an existing task's `dueDate` and the change persists.
      - Users can clear/remove the `dueDate` from a task and the task then has no `dueDate`.
      - Edits to `dueDate` are reflected immediately in the task list and persisted to local storage.
  - Story: Display `dueDate` in task list
    - Acceptance Criteria:
      - Each task that has a `dueDate` shows it in the task list in `YYYY-MM-DD` format.
      - Tasks without a `dueDate` show no date field or show a consistent empty state.

- Epic: Priority Support
  - Story: Add `priority` enum (P1/P2/P3) to tasks
    - Acceptance Criteria:
      - Tasks store a `priority` value that is one of `P1`, `P2`, or `P3`.
      - The UI exposes a control to choose `P1`, `P2`, or `P3` when creating or editing tasks.
  - Story: Default priority to P3
    - Acceptance Criteria:
      - When a task is created without a specified `priority`, it is saved with `priority: P3`.
      - Existing tasks without an explicit `priority` behave as `P3` in filters and displays.
  - Story: Select priority when creating/editing tasks
    - Acceptance Criteria:
      - Users can select a task's priority during creation and when editing; the selection persists.
      - The selected priority is shown in the task editor and reflected in the task list.
  - Story: Display priority in task list
    - Acceptance Criteria:
      - Each task shows its priority label (`P1`, `P2`, `P3`) in the task list.
      - Priority display is clear and consistent (text or badge) for all tasks.

- Epic: Filters & Views
  - Story: Add `All`, `Today`, and `Overdue` tabs
    - Acceptance Criteria:
      - The UI includes three distinct tabs/controls labeled `All`, `Today`, and `Overdue`.
      - Clicking a tab activates it and updates the visible task list accordingly.
  - Story: Implement `Today` filter for incomplete tasks due today
    - Acceptance Criteria:
      - `Today` shows only incomplete tasks whose `dueDate` equals the user's local current date.
      - Tasks due today but already completed do not appear in `Today`.
  - Story: Implement `Overdue` filter for incomplete past-due tasks
    - Acceptance Criteria:
      - `Overdue` shows only incomplete tasks with a `dueDate` strictly before the user's local current date.
      - Tasks without a `dueDate` are not included in `Overdue`.
      - Completed tasks are excluded from `Overdue`.
  - Story: Ensure `All` shows completed and incomplete tasks
    - Acceptance Criteria:
      - `All` displays both completed and incomplete tasks regardless of `dueDate`.
      - Switching to `All` preserves task order and any sorting behavior defined in the app.

- Epic: Task CRUD & Data Model
  - Story: Create task with required `title`
    - Acceptance Criteria:
      - Users cannot create a task with an empty `title`; the UI prevents submission.
      - Valid tasks with a non-empty `title` are created and persisted.
  - Story: Edit task `title`, `priority`, and `dueDate`
    - Acceptance Criteria:
      - Users can edit a task's `title`, `priority`, and `dueDate` and changes persist to local storage.
      - Editing a task with an invalid `dueDate` behaves as described in the `dueDate` validation criteria.
  - Story: Delete task
    - Acceptance Criteria:
      - Users can delete a task; deleted tasks are removed from the UI and from local storage.
      - Deletion is immediate and does not affect unrelated tasks.
  - Story: Enforce data model validation (title required)
    - Acceptance Criteria:
      - The app enforces the model: `title` required, `completed` boolean, `priority` in `P1|P2|P3`, `dueDate` optional ISO date.
      - Invalid `priority` values are coerced to the default `P3` or rejected by the editor.

- Epic: Local Persistence
  - Story: Persist tasks to `localStorage`
    - Acceptance Criteria:
      - Tasks are saved to `localStorage` after create, edit, delete, and on explicit state changes.
      - Data in `localStorage` reflects the data model shape shown in the PRD.
  - Story: Load tasks from `localStorage` on app start
    - Acceptance Criteria:
      - On app start or page reload, tasks are loaded from `localStorage` into the app state.
      - Corrupted or invalid entries in `localStorage` are handled gracefully (ignored or migrated) without crashing the app.

## Post-MVP Epics

- Epic: Visual Overdue Highlighting
  - Story: Add overdue styling to tasks
    - Acceptance Criteria:
      - Tasks that are incomplete and have a `dueDate` before the current date are visually styled as overdue (e.g., red text or background).
      - Styling is applied consistently across list views and respects accessibility contrast requirements.
  - Story: Add overdue badge indicator
    - Acceptance Criteria:
      - Overdue tasks show a visible badge or label indicating they are overdue.
      - The badge is present in list and detail views where tasks are shown.

- Epic: Priority Badges & Colors
  - Story: Add color-coded priority badges (P1 red, P2 orange, P3 gray)
    - Acceptance Criteria:
      - Priority badges are rendered with the specified colors for `P1` (red), `P2` (orange), and `P3` (gray).
      - Badges are accessible (text alternatives and sufficient contrast).
  - Story: Update task list to show priority badges
    - Acceptance Criteria:
      - Each task in the list displays the corresponding priority badge.
      - Badge placement does not break layout on narrow screens.

- Epic: Sorting & Ordering
  - Story: Sort overdue tasks first
    - Acceptance Criteria:
      - When sorting is enabled, overdue tasks appear before non-overdue tasks.
  - Story: Sort tasks by priority (P1→P3)
    - Acceptance Criteria:
      - Tasks are ordered by priority with `P1` before `P2` before `P3` within their groups.
  - Story: Sort by `dueDate` ascending with undated last
    - Acceptance Criteria:
      - Within the same priority group, tasks are ordered by `dueDate` ascending; tasks without a `dueDate` appear after dated tasks.
  - Story: Combine sorting rules into unified ordering
    - Acceptance Criteria:
      - The combined ordering is: overdue first → by priority (P1→P3) → by `dueDate` ascending → undated last.
      - The ordering is deterministic and stable across view updates and reloads.

- Epic: Advanced Filters & UX
  - Story: Add `Upcoming` filter
    - Acceptance Criteria:
      - `Upcoming` shows incomplete tasks with `dueDate` within the next configurable window (default: next 7 days).
  - Story: Add custom date range filter
    - Acceptance Criteria:
      - Users can provide a start and end date to filter incomplete tasks whose `dueDate` falls within the range.
  - Story: Improve filter UI and interactions
    - Acceptance Criteria:
      - Filter controls are discoverable and usable on desktop and mobile; state persists across short navigation.

- Epic: UX Polish
  - Story: Improve responsive layout and mobile usability
    - Acceptance Criteria:
      - Task list and editor layouts adapt to narrow screens without losing core functionality.
  - Story: Add minor accessibility improvements and focus states
    - Acceptance Criteria:
      - Interactive controls (tabs, form fields, buttons) have visible focus states and are keyboard accessible.
      - Color usage meets WCAG AA contrast where applicable.
      - Provide visible focus indicators for all focusable elements


    ## Technical Requirements

    The following technical requirements are derived from the acceptance criteria above and the project's frontend/backend guidelines. They are concise, actionable, and intended to guide implementation and testing.

    **Frontend**

    - **Data model (client):** Tasks are JS objects with fields: `id` (number), `title` (string, required), `description` (string), `due_date` (string | null in `YYYY-MM-DD`), `completed` (boolean), `priority` (`P1`|`P2`|`P3`, default `P3`), and `created_at` (ISO timestamp).
    - **Form validation & normalization:** `TaskForm` must require non-empty `title`; accept `dueDate` via an HTML date input and normalize/store as `YYYY-MM-DD`; invalid or unparsable dates must be treated as absent (not stored) and should not block save.
    - **Priority handling:** UI exposes a select control for `P1`/`P2`/`P3`; creating a task without priority sets `P3`. Invalid priority values from any source are coerced to `P3` client-side.
    - **Display & formatting:** `TaskList` displays `due_date` in `YYYY-MM-DD` format and also supports a localized, human-friendly display. Tasks without `due_date` show a consistent empty state; priority is shown as a clear label or badge per UI guidelines.
    - **Filters & view logic (client):** Implement `All`, `Today`, and `Overdue` views client-side (or via API parameters). `Today` shows incomplete tasks with `due_date ===` user's local date. `Overdue` shows incomplete tasks with `due_date` before local date. Completed tasks are excluded from `Today` and `Overdue`.
    - **Persistence & API usage:** Frontend should call backend `/api/tasks` endpoints when available; provide a `localStorage` fallback that uses the same JSON shape. On load, migrate or ignore malformed `localStorage` entries without crashing.
    - **Accessibility & UI toolkit:** Use MUI components, ensure keyboard accessibility, visible focus states, and WCAG AA contrast for badges and overdue styling.
    - **Testing & quality:** Add unit tests for date normalization, validation, filter logic, and component rendering. Follow ESLint/Prettier and coding guidelines (2-space indent, semicolons, single quotes).

    **Backend**

    - **API contract & JSON shape:** Expose REST endpoints (`GET /api/tasks`, `POST /api/tasks`, `PUT /api/tasks/:id`, `PATCH /api/tasks/:id`, `DELETE /api/tasks/:id`) that use snake_case fields (e.g., `due_date`). Validate and return tasks with fields: `id`, `title`, `description`, `due_date` (nullable `YYYY-MM-DD`), `completed` (boolean), `priority` (`P1`|`P2`|`P3`, default `P3`), `created_at`.
    - **Server-side validation:** Reject requests with empty `title` (400). Validate `due_date` format; if invalid, store `NULL` (do not error). Coerce invalid `priority` to `P3` or reject with 400 depending on endpoint semantics (prefer coercion for resilience).
    - **Filtering & query support:** Support query params for `completed`, search (`q` or `search`), and date-range or date-equality filters to enable server-side `Today`/`Overdue` views. Use parameterized queries to avoid injection.
    - **Persistence & schema:** Database schema must store `due_date` as a date/text in `YYYY-MM-DD` and `priority` as constrained enum/text. Provide graceful handling/migration for corrupted rows during load.
    - **Error handling & stability:** API must never throw unhandled errors on invalid dates or malformed payloads; return consistent JSON error objects and appropriate HTTP codes.
    - **Testing & ops:** Add integration tests for create/edit/delete flows, validation edge cases (invalid dates, missing titles, invalid priority), and filter endpoints. Log errors and return safe messages for clients.
    - **Security & limits:** Sanitize inputs, enforce reasonable max lengths for text fields, and paginate or limit list endpoints to avoid performance issues on large datasets.

    These technical requirements map directly to the acceptance criteria and follow the project's UI and coding guidelines. Implementers should use them as a checklist for both frontend and backend work and add tests that cover the specified behaviors.



