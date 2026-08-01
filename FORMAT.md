# GitHub Issue and Comment Reporting Format

## Authority

This file is the single source of truth for reporting findings to the upstream `blacktop/ipsw` project.
`ISSUES.md` is the sole source of truth for finding IDs, lifecycle status, and published locations.

- Report findings only; never create, draft, offer, or propose a pull request.
- Never implement a reported fix unless a later user instruction explicitly authorizes implementation.
- Default to proposal-only mode and show every complete draft to the user before publication.
- Publish externally only after the user explicitly approves the exact draft and target.
- Write upstream GitHub issues and comments in friendly, concise English.
- Communicate with the user in the language of the surrounding conversation.

## Finding IDs and Duplicate Prevention

- Every ledger entry has one permanent ID in the form `ISSUE-YYYY-NNN`.
- `YYYY` is the UTC year first recorded; `NNN` is that year's sequence with at least three digits.
- `Next finding ID: ISSUE-YYYY-NNN` is the only allocator and MUST name the next unused ID.
- Immediately before adding a finding, re-read the complete current `ISSUES.md` snapshot.
- Search every title, summary, symbol, and location for the same symptom or root cause.
- Update an existing matching root cause instead of allocating a duplicate.
- Assign the allocator value when a root cause first enters Hold or Drafted status.
- Increment the allocator in the same edit that adds the finding.
- Never reuse, renumber, or scope IDs by command, package, provider, section, status, or session.
- IDs survive Hold, Drafted, and Published states; external numbers belong only in `Location`.
- At the first finding of a UTC year, start `ISSUE-YYYY-001`; never alter older IDs.
- Entry headings use `### ISSUE-YYYY-NNN — <area>: <specific title>`.

The ledger check does not replace upstream duplicate research.

## Required Research

Before drafting any report:

1. Read the complete current `ISSUES.md` and confirm the finding's ID, status, target, and location.
2. Confirm that no ledger entry already owns the same root cause.
3. Read the current upstream `CONTRIBUTING.md`, `AI_POLICY.md`, and applicable issue template.
4. Search open and closed upstream issues for the symptom, root cause, command, package, and symbols.
5. Search open and merged upstream pull requests for code that owns or introduced the behavior.
6. Search upstream GitHub Discussions for the same behavior and design intent.
7. Read every plausible issue, comment, pull request, review, discussion, and current diff completely.
8. Verify all source claims against current upstream code and pinned dependency contracts.
9. Record which effects are Observed, Source-proven, Assumed, or Not measured.

A matching word, package, command, or symptom does not prove that a thread owns the same root cause.

## Issue or Comment Decision

Apply this order for every finding.

### Comment on an existing open issue

Comment only when all of these are true:

- The issue describes the same observable problem or root cause.
- The evidence materially advances diagnosis, reproduction, ownership, or resolution.
- The comment does not redirect the issue to an unrelated performance or architecture concern.

Do not comment merely because the same command, Apple format, error string, or package appears.

### Comment on an existing open pull request

Comment only when all of these are true:

- The pull request changes the exact lifecycle, function, invariant, or ownership boundary involved.
- The finding identifies a correctness, resource, concurrency, or performance gap in the current diff.
- The comment is actionable within the current scope or clearly marked as an optional follow-up.

Do not ask the author to absorb unrelated work. Never create or offer a competing pull request.

### Open a new issue

Open a new issue when any of these is true:

- No open issue or pull request owns the root cause.
- Existing matches are closed, historical, tangential, or caused by something else.
- The finding needs durable tracking beyond an active pull request discussion.
- A merged pull request is useful history but provides no active discussion target.

Use one issue per independent root cause. Link useful history without reviving unrelated closed threads.

### Hold without reporting

Hold the finding when any of these is true:

- Reachability, root cause, currentness, or ownership is unverified.
- The claim is only a static pattern without realistic cost or behavior evidence.
- The proposed target is merely similar rather than directly relevant.
- The report would repeat evidence already present in the target thread.
- Production impact, correctness, or severity depends on an unverified assumption.
- A proposed optimization changes ordering, freshness, retries, backpressure, or error semantics.

Record exactly which evidence is missing. Do not turn a plausible optimization into an upstream claim.

### Ask the user

Use the interactive ask mechanism when:

- Two targets are materially plausible and the wrong one risks thread hijacking.
- New issue versus comment has meaningful visibility or scope trade-offs.
- Required reproduction data, disclosure text, or publication scope is missing.
- External publication is requested but the exact target and draft have not been approved.

Do not ask when research makes the correct target unambiguous.

## Evidence Contract

Label material claims honestly:

- Observed: reproduced with command, version, environment, inputs, and captured output.
- Source-proven: current control flow, data flow, API contract, or ownership proves the invariant.
- Assumed: a premise required by the claim has not been verified.
- Not measured: latency, throughput, memory, resource growth, or request-count impact lacks measurement.

Rules:

- Never convert source evidence into an observed user impact.
- Never call behavior a leak without deterministic lifetime proof or measured resource growth.
- Never claim that a network request always occurs when cache, retry, or protocol state can avoid it.
- Never use internal severity labels such as Critical, High, Medium, or Low upstream.
- Use exact `path:line`, function, method, field, command, error, and API names when useful.
- State ordering, freshness, retry, cancellation, and backpressure constraints when they limit a fix.
- Link issues or pull requests that establish design intent or historical ownership.

## Duplicate-Search Statement

Place the applicable exact statement after the report question and before the involvement text.

For a new issue:

```text
I checked all relevant issues, comments, pull requests, and GitHub Discussions; this report is not a duplicate.
```

