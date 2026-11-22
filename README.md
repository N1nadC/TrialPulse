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

**Success at a glance:** TrialPulse aims to reject 100% of invalid enrollments, detect site risk within an assumed 24-hour window, deliver dashboard responses within an assumed 2 seconds, and complete batch evaluation of 500 activated sites within an assumed 5 minutes. Additional measures cover Apex and Jest quality, governor headroom, green CI status, and enrollment-cap safety under concurrency.

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
