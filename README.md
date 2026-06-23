# EEG Emotion Recognition with 1D and 2D CNNs

This repository contains the project developed for the **Computer Vision Technologies and Biometrics** course at the University of Cagliari.

The goal of the project is to build an **EEG-based emotion recognition system** able to classify five emotional states:

- Happiness
- Sadness
- Fear
- Disgust
- Neutrality

The system is based on a subset of the **SEED-V EEG dataset** and compares two deep learning approaches:

1. **1D Convolutional Neural Network**, designed to process EEG signals as temporal sequences.
2. **2D Convolutional Neural Network**, designed to process EEG segments as channel-time matrices.

The project also explores the effect of different EEG window durations on classification performance.

---

## Project Overview

The complete pipeline is implemented in a Jupyter Notebook and includes:

1. Dataset loading
2. EEG preprocessing
3. Signal segmentation into fixed-length windows
4. Dataset split into training, validation, and test sets
5. 1D-CNN model training and evaluation
6. 2D-CNN model training and evaluation
7. Window-duration experiments
8. Result saving and comparison

The default segmentation length is **1.5 seconds**, as required by the assignment. Additional experiments are performed with alternative window durations, such as **1 second** and **2 seconds**, to analyze the impact of temporal context on emotion recognition.

---

## Dataset

The project uses a reduced portion of the **SEED-V EEG dataset**.

The provided subset contains:

- **9 EEG recordings**
- **3 subjects**
- **3 sessions per subject**
- **5 emotion classes**

The raw dataset files are not included in this repository because of their size. They must be downloaded separately from the shared drive provided in the project assignment.

Expected local structure:

```text
project-root/
├── data/
│   └── EEG_raw/
│       └── SEED-V EEG files
│
│
├── outputs/
│   ├── figures/
│   ├── models/
│   ├── scores/
│   └── confusion_matrices/
├── CVT_EEG_Caravagna.ipynb
├── requirements.txt
└── README.md
```

The `data/EEG_raw/` folder should contain the original EEG recordings.

The `outputs/` folder stores experiment results, plots, trained models, scores, and confusion matrices.

---

## Methodology

### 1. Preprocessing

The EEG recordings are loaded and prepared for classification. The preprocessing pipeline includes:

- Resampling
- Band-pass filtering
- Channel selection
- Segmentation into fixed-duration windows

Each segmented EEG window is treated as an independent sample and assigned the corresponding emotional label.

---

### 2. Segmentation

The main experiment uses EEG windows of:

```text
1.5 seconds
```

Additional experiments are performed using different window lengths:

```text
1.0 second
2.0 seconds
```

This allows the project to evaluate how the amount of temporal information affects model accuracy and class stability.

Shorter windows generate more samples but may contain less emotional information.

Longer windows provide more temporal context but generate fewer samples and may include more intra-window variability.

---

### 3. Dataset Split

The segmented samples are divided into:

```text
70% training
15% validation
15% testing
```

The training set is used to optimize the model parameters.

The validation set is used to monitor training and reduce overfitting.

The test set is used only for the final evaluation.

---

## Models

### 1D-CNN

The 1D-CNN processes EEG data as temporal sequences.

Typical input shape:

```text
samples x channels
```

This model is designed to learn local temporal patterns directly from the EEG signal.

---

### 2D-CNN

The 2D-CNN processes each EEG segment as a two-dimensional representation.

Typical input shape:

```text
channels x samples
```

This representation allows the network to exploit both temporal and inter-channel information.

---

## Evaluation

The models are evaluated on the test set using:

- Accuracy
- Confusion matrix
- Per-class classification behavior
- Comparison between 1D-CNN and 2D-CNN
- Comparison between different window durations

The results are saved inside the `outputs/` directory.

For each experiment, the notebook stores:

- Training accuracy/loss plots
- Validation accuracy/loss plots
- Confusion matrix
- Final test scores
- Model-specific output files

Experiments with different random seeds or different parameter configurations are saved separately to avoid overwriting previous runs.

---

## How to Run

### 1. Clone the repository

```bash
git clone <repository-url>
cd <repository-name>
```

### 2. Create a virtual environment

```bash
python -m venv .venv
source .venv/bin/activate
```

On Windows:

