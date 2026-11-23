# TrialPulse

**Salesforce Clinical Trial Enrollment Risk Console**

TrialPulse is a Salesforce portfolio project focused on preventing invalid clinical trial enrollments and helping clinical operations teams identify sites that are falling behind. All project data is synthetic; no real patient or clinical data is used.

## The problem

Clinical operations managers can lose weeks before noticing that a trial site is falling behind on enrollment. Without safeguards, a trial can also be over-enrolled, or a participant can be added at a site that has not been activated.

## What TrialPulse does

_Planned capabilities — these features have not yet been implemented._

- **Blocks invalid enrollments:** prevents participants from being added at inactive sites, prevents enrollment beyond a trial's cap, and rejects illegal participant status changes.
- **Flags at-risk sites:** evaluates site enrollment risk every night and pushes live alerts to open dashboards.
- **Supports site coordination:** gives coordinators one place to review site progress and log new participants.

**Success at a glance:** Eight measures cover invalid enrollments, risk detection within the assumed 24-hour target, dashboard and batch performance, test quality, governor headroom, CI health, and cap safety under concurrency. The full measures and verification methods are listed below.

## Success measures

No success target has been measured yet. The targets below are project goals, not claims of achieved performance. Assumption-based targets are identified in the Notes column.

| ID        | Measure                      | How it is measured                                                                       | Target                                                                                 | Notes                                                                                        |
| --------- | ---------------------------- | ---------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| **SM-01** | Invalid enrollments blocked  | Test results for cap, inactive-site, and status-transition cases                         | 100% of invalid cases rejected, with exact error messages                              |                                                                                              |
| **SM-02** | Time to detect a risk        | A test showing a site flagged by the next nightly run                                    | Within 24 hours of the job running                                                     | The 24-hour target is an assumption. The weekly manual-check baseline is also an assumption. |
| **SM-03** | Dashboard response time      | Server time for the first page of 25 sites with 500 sites and 50,000 participants loaded | 2 seconds or less                                                                      | Assumption; record the actual measured figure.                                               |
| **SM-04** | Batch duration               | Apex Jobs page, with 500 Activated sites                                                 | 5 minutes or less                                                                      | Assumption; record the actual measured figure.                                               |
| **SM-05** | Test quality                 | Apex coverage report and Jest results                                                    | At least 90% overall Apex coverage, at least 85% per class, and all Jest tests passing | All three conditions must be met.                                                            |
| **SM-06** | Governor headroom            | Limits snapshot for the heaviest operations                                              | Below 50% of each governor limit used                                                  |                                                                                              |
| **SM-07** | CI health                    | GitHub Actions status on `main`                                                          | Green                                                                                  | CI is not yet configured.                                                                    |
| **SM-08** | Cap safety under concurrency | Manual race experiment, repeated runs                                                    | Final enrolled count never above the target                                            | Nice tier. Only claim this as tested after running and recording the experiment.             |

## Project scope

### In scope

- Trial, Site, Investigator, Participant, and Deviation data model.
- Apex trigger, batch job, and services.
- Three primary Lightning Web Component features, implemented through four components:
  - `tpSiteDashboard` — site dashboard.
  - `tpSiteRow` — child component for an individual site row.
  - `tpParticipantForm` — participant entry form.
  - `tpRiskAlertListener` — live risk-alert subscriber.
- Platform Event alerts.
- Permission sets.
- Continuous integration (CI).

The three primary LWC features are the site dashboard, participant entry, and live risk alerts. The detailed component design includes `tpSiteRow` as a child of the dashboard, making four components in total. The Phase 1 scope statement says three Lightning Web Components, while the detailed Phase 9 specification and resume guidance identify four components.

### Out of scope

- Experience Cloud.
- Approvals.
- External clinical systems.
- Real patient data.

### Potential future work

- Flow-based approvals.
- Agentforce actions.
- Integrations.

