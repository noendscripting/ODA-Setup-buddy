# Start Here - ODA Phase 1 Handoff and Next Steps

Updated: 2026-11-05
Status: Paused by owner. Phase 1 is not accepted or ready to publish.

## Resume instructions for a new chat

Read this document first, then the relevant sections of
[Phase 1 implementation](PHASE-1-IMPLEMENTATION.md) and
[main design](DESIGN.md). Inspect the current configuration before making changes.
Do not assume that previous local files and the remote draft are synchronized.

Suggested opening request:

> Read START-HERE-NEXT-SESSION.md in the ODA Setup buddy project. Summarize the
> outstanding evidence-validation issues and propose the smallest supported
> implementation using the existing resources. Preserve the tested original
> instructions and intake. Confirm the approach with me before adding a
> response-blocking guard, new integration, or source-configuration changes.

Project: `C:\CopilotStudio\ODA Setup buddy`.
The workspace is not a Git repository.

## 1. Objective and scope

Build an accurate, comprehensive On-Demand Assessment setup/configuration
reference, initially for Microsoft Entra ID and Active Directory Assessments.
Active Directory Security is a different assessment and is not initial coverage.

The initial deployment uses the owner's existing personal environment.
Technical reviewers and the pilot audience will be added later.
Use existing Learn MCP, Learn knowledge, and approved internal knowledge sources;
do not introduce replacement connectors without approval.

Later-phase Teams automation, automatic email/posting, and document-generation
capabilities are not authorized by this work.

## 2. Current implementation

- [Original instructions](ORIGINAL-AGENT-INSTRUCTIONS.md) are preserved verbatim
  as the prefix of [active agent instructions](agent.mcs.yml).
- [ODA setup intake](topics/ODASetupIntake.mcs.yml) asks the original setup
  questions and returns typed scenario context to generative orchestration.
  It no longer generates the technical answer directly.
- The [Learn MCP action](actions/MicrosoftLearnDocsMCP-MicrosoftLearnDocsMCPServer.mcs.yml)
  instructs staged retrieval across shared and workload-specific setup pages.
- [Search fallback](topics/Search.mcs.yml) contains corresponding evidence rules.
- [Settings](settings.mcs.yml) disable general model knowledge. Existing
  authentication and orchestration are retained.
- Nine legacy custom topics were removed at the owner's request; system topics
  and the new setup intake remain.
- The owner reported switching the remote model to Claude Sonnet 4.6, with a
  significant improvement in completeness. Preserve that selection; local
  model metadata may lag the remote setting.

Latest local additions:

- Use a genuine host-native Copy control if available; otherwise offer a
  copy-ready plain-text version. No custom clipboard button was implemented.
- When reporting documentation gaps, suggest a consolidated manual email to
  [Tech Discussion - On-Demand Assessment Setup VTeam](mailto:d6a4891c.microsoft.com@amer.teams.ms).
  Offer a draft, ask for sensitive-data redaction, and never send or post it.
- Relevant design and implementation documentation was updated.

Latest local validation: 26 configuration documents, zero language-server
diagnostics; original prefix unchanged; agent instructions 7,971/8,000 characters.
Do not append more instructions without checking this limit.
Local schema validation does not establish runtime accuracy.
Earlier uploads were owner-confirmed; reconcile the latest local additions
with the remote draft before further testing.

## 3. Latest live test: improved, but failed technical acceptance

The owner tested an AD ODA binary-delivery and Log Analytics pipeline question.
The answer was more complete, but still crossed evidence boundaries.

| Area | Observation | Required correction |
|---|---|---|
| Copy behavior | Suggested native Copy or a copy-ready alternative | Acceptable conditional guidance; actual UI availability is host-dependent |
| Gap reporting | Named the VTeam and advised redaction | Explicitly offer one consolidated draft; verify the actual mailto target because pasted link labels do not reveal it |
| DCR data path | Wrote `Scheduled Task -> Working Directory -> AMA -> DCR -> Log Analytics` | Do not put a DCR resource in the telemetry path |
| ODA ingestion mechanics | Asserted exact working-directory collection and DCR provisioning, then admitted those details were not established | Withhold unsupported ODA-specific mechanics rather than state them as facts |
| Cross-workload advice | Recommended Entra extension checks for AD while acknowledging missing confirmation | Require shared or AD-applicable evidence before recommending the procedure |
| Claimed credential gap | Compared different assessment cmdlet parameters and called the AD username parameter missing | AD documentation explicitly prompts for credentials after running the cmdlet; different cmdlets alone do not prove a defect |
| Confidence | Opened with "complete, source-grounded" despite unresolved claims | Avoid completeness claims without verified coverage |

Verified references:

