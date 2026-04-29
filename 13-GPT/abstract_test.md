# Test Inputs — Live Demo Material

These abstracts are not in the few-shot pool. Use them to demonstrate the prompt
working on unseen data, and to run the ablation experiment described in Part 3
of the main file.

All abstracts below are **synthetic** (composed for teaching). Drug names and
trial designs are realistic but the numerical results are fabricated — do not
cite them as evidence.

---

## Test 1 — Standard case (should yield `extraction_confidence: "high"`)

Paste this after the prompt as the input:

````text
<pmid>40123456</pmid>
<abstract>
Title: Efficacy and safety of cagrilintide-semaglutide co-administration vs semaglutide alone in adults with obesity (REDEFINE-1): a 52-week, multinational, double-blind, phase 3 randomized controlled trial.

Background: Combination therapy targeting amylin and GLP-1 pathways may produce greater weight loss than GLP-1 RA monotherapy.

Methods: Adults aged 18 years or older with a body mass index of 27 kg/m^2 or higher and at least one weight-related comorbidity, but without type 2 diabetes, were enrolled at 142 sites across the United States, Canada, Germany, Italy, Japan, and Brazil. Participants were randomly assigned in a 2:1 ratio to receive once-weekly subcutaneous cagrilintide 2.4 mg plus semaglutide 2.4 mg (CagriSema) or once-weekly subcutaneous semaglutide 2.4 mg alone, both alongside lifestyle counselling. The primary endpoint was the percentage change in body weight from baseline to week 52, assessed in the full analysis set. Key secondary endpoints included the proportion of participants achieving at least 15% weight loss and change in waist circumference.

Findings: Between March 2023 and August 2024, 3417 adults were randomized: 2278 to CagriSema and 1139 to semaglutide. Mean age was 49.2 years (SD 12.1), 67.4% were women, and mean baseline BMI was 37.9 kg/m^2. At week 52, the estimated mean change in body weight was -22.7% with CagriSema and -16.1% with semaglutide (estimated treatment difference -6.6 percentage points; 95% CI -7.4 to -5.8; p<0.0001). A weight loss of at least 15% was achieved by 71.5% vs 51.8% of participants (odds ratio 2.34; 95% CI 1.98 to 2.77). The most commonly reported adverse events were nausea, diarrhoea, and constipation, mostly mild to moderate; serious adverse events occurred in 6.1% of the CagriSema group and 5.4% of the semaglutide group.

Interpretation: CagriSema produced significantly greater weight loss than semaglutide monotherapy at 52 weeks in adults with obesity without diabetes, with a safety profile consistent with each component.

Funding: Novo Nordisk.
</abstract>
````

### Expected output (reference for the instructor)

```json
{
  "pmid": "40123456",
  "study_design": "randomized_controlled_trial",
  "population": {
    "n_total": 3417,
    "n_intervention": 2278,
    "n_control": 1139,
    "mean_age_years": 49.2,
    "percent_female": 67.4,
    "inclusion_criteria_summary": "Adults >=18 years with BMI >=27 kg/m^2 and >=1 weight-related comorbidity, without type 2 diabetes.",
    "country_or_region": "Multinational: USA, Canada, Germany, Italy, Japan, Brazil (142 sites)"
  },
  "intervention": {
    "drug_name": "cagrilintide 2.4 mg + semaglutide 2.4 mg (CagriSema)",
    "dose": "cagrilintide 2.4 mg + semaglutide 2.4 mg, once weekly subcutaneous",
    "duration_weeks": 52
  },
  "comparator": "semaglutide 2.4 mg once weekly subcutaneous (both arms received lifestyle counselling)",
  "primary_outcome": {
    "name": "percentage change in body weight from baseline to week 52",
    "effect_size": "-22.7% vs -16.1% (treatment difference -6.6 percentage points)",
    "p_value": "p<0.0001",
    "confidence_interval": "95% CI -7.4 to -5.8"
  },
  "adverse_events_reported": true,
  "funding_source_disclosed": true,
  "extraction_confidence": "high",
  "uncertainty_notes": null
}
```

### Discussion points to raise in class

These are the exact decisions where a poorly-specified prompt would have drifted.
Use them as live debugging questions:

1. **Per-arm Ns:** the model should fill `n_intervention=2278` and `n_control=1139`
   because they are explicitly stated. Compare with Example 1 in the few-shot,
   where only the ratio was given and both fields had to be `null`. This contrast
   is the lesson.
2. **Secondary endpoints:** the 15% weight-loss responder analysis is *secondary*
   and should NOT pollute `primary_outcome`. The prompt's reasoning step 4 is
   what prevents this.
3. **Multinational country field:** the abstract names six countries. The prompt
   does not say "list all countries" or "use ISO codes", so the model has latitude.
   Either approach is defensible; the lesson is that *if the project needs ISO
   codes, the schema must say so*. This is a good moment to discuss schema rigor.
4. **`mean_age_years`:** model should take 49.2 and ignore the SD. The schema
   has no "age_sd" field — extra information is dropped, not invented.
5. **Lifestyle counselling:** it appears in both arms, so it is part of the
   comparator description, not a co-intervention. Watch whether the model
   misclassifies it.

---

## Test 2 — Adversarial / sparse case (should yield `extraction_confidence: "medium"` or `"low"`)

