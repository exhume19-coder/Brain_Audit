<div align="center">
  <img src="banner.png" alt="Clinical Audit Banner">
  
  # 🧠 Clinical AI Audit: RSNA Intracranial Hemorrhage
  
  **Transforming a deep learning classification model into a clinically validated, interpretable Medical AI.**
</div>

---

## 🔬 Project Evolution & Audit Journey
This project was developed through a rigorous 18-step **Clinical Audit** process. We started with a standard EfficientNetB0 classification model and subjected it to "Dual-Validation" by a Clinical Lead. The journey involved:
- **Phase 1: Base Training & Domain Freezing:** Initial training on 1,600 patients to establish baseline feature extraction.
- **Phase 2: Scale-up & Fine-Tuning:** Expanding to a balanced 20,000-patient cohort with unfreezing of the visual cortex.
- **Phase 3: Adversarial Auditing:** Using Grad-CAM to intentionally "catch" the model's errors (Frontal Bone Bias & Calcification Trap).
- **Phase 4: Clinical Correction:** Applying Hard Negative Mining and Dropout tuning to optimize the Precision-Recall balance.

---

## 🛠️ Methodology & Innovations

### 1. Advanced Windowing (DICOM to RGB)
Instead of relying on single-channel grayscale, we utilized a multi-windowing technique to capture different anatomical structures. The 16-bit DICOM arrays were mapped to 3-channel RGB tensors:
- **Red Channel:** Brain Window (W:80, L:40) - Detects parenchymal density changes.
- **Green Channel:** Subdural Window (W:200, L:80) - Optimized for surface hemorrhages.
- **Blue Channel:** Bone Window (W:2800, L:600) - Contextualizes skull boundaries.

### 2. The "Clever Hans" Effect & Explainable AI (Grad-CAM)
We deployed Grad-CAM to visualize the model's spatial focus. 
**Audit Discovery:** The model initially exhibited a "Clever Hans" effect—it was occasionally right for the wrong reasons. It fixated on **Aneurysm Coils, Catheters, and Frontal Bone artifacts** instead of the actual blood.

### 3. Precision-Recall Trade-off (The Clinical Sweet Spot)
Our audit revealed that over-correcting for False Positives (using 50% Dropout) led to a massive loss in Sensitivity (Recall). By fine-tuning the model to a **20% Dropout rate**, we achieved a clinically robust balance, significantly reducing missed bleeds (False Negatives).

### 4. The Skull-Stripping Experiment (200-Patient Cohort)
We tested the impact of the skull bone on AI perception:
- **Intraparenchymal (Deep) Cohort:** Detection confidence improved in **28%** of cases.
- **Subdural/Epidural (Superficial) Cohort:** Detection confidence significantly improved in **36%** of cases.
**Conclusion:** Removing bone distraction allows the AI to "see" the blood density more clearly, especially in superficial hematomas.

---

## ⚖️ Academic Vulnerabilities (The Critic's Corner)
To maintain scientific integrity, we proactively identified these vulnerabilities:
- **The Prevalence Trap:** In clinical reality, bleeds are rare (~1-5%). A model trained on 50/50 balanced data may overestimate probability in a real-world ER setting.
- **Hounsfield Unit (HU) Loss:** Compressing 16-bit raw data to 8-bit RGB tensors loses subtle density nuances critical for identifying hyperacute vs. chronic blood.
- **ImageNet Bias:** EfficientNet was born identifying "cats and dogs." Deep medical textures require even more domain-specific pre-training.
- **Slice vs. Volume Gap:** 2D analysis ignores the 3D anatomical continuity of blood—the "Gold Standard" requires 3D-aware architectures.

---

## 🚀 How to Run

### 1. Dataset
The project uses the **RSNA Intracranial Hemorrhage Detection** dataset from Kaggle.
- **Dataset Link:** [Kaggle Dataset](https://www.kaggle.com/c/rsna-intracranial-hemorrhage-detection/data)

### 2. Execution
- **Step 1:** Download the [Brain_Audit_Final_Lite.ipynb](Brain_Audit_Final_Lite.ipynb) from this repo.
- **Step 2:** Upload to Kaggle and connect the RSNA competition dataset.
- **Step 3:** Ensure [pydicom](https://pydicom.github.io/), [opencv-python](https://pypi.org/project/opencv-python/), and [tensorflow](https://www.tensorflow.org/) are installed.
- **Step 4:** Run all cells. The "Clinical Audit" section at the bottom will generate the final metrics.

---

## 📁 Repository Structure
- [Brain_Audit_Final_Lite.ipynb](Brain_Audit_Final_Lite.ipynb): The core code (cleaned of heavy outputs for fast loading).
- [clinical_audit_report/](clinical_audit_report/): Detailed per-patient audit logs and [Executive Summary](clinical_audit_report/EXECUTIVE_SUMMARY.md).
- [banner.png](banner.png): Professional project branding.

---
> **Note:** This project is a hybrid outcome of radiological vision and technical engineering, built to serve as a robust medical AI portfolio piece.
