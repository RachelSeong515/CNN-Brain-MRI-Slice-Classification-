# CNN-Brain-MRI-Slice-Classification-


# Project Description
Built a patient-level CNN pipeline for brain MRI slice classification using ~80K images, with subject-stratified splits to avoid data leakage. By fine-tuning a pre-trained ResNet model, acheived 91.6% weighted test accuracy, demonstrating strong and reliable performance for clinical and hospital decision-support use cases.

# EDA (Data Description)

This project uses a preprocessed 2D version of the Open Access Series of Imaging Studies (OASIS-1) dataset, where 3D brain MRI volumes are converted into axial 2D slices to enable efficient training with standard CNN architectures. The task is framed as a multi-class classification problem with four dementia severity levels defined by the Clinical Dementia Rating (CDR) scale: Non-demented (CDR = 0), Very Mild Dementia (CDR = 0.5), Mild Dementia (CDR = 1), and Moderate Dementia (CDR = 2).

The dataset contains approximately 80,000 MRI slices from around 461 patients, with raw image dimensions of 248 × 496 × 3.

To ensure a realistic and leakage-free evaluation, the data is organized at the patient level. Patient identifiers are extracted from image filenames (e.g., OAS1_0028_MR1) and used to regroup individual slices into patient-level collections. This enables subject-stratified splitting and ensures that all slices from a given patient appear in only one dataset split. 


# Data Loading

We used a subject-level, class-stratified data split instead of randomly splitting individual slices. Within each dementia class, patients were shuffled and divided into approximately 64% training, 16% validation, and 20% test sets. These patient-level splits were then mapped back to slice indices for model training and evaluation.

# Model

We adopted a transfer learning approach using a pre-trained ResNet convolutional neural network as the backbone. Pre-trained ImageNet weights were leveraged to accelerate improve generalization, which is particularly important given the limited size and specialized nature of medical imaging datasets. The final fully connected layer was replaced with a new classification head configured for four dementia severity classes based on the Clinical Dementia Rating (CDR) scale .

The model also used the Adam optimizer to allow stable fine-tuning without overwriting learned feature representations. Cross-entropy loss was used for multi-class classification, with provisions for class-weighted loss to address class imbalance. To further control overfitting, L2 regularization and a ReduceLROnPlateau learning rate scheduler were applied, and early stopping was enforced by checkpointing the model with the lowest validation loss.


# Data Findings

Our model achieved its best generalization performance with a validation accuracy of 85.61%. When evaluated on a fully held-out test set containing patients never seen during training or validation, the final model achieved a weighted test accuracy of 91.6%, with weighted precision, recall, and F1-scores all near 0.92 .

Performance varied across classes, reflecting the inherent imbalance in dementia datasets. The dominant class was modeled extremely well, while minority classes showed lower precision and recall — a known and expected challenge in medical classification problems. Importantly, recall for dementia-positive classes remained strong, which is critical in clinical contexts where false negatives carry higher risk than false positives. The confusion matrix confirms that most errors occur between adjacent dementia severity levels, indicating clinically plausible misclassifications rather than random failure .


# Business Implications

These results demonstrate a strong potential as an AI-powered computer-aided diagnosis (CAD) tool for healthcare environments. By automating the initial screening of large volumes of MRI slices, the model can significantly reduce radiologist workload and accelerate time-to-diagnosis, allowing clinicians to focus on complex or ambiguous cases.

Accurate differentiation between non-demented and very mild dementia cases has high business and clinical value, as early detection enables earlier intervention, improved patient outcomes, and reduced long-term care costs. Standardizing assessments across institutions also reduces inter-radiologist variability, improving consistency and reliability in clinical decision-making.

From an operational standpoint, the model’s strong performance under patient-level validation suggests it can scale reliably in real-world hospital settings. When integrated into existing PACS or radiology workflows, the system could function as a decision-support layer rather than a replacement for clinicians, aligning well with regulatory and ethical expectations. Overall, this solution highlights how modern deep learning can deliver measurable efficiency gains, improved diagnostic confidence, and long-term cost savings in healthcare delivery .




