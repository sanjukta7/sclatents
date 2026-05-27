# evae — Project Summary

**One-liner:** A controlled study of latent-based generative models for single-cell perturbation prediction, showing that *output head choice dominates prior architecture* in determining latent space quality — achieving SOTA distribution metrics and near-scGPT biology performance with 1000× less pretraining data.

---

## Approach

**Model:** Vector-quantized VAE (`ExpressionVAE`) that encodes scRNA-seq gene expression into discrete latent codes, with a separately trained generative prior over those codes to predict post-perturbation cell states.

**Two design axes studied:**
- **Prior**: autoregressive, masked diffusion (MDLM), or flow matching — over discrete or continuous latent spaces
- **Output head**: cross-entropy/quantile (`ce-quantile`), hurdle, MSE, or negative binomial (`nb`) — treated as the "output tokenization" decision analogous to LLM design

**Key finding:** The output head, not the prior, controls how informative the latent space is. This is the central claim of the NeurIPS paper.

---

## Datasets

| Dataset | Cells | Perturbations | Use |
|---------|-------|---------------|-----|
| Parse 1M (PBMCs) | ~1M | 90 cytokines, 12 donors | Main benchmark |
| Replogle (HepG2/Jurkat/RPE1) | — | 372–1000+ CRISPR KOs | Main benchmark |
| Wholebrain CRISPR atlas (Shi et al. 2026) | 6.36M | ~2600 in-vivo | Case study |
| Pfizer IMRU (OOD) | 864k | 1732 CRISPRi targets | Biology/drug target eval |

---

## Results

### Distribution metrics (vs scLDM ω=1 baseline)

| Metric | evae best | scLDM | Improvement |
|--------|-----------|-------|-------------|
| Parse W2 ↓ | 11.89 (flow-mse) | 12.46 | ~5% |
| Parse MMD² ↓ | 0.0041 (AR-ce-quantile) | 0.027 | **6.6×** |
| Parse FD ↓ | 1.84 (AR-ce-quantile) | 18.14 | **9.9×** |
| Replogle W2 ↓ | 7.87 (AR-ce-quantile) | 11.29 | ~30% |
| Replogle MMD² ↓ | 0.0087 (AR-ce-quantile) | 0.20 | **23×** |

### Biology / OOD (Pfizer NF-κB reversion task)

| Model | Enrichment AUC | Selectivity AUC |
|-------|---------------|-----------------|
| Parse-trained fsq-hurdle VAE | **0.792** | 0.750 |
| Replogle-trained gaus-hurdle VAE | 0.758 | — |
| scGPT (reference) | ~0.80 | — |

Near-parity with scGPT on biology tasks using 1000× less pretraining data. Top-5 reversion targets correctly include RELA and RBCK1 (canonical NF-κB activators).

---

## Experiments

- **~96 final training runs**: 12 prior×head combinations × 4 seeds × 2 datasets (Parse 1M, Replogle HepG2)
- **3 quantizers**: FSQ, Gaussian (continuous), VQ
- **4 output heads**: ce-quantile, hurdle, MSE, negative binomial
- **3 priors**: autoregressive, MDLM (masked diffusion), flow matching
- Configs in `config/final/`; checkpoints in `outputs/experiments-final/`

---

## Key Scripts

| Script | Purpose |
|--------|---------|
| `scripts/train.py` | VAE training (Accelerate + W&B) |
| `scripts/eval.py` | Full eval suite (W2, MMD², FD, PR-AUC, Pearson, DEGs, etc.) |
| `scripts/eval_pfizer.py` | OOD biology / drug-target enrichment AUC |
| `scripts/embed_hf.py` | Encode cells via HF checkpoints |
| `scripts/final_paper_plots.py` | NeurIPS figure generation |
| `scripts/run_embed_and_auc.sh` | End-to-end embed + AUC pipeline |
