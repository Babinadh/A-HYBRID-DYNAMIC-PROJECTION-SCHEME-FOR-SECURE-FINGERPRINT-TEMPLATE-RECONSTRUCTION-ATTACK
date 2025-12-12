# A-HYBRID-DYNAMIC-PROJECTION-SCHEME-FOR-SECURE-FINGERPRINT-TEMPLATE-RECONSTRUCTION-ATTACK
A HYBRID DYNAMIC PROJECTION SCHEME

Secure Fingerprint Template Reconstruction Attack — GitHub README / Description

Short tagline: A hybrid dynamic projection method to harden fingerprint templates against reconstruction attacks while preserving recognition accuracy.

Table of contents

Overview

Key ideas

Features

Repository structure

Installation

Quick start / usage

Method (high-level)

Evaluation & experiments

Security considerations

How to cite

License & contact

Overview

This repository contains the implementation and experiments for "A Hybrid Dynamic Projection Scheme for Secure Fingerprint Template Reconstruction Attack" — a method that defends fingerprint recognition systems from template reconstruction attacks by transforming minutiae-based templates using a hybrid projection mechanism that is both dynamic (per-instance randomness) and invertibility-resistant, while maintaining recognition performance.

The goal: provide a practical, reproducible toolkit researchers and engineers can use to (1) reproduce results, (2) evaluate defense robustness under reconstruction attacks, and (3) integrate the protection into biometric pipelines.

Key ideas

Hybrid projection: Combine two complementary projection transforms (e.g., learned projection + random orthonormal projection) to make inversion difficult.

Dynamic per-sample transformation: Use per-enrollment randomness (salt) so that templates leaked from one system cannot be used elsewhere.

Non-linear obfuscation layer: Add a lightweight non-linear step to break linear reconstruction techniques while preserving discriminative structure.

Secure key management: Demonstrate how to derive per-user keys from secure seeds and store only key-derived helper data.

Evaluation against reconstruction attacks: Compare reconstructed ridge maps / minutiae from attacked templates to originals and show lowered attack success.

Features

Reference implementation of the hybrid dynamic projection pipeline.

End-to-end enrollment → protected template → matching.

Multiple attacker models included:

Inversion via linear regression / dictionary methods.

Deep-learning based reconstruction.

Scripts for evaluating recognition accuracy (FAR, FRR, ROC) and reconstruction quality (MSE, SSIM, visual comparisons).

Reproducible experiments with configuration files.

Jupyter notebooks for visualization and step-by-step demos.

Repository structure (suggested)
.
├── README.md                    # (this file)
├── LICENSE
├── data/
│   ├── README.md                # dataset notes (placeholders only — not the data)
│   └── sample/                  # small sample templates for demo (synthetic)
├── src/
│   ├── protections/
│   │   ├── hybrid_projection.py
│   │   ├── key_derivation.py
│   │   └── non_linear_obfuscation.py
│   ├── attack/
│   │   ├── linear_inversion.py
│   │   └── dl_reconstruction.py
│   ├── matching/
│   │   └── matcher.py
│   └── utils/
│       ├── preprocess.py
│       └── metrics.py
├── notebooks/
│   ├── demo_protection.ipynb
│   └── attack_vs_defense.ipynb
├── experiments/
│   ├── configs/
│   └── results/
├── requirements.txt
└── scripts/
    ├── enroll.py
    ├── protect_and_store.py
    ├── match.py
    └── run_experiment.sh

Installation

Clone the repo:

git clone https://github.com/<your-org>/hybrid-dynamic-projection-fingerprint.git
cd hybrid-dynamic-projection-fingerprint


Create a Python environment and install:

python -m venv venv
source venv/bin/activate      # Linux / macOS
# .\venv\Scripts\activate     # Windows PowerShell

pip install --upgrade pip
pip install -r requirements.txt


requirements.txt should include packages like numpy, scipy, scikit-learn, torch (if deep nets used), opencv-python, matplotlib, pandas.

Note: No real fingerprint datasets are bundled. See data/README.md for guidance on publicly available datasets and how to prepare them.

Quick start / Usage

Generate demo protected templates (uses included synthetic samples):

python scripts/enroll.py --input data/sample/minutiae/ --out protected_templates/


Match two protected templates:

python scripts/match.py --probe protected_templates/user1.pt --gallery protected_templates/user2.pt


Run an attack vs defense experiment:

bash scripts/run_experiment.sh experiments/configs/demo.yaml


Visual demo notebook: Open notebooks/demo_protection.ipynb to see step-by-step protection and reconstruction visualizations.

Method — High-level description

Minutiae extraction / preprocessing: Convert fingerprint images to standardized minutiae templates (x, y, θ, quality).

Hybrid projection:

Apply learned projection P_L (e.g., PCA or small neural encoder) to map minutiae features into a compact subspace.

Apply a randomized orthonormal projection P_R derived from a per-enrollment salt/key.

Concatenate or combine the two projected vectors and pass to non-linear obfuscation.

Non-linear obfuscation: Apply lightweight non-linear mapping (e.g., element-wise quantization + randomized rounding or small invertibility-breaking transform).

Template storage: Store only the protected template and the public salt/metadata. Keys remain secret or stored in secure keystore.

Matching: At authentication, reapply same steps (using key/seed) and compute distance in the protected space.

Attack model for evaluation: Attempt to reconstruct original minutiae or ridge maps from protected templates. Measure success by reconstruction similarity and whether reconstructed images can be matched against originals.

Evaluation & experiments

Metrics provided:

Recognition: FAR, FRR, EER, ROC curves.

Reconstruction: Mean Squared Error (MSE), Structural Similarity Index (SSIM), and attack success rate (can reconstructed prints be matched to original under a standard matcher?).

Baselines:

Unprotected templates (no defense)

Single projection defenses (learned only / random only)

Expected outcomes (reported in paper/experiments):

Comparable recognition performance (small drop in accuracy)

Significant reduction in reconstruction quality and attack success vs baselines

Example command to run a full benchmark:

python src/experiments/run_benchmark.py --config experiments/configs/benchmark_fvc.yaml

Security considerations

The scheme improves resistance to inversion/reconstruction attacks but is not a silver bullet.

Proper key management is crucial: if per-user keys/seeds leak, the defense degrades.

Differential attacks (multiple observed protected templates for same user) must be considered — the dynamic per-enrollment randomness mitigates but does not fully eliminate multi-instance linkage risk.

The repo contains scripts to simulate attacker knowledge levels:

White-box attacker: full knowledge of projection transforms (no keys)

Black-box attacker: only protected templates, attempts to learn inversion via data-driven models

Always follow legal and ethical guidelines when working with biometric data. Use only consented/public datasets and follow your institution's IRB if necessary.

Reproducibility

experiments/configs/*.yaml store experiment hyperparameters (projection dims, non-linear parameters, seeds).

Each experiment logs random seeds, software versions and result artifacts under experiments/results/<timestamp>/.

For publication, include requirements.txt, environment.yml and a Dockerfile if desired.

How to cite

If you use this code in your research, please cite the associated paper (replace with actual citation when available):

@inproceedings{yourname2025hybrid,
  title={A Hybrid Dynamic Projection Scheme for Secure Fingerprint Template Reconstruction Attack},
  author={Your Name and Collaborators},
  booktitle={Proceedings of XXX},
  year={2025}
}

License

This project is released under the MIT License — see LICENSE for details. (Change to your preferred license.)

Contact / Contribution

Author: Your Name — email: your.email@example.com

Contributions welcome: open an issue or a pull request. Please include tests and update notebooks when adding features.
