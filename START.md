# START.md

This file starts a project. Copy it and [`AGENTS.md`](AGENTS.md) into the root of the repository, then tell the agent to follow `START.md`.

The agent's job in this pass is to learn the product, write it down, bind `AGENTS.md` to it, and open `DECISIONS.md` with the choices that were actually made. The agent does not scaffold code, pick work for a second session, or run git.

After `AGENTS.md` says `binding: BOUND`, day-to-day work follows `AGENTS.md`. Run this file again only when the user asks to re-bind the project, or when `binding` is still `UNBOUND`.

This file is temporary in the project it starts. Once its job is done, delete that project's copy. Deleting is a separate step from writing the documents: ask the user whether startup is really done, and delete the file only after they explicitly say yes. Section 10 is the procedure. The template copy kept for other projects stays where it is.

---

## 1. Before asking anything

1. Read `AGENTS.md`.
2. If `README.md` contains the marker `template-readme` and this repository is a product project, delete it now. Do not ask. Do this on the user's first prompt, before questions and before any other file change. If that prompt already passed and the file is still here, delete it before continuing. Keep the file when this repository is the setup template and the user is not starting a product.
3. Look at the repository. Note whether it is empty, already has code, or already has the document set below.
4. If `AGENTS.md` is already `BOUND` and `PROJECT_SPECIFICATION.md` exists, stop. Tell the user the project is already started, and ask what they want changed. Do not regenerate the document set over their edits.
5. If the repository already contains code and no document set, read the code far enough to see the stack, the main entities, and how it is run. The interview then asks only what the code does not answer. Anything inferred from code is labeled as inferred and confirmed before it is written as fact.
6. Do not create or edit any other file until the user approves the summary in section 4. Deleting the template README is the exception. Do not run git.

---

## 2. Files this pass may create

Create a file only when the matching condition is true. Write facts you can stand behind. Do not leave "TBD", "TODO", or empty headings.

| File | Create it when |
| :--- | :--- |
| `PROJECT_SPECIFICATION.md` | Always, on an approved start. |
| `RULES_AND_STANDARDS.md` | Always, on an approved start. |
| `ARCHITECTURE.md` | Always, on an approved start. Mark designs that are not built yet as design. |
| `API_SPECIFICATION.md` | The product will expose an API, even if no endpoint exists yet. |
| `PROGRESS.md` | Always, on an approved start. |
| `DECISIONS.md` | Always, on an approved start. Important decisions only: the date, the choice, and the reason. |
| `README.md` | Always, on an approved start. This is the product README. Setup steps appear only for commands that exist. It replaces the template README, which was deleted on the first prompt. |
| `CHANGELOG.md` | The user wants versioned release notes from the start. |
| `user-guide.md` | The product has non-developer users **and** there is already true user-visible behavior to describe, or the user explicitly wants the manual opened now. |
| A specialist document | The user already knows a subsystem (rules, engine, import format) well enough to state its real rules. Otherwise wait until that subsystem is specified. |
| A feature plan | The user asked to plan a feature. Not part of a normal start. |

Update `AGENTS.md` in place: replace section 1, fill section 11 only with real commands, fill section 12 with the invariants. Leave sections 2 through 10 as they are.

---

## 3. Interview

Ask in three rounds. Wait for answers between rounds. Keep the questions in plain prose, grouped, so the user can answer in a paragraph rather than a form.

If the user pastes an official requirements statement, that statement is the source of truth. Save it verbatim later. Ask only for what it does not already answer.

If the user answers later rounds in the first message, do not ask those parts again.

### Round A — the product

Ask for:

- The project name, and what it is in one or two sentences
- The problem it exists to solve (or the official statement, pasted)
- The people and external systems that use it, and what each of them may and may not do
- What it must do
- What must never go wrong: the invariants (balances, immutability, tenancy, conflicts, privacy, safety, or whatever this domain treats as corruption)
- What is in scope for the first milestone, and what is explicitly later
- How they will know the first milestone is acceptable