```bash
.venv\Scripts\activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Add the dataset

Place the SEED-V EEG files inside:

```text
data/raw/
```

The dataset is not included in the repository.

### 5. Run the notebook

Start Jupyter:

```bash
jupyter notebook
```

Then open:

```text
CVT_EEG_Caravagna.ipynb
```

Run the notebook cells in order.

---

## Main Configuration Options

The notebook includes configuration variables to control the experiments.

Typical options include:

```python
WINDOW_SECONDS = 1.5
RESAMPLE_FREQ = 250
LOW_FREQ = 1.0
HIGH_FREQ = 50.0
RANDOM_SEED = 0
TRAIN_1D_CNN = True
TRAIN_2D_CNN = True
SAVE_OUTPUTS = True
```

The most important variables are:

| Variable | Description |
|---|---|
| `WINDOW_SECONDS` | EEG window duration in seconds |
| `RESAMPLE_FREQ` | Target sampling frequency |
| `LOW_FREQ` | Lower cutoff frequency for filtering |
| `HIGH_FREQ` | Upper cutoff frequency for filtering |
| `RANDOM_SEED` | Seed used for reproducible experiments |
| `TRAIN_1D_CNN` | Enables/disables 1D-CNN training |
| `TRAIN_2D_CNN` | Enables/disables 2D-CNN training |
| `SAVE_OUTPUTS` | Enables/disables saving of plots and scores |

---

## Outputs

Generated results are saved in the `outputs/` folder.

Example structure:

```text
outputs/
├── figures/
│   ├── loss_accuracy/
│   └── confusion_matrices/
├── scores/
│   └── experiment_scores.csv
├── models/
│   └── trained_models
└── logs/
    └── experiment_metadata
```

Output filenames include the main experimental parameters, such as:

- Model type
- Window duration
- Resampling frequency
- Filter frequencies
- Random seed

This prevents different runs from overwriting each other.

---

## Experiments

The main experiments are:

| Experiment | Window duration | Model |
|---|---:|---|
| Baseline 1D-CNN | 1.5 s | 1D-CNN |
| Baseline 2D-CNN | 1.5 s | 2D-CNN |
| Short-window test | 1.0 s | 1D-CNN / 2D-CNN |
| Long-window test | 2.0 s | 1D-CNN / 2D-CNN |

The window-duration analysis is used to study whether shorter or longer EEG segments improve classification accuracy and model stability.

---

## Results Discussion

The final analysis compares the models according to:

- Overall accuracy
- Confusion matrices
- Class-specific errors
- Effect of window duration
- Possible class imbalance
- Possible subject/session variability
- Differences between temporal and spatial-temporal representations

The 1D-CNN is generally more directly suited to temporal EEG signals.

The 2D-CNN can exploit relationships between EEG channels and time samples, but its performance depends strongly on how the 2D input representation is constructed.

---

## Limitations

The project has some important limitations:

- The dataset subset is small.
- Only 3 subjects and 3 sessions are available.
- EEG signals are highly variable across subjects and sessions.
- Random train/test splitting may overestimate performance if samples from the same subject or session appear in both training and test sets.
- More robust evaluation would require subject-aware or session-aware splits.

---

## Future Work

Possible extensions include:

- Subject-aware evaluation
- Session-aware evaluation
- Leave-one-subject-out testing
- Leave-one-session-out testing
- More advanced EEG preprocessing
- Frequency-band specific experiments
- Spectrogram-based 2D-CNN inputs
- Channel-topology-based EEG maps
- Model regularization and hyperparameter tuning
- Comparison with traditional machine learning classifiers

Subject-aware and session-aware evaluations would be especially useful to better understand whether the models are learning general emotion-related patterns or mostly subject/session-specific characteristics.

---

## Repository Contents

```text
.
├── CVT_EEG_Caravagna.ipynb   # Main project notebook
├── requirements.txt          # Python dependencies
├── README.md                 # Project documentation
├── data/                     # Local dataset folder, not tracked
└── outputs/                  # Generated experiment results
```

---

## Notes

The raw EEG dataset and generated intermediate files should not be committed to the repository.

Recommended `.gitignore` entries:

```text
data/raw/
data/processed/
outputs/models/
__pycache__/
.ipynb_checkpoints/
.venv/
```

Depending on the project submission requirements, selected result files such as plots, confusion matrices, and score tables can be included in the repository or exported separately.

---

## Author

Riccardo Caravagna

M.Sc. Computer Engineering, CyberSecurity and Artificial Intelligence  
University of Cagliari
