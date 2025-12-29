# Data-Invariant Beamforming and Multichannel ASR

This repository collects code and training scripts used in the paper [**"Data-Invariant Beamforming for End-to-End Multichannel Multi-Speaker ASR"**](https://arxiv.org/pdf/2509.10234). The work proposes a beamforming method that requires no data-driven training and integrates seamlessly with end-to-end ASR models for multi-speaker, multi-microphone scenarios.

---

## 📂 Repository Structure

This repository and its companion repositories are organized into two main components:

### 1. 🌀 Data-Invariant Beamformer

The beamformer extracts signals from predefined angular sectors without requiring microphone geometry calibration or training data.

- **Steering Vector Generation**  
  Path: [`egs/DAS/data_invariant.py`](https://github.com/can-cui/asteroid-related/blob/main/egs/DAS/data_invariant.py)  
  This script performs numerical integration over the 3D space to compute the `a` and `c` vectors defined in Equation (5) of the paper:  
`a = ∫Ω cos(θ) · d(θ, φ, f) · d(θ, φ, f)ᴴ dθ dφ`  
`c = ∫Ψₛ cos(θ) · d(θ, φ, f) dθ dφ`  
These quantities are then stored in a `.npy` file for efficient reuse.



- **Beamformer Response Generation**  
  Path: [`egs/DAS/data_invariant_response.py`](https://github.com/can-cui/asteroid-related/blob/main/egs/DAS/data_invariant_response.py)  
  Given the number of raw microphone channels and the desired number of angular sectors, this script generates a response `.npy` file.  
  This response can then be **applied directly to raw microphone array signals** to extract beamformed signals from each angular sector.

---

### 2. 🗣️ ASR Model Training

The ASR model is based on a multichannel Conformer architecture implemented in [SpeechBrain](https://github.com/speechbrain/speechbrain). The beamformed input is used as part of a multichannel-angle-aware front-end.

- **🧪 Pre-training on LibriSpeech (with beamformed input)**  
  Config:  
  [`conformer_large_MC_angle.yaml`](https://github.com/can-cui/speechbrain-related/blob/main/recipes/LibriSpeech/ASR/transformer/hparams/conformer_large_MC_angle.yaml)  
  Uses the above-generated response `.npy` file as input to the `response` parameter.

- **🧪 Fine-tuning on AMI**  
  Config:  
  [`conformer_large_MC_angle.yaml`](https://github.com/can-cui/speechbrain-related/blob/main/recipes/AMI/ASR/transformer/hparams/conformer_large_MC_angle.yaml)

---

## 📊 Baseline Configurations

The following training recipes are used for comparison in the paper:

### A. **Raw Multichannel Input (No Beamforming)**

- **Pre-training on LibriSpeech**:  
  [`conformer_large_MC.yaml`](https://github.com/can-cui/speechbrain-related/blob/main/recipes/LibriSpeech/ASR/transformer/hparams/conformer_large_MC.yaml)

- **Fine-tuning on AMI**:  
  [`conformer_large_MC.yaml`](https://github.com/can-cui/speechbrain-related/blob/main/recipes/AMI/ASR/transformer/hparams/conformer_large_MC.yaml)

### B. **MVDR + Single-Channel ASR**

- **Fine-tuning on AMI (MVDR input)**:  
  [`conformer_large_MVDR_noWPE.yaml`](https://github.com/can-cui/speechbrain-related/blob/main/recipes/AMI/ASR/transformer/hparams/conformer_large_MVDR_noWPE.yaml)

---

## 🎧 Visualization

To visually compare responses of different beamformers, use:

- [`data_invariant_fr_file.ipynb`](https://github.com/can-cui/asteroid-related/blob/main/egs/DAS/data_invariant_fr_file.ipynb)  
  This notebook plots responses

To visually compare audio quality (e.g., denoised vs. raw signals), use:

- [`Visualize_data_inv_beamformed.ipynb`](https://github.com/can-cui/asteroid-related/blob/main/egs/DAS/Visualize_data_inv_beamformed.ipynb)  
  This notebook plots spectrograms for qualitative comparison acr
  
---

## 📌 Citation

If you find this work helpful, please consider citing the original paper.

```bibtex
@inproceedings{cui2025data,
  title={Data-independent Beamforming for End-to-end Multichannel Multi-speaker ASR},
  author={Cui, Can and Magron, Paul and Sadeghi, Mostafa and Vincent, Emmanuel},
  booktitle={IEEE 27th International Workshop on Multimedia Signal Processing (MMSP 2025)},
  year={2025}
}
```
---

## 📬 Contact

For questions or collaboration inquiries, feel free to open an issue or contact the authors via GitHub.