### Round B — the shape

Ask for:

- Greenfield, or an existing codebase to document as it stands
- Which clients exist: web, desktop, mobile, API, CLI, or something else
- The technology stack. If they want a recommendation, propose one stack with a short justification for each choice and wait for a yes. Do not treat a proposal as chosen.
- Where data lives
- How people sign in, and whether data is separated per organization, per user, or not at all
- Locale details that change the product: language, timezone, currency, calendar, or formatting
- The machine they develop on, and how they want to start the stack once it exists
- Whether non-developers need a manual, whether that manual should appear inside the app, and whether a help index over the manual is in scope
- Whether they want a changelog from the first release
- Any subsystem they already want as its own document (rules, engine, file format)

### Round C — confirm the tracker

From their answers, propose:

- The module rows for `PROGRESS.md`, each `Not Started` except project foundation
- Any extra rule that should sit beside the standing rules in `AGENTS.md` (for example, a domain ban on deleting posted records)
- The open questions you still cannot answer without guessing

Ask them to correct the module list. Mention that git stays with them unless they say otherwise, and that agents will not choose the next session for them.

---

## 4. Reflect, then wait

Post a binding summary and stop. Do not write files in this turn.

The summary has:

- The product in one short paragraph
- The actors
- The invariants
- The stack, marked as confirmed or still proposed
- The files you will create, and the files you will deliberately skip
- The module table you will open
- Every assumption you made

Ask for an explicit approval to write the files. If their message already said to write the files from the answers they gave, that is approval only when the summary contains no unanswered assumption. Assumptions wait.

---

## 5. Write the files

Write in the order below. Use relative links between documents. Match the style already required by `AGENTS.md`: numbered sections, tables, no placeholder text.

### 5.1 `PROJECT_SPECIFICATION.md`

Use these sections, omitting any that truly do not apply and saying why in the session log:

1. **Project Title**
2. **Problem Statement** — if the user supplied an official statement, put it here verbatim under a heading that says it is the official statement. A structured restatement, if useful, comes after it and must not contradict it.
3. **Functional Requirements** — as a list of behaviors the product has, not a list of tasks
4. **Non-Functional Requirements**
5. **Users and Actors** — what each one can and cannot do
6. **Scope** — this milestone, later, and out of scope
7. **Technology Stack**
8. **Technology Justification** — one short reason per choice, tied to a requirement

### 5.2 `RULES_AND_STANDARDS.md`

1. **Prime directive** — the domain invariants, stated as rules an implementer can test. This is the long form. Examples of the kind of rule that belongs here: a posted record is never updated in place; a tenant query is always constrained to the caller's organization; a scheduled resource cannot be in two places at once. Use the rules this product actually has.
2. **Access boundaries** — who can see which data, and what happens on a violation
3. **Security, authentication, and audit** — only the controls this product uses or has committed to
4. **Architecture and coding conventions** — layering and validation for the confirmed stack. Specify conventions the user confirmed. Where the stack has a widely shared default and they did not object, record it and also list it in the session log as a decision awaiting confirmation.
5. **Testing and verification** — what must be tested for the invariants, and how UI changes are checked
6. **Git and version control** — the user runs git; agents prepare reviewable changes; Conventional Commits when the user commits
7. **Documentation** — point at `AGENTS.md` for the maintenance rules, and name any project-specific document this product adds

### 5.3 `ARCHITECTURE.md`

1. **System overview** — an ASCII context diagram of the clients, the service, and the data store
2. **Domain model** — entities, important fields, relationships, and invariants. Label the section as the design if the schema is not built yet.
3. **Where invariants are enforced** — application, database, or both. If that choice is not made, record the question in `PROGRESS.md` and describe the intended enforcement as pending their decision. Do not silently pick a database feature.
4. **Key flows** — the few flows the first milestone depends on (sign-in, the core write path, the core read path)
5. **Clients** — how more than one client shares the system, if it does
6. **Code map** — add this only when code exists. Package or directory names must be real.