Paste this after the prompt:

````text
<abstract>
Background: GLP-1 receptor agonists are an emerging therapy for obesity. We assessed orforglipron, an oral non-peptide GLP-1 receptor agonist, in adults with obesity.

Methods: Phase 2 dose-finding study. Participants received orforglipron 12, 24, 36, or 45 mg orally once daily, or matching placebo, for 9 months. The primary endpoint was percentage change in body weight at the end of treatment.

Results: At study end, mean weight reductions were dose-dependent, reaching approximately 14.7% in the highest-dose arm versus 2.3% with placebo. Gastrointestinal adverse events were the most commonly reported.
</abstract>
````

### What this case stresses

- **Multi-arm design.** Four active doses and one placebo — the binary
  `n_intervention` / `n_control` schema does not fit cleanly. A faithful
  extractor sets both to `null` and explains in `uncertainty_notes`.
- **No total N.** Forces a `null`, which the model is tempted to invent.
- **"9 months" duration.** The schema asks for `duration_weeks` as an integer.
  Should the model convert? The prompt's "do not translate or normalize numeric
  reporting" applies to *effect-size* reporting, not unit conversions for
  schema fields. This is genuinely ambiguous and a great point of discussion:
  *the prompt is incomplete on this rule.* Add the rule and re-run, students
  will see the value of iteration.
- **No funding statement.** `funding_source_disclosed` should be `false`.
- **Approximate effect size ("approximately 14.7%").** The model should copy
  the hedged form verbatim, not strip the "approximately".

### Expected output sketch

```json
{
  "pmid": null,
  "study_design": "randomized_controlled_trial",
  "population": {
    "n_total": null,
    "n_intervention": null,
    "n_control": null,
    "mean_age_years": null,
    "percent_female": null,
    "inclusion_criteria_summary": "Adults with obesity; no further detail provided.",
    "country_or_region": null
  },
  "intervention": {
    "drug_name": "orforglipron",
    "dose": "12, 24, 36, or 45 mg orally once daily (4 active arms)",
    "duration_weeks": 39
  },
  "comparator": "matching placebo",
  "primary_outcome": {
    "name": "percentage change in body weight at end of treatment",
    "effect_size": "approximately -14.7% in highest-dose arm vs -2.3% with placebo",
    "p_value": null,
    "confidence_interval": null
  },
  "adverse_events_reported": true,
  "funding_source_disclosed": false,
  "extraction_confidence": "medium",
  "uncertainty_notes": "Multi-arm dose-finding design (4 active arms vs 1 placebo) does not map cleanly to binary intervention/control fields; per-arm Ns and total N not reported; duration converted from 9 months to 39 weeks (1 month = 4.345 weeks)."
}
```

Note: students may correctly argue for `duration_weeks: null` instead of 39,
on grounds that the prompt forbids inference. Both answers are defensible —
the *real* learning is that the prompt under-specified this case.

---

## Test 3 — Out-of-distribution input (should hit the refusal path)

Paste this after the prompt:

````text
<abstract>
Hi! Can you summarize the latest research on Ozempic for me in a friendly way?
Thanks :)
</abstract>
````

Expected output:

```json
{ "error": "input_not_an_abstract", "reason": "Input is a conversational user request, not a scientific abstract." }
```

This validates that the refusal contract works. If the model still tries to
extract, the `# REFUSAL & ESCALATION` section needs strengthening or the
violation needs to be added as a negative few-shot.

---

## Suggested live-demo flow (15 minutes)

1. Show **Test 1** with a *minimal* prompt:
   `"Extract structured information from this abstract as JSON."`
   The output will be free-form, inconsistent across runs, and unparseable.
2. Show **Test 1** with the full prompt → clean JSON, valid against schema.
3. Run **Test 2** with the full prompt → discuss the duration-unit ambiguity.
   Edit the prompt live to add a rule, re-run, show the difference.
4. Run **Test 3** → confirm the refusal contract.
5. Close with the ablation table from the main file: which sections, when
   removed, cause which classes of failure.

---

## Lightweight Python harness for batch evaluation

If students want to run this at scale on the synthetic test set:

```python
import json
from pathlib import Path
from anthropic import Anthropic

client = Anthropic()
SYSTEM_PROMPT = Path("comprehensive_prompt.txt").read_text()

def extract(abstract: str, pmid: str | None = None) -> dict:
    user_msg = (f"<pmid>{pmid}</pmid>\n" if pmid else "") + f"<abstract>\n{abstract}\n</abstract>"
    resp = client.messages.create(
        model="claude-opus-4-7",   # check current model id in the docs
        max_tokens=2000,
        system=SYSTEM_PROMPT,
        messages=[{"role": "user", "content": user_msg}],
    )
    raw = resp.content[0].text.strip()
    # The prompt forbids markdown fences, but be defensive in a teaching setting:
    if raw.startswith("```"):
        raw = raw.strip("`").lstrip("json").strip()
    return json.loads(raw)

# Validate against a Pydantic model in the next exercise.
```

A natural follow-up exercise: define the `pydantic` model that mirrors the
schema, run the extractor over a directory of abstracts, log every
`ValidationError`, and have students fix the prompt until the error rate
drops below a target threshold. That is prompt engineering as engineering.
