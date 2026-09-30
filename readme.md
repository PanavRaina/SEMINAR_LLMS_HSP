# Does Garden-Path Underestimation by LLM Surprisal Scale with Model Size?

Code for the final project of the seminar *LLMs as models of human sentence processing* (SoSe 2026).

**Author:** Panav Raina

---

## Main findings

| Measure (vs. 10x more parameters) | Estimate | 95% bootstrap CI |
|---|---|---|
| Natural Stories fit (held-out ΔLL/obs, % of 70M fit) | **-15.0%** | [-27.5, -3.9] |
| Garden-path gap, human − predicted (ms, ROI 0) | **+0.26 ms** | [-0.26, 0.80] |
| Garden-path predicted effect (ms, ROI 0) | -0.26 ms | [-0.80, 0.30] |

- The Natural Stories fit declines with model size whereas the garden-path gap shows no detectable change.
- Spearman correlation between the two scaling curves (n = 6 sizes): ρ = -0.43, p = 0.40 (low power).
- Oh et al.'s frequency account does not explain the gap. The disambiguating words are rare (about the 24th to 25th frequency percentile), but excess confidence does not predict the gap (r from -0.08 to 0.12, all permutation p > 0.3).

---


## Requirements

- Python 3.10+
- A CUDA GPU. The analysis was run on a Google Colab **T4** GPU. All models are run in **fp16**; the 6.9B model is the most memory-hungry.
- Packages:

```bash
pip install torch transformers accelerate pandas numpy scipy statsmodels matplotlib tqdm gdown
```



Model weights are downloaded automatically from the Hugging Face Hub (`EleutherAI/pythia-*-deduped`).

---

## Data

Most of the data is downloaded automatically except for the two SAP files, that are not downloaded automatically, because downloading the Google Drive folder from a script is unreliable (the `gdown` line in the notebook is commented out).

### Manual download of the SAP files

1. Run the first code cell of the notebook (imports and configuration). This creates the folders `data/sap/`, `data/naturalstories/` and `results/`.
2. Open the Drive folder: <https://drive.google.com/drive/folders/1g-oyH-XuB2oolo1d8KZfuFtiimuNyhjc>
3. Download these **two files**:
   - `ClassicGardenPathSet.csv`
   - `Fillers.csv`
4. Move them into the `data/sap/` folder.
5. Continue with the remaining cells.

---

## How to run

1. Run the first code cell (imports, paths, `MODELS`, `N_BOOT`, `N_PERM`).
2. Place the two SAP files in `data/sap/` .
3. Run the remaining cellS.
   

---


## Output files (`results/`)

| File | Contents |
|---|---|
| `surp_<model>_fp16.csv` | Word-level surprisal (bits) for garden-path, filler and Natural Stories text |
| `gp_item_effects_all_models.csv` | Per-item human effect, predicted effect and bit difference, per model and ROI |
| `natural_stories_heldout_summary.csv` | Mean ΔLL/obs and its SE per model |
| `natural_stories_per_story.csv` | ΔLL/obs per held-out story and model |
| `excess_confidence.csv` | Excess-confidence correlation and permutation p-value per model |
| `fig2_mechanism.png` | Item-level tracking and excess-confidence test |
| `fig4_pred_vs_human.png` | Predicted vs human effect per construction and model size |
| `fig3_ns_vs_gp_comparison.png` | Scaling of both phenomena on one axis (used in the report) |

---

## How to read the output

- **Slope lines:** the Natural Stories slope is given as % of the 70M fit per 10x parameters. Garden-path slopes are in ms per 10x parameters. A CI that includes 0 means no detectable change with size.
- **Excess-confidence table:** `mean_excess_bits` is negative when the model is *less* confident than frequency predicts. `r` is the correlation with the gap after removing construction means, and `p_perm` is the permutation p-value.
- **`fig4_pred_vs_human.png`:** the solid lines (predicted effects) are far below the dashed lines (human effects) at every size. The gap is therefore close to the whole human effect, so a flat gap partly reflects a near-zero prediction.

---

## References

- Arehalli, Dillon & Linzen (2022). Syntactic surprisal from neural models predicts, but underestimates, human processing difficulty from syntactic ambiguities. *CoNLL*.
- Biderman et al. (2023). Pythia: A suite for analyzing large language models across training and scaling. *ICML*.
- Futrell et al. (2021). The Natural Stories corpus. *Language Resources and Evaluation*, 55.
- Huang et al. (2024). Large-scale benchmark yields no evidence that language model surprisal explains syntactic disambiguation difficulty. *Journal of Memory and Language*, 137.
- Oh & Schuler (2023). Why does surprisal from larger Transformer-based language models provide a poorer fit to human reading times? *TACL*, 11.
- Oh, Yue & Schuler (2024). Frequency explains the inverse correlation of large language models' size, training data amount, and surprisal's fit to reading times. *EACL*.
- Yoshida et al. (2026). An existence proof for neural language models that can explain garden-path effects via surprisal. *ACL*.

---

## AI use

Generative AI was used to check grammar, find relevant sources and for formatting this LATEX document. For the programming part, I used generative AI to look up syntax, debug errors and to generate graphs. All scientific arguments and final conclusions are my own.
