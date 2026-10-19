# Rosie: Using ROSI to Explain Security Fixes

**Career Simulation 4**

**Project type:** Solo project
**Report date:** October 7, 2026

## Executive summary

Security teams find problems that should be fixed, but company leaders also need to understand why a fix matters, what it costs, and what risk may remain. Rosie is a prototype return on security investment (ROSI) estimator that helps organize this information for a decision.

ROSI is a way to compare the estimated cost of a security problem with the cost of reducing it. It can help a company explain security work when it asks executives for time, staff, or money. A ROSI result is only as useful as its assumptions. It is an estimate, not a promise of savings or proof that an incident will happen.

This report describes how companies could use ROSI over time: connect a technical finding to a business risk, explain a proposed fix, and check later whether the fix worked. Rosie currently lets a user upload a STIG checklist, sort findings by category, enter estimates, save fixes in browser-based collections, and make printable reports. A STIG checklist is one possible source of security findings; the same decision process can be used for other security work.

As an entry-level security project, my goal is to show a clear way to think through these decisions. Rosie does not perform a complete risk assessment, verify a company's security controls, or choose which work executives should fund.

## Why I chose STIGs

I chose STIGs because they provide detailed, technology-specific security settings that can be checked against a system. The Defense Information Systems Agency (DISA), part of the Department of Defense (DoD), publishes STIGs for DoD systems. The DoD Cyber Exchange says this guidance links DoD requirements to NIST security controls and the Risk Management Framework.[1]

Organizations that do not work with the DoD may still find STIGs useful as a reference when improving system security. They can review the recommendations and decide which ones fit their technology, risks, and business needs. A STIG is not automatically required for every company. Organizations should check which requirements apply and test changes before deploying them widely.

I also chose STIGs because they cover many different technologies and give users a consistent set of requirements to review. DISA releases STIG updates quarterly, giving organizations a regular chance to review the guidance and decide what applies to their systems.[5] They must implement any relevant changes themselves. Their detail makes STIGs a useful starting point for discussing security gaps and possible fixes. They do not guarantee that a system is secure, but they can help make security expectations more specific and easier to track.

## 1. From a finding to a business risk

A security finding might say that a setting is missing or a system needs an update. That tells the security team what to investigate, but it does not tell the full business story. To explain why a fix matters, ask:

- What company information, service, or system could be affected?
- What could happen, and how might the weakness contribute to it?
- What security protections are already in place?
- What could the incident cost the company in time, money, or lost service?
- What fix is available, and what will it cost to put in place?
- How will the company check that the fix worked?

For example, a checklist might show that a mobile-device setting does not meet a security requirement. That could lead a company to ask whether a lost phone might expose work information. The answer depends on what information is on the phone, whether it is managed by the company, and what protections are already enabled. The checklist alone does not show that the phone was hacked or that an incident is likely.

### CAT severity is not the chance of an incident

STIG CAT levels help show how serious a requirement is within the security benchmark. A CAT level is not an annual incident percentage, a dollar amount, or an ROI result. Two companies may have the same finding but different levels of risk because they have different systems, information, users, and protections. The company needs to consider its own situation.[1]

## 2. What goes into a ROSI estimate

Rosie's calculation estimates the possible benefit of reducing one defined security risk and compares it with the cost of the fix.

- `p` = estimated chance, as a percentage, that the incident happens in one year;
- `I` = estimated cost if it happens;
- `r` = estimated percentage of this risk reduced by the fix;
- `F` = one-time cost to put the fix in place;
- `A` = cost that repeats each year; and
- `Y` = number of years being considered.

```text
Estimated benefit per year = (p / 100) × I × (r / 100)
Cost over the period       = F + (A × Y)
Simple ROSI (%)            = ((estimated benefit per year × Y − cost over the period)
                              / cost over the period) × 100
```

I based this calculation on the general ROSI approach described by ENISA: estimate the loss a security measure may prevent, subtract the measure's cost, and compare the result with that cost.[4] Rosie uses a simplified version of that idea. It estimates the loss before the fix as the annual chance of the incident multiplied by its estimated cost, then applies the estimated risk reduction. The multi-year version above also includes both one-time and recurring costs. I selected this format for the prototype; it is not a formula copied exactly from ENISA, and other ROSI models may handle probability, costs, and time differently.

The **planning period** (`Y`) is the number of years included in the comparison. It is a time window for adding up the estimated yearly benefits and recurring costs. It does not mean that an incident is expected to happen after that many years, or that the fix will stop working then. Rosie counts the implementation cost once, then repeats the estimated annual benefit and annual operating cost for each year in the selected period.

For example, suppose a company estimates a 10% chance of one kind of incident in a year, a $100,000 cost if it happens, and that a fix would reduce the risk by 40%. The estimated benefit is $4,000 per year. If the fix costs $2,500 once and $500 per year, the one-year cost is $3,000 and the simple ROSI is about 33%.

With the same estimates over a **three-year planning period**, Rosie counts $4,000 of estimated benefit each year, for $12,000 total. It counts the $2,500 implementation cost once, plus $500 of recurring cost for each of the three years, for $4,000 total investment. The resulting simple ROSI is 200%. This does not mean the fix is guaranteed to save $12,000; it shows what the model returns if the estimates stay the same for all three years. A longer period can change the result, so the user should choose a period that matches the decision being considered and explain that choice.

Those numbers only show how the math works. They do not prove the incident has a 10% chance of happening or that the company will save $4,000. The model also assumes the estimated annual benefit stays the same each year and does not adjust future costs or benefits for inflation or investment value. A real proposal should explain where its numbers came from. If the evidence is uncertain, it is better to show a reasonable range or say that the estimate has low confidence than to make it look exact.

