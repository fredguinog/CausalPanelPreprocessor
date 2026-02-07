# CausalPanelPreprocessor

![GitHub license](https://img.shields.io/github/license/fredericonogueira/PanelDataPreprocessor)
![PyPI version](https://img.shields.io/pypi/v/panel-preprocess)
![GitHub stars](https://img.shields.io/github/stars/fredericonogueira/PanelDataPreprocessor?style=social)

This repository implements a preprocessing framework for causal panel data analysis, as motivated in my manuscript, "Irrelevant Donors: Causes and Consequences of Spurious Pre-Treatment Fits in Causal Panel Methods." In the manuscript, I formalize the challenges arising from irrelevant donors in panel data methods for causal inference, such as synthetic controls and interactive fixed effects. These methods construct counterfactuals using linear combinations of control units, relying on relevance (the treated unit's latent structure residing within the donor span) and conditioning (stable Gram matrices for recovery). I introduce the contamination ratio κ, defined as the effective rank of irrelevant donors divided by the pre-treatment periods, which attenuates projection-based diagnostics proportionally. For instance, a κ of 0.5 masks 50% of relevance violations. The analysis reveals a phase transition at κ approaching unity, where residuals vanish, yielding spurious fits despite fundamental identification failures. I distinguish unstructured irrelevance, with rank approximating the donor count, from structured cases tied to confounding dimensions.

A unified taxonomy classifies failures into refinable (conditioning lapses, addressable via extended periods or regularization) and irreducible (relevance violations inducing bias). These results emphasize prioritizing ex-ante donor curation over ex-post selection and motivate an alternative diagnostic framework in a companion work, which this repository operationalizes using GBDTs to avoid circularity in residual-based approaches.

## Overview

This package serves as a preprocessing step for causal panel data methods, addressing the identification primitives of relevance and conditioning. It utilizes GBDTs to evaluate donor alignment, removing irrelevant units to ensure the treated loading lies within the donor span, and applies effective sample size weights to mitigate variance inflation from weak conditioning. This approach is derived from the alternative diagnostic framework referenced in the manuscript, enabling practitioners to prepare datasets for estimators like synthetic controls or difference-in-differences without estimating contamination ratios (κ), projection weights, or factor dimensions.

The tool is designed for econometricians and researchers analyzing policy impacts, economic shocks, or similar interventions using panel data. By curating donors ex-ante, it enhances the reliability of subsequent causal estimates.

## Theoretical Foundation

Traditional projection-based diagnostics, such as pre-treatment fit residuals, degrade under donor pool contamination. Geometric saturation via κ masks violations, leading to spurious robustness. This package's GBDT-based method circumvents these issues:

- **Relevance Assessment**: A binary classifier ranks the donors by relevance, separating the treated unit from the donors using their pre-treatment trajectories. Irrelevant donors are pruned to avoid irreducible bias.
- **Conditioning Enhancement**: Post-pruning, the data is optionally transformed and adjusted using unit- and time-effective sample size weights to address donor collinearity and sufficient variation in factors, respectively. Improvement is verified by a reduction in the Gram matrix condition number (ratio of largest to smallest eigenvalue of X'X / T_0).
- **Advantages**: It reestablishes adequate relevance and conditioning, enabling a reliable counterfactual.

For detailed motivation and proofs, refer to the manuscript.

## Reference

The methodology implemented in this package is described in the following articles. If you use this software in your research, please cite it.

> Nogueira, F. (2026a). Irrelevant Donors: Causes and Consequences of Spurious Pre-Treatment Fits in Causal Panel Methods. [Nogueira 2026a Irrelevant Donors](https://github.com/fredguinog/CausalPanelPreprocessor/blob/main/Nogueira%202026a%20Irrelevant%20Donors.pdf)

> (In development) Nogueira, F. (2026b). Companion Manuscript Part2.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
