# 🧠 Clinical Audit of Deep Learning for Intracranial Hemorrhage (ICH) Detection

![Project Banner](https://img.shields.io/badge/Domain-Medical%20AI-blue) ![Dataset](https://img.shields.io/badge/Dataset-RSNA%20ICH-lightgrey) ![Status](https://img.shields.io/badge/Status-Completed-success)

## 📌 Executive Summary
This project is an advanced, radiologist-led **Clinical Audit** of Convolutional Neural Networks (CNNs) applied to Intracranial Hemorrhage (ICH) detection. Moving beyond standard data science metrics (e.g., raw accuracy), this studya rigorously dissects the AI's predictions using **Explainable AI (Grad-CAM)** and medical domain knowledge to expose false positives, spurious correlations, and anatomical illusions.

## 🎯 Project Objectives
1. **Develop** a baseline robust multi-label classification model using transfer learning (EfficientNet) for 6 hemorrhage subtypes.
2. **Diagnose** the "Accuracy Paradox" where an imbalanced dataset leads to algorithmic amnesia (memorizing skull boundaries instead of learning pathology).
3. **Perform an Adversarial Clinical Audit** on the model using 2D CT axial slices to evaluate its true medical reliability.
4. **Propose Radiological Defenses** (e.g., Skull-Stripping and Domain Shift analysis) to mitigate AI hallucinations.

---

## 🏗️ Technical Pipeline & Methodology

### 1. Radiological Preprocessing (HU Windowing)
Standard 16-bit DICOM pixel arrays were mapped to an 8-bit RGB tensor using medically validated Hounsfield Unit (HU) windows to preserve pathology-specific contrast:
- **Red Channel:** Brain Window (W:80, L:40)
- **Green Channel:** Subdural Window (W:200, L:80)
- **Blue Channel:** Bone Window (W:2800, L:600)

### 2. Overcoming the Representational Bottleneck
- **Class Balancing:** To prevent the model from collapsing into trivial "Healthy" predictions (due to the ~90% negative prevalence in the dataset), the training pipeline was artificially balanced to a 50/50 ratio.
- **Two-Stage Training:** 
  - *Stage 1 (Frozen Base)*: Trained on 8,000 images to prevent catastrophic forgetting.
  - *Stage 2 (Fine-Tuning)*: Top 20 layers unfrozen and trained with an ultra-low learning rate (`1e-4`) on an expanded 20,000-image dataset to capture microscopic textural features.

---

## 🔎 The Clinical Audit (Key Findings)

Despite achieving an excellent general prediction calibration (e.g., separating healthy from bleed with >90% precision in tests), a **Radiologist-led Grad-CAM evaluation** exposed critical behavioral flaws in the AI:

### ❌ Illusion 1: Physiological Calcifications (False Positives)
The AI heavily flagged normal physiological calcifications (e.g., inside the Choroid Plexus or Pineal Gland) as **Intraventricular** or **Intraparenchymal Hemorrhages**. Because the model lacks 3D context and purely looks for "bright textures", it equated calcium density with blood density.

### ❌ Illusion 2: The "Spurious Scalp" Correlation
In several cases, the Grad-CAM heatmaps revealed that the AI detected external scalp hematomas (soft tissue swelling) and erroneously predicted internal **Subdural** bleeds in the adjacent space. It learned a *spurious correlation*: "If the scalp is injured (due to trauma), the brain underneath must be bleeding."

### ❌ Illusion 3: Bone Artifacts & Beam Hardening
Areas with thick, asymmetric skull bone or beam hardening streak artifacts caused the model to panic and predict bleeding with >80% confidence, confusing bone scattering with subarachnoid blood.

---

## 🛠️ Solutions & The "Domain Shift" Shock

To counter the bone artifacts, a mathematical **Skull-Stripping algorithm** was implemented using OpenCV to isolate the brain parenchyma. 

**The Finding:** When the model was continuously fed "naked" (skull-stripped) brains during inference, its confidence fell significantly, demonstrating **Domain Shift**. Because the model was trained *with* skulls, removing the skull broke the AI's internal baseline of "what a normal head looks like." 
**Conclusion:** Any preprocessing (like Skull-Stripping) must be integrated at the *training phase*, not just as a band-aid during inference.

---

## 💡 Conclusion
This project demonstrates that high-accuracy medical AI models can harbor deadly clinical flaws if left unchecked. A machine learning model in radiology must be rigorously audited by clinical experts to uncover "Clever Hans" effects and spurious correlations. True robustness requires integrating medical preprocessing (Skull Stripping, 3D Context, Segmentation) directly into the deep learning pipeline.

🤝 Author & Contact
This project was developed and audited by [Emrah Seker], a Medical Doctor specializing in Clinical AI Governance.

LinkedIn: [Connect on LinkedIn]
Email: [exhume19@gmail.com]
Project Goal: Bridge the gap between engineering and clinical reality.
