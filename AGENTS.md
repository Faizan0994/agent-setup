# AGENTS.md

Any AI agent working in this repository must read this file and follow it. The behavioral rules in sections 2 through 10 apply in every repository this file is copied into. Section 1, section 11, and section 12 are filled in per project.

If section 1 says `binding: UNBOUND`, do not write product code and do not invent the product. Follow [`START.md`](START.md), then come back to this file.

`README.md` marked `template-readme` belongs to this setup, not to a product. In a product project, delete it on the user's first prompt, without asking. If it is still present on a later prompt, delete it then. Keep it when the repository is the setup template and the user is not starting a product. The project README written by `START.md` is a different file and does not carry that marker.

---

## 1. Project Binding

```
binding: UNBOUND
```

This repository has not been bound to a product yet.

- Do not scaffold code, pick a stack, or name domain concepts on the user's behalf.
- Do not create `PROJECT_SPECIFICATION.md`, `RULES_AND_STANDARDS.md`, `ARCHITECTURE.md`, `API_SPECIFICATION.md`, `PROGRESS.md`, or any other project document until [`START.md`](START.md) says to.
- Sections 2 through 10 already apply, including the ban on git commands and the duty to ask when something is doubtful.

When this project is bound, replace this section only. Keep sections 2 through 10 intact. Fill section 11 with commands that have actually been run, and section 12 with the project's invariants. The bound section must contain:

- `binding: BOUND`
- Project name and one-line purpose
- A document index of the files that exist, with relative links
- The technology stack
- A pointer to the prime invariants in section 12

---

## 2. Mandatory Behavioral Rules

These rules are non-negotiable.

1. **Update `PROGRESS.md` after every prompt that changes the project.** Record what changed, what was verified, and the current state. A prompt that only answers a question still gets a short session note if `PROGRESS.md` exists. If `PROGRESS.md` does not exist yet because the project is unbound, skip this until `START.md` creates it.
2. **Never mark work complete without asking.** Status `Completed` requires an explicit confirmation from the user in chat. Until then the status is `Pending User Review`. Ask in the same turn you would otherwise have marked it complete.
3. **Never decide the next session.** You may list options. You may not choose one, write it up as the plan, or start it in the same turn. End with the current state and wait.
4. **Git stays with the user.** Do not run `git` or `gh` (status, diff, add, commit, push, checkout, branch, reset, or anything else). Do not stage files. Leave the tree ready for the user to review. When they commit, Conventional Commits fit this workflow: `feat:`, `fix:`, `docs:`, `test:`, `refactor:`.
5. **Ask before you guess.** If a requirement is doubtful, ambiguous, or underspecified, or if a design choice has more than one reasonable option, stop and ask. Do this before writing the code, the schema, or the document that would freeze the choice.
6. **Log any choice you made without an explicit yes.** If you proceeded and a behavior, schema, API, dependency, or default was your choice, list it in `PROGRESS.md` under "Decisions made without explicit approval" and ask the user to confirm or reject it. Silence is not approval. When the user accepts an important decision, append one dated entry to `DECISIONS.md`, including the reason. Leave unconfirmed choices out of `DECISIONS.md`.
7. **Documentation is part of finishing the task**, not a follow-up. Update every document the change touches before you report the task done.
8. **Do not print secrets.** Never echo passwords, tokens, connection strings, or key material. Document variable names in `.env.example` and `README.md`. Keep real `.env` files out of the review surface.

### What counts as a decision you must ask about

Stop and ask before choosing any of the following:

- A new dependency, framework, or runtime version
- Schema shape, migration strategy, or where an invariant is enforced
- Authentication, authorization, tenancy, or session design
- A public API path, request or response shape, or error code
- A user-visible label, default, or workflow
- Either of two valid designs

You may proceed, without asking, when you are matching a pattern already present in this repository: formatting, private names that follow local convention, and mechanical edits inside a design the user already approved. If that matching still changes observable behavior, log it.

---

## 3. Document Set

Keep documents in the role each one has. Do not merge them, and do not duplicate a fact into a second file when a link will do — except the short invariant summary in section 12, which restates the rules agents must see on every turn.

