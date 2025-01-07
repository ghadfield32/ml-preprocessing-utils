# DataPreprocessor

**DataPreprocessor** is a comprehensive Python package designed to streamline the data preprocessing workflow for various machine learning models. It offers functionalities such as handling missing values, outlier detection, feature encoding, scaling, and addressing class imbalances using techniques like SMOTE.

## 🧰 Features

- **Flexible Preprocessing Pipeline:** Customize preprocessing steps based on your model requirements.
- **Automatic Outlier Detection:** Utilize methods like Z-Score Filtering, IQR Filtering, and Isolation Forest.
- **Categorical Encoding:** Support for Ordinal and One-Hot Encoding.
- **Scaling Options:** Apply StandardScaler, MinMaxScaler, or RobustScaler.
- **Class Imbalance Handling:** Implement SMOTE and its variants for classification tasks.
- **Configuration Driven:** Easily adjust preprocessing steps via YAML configuration files.
- **Inverse Transformation:** Reconstruct original feature values from transformed data for interpretability.

## 📋 Table of Contents

- [🧰 Features](#-features)
- [🚀 Installation](#-installation)
- [📝 Usage](#-usage)
  - [Basic Example](#basic-example)
  - [Configuration](#configuration)
  - [Running Preprocessing in Predict Mode](#running-preprocessing-in-predict-mode)
- [🧪 Testing](#-testing)
- [🛠️ Contributing](#️-contributing)
- [📄 License](#-license)
- [🔧 Continuous Integration](#-continuous-integration)

## 🚀 Installation

Ensure you have Python 3.6 or higher installed.

1. **Clone the Repository:**

    ```bash
    git clone https://github.com/ghadfield32/ml_preprocessor.git
    cd ml_preprocessor
    ```

2. **Install the Package:**

    ```bash
    pip install .
    ```

    Alternatively, for development purposes:

    ```bash
    pip install -e .
    ```

3. **Install Dependencies:**

    ```bash
    pip install -r requirements.txt
    ```

4. **Install Custom Preprocessing Package:**

    To include the custom preprocessing package directly from GitHub, use:

    ```bash
    pip install git+https://github.com/ghadfield32/ml_preprocessor.git
    ```

## 📝 Usage

### Basic Train Example

```python
from datapreprocessor import DataPreprocessor
import pandas as pd

# Load your dataset
df = pd.read_csv('path_to_your_dataset.csv')

# Initialize the DataPreprocessor
preprocessor = DataPreprocessor(
    model_type="Tree Based Classifier",
    y_variable=["target"],
    ordinal_categoricals=["ordinal_feature1", "ordinal_feature2"],
    nominal_categoricals=["nominal_feature1", "nominal_feature2"],
    numericals=["num_feature1", "num_feature2"],
    mode="train",
    # options=your_config_options,  # uncomment if custom scaling/encoding/transforming or want to adjust paths or anything
    debug=True
)

# Execute Preprocessing
X_train, X_test, y_train, y_test, recommendations, X_test_inverse = preprocessor.preprocess_train(
    df.drop('target', axis=1), 
    df['target']
)
```

### Train Preprocessing Adjustment Configuration

Customize preprocessing steps via the `preprocessor_config.yaml` file located in the `config/` directory. Adjust parameters like imputation strategies, encoding methods, scaling techniques, and SMOTE configurations based on your project needs.

### Running Preprocessing in Predict Mode

```python
from datapreprocessor import DataPreprocessor
import pandas as pd

# Load new data for prediction
new_data = pd.read_csv('path_to_new_data.csv')

# Initialize the DataPreprocessor in predict mode
preprocessor = DataPreprocessor(
    model_type="Tree Based Classifier",
    y_variable=["target"],
    ordinal_categoricals=["ordinal_feature1", "ordinal_feature2"],
    nominal_categoricals=["nominal_feature1", "nominal_feature2"],
    numericals=["num_feature1", "num_feature2"],
    mode="predict",
    # options=your_config_options,  # uncomment if custom scaling/encoding/transforming or want to adjust paths or anything
    debug=True
)

# Execute Preprocessing
X_preprocessed, recommendations, X_inversed = preprocessor.preprocess_predict(new_data)
```

### Predict Preprocessing Adjustment Configuration

Customize preprocessing steps via the `preprocessor_config.yaml`. Ensure that predict paths are same as training and if custom Scaling/Encoding/Transforming, then just point to the folder where those are located


### Running Preprocessing in Cluster Mode

```python
from datapreprocessor import DataPreprocessor
import pandas as pd

# Load new data for prediction
new_data = pd.read_csv('path_to_new_data.csv')

# Initialize the DataPreprocessor in predict mode
preprocessor = DataPreprocessor(
    model_type="Kmeans",
    y_variable=["target"],
    ordinal_categoricals=["ordinal_feature1", "ordinal_feature2"],
    nominal_categoricals=["nominal_feature1", "nominal_feature2"],
    numericals=["num_feature1", "num_feature2"],
    mode="cluster",
    # options=your_config_options,  # uncomment if custom scaling/encoding/transforming or want to adjust paths or anything
    debug=True
)

# Execute Preprocessing
X_preprocessed, recommendations = preprocessor.preprocess_predict(new_data)
```
## 🧪 Testing

Run unit tests using `pytest` to ensure the package functions as expected:

```bash
pytest
```

## 🛠️ Contributing

Contributions are welcome! Please fork the repository and submit a pull request for any enhancements or bug fixes.

## 📄 License

This project is licensed under the MIT License.


🔧 Continuous Integration

Continuous Integration (CI) ensures that every change made to the DataPreprocessor package is automatically tested and validated. This guarantees that new updates do not break existing functionalities and maintains the package's reliability.
Purpose:

    Automate Testing: Automatically run tests on every push and pull request to detect issues early.
    Maintain Code Quality: Ensure that the codebase remains clean, efficient, and free from regressions.
    Facilitate Collaboration: Allow multiple contributors to work seamlessly without interfering with each other's work.

Action Items:

    Define CI Workflow:
        Use GitHub Actions to automate testing on every push and pull request.
    Ensure Environment Consistency:
        Specify and test against multiple Python versions to ensure compatibility.
    Install Dependencies and Run Tests:
        Automatically install required packages and execute your test suite to verify functionality.

Implementation:

    Move and Rename the Workflow File:

    Move your existing gitactions file to .github/workflows/ci.yml:

mkdir -p .github/workflows
mv .actions/gitactions .github/workflows/ci.yml

Sample ci.yml Workflow:

Here's an example of what your .github/workflows/ci.yml might look like:

    name: CI

    on:
      push:
        branches: [ main ]
      pull_request:
        branches: [ main ]

    jobs:
      build:

        runs-on: ubuntu-latest

        strategy:
          matrix:
            python-version: [3.6, 3.7, 3.8, 3.9, 3.10]

        steps:
        - uses: actions/checkout@v2
        - name: Set up Python ${{ matrix.python-version }}
          uses: actions/setup-python@v2
          with:
            python-version: ${{ matrix.python-version }}
        - name: Install dependencies
          run: |
            python -m pip install --upgrade pip
            pip install -r requirements.txt
            pip install .
        - name: Run tests
          run: |
            pytest

    Explanation:
        Triggers: The workflow runs on every push and pull request to the main branch.
        Environment: Tests are executed across multiple Python versions to ensure compatibility.
        Steps:
            Checkout Code: Retrieves the latest code from the repository.
            Setup Python: Sets up the specified Python version.
            Install Dependencies: Installs necessary packages and the DataPreprocessor package itself.
            Run Tests: Executes the test suite using pytest.

    This CI setup ensures that any changes to your package are immediately tested, maintaining the integrity and reliability of DataPreprocessor.

🛠️ Contributing

Contributions are welcome! Please fork the repository and submit a pull request for any enhancements or bug fixes.
📄 License

This project is licensed under the MIT License.
📚 Resources

    Creating Great README Files for Your Python Projects:
    Learn best practices for crafting effective README files.
    Read the Tutorial

    Awesome Badges:
    A repository of useful badges for your README.
    Check it out

    GitHub README Templates:
    Explore various README templates to inspire your own.
    Visit GitHub README Templates