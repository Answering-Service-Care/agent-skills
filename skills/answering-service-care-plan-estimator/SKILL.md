---
name: answering-service-care-plan-estimator
description: Pick the Answering Service Care plan that fits a business's expected monthly call minutes and estimate the monthly cost, including overage. Use when a user asks which ASC plan they need or what ASC would cost for their call volume.
---

# Answering Service Care plan estimator

Estimate what Answering Service Care (ASC) would cost for a given call volume and recommend a plan.

## Steps

1. Ask for (or estimate) expected minutes of calls per month. If the user only knows calls per month, assume about 2 minutes per call and say so.
2. `GET https://answeringservicecare.com/api/v1/pricing`. Use the group whose `service` is "Answering Service" unless the user asked about another service, such as Live Chat.
3. For each plan with `billingCycle` = `monthly`:
   - included minutes = the number in `included` (e.g. "100 minutes" -> 100; "0 Minutes" -> 0)
   - overage rate = the dollar amount in `overage` (e.g. "$1.52 Per Add'l Min" -> 1.52)
   - estimated monthly cost = `priceValue` + max(0, expected minutes - included minutes) x overage rate
4. Recommend the plan with the lowest estimated cost. Mention the next plan up if the user's volume is close to its included minutes, since volume varies month to month.
5. If the user can pay upfront, check the `annual` plans too and compare using each plan's `billing` text.

## Output

Give the recommended plan, its estimated monthly cost with the arithmetic shown, the plan's `url`, and any relevant `features`. Note that estimates assume steady volume, and that the pricing page's `notes` (e.g. money-back guarantee) and terms apply.