| Document | Role | Update when |
| :--- | :--- | :--- |
| `PROJECT_SPECIFICATION.md` | What the product is, who it is for, and what it must do. An official statement from the user is kept verbatim. | The user changes scope, actors, or requirements. |
| `RULES_AND_STANDARDS.md` | The laws: domain invariants, security, layering, testing bar, git policy. | A rule is added, tightened, or relaxed. |
| `ARCHITECTURE.md` | How the system is shaped: context diagram, domain model, where invariants live, request flows, package map once code exists. | Domain models, schema, components, or flows change. |
| `API_SPECIFICATION.md` | The living API contract: conventions, envelopes, error codes, implemented endpoints, planned resources. | An endpoint, DTO, auth scheme, or error code is added or changed. |
| `PROGRESS.md` | Module status and the session log. The memory of the project. | Every prompt that changes the project. |
| `DECISIONS.md` | Important decisions only: the date, the choice, and the reason. Newest first. | The user settles or reverses an important decision. |
| `README.md` | Install, configure, and run. Operator and developer audience. | Setup, environment variables, or run commands change. |
| `CHANGELOG.md` | User-visible release notes. Developer audience. Create it only if this project versions releases. | A user-visible release is cut. If the UI shows a version, update every place that shows it in the same change. |
| `user-guide.md` | End-user manual. Create it only if this product has non-developer users. | Anything the user can see or do changes. See section 5. |
| Specialist documents | One subject each: domain rules, engine or algorithm internals, import or file formats, or a route catalog when it is kept separate from `API_SPECIFICATION.md`. | The subject they own changes. |
| Feature plans (`plan.md`, `<feature>_plan.md`, `implementation_plan.md`) | A working plan for one slice of work. | While that slice is being designed. When it ships, fold the outcome into the permanent documents and the session log. |

Specialist documents are created when a subsystem has rules, algorithms, or formats that would clutter the specification. They are not created in advance "to be filled in later."

If a document in the table does not exist, do not create it mid-task unless the user asked for it or the change genuinely needs a new specialist document. Mention the gap and ask.

### Two audiences

- **End-user documents** (`user-guide.md`, and in-app help derived from it) describe what the person using the product sees and does.
- **Developer documents** (everything else, including `README.md` and `CHANGELOG.md`) may name files, commands, schema, and vendors.

A fact belongs in exactly one audience. Installation belongs in `README.md`. API contracts belong in `API_SPECIFICATION.md` or the route catalog. Algorithms belong in the engine document. The user guide does not link to developer documents.

---

## 4. Keeping Documents Aligned

A change is unfinished while any document that describes the changed behavior still disagrees with the code or with another document. Wording counts: two documents that describe the same behavior must say the same thing.

Before finishing:

1. List the documents whose subject you touched. Read those sections before editing them.
2. Edit only the affected sections. Keep the existing heading levels, numbering, and table style. If you add, remove, or rename a heading, update that document's table of contents and every anchor that pointed at the old heading.
3. If a feature is removed, remove its documentation. If it is renamed, update every reference, including anchors.
4. Cross-check the other surface that owns the same fact: the user guide section, the rules document, the engine document, the API contract, and any import-format guide.
5. Record the documentation updates in the `PROGRESS.md` entry for this prompt.

### Surfaces that must agree

When a behavior is reachable more than one way, those ways must accept the same values, apply the same defaults, and reject the same input.

- Import or bulk-load logic, the manual entry UI, and the document that describes that data
- Client validation, server validation, and the documented error
- Generated output and what the editor later allows or blocks

If you change one surface, check the others. Search the repository for the field or rule name and compare allowed values and defaults.

---

## 5. User Guide Policy

Apply this section only when `user-guide.md` exists.

The guide reads like a product manual for the person the software was built for. It describes outcomes: screens, fields, buttons, and what happens next.

Keep out of `user-guide.md`:

- Code, file paths, component names, algorithms, schema, dependency names, and CLI commands
- Technology stack, vendor names, environment variable names, and internal tuning values
- Request and response shapes, auth mechanics, and protocol details
- Links or references to developer documents, including this file, `PROGRESS.md`, `DECISIONS.md`, and `CHANGELOG.md`

When a feature is also documented for developers, the guide gets the outcome and the developer document gets the mechanism.

### In-app manual and retrieval index

If the application renders `user-guide.md` itself, there is one copy of the text. Do not paste the guide into frontend source. After an edit, verify the in-app page still renders, and keep its table-of-contents anchors in GitHub form: lowercase, spaces become hyphens, punctuation dropped.

If a help index is built from the guide (chunking, embeddings, or a similar pipeline), rebuild it after every guide edit using the project's recorded commands, and note the rebuild in `PROGRESS.md`. Leave those commands in section 11 once they exist. Do not invent a pipeline the project does not have.

### Guide style

| Rule | Guideline |
| :--- | :--- |
| Headings | `##` for sections, `###` for subsections, `####` below that. Match the depth already in the file. |
| Tables | Markdown tables with a separator row. |
| Notes | Blockquotes of the form `> **Note:**`, `> **Important:**`, `> **Warning:**`, `> **Caution:**`, `> **Tip:**`. |
| Tone | Clear enough for the product's user. No developer asides. |
| Coverage | Every user-facing feature has a description. A removed feature loses its description. |
| Placeholders | Do not write "TBD", "TODO", or empty headings. If a section cannot be written truthfully yet, it does not exist yet. |

---

## 6. Progress Tracker

`PROGRESS.md` opens with a notice that restates rules 1 through 3, then a module table, then a session log. Newest session at the top of the log.

