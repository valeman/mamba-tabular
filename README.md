<div align="center">
  <img src="./docs/images/logo/mamba_tabular.jpg" width="400"/>


[![PyPI](https://img.shields.io/pypi/v/mambular)](https://pypi.org/project/mambular)
![PyPI - Downloads](https://img.shields.io/pypi/dw/mambular)
[![docs build](https://readthedocs.org/projects/mambular/badge/?version=latest)](https://mambular.readthedocs.io/en/latest/?badge=latest)
[![docs](https://img.shields.io/badge/docs-latest-blue)](https://mambular.readthedocs.io/en/latest/)
[![open issues](https://img.shields.io/badge/contributions-welcome-brightgreen.svg?style=flat)](https://github.com/basf/mamba-tabular/issues)


[📘Documentation](https://mambular.readthedocs.io/en/latest/index.html) |
[🛠️Installation](https://mambular.readthedocs.io/en/latest/installation.html) |
[Models](https://mambular.readthedocs.io/en/latest/api/models/index.html) |
[🤔Report Issues](https://github.com/basf/mamba-tabular/issues)
</div>

# Mambular: Tabular Deep Learning with Mamba Architectures

Mambular is a Python package that brings the power of Mamba architectures to tabular data, offering a suite of deep learning models for regression, classification, and distributional regression tasks. Designed with ease of use in mind, Mambular models adhere to scikit-learn's `BaseEstimator` interface, making them highly compatible with the familiar scikit-learn ecosystem. This means you can fit, predict, and evaluate using Mambular models just as you would with any traditional scikit-learn model, but with the added performance and flexibility of deep learning.

## Features

- **Comprehensive Model Suite**: Includes modules for regression (`MambularRegressor`), classification (`MambularClassifier`), and distributional regression (`MambularLSS`), catering to a wide range of tabular data tasks.
- **State-of-the-Art Architectures**: Leverages the Mamba architecture, known for its effectiveness in handling sequential and time-series data within a state-space modeling framework, adapted here for tabular data.
- **Seamless Integration**: Designed to work effortlessly with scikit-learn, allowing for easy inclusion in existing machine learning pipelines, cross-validation, and hyperparameter tuning workflows.
- **Extensive Preprocessing**: Comes with a powerful preprocessing module that supports a broad array of data transformation techniques, ensuring that your data is optimally prepared for model training.
- **Sklearn-like API**: The familiar scikit-learn `fit`, `predict`, and `predict_proba` methods mean minimal learning curve for those already accustomed to scikit-learn.
- **PyTorch Lightning Under the Hood**: Built on top of PyTorch Lightning, Mambular models benefit from streamlined training processes, easy customization, and advanced features like distributed training and 16-bit precision.


## Models

| Model               | Description                                                                                      |
|---------------------|--------------------------------------------------------------------------------------------------|
| `Mambular`          | An advanced model using Mamba blocks specifically designed for various tabular data tasks.       |
| `FTTransformer`     | A model leveraging transformer encoders, as introduced by [Gorishniy et al.](https://arxiv.org/abs/2106.11959), for tabular data. |
| `MLP`               | A classical Multi-Layer Perceptron (MLP) model for handling tabular data tasks.                  |
| `ResNet`            | An adaptation of the ResNet architecture for tabular data applications.                          |
| `TabTransformer`    | A transformer-based model for tabular data introduced by [Huang et al.](https://arxiv.org/abs/2012.06678), enhancing feature learning capabilities. |

All models are available for `regression`, `classification` and distributional regression, denoted by `LSS`.
Hence, they are available as e.g. `MambularRegressor`, `MambularClassifier` or `MambularLSS`



## Documentation

You can find the Mamba-Tabular API documentation [here](https://mamba-tabular.readthedocs.io/en/latest/index.html).

## Installation

Install Mambular using pip:
```sh
pip install mambular
```

## Preprocessing

Mambular simplifies the preprocessing stage of model development with a comprehensive set of techniques to prepare your data for Mamba architectures. Our preprocessing module is designed to be both powerful and easy to use, offering a variety of options to efficiently transform your tabular data.

### Data Type Detection and Transformation

Mambular automatically identifies the type of each feature in your dataset and applies the most appropriate transformations for numerical and categorical variables. This includes:
- **Ordinal Encoding**: Categorical features are seamlessly transformed into numerical values, preserving their inherent order and making them model-ready.
- **One-Hot Encoding**: For nominal data, Mambular employs one-hot encoding to capture the presence or absence of categories without imposing ordinality.
- **Binning**: Numerical features can be discretized into bins, a useful technique for handling continuous variables in certain modeling contexts.
- **Decision Tree Binning**: Optionally, Mambular can use decision trees to find the optimal binning strategy for numerical features, enhancing model interpretability and performance.
- **Normalization**: Mambular can easily handle numerical features without specifically turning them into categorical features. Standard preprocessing steps such as normalization per feature are possible
- **Standardization**: Similarly, Standardization instead of Normalization can be used.
- **PLE**: Periodic Linear Encodings for numerical features can enhance performance for tabular DL methods.


### Handling Missing Values

Our preprocessing pipeline effectively handles missing data by using mean imputation for numerical features and mode imputation for categorical features. This ensures that your models receive complete data inputs without needing manual intervention.
Additionally, Mambular can manage unknown categorical values during inference by incorporating classical <UNK> tokens in categorical preprocessing.


## Fit a Model
Fitting a model in mambular is as simple as it gets. All models in mambular are sklearn BaseEstimators. Thus the `.fit` method is implemented for all of them. Additionally, this allows for using all other sklearn inherent methods such as their built in hyperparameter optimization tools.

```python
from mambular.models import MambularClassifier
# Initialize and fit your model
model = MambularClassifier(
    d_model=64,
    n_layers=8,
    numerical_preprocessing="ple",
    n_bins=50
)

# X can be a dataframe or something that can be easily transformed into a pd.DataFrame as a np.array
model.fit(X, y, max_epochs=150, lr=1e-04)
```

Predictions are also easily obtained:
```python
# simple predictions
preds = model.predict(X)

# Predict probabilities
preds = model.predict_proba(X)
```


## Distributional Regression with MambularLSS

Mambular introduces a cutting-edge approach to distributional regression through its `MambularLSS` module, empowering users to model the full distribution of a response variable, not just its mean. This method is particularly valuable in scenarios where understanding the variability, skewness, or kurtosis of the response distribution is as crucial as predicting its central tendency.

### Key Features of MambularLSS:

- **Full Distribution Modeling**: Unlike traditional regression models that predict a single value (e.g., the mean), `MambularLSS` models the entire distribution of the response variable. This allows for more informative predictions, including quantiles, variance, and higher moments.
- **Customizable Distribution Types**: `MambularLSS` supports a variety of distribution families (e.g., Gaussian, Poisson, Binomial), making it adaptable to different types of response variables, from continuous to count data.
- **Location, Scale, Shape Parameters**: The model predicts parameters corresponding to the location, scale, and shape of the distribution, offering a nuanced understanding of the data's underlying distributional characteristics.
- **Enhanced Predictive Uncertainty**: By modeling the full distribution, `MambularLSS` provides richer information on predictive uncertainty, enabling more robust decision-making processes in uncertain environments.


### Available Distribution Classes:

`MambularLSS` offers a wide range of distribution classes to cater to various statistical modeling needs. The available distribution classes include:

- `normal`: Normal Distribution for modeling continuous data with a symmetric distribution around the mean.
- `poisson`: Poisson Distribution for modeling count data that for instance represent the number of events occurring within a fixed interval.
- `gamma`: Gamma Distribution for modeling continuous data that is skewed and bounded at zero, often used for waiting times.
- `beta`: Beta Distribution for modeling data that is bounded between 0 and 1, useful for proportions and percentages.
- `dirichlet`: Dirichlet Distribution for modeling multivariate data where individual components are correlated, and the sum is constrained to 1.
- `studentt`: Student's T-Distribution for modeling data with heavier tails than the normal distribution, useful when the sample size is small.
- `negativebinom`: Negative Binomial Distribution for modeling count data with over-dispersion relative to the Poisson distribution.
- `inversegamma`: Inverse Gamma Distribution, often used as a prior distribution in Bayesian inference for scale parameters.
- `categorical`: Categorical Distribution for modeling categorical data with more than two categories.

These distribution classes allow `MambularLSS` to flexibly model a wide variety of data types and distributions, providing users with the tools needed to capture the full complexity of their data.


### Getting Started with MambularLSS:

To integrate distributional regression into your workflow with `MambularLSS`, start by initializing the model with your desired configuration, similar to other Mambular models:

```python
from mambular.models import MambularLSS

# Initialize the MambularLSS model
model = MambularLSS(
    dropout=0.2,
    d_model=64,
    n_layers=8,
 
)

# Fit the model to your data
model.fit(
    X, 
    y, 
    max_epochs=150, 
    lr=1e-04, 
    patience=10,     
    family="normal" # define your distribution
    )

```


### Implement your own model:
mambular allows users to easily integrate their custom models into the existing logic. Simply create a pytorch model and define its forward pass. Instead of inheriting from nn.Module, inherit from mambulars BaseModel. Each mambular model takse three arguments. The number of classes, e.g. = 1 for regression or = 2 for binary classification. For distributional regression, while this argument must be provided, it is determined automatically depending on the chosen distribution. Additionally, it takes two arguments directly passed from preprocessor. The cat_feature_info and num_feature_info for categorical and numerical feature information of e.g. the provided shape. Additionally, you can  provide a config argument, which you can either use simialr to the implemented models, or leave empty as shown below. A custom model could hence look just like this:


```python
from mambular.base_models import BaseModel

class MyCustomModel(BaseModel):
    def __init__(
        self,
        cat_feature_info,
        num_feature_info,
        num_classes: int = 1,
        config=None,
        **kwargs,
    ):
        super().__init__(**kwargs)
        self.save_hyperparameters(ignore=["cat_feature_info", "num_feature_info"])

        input_dim = 0
        for feature_name, input_shape in num_feature_info.items():
            input_dim += input_shape
        for feature_name, input_shape in cat_feature_info.items():
            input_dim += 1 

        self.linear = nn.Linear(input_dim, num_classes)

    def forward(self, num_features, cat_features):
        x = num_features + cat_features
        x = torch.cat(x, dim=1)
        
        # Pass through linear layer
        output = self.linear(x)
        return output
```

To leverage the mambular API, you can build a regression, classification or distributional regression model that can leverage all of mambulars built-in methods, by using the following:

```python
from mambular.models import SklearnBaseRegressor

class MyRegressor(SklearnBaseRegressor):
    def __init__(self, **kwargs):
        super().__init__(model=MyCustomModel, config=None, **kwargs)
```

Subsequently, you can fit, evaluate and predict with your model just like with any other mambualr model.
To achieve the same for classification or disrtibutional regression, instead of inheriting from the SklearnbaseRegressor, simply inherit from the SklearnBaseClassifier and SklearnBaseLSS.

## Citation

If you find this project useful in your research, please consider cite:
```BibTeX
@misc{2024,
    title={Mambular: Tabular Deep Learning with Mamba Architectures},
    author={Anton Frederik Thielmann, Manish Kumar, Christoph Weisser, Benjamin Saefken, Soheila Samiee},
    howpublished = {\url{https://github.com/basf/mamba-tabular}},
    year={2024}
}
```

## License