### Choosing the estimates

**Chance of an incident:** Describe one specific event and the systems it could affect. Use relevant company history, security reports, known exposure, and existing protections if available. Do not convert a CAT level into a probability.

**Cost if it happens:** Consider costs that fit the event, such as investigation, recovery, work delays, or customer support. Explain what is included and avoid counting the same loss twice.

**Risk reduced by the fix:** Estimate how much the fix helps with this event. Most fixes reduce risk; they do not remove every possible cause of an incident. The company also needs to check that the fix reached the systems it was meant to protect.

**Cost of the fix:** Include staff time and other costs that apply, such as testing, software, deployment, training, and ongoing support. Separate the one-time cost from costs that repeat each year.

It is useful to write down the source, date, systems covered, and person responsible for each estimate. NIST guidance recommends choosing security measures that fit the organization's goals and reviewing information over time to improve decisions.[2]

### Limits of the calculation

Rosie's calculator is intentionally simple. It assumes the estimated benefit stays the same each year. It does not adjust future amounts for inflation or investment value, calculate a statistical confidence range, or account for several incidents in one year. It also cannot tell when two fixes address the same risk, so a person must avoid counting the same possible loss twice.

Some security work may still be necessary even when the estimated ROSI is low or uncertain. A company may need a fix to meet a rule, contract, or business requirement. The final decision should consider the estimate along with security needs and operational impact.

## 3. Explaining a fix to executives

An executive summary should make the request clear without hiding uncertainty. For each fix, a company could explain:

| Topic | What to include |
| --- | --- |
| Business need | What company service or information the fix helps protect. |
| Finding | What was found, which systems are affected, and when it was checked. |
| Possible incident | What could happen and how the finding could contribute. |
| Fix and other choices | The recommended change and any reasonable alternatives. |
| Cost and timing | Staff time, other costs, who will do the work, and the expected date. |
| ROSI estimate | The assumptions, evidence, time period, and uncertainty. |
| Remaining risk | What the fix will not prevent or what is still unknown. |
| How success will be checked | What evidence will show the fix is working. |
| Decision requested | The funding, priority, or approval needed from executives. |

It is important to label facts and estimates separately. “The checklist reported four findings” is a statement about the uploaded file. “The fix may lower the chance of a certain incident” is an estimate. “The company will save this amount” should not be stated as a fact unless actual results support it.

## 4. Using ROSI over time

ROSI can be more useful when a company reviews security work regularly, such as every quarter. The company can compare proposed fixes and later update its estimates with better information. NIST's enterprise risk guidance describes using recorded cybersecurity risks to support decisions and ongoing review.[3]

### Before the fix

Write down what systems are affected, what protections are already active, how many systems have the finding, and where the estimates came from. This is the starting point for checking progress.

### While the fix is being done

Record who is responsible, the planned date, the actual cost, how many systems received the fix, and any systems that could not be updated. A security setting being created does not mean it is active everywhere.

### After the fix

Check whether the systems now meet the requirement and whether any exceptions remain. Depending on the fix, the company could also track how long fixes take, how often systems fall out of compliance, or whether the incident type becomes less common. Each measure should have a clear source and owner.

### At the next review

Update the estimate if the systems, protections, threat information, incident history, or costs have changed. If coverage is incomplete, the fix may reduce less risk than expected. If the company learns more, it can improve its earlier estimate. Keep related findings separate when they describe different problems, and avoid adding the same potential savings more than once.

This repeatable process can help executives see what was requested, what was completed, and what still needs attention. A combined report can help compare work across technologies, but one large dollar total should not hide uncertain assumptions or different types of risk.

## 5. Project scope and rubric alignment

Rosie is a solo project and an early prototype. It reads an uploaded STIG checklist in the browser, groups findings by category, lets the user enter estimates, saves collections in the current browser profile, and makes printable reports. It does not connect to a company's device-management, incident, asset, or finance systems. The user must provide and check the estimates.

| Rubric area | Project evidence |
| --- | --- |
| Technical competencies | I explain how a security finding can relate to a threat, company information, possible impact, and a fix. I show the ROSI math and describe its limits. |
| Problem solving | I separate STIG severity from incident chance, explain why estimates need evidence, and describe how to compare fixes without counting the same loss twice. |
| Communication | I organize the information so a technical reader can follow the security reasoning and an executive can understand the cost and decision requested. |
| Interpersonal skills | Teamwork, group consensus, and peer feedback do not apply because this is a solo project. |

## Conclusion

ROSI can help a company explain why a security fix matters, compare its cost with other work, and show executives what is known and what is still an estimate. Its long-term value comes from checking whether fixes were put in place, measuring the results, and updating assumptions when the company learns more.

Rosie is a simple tool to help organize that discussion. It does not guarantee savings or make the decision for the company. Clear assumptions, honest uncertainty, and follow-up checks make the estimate more useful.

## References

1. DoD Cyber Exchange, [Security Technical Implementation Guides (STIGs)](https://public.cyber.mil/stigs/).
2. NIST, [SP 800-55 Vol. 1, *Measurement Guide for Information Security: Identifying and Selecting Measures*](https://csrc.nist.gov/pubs/sp/800/55/v1/final).
3. NIST, [IR 8286 Rev. 1, *Integrating Cybersecurity and Enterprise Risk Management*](https://csrc.nist.gov/pubs/ir/8286/r1/final).
4. European Union Agency for Cybersecurity (ENISA), [*Introduction to Return on Security Investment*](https://www.enisa.europa.eu/publications/introduction-to-return-on-security-investment).
5. DoD Cyber Exchange, [STIG Quarterly Release Schedule and Summary](https://public.cyber.mil/stigs/release-schedule/).