### 5.4 `API_SPECIFICATION.md`

Skip this file if the product has no API.

1. A maintenance line: update this document whenever an endpoint or a payload changes
2. Conventions: base path, format, authentication, and idempotency if the domain needs it. Use only conventions the user confirmed. If they did not confirm a base path, propose one in the summary before this file exists; do not invent it at write time.
3. The success envelope and the error envelope, with the error codes that exist today. Planned codes wait until an endpoint needs them.
4. Implemented endpoints, one section each, with method, path, auth, request, success, and errors
5. A table of planned resources, clearly marked as not implemented

### 5.5 `README.md`

Write the product README. Do not reuse the template README, and do not include the `template-readme` marker.

- What the product is, in a few sentences a new developer can use
- The stack
- Prerequisites and setup for anything that already runs
- If nothing runs yet, say that setup will be added when the project is scaffolded. Do not invent clone paths, ports, or environment variables.

If the template README was deleted at the start of this pass, say so in the Session 1 log.

### 5.6 `CHANGELOG.md`

Only if they asked for it. Open it with a short note that a version number shown in the product must be updated everywhere it appears, and list those places once they exist. Do not invent a `1.0.0`.

### 5.7 `user-guide.md`

Only under the condition in section 2. Write the overview and the terminology that are already true. Add pages, workflows, and rules when those behaviors exist. No stack, no file paths, no links to the other markdown files, no empty sections.

### 5.8 Specialist documents

Use the names the user prefers. When they have no preference, plain names work: `RULES.md` for domain constraints, a dedicated engine document for algorithms, a format guide for an import file, a route catalog only when they want it separate from `API_SPECIFICATION.md`.

A specialist document states the current rules in tables and short sections, including the exceptions and the user-visible overrides. It is updated in the same change as the code it describes.

### 5.9 `DECISIONS.md`

Create a short log, not a second specification. Record a decision only when it will constrain later work: stack, data store, authentication, tenancy, a domain invariant, a public API shape, a user-visible workflow, an explicit scope cut, or a rejected alternative. Leave requirements in `PROJECT_SPECIFICATION.md` and session-by-session activity in `PROGRESS.md`.

One entry per decision, newest first. Date it with the day it was settled. Include the reason. "The user directed it" is a valid reason when they gave no further rationale. Do not record formatting, private names, or other mechanical choices. Do not record a choice that is still awaiting confirmation; that stays in `PROGRESS.md` until the user accepts or rejects it, and only an acceptance is appended here.

Use this shape:

```markdown
# Decisions

Important decisions only. Newest first.

## YYYY-MM-DD — <short title>

- **Decision**: <what was chosen>
- **Reason**: <why>
```

When a later decision replaces an earlier one, add a new entry. Leave the old entry in place and add `- **Supersedes**: YYYY-MM-DD — <old title>` to the new one.

Seed the file with the decisions the user confirmed in the binding summary. If none rise to the bar above, the file is still created, with the heading and the one-line description and no invented entries.

### 5.10 `PROGRESS.md`

Open with this notice:

```markdown
> **Notice to AI Agents**:
> - Record progress in `PROGRESS.md` after **every** prompt that changes the project.
> - **Do not mark a module as complete without asking the user first.**
> - **Do not plan the next session without asking the user first.**
```

Then:

1. **Overall project status** — the module table they approved. Foundation and documentation are `In Progress`. Everything else is `Not Started`. Under the table, one line defining the four statuses from `AGENTS.md`.
2. **Session log** — Session 1, dated today, listing each file written and every decision still awaiting confirmation. Point at `DECISIONS.md` for the choices already settled. Current state: documents are drafted and waiting for review. Next session: not planned — awaiting user direction.

### 5.11 Bind `AGENTS.md`

Replace section 1 with a bound section:

