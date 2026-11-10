# Benchmark: how accurate are RuleCrew's rule changes?

RuleCrew is measured on fixed sets of real change requests, run against IBM's public ODM sample projects (Miniloan,
Loan Validation). Every case is checked on ODM's own engine, not by reading the text the AI wrote.

## How a case is scored
A case passes only when all of these hold:
- the change touches the files the request is about, and only the projects it names;
- it builds with ODM's own build command;
- its example decisions run on ODM's engine and give the outcome the request asks for, including the boundaries
  (for example "up to 100000" includes 100000);
- the case's own boundary inputs, written by a person, fire or do not fire as stated;
- where the request is ambiguous, RuleCrew asks instead of guessing, and changes nothing.

A failing case is never hidden: the run's report lists every check that failed.

## Results (development build after RuleCrew 0.3.1, 2026-11-02, model: Anthropic Claude Haiku)
| Set | Cases | Passed | Typical time per case |
|---|---|---|---|
| Rule workspace / Git folder: thresholds, new rules, tiers and bands, decision tables, ruleflows (new tasks, tasks between others, conditional branches, removed tasks), BOM attributes, several changes in one request, questions about decisions | 39 | 35 of 39 in the latest runs (varies between runs; the 4 harder ruleflow cases are new in this build) | 1-5 min |
| Decision Center, complex set (one planner path): grouped decision tables, 'is one of' and 'otherwise', decimal ratios, ruleflow actions and transitions, cross-project variables, object-model hand-off | 9 | 7 of 9 in the latest run (5-7 of 9 across runs) | about 2-7 min |

RuleCrew 0.3.1 passed 35 of 35 cases of the earlier workspace set.

These figures are from that development build; they were not run again on 0.3.4. They count a case as passed after
the crew's own repair rounds. First-attempt results were not recorded for these two sets. On a separate, earlier set
of 10 hard rule-change cases on a small model, 4 to 6 of 10 passed on the first attempt and 8 to 9 of 10 after repair.

## What the numbers mean
- Results vary between runs because the AI model varies; the checks do not. A change that fails a check is not
  proposed, so that kind of miss costs time. No check catches a policy misread the same way in the rule and in its
  examples: that passes every gate, which is why a person reads every change. "Proven" means the checks that ran
  passed on the examples and known decisions available; it does not cover inputs nobody tried.
- These are sample projects. Your rules will raise cases the benchmark does not have - tell us about them.
