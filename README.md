# MEGA.dev S01E03 · what it shows

Ticket **#4471** of the support desk (412 reports, merged and marked known) holds **two** different
bugs. This repo files one ticket per problem and adds a page about them to the kb.

| ticket | problem | reports it covers |
| --- | --- | --- |
| [#1](../../issues/1) | the shared date parser reads every date month first, and every tenant writes the day first | **379** |
| [#2](../../issues/2) | `booking-v3` prints `MM/DD/YYYY` for day-first tenants, so people type month first and the form reads it day first | **33** |

379 + 33 = 412, the ticket as the desk counted it.

## How the counts were made

The kb export (`reports-4471.json`) has **427 rows for 412 unique report ids** — the export pages
overlap, so 15 rows repeat an earlier row word for word. Every unique report is merged into #4471.

Each report is then sorted by the desk's own logic, not by intake's tag:

- `src/lib/kb/causes.ts` → `uniqueById`, then `causeOf(r, tenantOf(r))` / `groupByCause`
- `src/lib/kb/locales.ts` → the tenant directory: locale and booking form per tenant
- a report is `hint` only when its channel is `form` **and** its tenant's form prints `MM/DD/YYYY`
  (booking-v3: `alder.physio`, `copper.studio`, `pale.harbour`); otherwise `parser`

Intake's `meta.intakeTag` is guessed from the reporter's browser, so it disagrees with the tenant:
23 `parser` reports were tagged `hint`, and 15 `hint` reports were tagged `parser`. No report was
left unplaced: all 11 tenants on file write the day first.

## The page

- `kb/incidents/4471-two-causes/+page.svx` — the page for the desk's kb. It goes in as
  `src/routes/kb/incidents/4471-two-causes/+page.svx`. It carries the page signature in its
  frontmatter, ends with `<RenderStamp {signature} />`, and takes its numbers from the existing
  `<CauseTable ticket={4471} />` component, so no count is typed into the page.
- `kb/rendered/4471-two-causes.html` — the same page as the desk will see it, rendered with the
  runbook layout, the table and the desk ref.
