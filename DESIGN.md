# ODA Setup and Configuration Assistant - Main Design

Status: Phase 1 configuration uploaded to the personal environment, as confirmed
by the owner on 2026-11-05; live validation and publication pending.
Date: 2026-11-04
Updated: 2026-11-05

This document records the redesigned agent direction. The agent should become a
trusted ODA setup and configuration assistant, not primarily an intent router.
Routing becomes supporting infrastructure.

Implementation companion: [Phase 1 implementation guide](PHASE-1-IMPLEMENTATION.md).

## 1. Mission

Provide accurate, source-backed ODA setup and configuration guidance; answer
routine questions in selected Teams channels; create tailored scoping emails and
implementation guides; and teach users how to perform the work.

Initial workloads are On-Demand Assessment - Microsoft Entra ID and On-Demand
Assessment - Active Directory. Shared setup/configuration guidance and the
assessment-specific instructions are all Microsoft Learn web-page content,
retrieved through the project's existing Learn MCP and knowledge source.

Initial deployment is to the owner's personal environment. The owner confirmed
the current "Mike Resnick (CSA)" project connection as the target on 2026-11-05.
The requesting user is the knowledge owner. Technical reviewers and
the pilot audience will be assigned after initial preparation.

## 2. Implementation priorities

| Phase | Capability | Release requirement |
|---|---|---|
| 1 | Authoritative ODA reference | Answers are supported by approved sources, distinguish workload-specific requirements, and clearly identify uncertainty. |
| 2 | Automatic Teams answers | Responds in selected channels without an @mention, but only when the question passes a conservative answerability check. |
| 3 | Scoping email assistant | Collects missing engagement details and generates an email containing the applicable prerequisites. |
| 4 | Word/PDF setup guides | Produces a tailored, source-backed, end-to-end implementation document. |
| 5 | ODA trainer | Delivers guided lessons, exercises, checkpoints, and corrective feedback using the same validated technical foundation. |

All five capabilities must use the same maintained ODA knowledge, not five
independently written sets of technical instructions.

## 3. Phase 1 - Reference foundation

### Source policy

1. Current official Microsoft ODA web pages establish technical requirements.
   Do not use PDF documents hosted on learn.microsoft.com as knowledge sources;
   they are excluded from this design because they are outdated.
2. Approved internal guides supplement delivery practices and
   organization-specific procedures.
3. Conflicts are surfaced for review, not silently resolved by the model.

The owner identified the Microsoft Learn `assessment-prereq-docs` page as
outdated. Exclude it from answer evidence even when linked by another ODA page;
the owner will request correction of the upstream link.

The shared setup landing page is a starting point, not the whole procedure.
Required linked Microsoft Learn web pages form part of the evidence for the
confirmed scenario. Phase 1 intake returns confirmed context to the agent for
retrieval through the existing Learn MCP rather than generating a complete guide
from one landing-page summary. Missing linked evidence blocks the affected
procedure; configured tools do not imply automatic or complete link traversal.

For each supported workload, maintain a reviewed coverage map:

- Supported scenarios and limitations.
- Scoping questions.
- Permissions and prerequisites.
- Setup and configuration sequence.
- Validation and expected results.
- Common failures and troubleshooting.
- Source links, review date, and content owner.

### Answer contract

Every technical answer should:

- Identify the applicable workload and scenario.
- Ask for missing information when it changes the instructions.
- Cite supporting sources using the platform's supported citation behavior.
- Separate mandatory prerequisites from recommendations.
- Avoid inventing commands, permissions, versions, or successful outcomes.
- Say when the available evidence is insufficient.
- Offer host-supported Copy functionality or a truthful copy-ready alternative.
- When reporting documentation gaps, suggest a consolidated manual email to
  [Tech Discussion - On-Demand Assessment Setup VTeam](mailto:d6a4891c.microsoft.com@amer.teams.ms).
  Offer a draft for user review/redaction; do not send or post automatically.

For from-the-beginning setup, explain Engage Center/Connector responsibilities,
how assessment binaries reach the collector, and how assessment output is
collected and ingested into Log Analytics. Require supporting evidence for each
applicable stage; scheduled-task instructions alone are not end-to-end coverage.

"Complete and accurate" is a tested coverage goal, not a promise that the model
can answer everything.

## 4. Phase 2 - Conservative Teams responder