- [DCR overview](https://learn.microsoft.com/en-us/azure/azure-monitor/data-collection/data-collection-rule-overview):
  a DCR is control-plane configuration, not a data-plane destination.
  Telemetry does not flow through the DCR resource.
- [AD assessment setup](https://learn.microsoft.com/en-us/services-hub/microsoft-engage-center/health/getting-started-ad):
  documents the task-creation command followed by supplying credentials.
- [Shared ODA setup](https://learn.microsoft.com/en-us/services-hub/microsoft-engage-center/health/get-started-with-on-demand-assessments):
  documents shared setup steps and links to supporting pages, but a link does
  not automatically establish applicability or currency.

The latest review made no further runtime configuration changes.

## 4. Recommended next work, in priority order

These are recommendations, not an already implemented or approved enforcement
design. Do not solve this by repeatedly appending prompt wording alone.

### A. Inspect the actual retrieval and generation trace

Use the owner's sanitized activity-map evidence to establish:

1. Which queries and tools were invoked.
2. Which pages/passages were actually returned.
3. Whether claims came from applicable evidence, unrelated retrieval, or inference.
4. Whether source restrictions were applied before generation.

The owner previously chose trace inspection before a proposed response guard.
That guard was not implemented. Avoid guessing at the cause without trace data.

### B. Define source applicability, not just domain trust

Classify evidence by purpose:

| Category | Permitted use |
|---|---|
| Shared ODA pages | Applicable common setup and prerequisites |
| AD assessment pages | AD-specific procedures and requirements |
| Entra assessment pages | Entra-specific procedures; not automatic AD guidance |
| General Azure pages | Component semantics; not proof of ODA-specific wiring |
| Approved internal material | Supplementary context; not an override for unclear official requirements |
| Unrelated pages, excluded pages, Learn PDFs | Not evidence for an ODA technical recommendation |

Check assessment, deployment scenario, component, and supported claim for every
source, including pages reached through links. A Microsoft Learn hostname is
necessary for official Learn evidence, but is not sufficient for relevance.
Do not block useful Azure documentation merely because it is outside the ODA
URL subtree; constrain what it can establish.

### C. Design the smallest supported evidence-validation workflow

For each procedural claim, require an applicable retrieved passage and source
URL. Distinguish documented ODA behavior from general background and unknowns.

Investigate enforceable retrieval filtering and pre-delivery validation using
supported Copilot Studio capabilities and existing resources. Do not assume the
current MCP action provides deterministic source filtering: its operation uses
an MCP/JSON-RPC envelope, not a verified simple query binding.

If proposing a generated-response guard, confirm supported access to response
text and citations, its limitations, and owner approval before implementing it.
A URL-blocking guard alone cannot detect unsupported inference or prove
completeness. A second model-based checker is also not a correctness guarantee.

Fail closed for the affected section: state what the retrieved evidence does
not establish, retain independently supported guidance, and offer clarification
or technical review. Do not provide an unverified procedure and disclaim it later.

### D. Re-run a small acceptance set

- AD binary delivery: identify supported mechanisms without inventing triggers,
  order, timing, or unsupported cross-workload steps.
- AD ingestion: separate DCR configuration from transport; do not infer exact
  ODA file mappings, DCR creation, or associations from generic Azure guidance.
- Entra versus AD: prevent procedure transfer without applicability evidence.
- Credentials: recognize the documented AD credential prompt.
- Excluded sources: reject the outdated page and PDFs even when linked.
- Genuine gaps: distinguish missing retrieved evidence from a demonstrated
  documentation contradiction; include linked examples.
- Copy/email UX: truthful Copy fallback, exact recipient, consolidated draft
  offer, redaction, and no delivery claim.
- Existing intake, native citations, and inline reference links remain intact.

Run local configuration validation after implementation and owner-operated
runtime tests after applying changes. Keep failed technical cases open until
the expected behavior is observed; do not equate schema success with acceptance.

## 5. Non-negotiable working boundaries

- Do not use the built-in browser: the owner says it violates organizational
  policy. The owner performs live browser tests and shares results.
- Preserve the original instruction prefix exactly, including its original text.
- Preserve the working intake question order and generative handoff.
- Do not overwrite the owner's model selection.
- Preserve native citations; add verified links in relevant descriptive text.
- Exclude all Microsoft Learn PDFs and the owner-confirmed outdated HTML page:
  `https://learn.microsoft.com/en-us/services-hub/microsoft-engage-center/health/assessment-prereq-docs`.
  It remains linked from the connector features guide; linked does not mean current.
- Do not publish, send email, post to Teams, run customer setup commands, or
  claim environment validation.
- Do not silently add connections or modify synchronization metadata.
- Upload/apply through the owner's normal Copilot Studio extension workflow;
  applying updates the remote draft, not a published release.

## 6. Local validation helper

The existing session helper validates Copilot Studio configuration with the
installed language server:

```powershell
node 'C:\Users\miresnic\.copilot\session-state\4e4161bb-15c4-4260-bfde-54c8522c4861\files\validate-studio.cjs'
```

This helper is outside the project and may not be available in another machine
or session. Prefer the installed extension's supported validation if unavailable.
Also check instruction length, exact baseline preservation, and any changed
tool-description limits. Documentation-only updates do not need code validation.
