# interface.ai Knowledge Hackathon: audited candidate submission

This folder contains only deliverable source files, **not** the provided website snapshots or evaluation data.

## Running in the provided participant kit

Copy this entire `submission/` folder into the participant kit root (where `harness/`, `eval/`, and `snapshots_zip/` are present). From that root:

```bash
python harness/run_eval.py --submission submission --eval eval/public_eval.jsonl
```

Run the official bundled miniature self-test, which includes a sample conflict gold file:

```bash
python harness/run_eval.py \
  --submission submission \
  --snapshots harness/selftest/snapshots \
  --eval harness/selftest/eval.jsonl \
  --conflicts-gold harness/selftest/conflicts_gold.json
```

The code can also be copied independently into another folder: it does not require `harness/` to be a sibling. However, the actual grader supplies its `contract.py` when invoking the engine.

## Delivering via GitHub

Push the **source files only** (`solution.py`, `requirements.txt`, `NOTES.md`, optionally this README) to a private repository and grant the organizers read access. Do not commit `snapshots_zip/`, extracted `snapshots/`, or any full dataset copies. The assignment explicitly prohibits redistributing the event-only data. If the entire participant kit is the Git repository, ensure the root `.gitignore` excludes both `snapshots_zip/` and `snapshots/` and verify staged files with `git diff --cached --name-only` before committing.

## Recorded local validation

See `TEST_RESULTS.json` and `NOTES.md`. With the public offline scorer, the best achievable score is 0.968992... because one practice item has no required tokens and is not a refusal question. The real hidden benchmark and conflict labels are unavailable. A high public score must not be represented as a guarantee of hidden-evaluation performance.


## Extra variation tests

See the separately distributed `interface_ai_variation_tests_336.zip`. Unpack it anywhere and run its `run_variant_tests.py` against the challenge kit and your submission folder. It exercises 336 cases independent of the public evaluation file. These are local development tests; the organizers' private gold labels are not available.