```markdown
## 1. Project Binding

```
binding: BOUND
```

**`<Project name>`** — `<one line>`

### Document index

| Document | Purpose |
| :--- | :--- |
| ... | ... |

### Stack

- ...

Prime invariants for day-to-day work are in section 12. The full statement is in `RULES_AND_STANDARDS.md`.
```

Fill the index with the files you actually created, relative links, one purpose line each.

Replace section 12 with the short form of each invariant, numbered. Each item is one or two sentences an agent can apply while editing.

Replace section 11 only when a command has been run and succeeded in this repository. Otherwise leave the "Not recorded yet" paragraph.

If the user stated an extra standing rule (a domain prohibition, a documentation trigger this product always has), add it as a new numbered rule at the end of section 2 and mention it in the session log. Do not weaken rules 1 through 8.

---

## 6. After the files exist

In the same turn, after writing:

- List the files you created and the ones you skipped
- Quote the invariant list
- Point at the decisions still awaiting confirmation
- Ask them to review the foundation
- Leave every product module at `Not Started` or `In Progress`, and leave foundation as `Pending User Review` only if you believe the documents are ready for that review
- Do not mark anything `Completed`
- Do not propose a populated plan for the next session. You may ask which of these they want: corrections to the documents, a decision on an open question, or the name of the module to start later
- Do not delete this file in this turn. Writing the documents does not finish its job. Ask about deletion only after the user has reviewed the foundation, and only by the procedure in section 10.

---

## 7. Starting from an existing codebase

When the code is already there:

- Prefer the code and an official statement over a guess
- Document the behavior the code actually has. Where the code disagrees with what the user just said, show both and ask which one wins. Do not "fix" the disagreement during start.
- Record real run commands in `AGENTS.md` section 11 only after you have run them, or the user has told you the exact command they use
- Session 1 says the documents were drafted from the existing code plus their answers

---

## 8. Feature plans, later

When the user asks for a plan, write one working document for that slice. Use this shape:

1. **Goal** — the outcome in product language
2. **Terms** — a short table of any words the slice introduces
3. **Behavior and rules** — what the user will see, and the constraints
4. **Proposed changes** — files to add or modify, each with the concrete change
5. **Decisions needed** — anything you refused to assume
6. **Verification** — tests, commands, and the user-visible checks
7. **Documents to update when it ships**

Call out decisions with a blockquote (`> **Important:**`). When the slice is implemented, update the permanent documents and `PROGRESS.md`. Keep the plan as the record of the design unless the user asks you to remove it.

---

## 9. Re-running this file

If the user asks to start over, or binding is still `UNBOUND` after a partial pass:

- Re-read the repository and the current documents
- Ask only about what is missing or what they said has changed
- Edit the existing documents in place
- Add a new session to `PROGRESS.md` rather than rewriting Session 1
- Leave this file in place. A re-run means the job is not done, so section 10 does not apply yet.

---

## 10. Deleting this file

Delete `START.md` from the project it started once that job is finished. Ask first, every time, and wait for an explicit yes.

The job is finished only when all of these are true:

- `AGENTS.md` in that project says `binding: BOUND`
- The document set the user approved is written
- The user has reviewed it and has nothing further they want this start pass to change

When those are true, ask in plain language whether startup is really done, and say that a yes means this project's `START.md` will be deleted. Wait for the answer.

- A yes to writing the files, a "looks good" about a draft, or silence is not a yes to deletion. The user has to confirm that the job is done.
- If they say no, or they are unsure, leave the file and ask what is still open.
- If you are not sure this copy is the working copy inside the started project, rather than the template kept for other projects, ask before deleting. Delete only the working copy. The template stays.

After they confirm:

1. Record the confirmation and the deletion in `PROGRESS.md`.
2. In that project's `AGENTS.md`, remove the instructions that tell agents to follow `START.md`, so the file does not point at a document that is about to disappear. Leave the rest of `AGENTS.md` as it is. Re-binding later means copying `START.md` back in.
3. Delete this project's `START.md`.
