# 🛡️ Adversarial Defence System

## 🔍 Project overview

Adversarial Defence System is a research prototype for studying poisoning attacks against a malware-classification model. It uses EMBER-style Portable Executable (PE) feature data to train an XGBoost classifier, generate adversarial training samples, and measure whether detection methods can distinguish clean from poisoned data.

The project covers three attack types:

- **FGSM** - adds a one-step gradient-based perturbation to feature vectors.
- **PGD** - adds an iterative gradient-based perturbation to feature vectors.
- **Backdoor** - inserts fixed feature triggers and changes selected benign labels to malware.

It also contains experimental detector pipelines and an FGSM correction workflow based on purification, anomaly isolation, and retraining. This repository is intended for academic experimentation, not production malware analysis.

## Main workflow

```text
EMBER feature files
       |
       v
Train XGBoost malware classifier
       |
       +--> FGSM / PGD / Backdoor poisoning
       |             |
       |             v
       |       Detector experiments
       |
       +--> FGSM correction experiments
```

## 🚀 Installation

### Option 1: Google Colab (recommended)

The code was written for Google Colab and Google Drive. Open `Adversarial_Defence_System.ipynb` in Colab and run its sections from top to bottom. The notebook contains package-installation cells.

When Colab asks, mount Google Drive. The code uses this project folder:

```text
/content/drive/My Drive/AI_Security_Project/
```

Place the EMBER feature files here:

```text
/content/drive/My Drive/AI_Security_Project/EMBER2018/
```

Required dataset files:

```text
train_features_0.jsonl
train_features_1.jsonl
train_features_2.jsonl
train_features_3.jsonl
train_features_4.jsonl
train_features_5.jsonl
test_features.jsonl
```

### Option 2: Local machine

Local execution is possible, but the current code needs small edits because it contains Colab-specific imports, Drive paths, and `!pip install` notebook commands.

1. Clone or download the repository.

   ```bash
   git clone <repository-url>
   cd Adversarial__Defence__System-main
   ```

2. Create and activate a virtual environment.

   **Windows (PowerShell):**

   ```powershell
   py -m venv .venv
   .\.venv\Scripts\Activate.ps1
   ```

   **macOS/Linux:**

   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   ```

3. Install the packages used by the project.

   ```bash
   pip install numpy pandas scikit-learn xgboost lightgbm tensorflow==2.15.0 torch tensorly scipy adversarial-robustness-toolbox
   pip install torch-geometric
   pip install jupyter
   ```

4. Create a local project-data folder, for example:

   ```text
   C:\AI_Security_Project\
   └── EMBER2018\
   ```

5. In the notebook or Python files, replace every occurrence of:

   ```text
   /content/drive/My Drive/AI_Security_Project
   ```

   with your local project-data path. Also remove `from google.colab import drive` and `drive.mount(...)`.

6. For the simplest local workflow, start Jupyter and run the notebook section by section:

   ```bash
   jupyter notebook
   ```

   If you run the exported `.py` files directly, remove their `!pip install ...` lines first; those lines are valid in notebooks but not regular Python scripts.

##⚒️ How to run

Run the notebook sections in this order:

1. **Model 1** - loads EMBER JSONL files, extracts features, trains the XGBoost model, and saves clean arrays.
2. **FGSM**, **PGD**, or **Backdoor** - creates one or more poisoned datasets.
3. **Detector 1/2/3** - runs the simple Isolation Forest detector for the corresponding attack type.
4. **BEAST detector** sections - optional, advanced detector experiments.
5. **FGSM corrector** sections - optional, experimental purification and retraining work.

The baseline and poisoning stages create these files in the project-data folder:

```text
xgb_model_1.json
X_train_clean.npy
y_train_clean.npy
X_test.npy
y_test.npy
X_train_fgsm.npy
X_train_pgd.npy
X_train_backdoor.npy
y_train_backdoor.npy
```

## 💾 Project structure

```text
Adversarial__Defence__System-main/
├── Adversarial_Defence_System.ipynb     # Main Colab notebook
├── Malware__Detection__System.py        # Baseline XGBoost training
├── FGSM_Poisoning_Bot.py                # FGSM poisoning experiment
├── PGD_Poisoning_Bot.py                 # PGD poisoning experiment
├── Backdoor_Poisoning_Bot.py            # Backdoor poisoning experiment
├── FGSM_Detector.py                     # FGSM BEAST detector experiment
├── PGD_Detector.py                      # PGD BEAST detector experiment
├── Backdoor_Detector.py                 # Backdoor BEAST detector experiment
├── FGSM_Corrector/
│   ├── QTP_&_HAI_Trainer.py              # Purification and anomaly encoding
│   ├── NSFE_&_ARE.py                     # Feature enforcement and retraining setup
│   └── Incremental_Learning.py           # FGSM correction experiment
└── README.md
```

## 📝 Important notes

- The EMBER dataset and generated models are not included in this repository.
- The notebook is the primary runnable version of the project.
- The advanced detector and correction files are experimental. Some rely on models, paths, or variables created in earlier notebook sections, so they may need refactoring for standalone execution.
- Run only on feature datasets that you are authorised to use.

## 🔧 Future customizations

- Replace hard-coded Colab/Drive paths with a configuration file or environment variables.
- Convert notebook cells into reusable Python modules and a command-line interface.
- Add support for additional attacks.
- Add dashboards or plots for model performance, anomaly rates, and attack impact.