The selected behavior is monitoring selected channels without requiring an
@mention.

Publishing an agent to Teams alone does not establish that capability. Microsoft
documents additional permissions for receiving unmentioned channel messages; the
supported integration path must be verified for the Copilot Studio deployment.
See [Teams message-access documentation](https://learn.microsoft.com/en-us/microsoftteams/platform/agents-in-teams/enable-receive-all-chat-messages).

Recommended processing:

```text
New message in an approved channel
    -> Is this an ODA question?
    -> Is enough scenario information available?
    -> Retrieve approved evidence
    -> Does the evidence directly support a routine answer?
        Yes -> Reply in the original thread with citations
        No  -> Do not post a speculative answer
```

Define "does not require advanced research" operationally:

- Covered by approved documentation.
- No unresolved source conflict.
- No required inspection of private logs or customer systems.
- No novel diagnosis or unsupported scenario.
- No missing detail that materially changes the answer.

Do not use model-reported confidence alone to authorize posting.

Start in shadow mode: generate proposed answers for reviewer approval before
enabling automatic replies. Include duplicate suppression, bot-message
filtering, permission checks, and an operational stop switch.

Channel answers must not expose internal information to an audience that lacks
access. Authorization must be enforced by the integration and data access
configuration, not by prompt instructions alone.

## 5. Phase 3 - Guided scoping workflow

This capability is more than "write an email":

1. Identify the assessment workload.
2. Collect the customer's relevant environment and delivery details.
3. Ask only the clarification questions needed for that scenario.
4. Select prerequisites from reviewed workload guidance.
5. Present a scope summary for confirmation.
6. Generate the email, distinguishing confirmed facts from outstanding questions.

Drafting does not imply sending. Email delivery is outside the initial scope
unless explicitly added.

## 6. Phase 4 - Implementation documents

Generate the document from the confirmed scope and validated guidance.

Include:

- Scope and assumptions.
- Prerequisites and responsibilities.
- Ordered setup/configuration steps.
- Verification checkpoints and expected results.
- Troubleshooting and escalation guidance.
- Sources and generation date.

Use Word first, with PDF conversion as a separate step. Copilot Studio currently
documents Word document output as a preview feature; a template-based flow is
another option to evaluate against tenant policy and licensing.
See [document output documentation](https://learn.microsoft.com/en-us/microsoft-copilot-studio/generate-document-output-prompt).

## 7. Phase 5 - Trainer

Reuse the same technical content in a different interaction style:

- Assess the learner's experience.
- Offer workload-specific learning paths.
- Explain concepts before procedures.
- Provide exercises and knowledge checks.
- Request verification evidence before advancing.
- Explain mistakes rather than simply supplying answers.

The trainer must distinguish "the learner understands the step" from "the
environment has actually passed validation."

## 8. Replacement for the original Topic 02 design

The obsolete numbered custom topics and incomplete JSON classifier/router have
been removed from the local project at the user's request. Phase 1 uses source
retrieval and the existing system topics rather than replacement scaffold topics.
System topics for grounded answers, authentication, fallback, errors, and
conversation lifecycle remain.

Use clear capability entry points:

- Ask an ODA question.
- Scope an engagement.
- Create a setup guide.
- Learn ODA.

Keep a small clarification topic for ambiguous requests. Treat Teams monitoring
as a separate intake mechanism that reuses the reference-answering capability.

## 9. Open decisions and implementation boundary

Before personal-environment readiness is approved, confirm:

- Supported deployment scenarios for the two selected workloads.
- Approved official web pages and internal evidence, and their live retrieval.
- Authenticated source connections in the confirmed personal environment.
- Available knowledge connectors, authentication, and access restrictions.
- Source review cadence and evaluation-set approval.

Technical reviewers and the pilot audience are intentionally deferred until the
owner has completed initial preparation. They remain prerequisites for a
reviewed pilot, not prerequisites for editing the personal-environment draft.

Later phases additionally require decisions about Teams integration and consent,
document-generation tooling, storage, licensing, and training delivery.

Current authorization covers Phase 1 implementation using existing resources
and removal of obsolete custom topics. Local instructions and grounded-answer
guidance have been updated, and general model-knowledge answers disabled.
Original instructions remain verbatim at the beginning of the instruction field.
No new knowledge connections, permissions, flows, or later-phase integrations
are introduced. Local changes are not evidence of deployment or runtime quality.
