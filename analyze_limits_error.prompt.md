---
name: analyze-limits-error
about: "Investigate why `get_limits` in peptide.py sometimes prints 'error getting limits - exiting' and crashes on large analyses."
description: |
  This prompt helps the assistant perform a full-codebase analysis to identify possible causes for the `get_limits` failure
  in `synthedia/peptide.py`. The issue manifests only during long (>1h) runs, making it hard to replicate. The failure occurs
  when the try/except in `get_limits` catches an exception while computing `lower_limit`, `higher_limit`, or `indicies`.
  
  The assistant should:
  1. Search the repository for all references to `get_limits` and `ms_clip_window` to see how inputs are generated.
  2. Examine any related modules that build `mzs` arrays or modify options that influence the clipping window.
  3. Reason about scenarios where `mz_mask` could be empty or invalid (e.g. no spectra in the clip window, NaNs, wrong types).
  4. Identify indirect causes such as options being mutated during long runs, numerical drift, or memory issues.
  5. Suggest additional logging, validation, or refactoring to catch the problem earlier and to aid debugging.
  
  Output should include a summary of suspicious code paths, hypotheses for the crash, and concrete next steps for
  acquiring more information during long simulations.

tags:
  - debugging
  - search
  - repository-analysis
  - peptide
arguments: []
usage: |
  "Help me analyse the codebase for potential causes of the get_limits crash"
  or simply invoke the prompt name from Copilot Chat.
---

# Instructions for the assistant

You're a repository-wide analysis agent. Treat the workspace as a Python project and scan all files.
Focus on the `get_limits` method in `synthedia/peptide.py` and any places that call it. Look for variables
passed in (`options`, `mzs`, `groupi`, `samplei`) and how they are produced. Consider how long-running
analyses might affect these values (e.g., accumulating NaNs, exhausting memory, mutated options).

Enumerate any other functions or modules that manipulate `mzs` arrays or clip windows. Explain why the
`AttributeError` could be raised (most likely because `mz_mask[0]` is empty causing `.min()` or `.max()`
to fail) and what upstream conditions would create that situation.

Provide a detailed response listing suspicious code segments, potential root causes, and actionable advice
for gaining insight during extended runs (e.g., extra debug prints, early validation, unit tests with
edge cases).