## Portfolio and data disclaimer

TrialPulse is a portfolio project, not a production clinical system. All data used in the project is synthetic. No real patient or clinical data is used or stored. The project does not make regulatory or compliance claims.

## Phase 0 — Environment and ground rules

### Ground rules

1. **Source control is the truth.** Orgs are disposable. Every piece of metadata is retrieved or written to the repository and committed.
2. **One change per branch and pull request**, with a short description.
3. **Measure before you claim.** No number goes in the README or on a resume unless it has been measured.
4. **Write and understand every class and component yourself.** If AI help is used for a first draft, rewrite it and be ready to explain every decision live.

### Phase 0 verification

All six Phase 0 checklist items in the project blueprint are complete:

1. Developer Edition org created and Dev Hub enabled.
2. Salesforce CLI installed and authorized to the Dev Hub with an alias.
3. The `TrialPulseDev` scratch org was created, opened, and verified as Active.
4. Node.js, Git, and Python are installed.
5. GitHub repository created and the `main` branch protected.
6. The blueprint ground rules are recorded in this README, including that all data is synthetic.

**Deployment exit-gate note:** The Phase 0 deployment exit gate has not been demonstrated. `sf project deploy preview` completed with no conflicts, but reported no files to deploy. The `force-app/main/default` package directory contained no metadata, so an actual empty-project deployment could not be demonstrated. No placeholder metadata, Apex classes, or Lightning Web Components were added for this check.

## Phase 3 — Scope, tiers and effort

### Delivery tiers

The project is divided into three delivery tiers so that scope can be reduced in a controlled order if time becomes limited.

| Tier       | Included work                                                                                                                                                                                                                                              | Estimated effort |
| ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------: |
| **Must**   | Data model and metadata; security; trigger with cap, site and transition rules; risk evaluator, batch, scheduler and Platform Event; controllers; three components with Jest tests; `Error_Log`; small data set; README, ERD, decision log and demo video. |    100–110 hours |
| **Should** | 50,000-row volume proof with measurements; CI workflows; idempotent loader polish; deviations on the dashboard; accessibility pass.                                                                                                                        |      25–30 hours |
| **Nice**   | Inbound REST endpoint; race experiment; denormalized Trial lookup optimization; static analysis; release changelog.                                                                                                                                        |      15–20 hours |

**Estimated total:** 145–160 hours.

### Schedule baseline

The blueprint estimates that the full project requires approximately four weeks at eight hours per day, or ten weeks at 15 hours per week. The Must tier alone is estimated at approximately three weeks at eight hours per day.

For planning, this README uses the following **provisional baseline**:

| Planning item                 | Baseline          |
| ----------------------------- | ----------------- |
| Planned start date            | 23 November 2026  |
| Target completion date        | 1 February 2027   |
| Planned capacity              | 15 hours per week |
| Estimated full-project effort | 145–160 hours     |

The dates are planning targets based on an assumed availability of 15 hours per week. They are not evidence that the work has been completed or a guaranteed delivery commitment. Revise the target completion date if the available weekly time changes.

### Scope reduction order

If time becomes limited, reduce scope in this order:

1. Cut the **Nice** tier first.
2. Cut the second pass of the 50,000-row volume proof.
3. Cut the scratch-org job in CI, but keep lint and Jest.
4. Cut deviations on the dashboard.

**Do not cut tests, security or the README.**

### Phase 3 checklist

- [x] Must, Should and Nice scope and effort estimates copied into the README roadmap.
- [ ] Delivery tiers confirmed for execution.
- [x] Provisional start date and target end date selected and recorded.
- [ ] Weekly time availability (assumed at 15 hours per week) and the schedule confirmed.
- [ ] Scope reduction order reviewed and agreed.

**Phase 3 exit gate:** A dated project plan exists. The dates above are provisional until the weekly-capacity assumption is confirmed. The day-by-day plan is defined in Phase 14.
