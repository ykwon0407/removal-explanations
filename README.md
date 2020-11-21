# Removal-based explanations

This repository implements a large number of *removal-based explanations*, a class of model explanation approaches that unifies many existing methods (e.g., SHAP, LIME, Meaningful Perturbations, L2X, permutation tests). Our paper (available on [arXiv](https://arxiv.org/abs/2011.03623)) presents a framework that allows us to implement these methods in a lightweight, modular codebase.

Our implementation does not take advantage of certain approximation approaches that make these methods fast in practice, so you may prefer to continue using the original implementations (e.g., [SHAP](https://github.com/slundberg/shap), [LIME](https://github.com/marcotcr/lime), [SAGE](https://github.com/iancovert/sage/)). We also haven't implemented every method, e.g., we do not support image blurring or any feature selection approaches.

## Usage

To begin, you need to clone the repository and install the library into your Python environment:

```bash
pip install .
```

Our code is designed around the framework described in the paper. Each model explanation method is specified by three choices:

1. **Feature removal:** how the model is evaluated when features are held out
2. **Model behavior:** a target quantity that's analyzed as features are removed (e.g., an individual prediction, the model loss)
3. **Summary technique:** how each feature's influence is quantified (e.g., using Shapley values)

The general use pattern looks like this:

```python
from rexplain import removal, behavior, summary

# Get model and data
X, Y = ...
model = ...

# 1) Feature removal
extension = removal.MarginalExtension(X[:512], model)

# 2) Model behavior
game = behavior.PredictionGame(X[0], extension)

# 3) Summary technique
attr = summary.BanzhafValue(game)
plt.bar(np.arange(len(attr)), attr)
```

For usage examples, see the following notebooks:

- [Census]() shows how to explain individual predictions.
- [MNIST]() shows how to explain the model's loss for individual predictions.
- [Breast cancer (BRCA)]() shows how to explain the dataset loss.

# Authors

- Ian Covert (<icovert@cs.washington.edu>)
- Scott Lundberg
- Su-In Lee
