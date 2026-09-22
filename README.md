# Disaster Image Identification

Deep learning project for classifying disaster-related images into six visual categories using a baseline Convolutional Neural Network (CNN) and Inception ResNet V2 with transfer learning and fine tuning.

This repository contains the training and evaluation workflow associated with the study:

**“Identifikasi Kejadian Bencana Berdasarkan Gambar Unggahan di Media Sosial dalam Upaya Mitigasi Bencana Menggunakan Inception ResNet V2”**


## Overview

The project evaluates four experimental configurations:

1. CNN
2. CNN with fine tuning
3. Inception ResNet V2
4. Inception ResNet V2 with fine tuning

The classification task contains six classes:

- Damaged Infrastructure
- Fire Disaster
- Human Damage
- Land Disaster
- Non Damage
- Water Disaster

The workflow includes dataset acquisition, corrupted-image validation, image preprocessing, data augmentation, stratified train/validation/test splitting, model training, checkpoint saving, and evaluation.

## Dataset

The project uses the **Disaster Images Dataset** available on Kaggle:

https://www.kaggle.com/datasets/varpit94/disaster-images-dataset

The original research dataset contains **13,555 valid images** distributed across six classes.

| Class | Images |
|---|---:|
| Damaged Infrastructure | 1,453 |
| Fire Disaster | 933 |
| Human Damage | 240 |
| Land Disaster | 657 |
| Non Damage | 9,237 |
| Water Disaster | 1,035 |
| **Total** | **13,555** |

The notebook downloads the dataset automatically with `kagglehub`.

During validation, unreadable or truncated image files are excluded before the dataset is split.

## Preprocessing

The training pipeline uses:

- Input size: `299 × 299 × 3`
- Pixel normalization
- Rotation augmentation
- Zoom augmentation
- Horizontal flipping
- Stratified train/validation/test split
- No oversampling or undersampling

The current reproduction uses a `70% / 15% / 15%` train/validation/test split.

## Models

### CNN

The baseline CNN consists of convolutional layers, max-pooling layers, a fully connected layer, dropout, and a softmax classifier.

Because the paper does not specify every CNN layer configuration in detail, the CNN implementation in the notebook is a transparent reconstruction of the described architecture.

### CNN Fine Tuning

The CNN fine-tuning stage continues training from the best baseline CNN checkpoint using a lower learning rate.

### Inception ResNet V2

The transfer-learning model uses:

- Inception ResNet V2 backbone
- ImageNet pretrained weights
- Frozen backbone during the initial training stage
- Global Average Pooling
- Dropout
- Softmax classifier

### Inception ResNet V2 Fine Tuning

After the initial training stage, the last **40 layers of the Inception ResNet V2 backbone** are made trainable and optimized with a lower learning rate.

## Evaluation

Each model is evaluated on the test set using:

- Accuracy
- Sensitivity / Recall
- Specificity
- Precision
- F1-Score
- Confusion Matrix

### Experimental Results

| Model | Accuracy | Sensitivity | Specificity | Precision | F1-Score |
|---|---:|---:|---:|---:|---:|
| CNN | 0.8692 | 0.5576 | 0.9730 | 0.6193 | 0.5552 |
| CNN Fine Tuning | 0.8786 | 0.5864 | 0.9743 | 0.6999 | 0.5954 |
| Inception ResNet V2 | 0.9194 | 0.7862 | 0.9780 | 0.8466 | 0.8133 |
| **Inception ResNet V2 Fine Tuning** | **0.9562** | **0.8868** | **0.9917** | **0.9007** | **0.8929** |

The best-performing configuration is **Inception ResNet V2 with fine tuning**, reaching **95.62% test accuracy**.

## Confusion Matrices

### CNN

![CNN Confusion Matrix](results/plots/CNN%20Confusion%20Matrix.png)

### CNN Fine Tuning

![CNN Fine Tuning Confusion Matrix](results/plots/CNN%20Fine%20Tuning%20Confusion%20Matrix.png)

### Inception ResNet V2

![Inception ResNet V2 Confusion Matrix](results/plots/Inception%20Resnet%20V2%20Confusion%20Matrix.png)

### Inception ResNet V2 Fine Tuning

![Inception ResNet V2 Fine Tuning Confusion Matrix](results/plots/Inception%20Resnet%20V2%20Fine%20Tuning%20Confusion%20Matrix.png)

## Training Curves

### CNN

| Accuracy | Loss |
|---|---|
| ![CNN Accuracy](results/plots/CNN%20Accuracy.png) | ![CNN Loss](results/plots/CNN%20Loss.png) |

### Inception ResNet V2

| Accuracy | Loss |
|---|---|
| ![Inception ResNet V2 Accuracy](results/plots/Inception%20Resnet%20V2%20Accuracy.png) | ![Inception ResNet V2 Loss](results/plots/Inception%20Resnet%20V2%20Loss.png) |

## Repository Structure

```text
.
├── notebooks/
│   └── Disaster.ipynb
├── results/
│   ├── metrics/
│   │   └── All Model Metrics.csv
│   └── plots/
│       ├── CNN Confusion Matrix.png
│       ├── CNN Fine Tuning Confusion Matrix.png
│       ├── Inception Resnet V2 Confusion Matrix.png
│       ├── Inception Resnet V2 Fine Tuning Confusion Matrix.png
│       ├── CNN Accuracy.png
│       ├── CNN Loss.png
│       ├── Inception Resnet V2 Accuracy.png
│       └── Inception Resnet V2 Loss.png
├── README.md
├── requirements.txt
├── .gitignore
├── LICENSE
└── CITATION.cff
```

## Running the Notebook

The notebook is designed for Google Colab.

1. Open `notebooks/Disaster.ipynb` in Google Colab.
2. Enable a GPU runtime.
3. Run the notebook from the first cell.
4. The dataset will be downloaded automatically from Kaggle.
5. Training outputs are written to Google Drive under:

```text
/content/drive/MyDrive/Disaster/
```

The notebook saves model checkpoints, histories, evaluation metrics, and plots during execution.

## Installation

For a local Python environment:

```bash
pip install -r requirements.txt
```

The main dependencies are TensorFlow, NumPy, pandas, scikit-learn, Matplotlib, Pillow, tqdm, and KaggleHub.

## Trained Models

Large `.keras` model checkpoints are intentionally excluded from the Git repository.

The recommended model for deployment or further experimentation is:

```text
04_inception_resnet_v2_fine_tuning.keras
```

Large trained-model files can be distributed separately, for example through a GitHub Release.

## Notes on Reproducibility

The repository reproduces the experimental workflow as closely as possible from the available research documentation.

Some implementation details, particularly the exact CNN layer configuration and CNN fine-tuning procedure, are not fully specified in the paper. Those parts are explicitly reconstructed in the notebook rather than presented as exact unpublished settings.

## Citation

If you use this repository in academic work, please cite the associated paper:

> *Identifikasi Kejadian Bencana Berdasarkan Gambar Unggahan di Media Sosial dalam Upaya Mitigasi Bencana Menggunakan Inception ResNet V2*.  
> Jurnal Riset Aplikasi Ilmu Data dan Sistem, 1(1), 1–15, 2026.

Citation metadata is also provided in [`CITATION.cff`](CITATION.cff).

Paper link: https://ejournal.unesa.ac.id/index.php/rapids/article/view/76884

## License

This repository is released under the [MIT License](LICENSE).

