# Test Plan: <feature / release name>

> Status: Draft · Owner: <name> · Last updated: <date> · Related: <PRD / ticket / PR links>

This template is a pragmatic middle ground between IEEE 829 (familiar section
names, credible with QA) and modern Agile practice (lean, risk-first, treated
as a living document). Delete sections that don't apply — an honest short plan
beats a padded long one.

---

## 1. Overview & objectives

- **What is being tested:** <one-paragraph description of the feature/change>
- **Why / goal of testing:** <what "quality" means here — what must hold true>
- **References:** <links to PRD, design doc, tickets, PRs>

## 2. Scope

**In scope**
- <feature / flow / component to be tested>

**Out of scope** *(state this explicitly — it prevents most disagreements)*
- <what this plan deliberately does NOT cover, and why>

## 3. Test strategy

- **Levels:** <unit / integration / end-to-end / manual exploratory>
- **Automated vs. manual:** <what's automated, what's checked by hand, and why>
- **Types:** <functional, regression, performance, security, accessibility, …>
- **Tools / frameworks:** <e.g. Jest, Playwright, pytest, k6>
- **Platforms / environments under test:** <browsers, OSes, devices, API versions>

## 4. Risk register

Keep this alive — update likelihood/impact as the work evolves.

| Risk | Likelihood (H/M/L) | Impact (H/M/L) | Mitigation | Owner |
|---|---|---|---|---|
| <what could go wrong> | M | H | <how we reduce or catch it> | <name> |

## 5. Test cases / scenarios

| ID | Description | Preconditions | Steps | Expected result | Priority | Status |
|---|---|---|---|---|---|---|
| TC-01 | <what this verifies> | <setup/data> | <1. … 2. …> | <expected> | H/M/L | Not run |

## 6. Entry & exit criteria

- **Entry (can start testing when):** <e.g. feature deployed to staging, test data seeded>
- **Exit (done enough to ship when):** <e.g. all H/M cases pass, no open critical defects, X% automated coverage>

## 7. Environment & test data

- **Environment(s):** <staging URL, config, versions>
- **Test data / fixtures:** <accounts, seeded records, secrets handling>

## 8. Roles & responsibilities

| Role | Person | Responsibility |
|---|---|---|
| Test author | <name> | writes cases |
| Executor | <name> | runs tests, logs defects |
| Sign-off | <name> | approves release |

## 9. Schedule / milestones *(optional, keep light)*

| Milestone | Target date |
|---|---|
| Test cases ready | <date> |
| Test execution complete | <date> |

## 10. Deliverables & sign-off

- **Deliverables:** <test cases, defect reports, execution log, summary report>
- **Sign-off:** testing is complete and the exit criteria above are met.
  - Approved by: __________________  Date: __________