Status values are exactly these:

| Status | Meaning |
| :--- | :--- |
| `Not Started` | No work beyond the specification. |
| `In Progress` | Work has started and is not ready for review. |
| `Pending User Review` | The agent believes the slice is ready. Waiting for an explicit yes. |
| `Completed` | The user confirmed it, in chat, on a recorded date. |

Moving a row to `Pending User Review` is an action you take and then ask about. Moving a row to `Completed` happens only after that yes, and the session note records the date and that the user confirmed it.

### Session entry

```markdown
### Session N: <short title>
- **Date**: YYYY-MM-DD
- **User decisions**: <what the user chose this session; omit if none>
- **Activities**:
  - <what changed, with file paths, behavior, and why it changed>
  - <verification: command run, result, and anything you could not verify>
  - <documents updated>
- **Open points**: <left unfinished on purpose>
- **Decisions made without explicit approval**: <numbered; omit the heading if there were none>
- **Current State**: <module statuses that matter, in one or two sentences>
- **Next session**: not planned — awaiting user direction.
```

Activity bullets name files and behavior. "Updated the backend" is not a session entry. Test results include the count when a suite was run (`12 tests, 0 failures`). Documentation bullets name the file and the section.

When a prompt settles an important decision, append it to `DECISIONS.md` in the same turn. One entry: date, decision, reason. A session note can point at that entry in a single line. Do not copy the session log into `DECISIONS.md`. When a decision is reversed, add a new entry that names the one it supersedes, and leave the old entry in place.

---

## 7. Working a Task

1. Read this file, the top of `PROGRESS.md`, and the sections of the specification, rules, and architecture that the task touches.
2. If the task needs a decision from section 2, ask and wait.
3. Do the work inside the existing architecture. New structure is a decision.
4. Update the documents from section 3 that the change affects. Follow sections 4 and 5.
5. Verify. Run the tests, lint, and build commands recorded in section 11. For a change a person can see in a UI, exercise the flow the way a user would: the main path, the nearby screens that share the data, and the empty and error states the change touches. Record what you ran and what you could not run.
6. Append the session entry. If an important decision was settled, append it to `DECISIONS.md`. Set the module to `Pending User Review` if the slice is ready, and ask. Leave the next session unplanned.

A feature plan is written when the user asks for a plan, or when they agree a slice should be planned before code. A plan names the goal, the terms it introduces, the files it will add or change, the decisions the user still has to make, and how the result will be verified. After the slice ships, the permanent documents carry the outcome.

---

## 8. Documentation Style

Use this style in developer documents as well as the guide.

- Numbered sections, `##` and `###`, and a table of contents once a document has more than a handful of sections.
- Tables for statuses, commands, error codes, field lists, and document maps.
- Relative links between project documents (`[RULES_AND_STANDARDS.md](RULES_AND_STANDARDS.md)`), never absolute filesystem URLs.
- Fenced code for commands, JSON envelopes, and configuration examples in developer docs.
- ASCII diagrams for system context in `ARCHITECTURE.md` when a diagram makes the tiers clearer than prose.
- Domain models listed as entities with their fields, relationships, and the invariants that constrain them.
- Official wording from the user preserved verbatim and marked as such. Paraphrase lives in a separate section and must not contradict the original.

---

## 9. Verification and Local Setup

- Prefer commands already recorded in section 11. When a scaffold first gains a real build, test, lint, or run command, record the exact command, the directory it runs from, and the toolchain version.
- When the project grows a local bootstrap, make it idempotent, safe to re-run, and silent about secrets. Document it in section 11 in the same turn it is added.
- When the project uses Windows batch files, save them as CRLF. A `goto` in a batch file fails on LF-only content.
- Database schema changes go through the project's migration mechanism. Do not edit a migration that has already been applied. Record that mechanism in section 11 once it exists.
- Do not claim a check passed unless you ran it in this session.

---

## 10. Ownership

The agent doing the task updates the documents as part of that task. Reviewers should treat a user-facing change with a stale guide, a stale API contract, or a missing session entry as an unfinished change.

---

## 11. Toolchain

Not recorded yet. When the project is scaffolded, replace this paragraph with the real commands. Until then, do not invent install or run steps.

Expected layout once recorded:

### Backend

| Task | Directory | Command |
| :--- | :--- | :--- |
| — | — | — |

### Frontend and other clients

| Task | Directory | Command |
| :--- | :--- | :--- |
| — | — | — |

### Full local stack

| Task | Command |
| :--- | :--- |
| — | — |

Database and migration rules go under this section as soon as they exist.

---

## 12. Prime Invariants

None recorded yet.

When the project is bound, copy the short form of each invariant here. The full statement stays in `RULES_AND_STANDARDS.md`. Agents treat this list as always in force. If the list and the rules document ever disagree, stop and ask the user which one wins, then make them agree.
