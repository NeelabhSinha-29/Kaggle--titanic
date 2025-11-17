# Titanic Survival Prediction

A complete end-to-end machine learning workflow for the Kaggle Titanic Survival Prediction challenge. This project includes exploratory data analysis, feature engineering, statistical testing, and Support Vector Machine (SVM) modeling to predict passenger survival.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Project Structure](#project-structure)
- [Installation](#installation)
- [Usage](#usage)
- [Methodology](#methodology)
- [Model Performance](#model-performance)
- [Key Findings](#key-findings)
- [License](#license)

---

## 🎯 Overview

This project builds an SVM classifier to predict passenger survival on the Titanic. Through comprehensive exploratory data analysis, the project reveals that survival was heavily influenced by:

- **Gender**: Females had significantly higher survival rates
- **Age Group**: Children and infants prioritized in evacuation ("women and children first" protocol)


**Final Model Performance:** 80% accuracy on validation set using RBF kernel SVM

---

## 📁 Project Structure

```
Kaggle--titanic/
├── README.md                          # Project documentation
├── requirements.txt                   # Python dependencies
├── notebooks/
│   └── Titanic.ipynb                 # Main analysis and modeling notebook
├── titanic/
│   ├── train.csv                     # Training data (891 passengers)
│   ├── test.csv                      # Test data (418 passengers)
│   └── gender_submission.csv         # Baseline submission
└── results_submission/
    └── submission.csv                # Final model predictions
```

---

## 🔧 Installation

### Prerequisites
- Python 3.8 or higher
- pip or conda package manager

### Steps

1. **Clone the repository**
   ```bash
   git clone https://github.com/NeelabhSinha-29/Kaggle--titanic.git
   cd Kaggle--titanic
   ```

2. **Create a virtual environment** (optional but recommended)
   ```bash
   python -m venv venv
   # On Windows
   venv\Scripts\activate
   # On macOS/Linux
   source venv/bin/activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Download the Titanic dataset from Kaggle**
   
   Before running the notebook, you need to download the Titanic dataset. This requires Kaggle API credentials.

   a. **Set up Kaggle API** (if not already done):
      - Go to https://www.kaggle.com/settings/account
      - Click "Create New API Token" to download `kaggle.json`
      - Place `kaggle.json` in `~/.kaggle/` directory
      - On Windows: `C:\Users\<YourUsername>\.kaggle\kaggle.json`
      - On macOS/Linux: `~/.kaggle/kaggle.json`

   b. **Download the dataset**:
      ```bash
      kaggle competitions download -c titanic -p ./titanic
      ```

   c. **Unarchive the downloaded file**:
      ```bash
      cd titanic
      unzip titanic.zip -d .
      cd ..
      ```

   The dataset will be extracted to the `titanic/` folder with files: `train.csv`, `test.csv`, and `gender_submission.csv`.

---

## 🚀 Usage

### Running the Analysis

Open and run the Jupyter notebook:
```bash
jupyter notebook notebooks/Titanic.ipynb
```

The notebook executes in the following order:

1. **Data Loading** - Load training, validation, and test datasets
2. **Exploratory Data Analysis (EDA)** - Understand data structure, missing values, and distributions
3. **Missing Data Analysis** - Investigate patterns in missing Age and Cabin data
4. **Feature Engineering** - Create age groups and missingness indicators
5. **Modeling** - Train linear and RBF SVM models
6. **Prediction** - Generate test predictions for Kaggle submission

### Generating Predictions

The notebook automatically:
- Trains the final RBF SVM on combined training + validation data
- Makes predictions on the test set
- Saves results to `results_submission/submission.csv`

---

## 🔬 Methodology

### Data Preprocessing

**Missing Values Treatment:**
- **Age** (~19% missing): Imputed with median; missingness indicator retained as `age_missing` feature
- **Cabin** (~77% missing): Not used directly; missingness indicator `cabin_missing` retained as proxy for socioeconomic status
- **Fare** (~0.1% missing): Imputed with median
- **Embarked** (~0.2% missing): Not explicitly handled in current version

**Feature Engineering:**
- **Age Grouping**: Continuous age converted to meaningful categories
  - Infant: 0–2 years
  - Child: 3–15 years
  - Adult: 16–59 years
  - Elderly: 60+ years
- **Missingness Indicators**: Binary flags for missing Age and Cabin data, capturing MNAR (Missing Not At Random) patterns
- **Categorical Encoding**: One-hot encoding for Sex, Embarked, and Age_group

**Feature Selection:**
- Excluded: Name, Ticket, Cabin (raw values)
- Included: Pclass, Sex, SibSp, Parch, Fare, Embarked, cabin_missing, age_missing, Age_group

### Model Selection

Two SVM variants evaluated on validation set:

| Kernel | Accuracy | Notes |
|--------|----------|-------|
| **Linear** | 79% | Simpler, faster; assumes linear decision boundary |
| **RBF** | 80% | Better captures nonlinear relationships; selected for final model |

The RBF kernel was chosen to account for complex interactions between gender, age group, and passenger class.

---

## 📊 Model Performance

### Validation Set Results (RBF SVM)

```
Confusion Matrix:
[[89  9]    (True Negatives=89, False Positives=9)
 [26 56]]   (False Negatives=26, True Positives=56)

Accuracy:   0.80
Precision:  0.86 (of predicted survivors, 86% actually survived)
Recall:     0.68 (of actual survivors, 68% were correctly predicted)
F1-Score:   0.76
```

---

## 🎓 Key Findings

### Survival by Demographics

1. **Gender Effect (Strongest Predictor)**
   - Female survival rate: High
   - Male survival rate: significantly lower 
   - Clear evidence of "women first" evacuation priority

2. **Age Group Pattern**
   - Infants (0–2): Highest survival 
   - Children (3–15): High survival 
   - Adults (16–59): Lower survival 
   - Elderly (60+): Lower survival 



### Missingness as Signal

- Passengers with missing Cabin data had lower survival rates 
- Passengers with missing Age data had lower survival rates 
- This pattern suggests missingness correlates with socioeconomic status and record-keeping practices

---

## 📦 Dependencies

All required packages listed in `requirements.txt`:
- **pandas** - Data manipulation and analysis
- **numpy** - Numerical computing
- **scikit-learn** - Machine learning models and preprocessing
- **scipy** - Statistical testing
- **matplotlib** - Visualization
- **seaborn** - Advanced visualization (optional)
- **ipykernel** - Jupyter notebook kernel

---

## 📝 License

This project is open source and available under the MIT License.

---

## 🤝 Contributing

Contributions are welcome! Feel free to:
- Report issues or bugs
- Suggest improvements or new features
- Submit pull requests

---

## 📬 Contact

For questions or feedback, reach out via GitHub or email.

---

**Last Updated:** November 2025  
**Data Source:** [Kaggle Titanic Dataset](https://www.kaggle.com/c/titanic)