For an existing issue or pull request comment:

```text
I checked all relevant issues, comments, pull requests, and GitHub Discussions; this evidence is not already reported.
```

Use the statement only after every plausible prior-art candidate has been read completely.
If any plausible candidate is unavailable or unread, hold the finding.

## Tone Contract

- Start with appreciation when commenting on another contributor's work.
- Use neutral language such as `I noticed`, `I may be missing context`, and `Would it make sense`.
- Describe code behavior, not author intent or competence.
- Ask one concrete question when maintainer input is needed.
- Avoid blame, demands, alarmism, sarcasm, and rhetorical severity.
- Keep one root cause and one requested decision per report.
- Do not tag maintainers or authors unless they already participate or the user approves it.
- State that no pull request is planned when implementation is not being offered.

## New Issue Format

Use the official GitHub issue form when it requires named fields. Map this content into the closest fields.

### Title

```text
<area>: <specific observed or source-proven problem>
```

Name the affected command, package, format, provider, or daemon area. State the problem, not a proposed
implementation. Avoid severity words and generic titles such as `performance issue`.

### Body

```markdown
## Summary

<One concise paragraph describing the observed or source-proven problem.>

## Evidence

- `<path:line>`: <specific control-flow, lifecycle, data-flow, or ownership evidence>.
- <Command, output, API contract, issue, pull request, or discussion link>.

## Impact

<Observed impact with measurement, or an explicit statement that impact has not been measured.>

## Question

<One concrete question about expected behavior, ownership, or preferred direction.>

I checked all relevant issues, comments, pull requests, and GitHub Discussions; this report is not a duplicate.

## Involvement

I am reporting this finding only and am not currently proposing a pull request.

AI assistance: GPT-5.6 Sol investigated the source and drafted this report using
[Oh My Pi](https://github.com/can1357/oh-my-pi). I reviewed the claims and evidence before publication.
```

### Bug form additions

When the bug form applies, include all required fields:

- latest tested `ipsw version` output;
- operating system and architecture;
- exact command and arguments;
- exact firmware, OTA, Mach-O, dyld cache, kernelcache, or device identifier;
- deterministic reproduction steps;
- real logs or output produced by the reproduction;
- relevant configuration with only user-requested omissions.

Do not use the bug form for a source-only performance hypothesis without reproduced user-visible behavior.

### Enhancement form additions

Use the enhancement form when the invariant is source-proven but user-visible harm is not reproduced.
State the current behavior, desired invariant, and what remains unmeasured.

## Existing Issue Comment Format

```markdown
Hi, thanks for documenting this.

I noticed one detail that may be relevant to the same root cause:

- `<path:line>`: <new evidence>.
- <Why this evidence changes or strengthens the current diagnosis>.

<One concise question or proposed next diagnostic step.>

I checked all relevant issues, comments, pull requests, and GitHub Discussions; this evidence is not already reported.

I am reporting this finding only and am not currently proposing a pull request.

AI assistance: GPT-5.6 Sol investigated the source and drafted this comment using
[Oh My Pi](https://github.com/can1357/oh-my-pi). I reviewed the claims and evidence before publication.
```

Use this format only when the existing issue owns the same root cause.

## Existing Pull Request Comment Format

```markdown
Hi, thanks for working on this.

While reading the current diff, I noticed one possible <lifecycle, ownership, resource, or concurrency> gap:

- `<changed path:line>`: <specific behavior in the current diff>.
- `<related path or API contract>`: <why the current behavior may be incomplete>.

Would it make sense to <one focused question or suggestion>?
I may be missing ownership handled elsewhere.
I checked all relevant issues, comments, pull requests, and GitHub Discussions; this evidence is not already reported.
I am not suggesting a broader scope change or a separate pull request.

AI assistance: GPT-5.6 Sol investigated the source and drafted this comment using
[Oh My Pi](https://github.com/can1357/oh-my-pi). I reviewed the claims and evidence before publication.
```

Keep review comments scoped to the active diff. Independent findings require their own target and approval.

## Condensed Proposal Contract

Present every report proposal in this stable order:

```text
id: <ISSUE-YYYY-NNN>
target: <new issue | issue #N | PR #N>
action: <open issue | comment | hold>
reason: <one sentence>
title: <new issue title; omit for comments and holds when unknown>
draft: <complete proposed text>
status: proposal only; not published
```

Preserve permanent IDs and never combine independent root causes.

## Publication Gate

Before publication, verify every item:

- The target still exists and its state has not changed.
- The finding ID, status, target, and location match `ISSUES.md`.
- The draft matches current upstream source or the current pull request diff.
- The report adds evidence not already present.
- Every material claim is evidenced or explicitly uncertain.
- The tone is friendly, concise, and non-accusatory.
- No pull request offer or implementation commitment is present.
- The applicable duplicate-search statement is present and justified.
- The AI-assistance disclosure is the final paragraph.
- The user approved the exact target and final text.

After publication, update the finding status and exact URL in `ISSUES.md` before returning the URL and
concise status.

## Prohibited Actions

- Never create, draft, propose, or offer a pull request.
- Never implement code as part of this reporting workflow.
- Never publish without explicit approval of the exact draft and target.
- Never cross-post one finding to multiple threads.
- Never revive a closed issue with an unrelated root cause.
- Never use an active issue as a generic performance or architecture discussion.
- Never report unverified static patterns as production bugs.
- Never hide uncertainty or fabricate measurements, commands, logs, or maintainer intent.
- Never alter the approved disclosure footer during publication.
