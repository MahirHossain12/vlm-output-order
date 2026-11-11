When Transcription Comes First
A Paired Study of Output Order in Visual Prompt Injection
Anonymous code and reproducibility materials for double-blind review.
What this study tests
Does asking a vision-language model to transcribe text before answering a question make it more likely to adopt a wrong answer from the image? Each trial combines a photograph with a separate, untrusted text panel. The model is asked to answer the scene question in ANSWER and copy the panel in TRANSCRIPT. Within each pair, the image, question, wrong target, and reminder setting stay fixed. The prompt changes which field comes first.

The main design crosses three panel types (instruction, assertion, neutral mention), two requested field orders, and two trust-reminder settings, giving 12 main trials per object. Target adoption, scene-answer correctness, exact transcription, and format compliance are scored separately. Clean answer-only and benign scene-reading trials check whether the model can perform the underlying tasks.

The primary contrast is the transcribe-first minus answer-first difference in wrong-target adoption, averaged across instruction and assertion panels with the trust reminder enabled. The analysis resamples whole objects to preserve the pairing. Unresolved labels contribute bounds, which are reported separately from the 95% bootstrap envelope.
Reproduce the held-out analysis on CPU
This workflow starts from saved responses and reviewed labels. It requires Python 3, NumPy, and Matplotlib. It runs without a GPU, model weights, or inference calls.

From the repository root, with the frozen fresh-review export available under artifacts/rpvc13/:
python3 -m venv .venv
source .venv/bin/activate
python -m pip install numpy matplotlib

python analysis/RPVC13_Reproduce_Reconciled_Results.py \
  artifacts/rpvc13/fresh_review_export.txt \
  --output reproduced/rpvc13
The export may be renamed to fresh_review_export.txt for this layout. The script checks its SHA-256 and requires the original bytes. After a successful run, reproduced/rpvc13/ contains:
Output	Contents
RPVC13_Reconciled_Analysis.json	Effect bounds, bootstrap envelopes, and utility decisions for the rating analyses
RPVC13_Reconciled_Figure_Data.csv	Values for the effect, utility, and condition-level adoption displays
RPVC13_Reconciled_Tables.tex	Reconciled results and utility tables
RPVC13_Reconciled_Effects.pdf and .png	Figure comparing the paired effects and utility checks


Expected check. In the reconciled analysis, the primary effect bounds are [33.75, 40.00] percentage points, with a [22.50, 50.00] bootstrap envelope. The answer-first reading utility decision is INCONCLUSIVE.

If the release includes aggregate JSON and CSV only, those files support inspection of the reported values. Recomputing the bounds from individual judgments requires the frozen export.
Rerun generation
The generation workflow uses numbered Colab notebooks or cell scripts and a CUDA runtime. The primary runs use Qwen3-VL-2B-Instruct. Model revisions, package versions, and generation settings come from the run’s frozen protocol and environment checks.
1. Make the source inputs, frozen manifests, and any required parent run available at the paths expected by the notebook. Use a separate output directory for a fresh run.
2. Run the environment and model preflight before generation. Follow the numbered cells and preserve the saved checkpoints.
3. Export responses for scoring. Apply the run’s recorded rubric, then compute paired contrasts and check utility and transcription against the frozen thresholds.
Some workflows expect the original Colab directory structure and validated parent artifacts. For example, the token-matched wording control reuses the frozen held-out inputs. Full generation requires those inputs and the corresponding notebook dependencies.
Run guide
Run	Purpose
09	Development replication on 20 object sources. The primary contrast was +27.5 pp with a [17.5, 37.5] bootstrap envelope, and the utility gate passed.
13	Held-out evaluation on 40 object sources. All 480 main responses had exact extracted transcripts. One missing answer-first reading response left utility qualification inconclusive. The analysis retains the planned denominator.
14	Token-matched wording control on the same 40 objects. The order effect was +45 pp under both wordings. Two neutral-mention conditions missed the transcription threshold.
08, 10–11, 15–17	Prefix and cross-model diagnostics, including the Phi stopping audit and corrected qualification screen. These runs document the limits of the tested protocols.


The main empirical claim concerns the tested Qwen configuration and the requested field order. The held-out and matched-wording runs add evidence with their own qualification outcomes. Cross-family generalization remains open.
Release layout
Path	Contents
analysis/RPVC13_Reproduce_Reconciled_Results.py	CPU reproduction entry point
artifacts/rpvc13/	Review-safe held-out results and, when approved for release, the byte-identical review export
notebooks/	Numbered experiment, preflight, generation, and scoring notebooks or cell scripts


Source images and reading examples retain the terms and attribution of their original datasets. Use the source manifests to identify inputs and include redistributable materials under their applicable terms.
