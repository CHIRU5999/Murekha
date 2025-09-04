# murekha: XML + DTD validation demo (Song Generation Domain)

This repository contains a minimal DTD and example XMLs for the **murekha** song-generation domain, plus a Colab notebook that validates both a passing and a failing file.

## Files
- `domain.dtd` — core constraints for generated songs
- `sample.xml` — **valid** XML
- `broken.xml` — **invalid** XML (teachable failures)
- `validate.ipynb` — Colab notebook that validates both XMLs

---

## Framework

**Decision — What must be answered/acted?**  
Should a newly generated **murekha** track be **published** to the catalog (vs. kept as draft or sent back for edits)?

**Signals — (5–7) concrete, decision-relevant observations**  
1. **Duration (s)** — reject < 30s or > 10 min; target 120–240 s (freshness: per-track).  
2. **Tempo (BPM)** — must match requested style window (e.g., lofi 70–110 BPM).  
3. **Integrated Loudness (LUFS)** — mix/master target around −14 LUFS (streaming-safe).  
4. **Explicit (yes/no)** — content-safety flag must be `no` for general catalog.  
5. **Model version** — must be ≥ current approved baseline (e.g., `0.9.3`).  
6. **Genre coverage** — at least one labeled genre; no “unknown”.  
7. **Created date** — ISO date present; not in the future.

**Gaps — What’s missing that would change confidence?**  
- Automated **audio QC** (clipping, DC offset, silence %).  
- **Style-consistency score** from a classifier.  
- **Human listening notes** for edge cases (e.g., artifacts).

**Risk — Harm if wrong or stale**  
- Publishing tracks with offensive content (reputational/legal).  
- Listener churn from poor loudness/quality.  
- Backfill regressions if **model version** policy is stale.

**Stewardship — Who can see/keep it, and for how long?**  
- Catalog metadata (this XML) visible to internal review + public when `status=published`.  
- Raw audio stored per policy (e.g., 12 months), with access limited to the audio team.  
- Safety signals retained for audit for 18 months.

**Data Domain**  
- **Primary:** [Music Generation]  
- **Adjacent:** [Content Safety], [Licensing]

---

## How to run validation (Colab)
Open `validate.ipynb` in Colab and run all cells. Expected: `sample.xml` **VALID**, `broken.xml` **NOT valid** with error log.

## Why `broken.xml` fails (one sentence)
**Violation:** `song` requires `genre+` and fixed child order; file omits `genre` and places `<model>` before `<loudness>`.

---

*AI use:* Portions of this README and XML/DTD scaffolding were drafted with assistance from ChatGPT.
