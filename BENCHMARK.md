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

## Results (2026-11-06/07, development build before 0.3.4, model: Anthropic Claude Haiku, one run per case unless stated)
"First attempt" means the crew's first plan passed every check; "after repairs" counts the crew's own repair rounds.

| Set | Cases | First attempt | After repairs |
|---|---|---|---|
| Hard change cases on the sample projects, 3 repeats (as shipped, critics on) | 10 x 3 | 12 of 30 | 27 of 30 |
| Complex workspace requests: changes wanted | 9 | 2 of 9 | 4 of 9 |
| Complex workspace requests: should ask, refuse or explain | 6 | - | 3 of 6 |
| Real requirement texts: changes wanted | 3 | 0 of 3 | 1 of 3 |

In these 95 case runs no wrong change was offered as ready: a case that failed was stopped by a check, or the crew
asked instead. These figures were not run again on a later build. Earlier figures (2026-11-02, 39 workspace cases
and 9 Decision Center cases) counted only results after repairs and are no longer shown.

## What the numbers mean
- Results vary between runs because the AI model varies; the checks do not. A change that fails a check is not
  proposed, so that kind of miss costs time. No check catches a policy misread the same way in the rule and in its
  examples: that passes every gate, which is why a person reads every change. "Proven" means the checks that ran
  passed on the examples and known decisions available; it does not cover inputs nobody tried.
- These are sample projects. Your rules will raise cases the benchmark does not have - tell us about them.
