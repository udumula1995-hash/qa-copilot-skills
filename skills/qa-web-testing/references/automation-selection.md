# Choosing which manual test cases to automate

## Step 1: Exclusions (any one of these = Keep manual, whatever the score)
- Needs human judgement: look and feel, usability, readability, visual layout.
- CAPTCHA, or OTP / multi-factor codes with no test bypass.
- Depends on email, SMS, a physical device, a printer or a third-party system that the
  test cannot control or read.
- Feature is still changing (UI or rules expected to change within the next sprint or two).
- Runs once only (one-off migration check, ad hoc verification).
- Expected result is unknown or disputed (source is OBS and unconfirmed).
- Exploratory by nature.

## Step 2: Score (0, 1 or 2 for each; total out of 14)
| Criterion | 0 | 1 | 2 |
|---|---|---|---|
| Run frequency | rarely | each release | every build / smoke / regression |
| Business criticality | cosmetic | standard function | core flow, money, data, access |
| Stability of feature and UI | changing | mostly stable | stable |
| Deterministic result | subjective or flaky | checkable with effort | clear pass/fail on screen |
| Test data control | cannot set up | manual setup needed | data easy to create or reset |
| Manual effort saved | under 2 minutes | 2 to 10 minutes | over 10 minutes or many data sets |
| Feasible in Avo Assure | blocked | needs workaround (custom element, waits) | standard keywords |

## Step 3: Decision
- **10 to 14: Automate now.**
- **7 to 9: Automate later.** State what must change first (stabilise, get test data, confirm rule).
- **0 to 6: Keep manual.**

## Step 4: Order of automation
1. Smoke: login plus the one core path per module.
2. High-priority regression cases on stable features.
3. Data-driven cases (same steps, many data sets): one design plus a data table.
4. Negative and boundary cases with clear on-screen messages.

## Good practice
- Automate the check, not the whole manual script: drop steps that add no verification.
- Prefer several short automated cases over one long end-to-end chain; long chains fail
  for unrelated reasons and hide the cause.
- Each automated case sets up its own data or uses dedicated stable test data.
- A typical healthy split: most smoke and core regression automated, most negative,
  UI and one-off checks manual. Do not force a percentage.
