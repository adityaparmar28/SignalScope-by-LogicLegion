# 🔍 SignalScope by Logic Legion

**Telling Real From Synthetic in the Age of Generative Media**

![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)
![PyTorch](https://img.shields.io/badge/PyTorch-Deep%20Learning-EE4C2C.svg)
![Streamlit](https://img.shields.io/badge/Streamlit-Web%20App-FF4B4B.svg)
![SIH 2026](https://img.shields.io/badge/SIH%202026-Problem%202-brightgreen.svg)

> **SIH 2026 Internal Hackathon** | L.J. Institute of Engineering and Technology [C-433]  
> **Domain:** AI / Media Forensics / Trust & Safety

---

## 🌟 Why SignalScope?
With text-to-image models generating photorealistic deepfakes in seconds, visual misinformation is at an all-time high. **SignalScope** is built for journalists, fact-checkers, and everyday users. It doesn't just give a "Real or Fake" verdict—it *explains* why, by highlighting visual inconsistencies (lighting, geometry) and detecting invisible generator fingerprints (SRM high-pass filtering).

---

## 👥 Meet The Team (Logic Legion)

| Member | GitHub Username | Role / Contribution |
| :--- | :--- | :--- |
| **Aditya Parmar** | `@adityaparmar28` | **Frontend & MLOps Lead** (Built Streamlit UI & Integration) |
| **Tapan** | `@tapansoni2007-dotcom` | **Core ML Lead** (Dual-Branch Architecture & Training) |
| **Krina Malviya** | `@KrinaMalaviya` | **Data Engineering Lead** (Data Pipeline & SRM Filters) |
| **Yuvraj** | `@YUXRAJ21` | **Explainability (XAI) Lead** (Grad-CAM Heatmaps & Text Gen) |
| **Athul Nair** | `@athul2917-tech` | **Testing Lead** (Degradation Benchmarks & Robustness) |
| **Pari Doshi** | `@paridoshi25` | **Docs & Analytics Lead** (Metrics, Reports & Repo Structure) |

---

## 📸 Application Screenshots

*(Add screenshots of your application here before final submission)*

<div align="center">
  <img src="https://via.placeholder.com/400x250.png?text=Streamlit+UI+Dashboard" alt="UI Dashboard" width="45%">
  <img src="https://via.placeholder.com/400x250.png?text=Grad-CAM+Heatmap+Output" alt="Grad-CAM Output" width="45%">
</div>

---

## 📋 Modules Built (Core + Bonus)

| Module | Status | Description |
|--------|--------|-------------|
| **Core: Real vs AI-Generated Classification** | ✅ Built | Binary classifier with calibrated confidence |
| **Module A: Faithful Explanation** | ✅ Built | Grad-CAM heatmaps + template-based text explanations |
| **Module B: Generator Attribution** | 🔧 Architecture Ready | Multi-class head for GAN vs Diffusion attribution |
| **Module C: Robustness to Degradation** | ✅ Built | JPEG, resize, noise, screenshot robustness analysis |
| **Module D: Provenance & Metadata** | ✅ Built | EXIF/C2PA signature detection integrated into UI |
| **Module F: Deployable Interface** | ✅ Built | Streamlit drag-and-drop web application |
| **Module G: Active Defence Analysis** | ✅ Built | FGSM adversarial attack testing in robustness suite |

---

## 🚀 Quick Start (Setup & Run)

### Prerequisites
- Python 3.10+
- Virtual Environment (recommended)

### 1. Install Dependencies
```bash
# Create and activate virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install requirements
pip install -r requirements.txt
```

### 2. Download Dataset
```bash
python -m data.download_data --output_dir data --max_samples 60000
```

### 3. Train the Model
```bash
# GPU (recommended)
python -m src.model.train --data_dir data --epochs 15 --batch_size 32 --backbone efficientnet_b4
```

### 4. Run a Prediction
```bash
python -m src.model.predict --image path/to/image.jpg --model_dir model/weights
```

### 5. Launch ZeroGPT-Grade Web UI
```bash
streamlit run app/app.py
```

> ⏱️ A judge should be able to reproduce a prediction in under 10 minutes by running steps 1, then 4.

---

## 📊 Datasets Used

| Dataset | Source | Generator Family | Images |
|---------|--------|------------------|--------|
| **Midjourney Dataset** | [HuggingFace (ehristoforu)](https://huggingface.co/datasets/ehristoforu/midjourney-images) | Midjourney v4 / v5 | 500+ |
| **Diverse AI Art & Real** | [HuggingFace (Hemg)](https://huggingface.co/datasets/Hemg/AI-Generated-vs-Real-Images-Datasets) | DALL-E / Generative Art & Real | 800+ |
| **CIFAKE** | [HuggingFace (dragonintelligence)](https://huggingface.co/datasets/dragonintelligence/CIFAKE-image-dataset) | Stable Diffusion v1.4 | 7,200 |
| **ImageNet-1K (via EfficientNet)** | Pretrained backbone weights (`timm`) | Natural photography | 1.4M (Pretrained) |

---

## 📈 Reported Metrics (Multi-Generator Evaluation)

*Evaluated on 1,459 held-out images covering Midjourney, AI Art, Stable Diffusion, and Real Photos:*

| Metric | Operating Threshold (0.65) | Balanced Threshold (0.50) |
|--------|----------------------------|----------------------------|
| **Overall ROC-AUC** | **0.9732 (97.32%)** | **0.9732 (97.32%)** |
| **Unseen-Generator-Split AUC** | **0.9685 (96.85%)** | **0.9685 (96.85%)** |
| **Macro-F1 Score** | **0.8461** | **0.9015** |
| **Test Accuracy** | **84.65%** (1,235 / 1,459) | **90.75%** (1,324 / 1,459) |
| **False Positive Rate (FPR)** | **2.24%** | **4.48%** |

> 📊 Confusion matrix visualization: `report/confusion_matrices.png`  
> 📈 ROC & Degradation curves: `report/evaluation_metrics.png`  
> 📄 Official model report: `report/model_report.md`

---

## 🏗️ Architecture Overview

### Why Dual-Branch?
- **Spatial branch** (EfficientNet-B4): Detects visual artifacts — texture inconsistencies, geometry errors, lighting/shadow issues.
- **Frequency branch** (SRM filters): Detects spectral fingerprints left by generators. These are **generator-agnostic** and help generalize to unseen generators.
- **Feature fusion**: Combines both signal types for highly robust classification.

### Robustness & Calibration
- **Training augmentation**: JPEG compression, random resize/crop, Gaussian noise — simulates real-world degradation.
- **Temperature scaling**: Post-hoc calibration ensures confidence scores are honest (reduces overconfidence).

### Explainability (XAI)
- **Grad-CAM**: Generates spatial heatmaps highlighting the exact regions the model finds suspicious.
- **Template-based explanation**: Analyzes regions and generates faithful, hedged natural-language explanations (e.g., "Likely AI due to texture artifacts in highlighted region").

---

## 🔒 Known Limitations

1. **Low-Res Training**: CIFAKE images are 32×32. Fine-grained artifact detection requires higher-resolution datasets.
2. **Unseen generators**: Performance may degrade on fundamentally new architectures released post-training.
3. **Adversarial attacks**: The model is not hardened against deliberate adversarial noise (e.g., FGSM).

---

## 🎬 Demo Video

👉 **[Insert YouTube / Drive Demo Video Link Here]** 👈

---

## 📜 Originality Declaration

- **Architecture**: Custom dual-branch design combining EfficientNet (from `timm` library) with SRM steganalysis filters.
- **Libraries**: PyTorch, timm, pytorch-grad-cam, Streamlit.
- **AI assistants**: Used strictly for code scaffolding and formatting; the working system, logic, and evaluation are entirely our own.
- **Compliance**: We strictly adhere to the scope (No face-swap targeting, no real-world political figures). All data is MIT/Open source.
