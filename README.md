# AML transaction flagger

**GitHub Copilot Hackathon: Build Smart, Spend Smart** · FSI breakout · Challenge A: Rules + dashboard

Flag suspicious bank transactions with clear rules, and show the rule behind every flag.

> [!IMPORTANT]
> Synthetic data only. Never bring real customer data, credentials or production systems into this repo.

## Start here

1. Select **Use this template** > **Create a new repository** to create your team's copy. Make it public, or give the judges access.
2. Clone it and open it in your IDE with GitHub Copilot Chat.
3. Read this brief and the [data dictionary](data/README.md).
4. **Before you build**, plan with Copilot Plan mode and estimate your AI Credits per phase in [docs/ai-credit-budget.md](docs/ai-credit-budget.md).
5. Build, test, review and document with Copilot. Commit often: your commit history is evidence.
6. Before your demo, complete [Your solution](#your-solution) at the end of this README.

## The day at a glance

The day has two goals:

1. Use GitHub Copilot beyond autocomplete: plan, agent, instructions, prompt files and review.
2. Build the habit of planning AI Credit consumption before you start prompting.

| | |
|---|---|
| **Team** | 3–5 developers |
| **Build time** | 4 hours |
| **Usage checkpoints** | Your facilitator records each seat's AI Credit usage before the build starts, at the mid-point check (2:00) and at the end. |
| **Demo** | 5 minutes, then Q&A |
| **Scoring** | 100 points. See the [scoring rubric](#scoring-rubric). |

## The challenge

**Who it's for:** a compliance analyst in a bank's financial crime team who reviews alerts every morning.

**The problem:** the analyst needs to see which transactions look suspicious, and why, without reading thousands of rows. A flag that can't be explained can't be acted on.

**What to build:** a rules engine and a dashboard. The engine reads one month of synthetic transactions and applies three clear rules. The dashboard shows the flagged transactions, and every flag shows the rule that fired and the values that triggered it.

### Rules

| Rule ID | Flags a transaction when | Suggested default |
|---|---|---|
| `LARGE_AMOUNT` | Its amount is over the threshold. | `amount_kwd` ≥ **3,000.000 KWD** |
| `VELOCITY` | Its account has many transactions in a short window. | **5 or more** transactions on the same account within **60 minutes** |
| `HIGH_RISK_COUNTRY` | The counterparty is in a high-risk country. | `counterparty_country` is in [`high_risk_countries.csv`](data/high_risk_countries.csv) |

The defaults are a starting point. You can change them if you record the change in your plan and test it. Keep limits in configuration, not in code.

Decide these in your plan and cover them with tests:

- **Explainability:** what the analyst sees for each flag, for example `VELOCITY: 6 transactions in 38 min (limit: 5 in 60 min)`.
- **More than one rule:** a transaction can fire several rules. Show all of them.
- **Bursts:** which transactions in a burst get the `VELOCITY` flag.
- **Boundaries:** test values exactly at the limits, such as 3,000.000 KWD and a gap of exactly 60 minutes.
- **Messy data:** the data has deliberate quality issues. See [Things to know](data/README.md#things-to-know).

### What to build

**Must have**

- [ ] Load the CSV files in [`data/`](data/).
- [ ] A rules engine with the three rules and configurable limits.
- [ ] For each flag, the rule ID and a reason that includes the actual values.
- [ ] A dashboard that lists flagged transactions with their rules and reasons, shows a count per rule, and filters by rule and account.
- [ ] Automated tests on the rules, including boundaries and data-quality cases.

**Stretch goals (optional)**

- Thresholds per customer segment (`RETAIL`, `SME` or `CORPORATE` in `accounts.csv`).
- A fourth rule, `STRUCTURING`: several cash deposits just under the threshold within 24 hours.
- An analyst workflow: clear or escalate a flag with a note, and keep an audit trail.
- Export flagged cases to CSV for a case-management system.

Use any language, framework or database, as long as it runs locally from your README. No machine learning is needed: plain business rules only.

## Starter data

| File | Rows | Contents |
|---|---:|---|
| [`transactions.csv`](data/transactions.csv) | 3,079 | One month (October 2026) of transactions for 150 accounts |
| [`accounts.csv`](data/accounts.csv) | 150 | Account, customer and customer segment |
| [`high_risk_countries.csv`](data/high_risk_countries.csv) | 4 | The list used by `HIGH_RISK_COUNTRY` |

All data is synthetic. The codes `XA` to `XD` are fictional countries, so no real country is labelled high-risk. The [data dictionary](data/README.md) explains every column and the data-quality issues.

## Copilot focus for this challenge

Every team should use the practices the rubric scores: Plan mode, agent mode, a `.github/copilot-instructions.md` file with your team's conventions, reusable prompt files and Copilot Chat review. This challenge puts extra weight on three of them:

| Practice | How to apply it here |
|---|---|
| **Plan mode for rule design** | Turn the rules into a plan with acceptance criteria and edge cases for each rule, and a model choice and AI Credit budget for each phase. Save it as `docs/plan.md`. |
| **Agent-generated tests** | Have agent mode write tests for each rule, including boundaries and data-quality cases. Save the test prompt as a reusable file in `.github/prompts/`. |
| **Copilot Chat review** | Ask Copilot Chat to review the rules engine for bugs and missed edge cases. Fix the findings and note what it caught. |

<details>
<summary>Example prompts</summary>

**Plan mode**

```text
Read README.md and data/README.md. Plan a rules engine and dashboard for the three rules.
For each rule, list acceptance criteria and edge cases. For each phase, recommend a model
tier and estimate the AI Credits it will use.
```

**Tests** (for example, saved as `.github/prompts/rule-tests.prompt.md`)

```text
Write unit tests for the rule in the current file. Cover the normal case, the boundary
values and the data-quality issues described in data/README.md. Follow our test conventions.
```

**Review**

```text
Review the rules engine for bugs, unhandled edge cases and unclear flag reasons.
List the findings by severity. Don't change any code yet.
```

</details>

## AI Credit budget

Estimate AI Credits per phase before you build, then track what you actually use in [docs/ai-credit-budget.md](docs/ai-credit-budget.md). The example below is from the kickoff and assumes a team of 3–5 (950 credits is about USD 9.50). Make your own estimate: the point is the habit of estimating, not hitting an exact number.

| Phase | Copilot feature | Model tier | Budget (AI Credits) |
|---|---|---|---:|
| Plan | Plan mode / Ask | Frontier | 150 |
| Scaffold | Agent mode + instructions file | Low-cost | 100 |
| Build features | Agent / Edit mode | Mixed | 500 |
| Tests + review | Prompt file for tests, chat review | Low-cost | 150 |
| Docs + demo | Ask, README generation | Low-cost | 50 |
| **Total** | | | **950** |

Tips to spend smart:

- Match the model to the task. Save frontier models for planning and tricky logic, and use low-cost models for scaffolding, tests and docs.
- Keep context scoped. Attach only the files Copilot needs.
- Reuse prompt files instead of retyping long prompts.

## Scoring rubric

The rubric awards 100 points across the lifecycle. It's the same in all three sectors, so winners are comparable.

| Stage | Points |
|---|---:|
| Plan | 15 |
| Build & quality with Copilot | 40 |
| Token efficiency | 25 |
| Impact & demo | 20 |
| **Total** | **100** |

### How scoring works

- Judges pick one level per stage: **Solid**, **Strong** or **Outstanding**. Each level is worth the points shown.
- Your score is the sum of the four stage scores. A stage with no evidence scores 0.
- Build & quality plus Token efficiency carry 65 points: they're the two goals of the day.
- Judges score from evidence, not claims: your repo, budget sheet, usage export and demo.
- Tie-break: the team with the higher Token efficiency score wins.
- Solid is the expected baseline. Most teams should reach Strong in four hours. Outstanding should stand out.

### Levels

| Stage | Solid | Strong | Outstanding |
|---|---|---|---|
| **Plan**<br>15 points | **5 pts**<br>Plan written with Copilot Plan mode; credit budget estimated per phase. | **10 pts**<br>Plan has acceptance criteria and a model choice per phase, with the reasoning explained. | **15 pts**<br>Plan drove the build, and the team re-planned at the mid-point check using actual usage. |
| **Build & quality with Copilot**<br>40 points | **16 pts**<br>Agent mode built the main features; `copilot-instructions.md` sets team conventions; runs end to end with tests on the core logic. | **28 pts**<br>Adds reusable prompt files for repeat tasks (tests, docs); edge cases tested; Copilot asked in chat to review the code and findings fixed. | **40 pts**<br>Copilot used across the whole loop (plan, build, test, review, docs); team shows where it saved the most time and where they had to correct it; another team could run it from the README. |
| **Token efficiency**<br>25 points | **10 pts**<br>Usage tracked; actual within ±30% of budget. | **18 pts**<br>Within ±15% of budget; models matched to tasks; context kept scoped. | **25 pts**<br>A concrete before/after where a prompt, context or model change cut credits without hurting the output. |
| **Impact & demo**<br>20 points | **8 pts**<br>Clear user and problem; working demo in 5 minutes. | **14 pts**<br>Rules or workflow are realistic for the sector; Q&A handled well. | **20 pts**<br>Credible path to a real pilot; judges can picture it being used. |

### What judges look at

| Stage | Evidence |
|---|---|
| Plan | `docs/plan.md` from Plan mode, and the budget and mid-point re-plan in `docs/ai-credit-budget.md` |
| Build & quality | `.github/copilot-instructions.md`, `.github/prompts/*.prompt.md`, tests, commit history and this README |
| Token efficiency | Budget versus actual usage in `docs/ai-credit-budget.md`, the usage export, and your before/after example |
| Impact & demo | Your 5-minute demo and Q&A |

### Clarifications

- **Token efficiency isn't about using the fewest credits.** It's judged against your own budget plus proof of optimization, so a team that barely used Copilot can't win it by spending little. Judges reward value delivered per credit. The ±30% and ±15% bands apply to overspending and underspending.
- **IDE features are enough.** Every level can be reached with Copilot Chat features in the IDE: Plan mode, agent mode, instructions, prompt files and chat review. If your organization also has Copilot code review on pull requests, MCP or the Copilot coding agent, they count as extra evidence for Outstanding, but they're never required.
- **Sector judgement counts.** Sector judges apply domain judgement in Impact & demo. For this challenge, strong teams show traceability from each rule to a test.

## Your solution

> [!NOTE]
> Complete this section before your demo. Judges, and other teams, will run your app from it.

- **Team name:**
- **Team members:**
- **Stack:**

### Run it

```bash
# Replace with the exact steps to install and run your app.
```

### Test it

```bash
# Replace with the exact command that runs your tests.
```

### Rule-to-test traceability

| Rule | Implemented in | Tested in |
|---|---|---|
| `LARGE_AMOUNT` | | |
| `VELOCITY` | | |
| `HIGH_RISK_COUNTRY` | | |

### Copilot retrospective

- **Where Copilot saved us the most time:**
- **Where we had to correct it:**

### Evidence checklist

- [ ] Plan: `docs/plan.md`
- [ ] Budget, actual usage, mid-point re-plan and before/after example: [`docs/ai-credit-budget.md`](docs/ai-credit-budget.md)
- [ ] Usage export: `docs/usage/`
- [ ] Team conventions: `.github/copilot-instructions.md`
- [ ] Prompt files: `.github/prompts/`
