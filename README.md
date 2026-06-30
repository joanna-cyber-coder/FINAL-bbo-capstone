Black-Box Optimisation (BBO) Capstone Project

This repository contains the dataset history and the optimization approach developed over a 10-week iterative optimization protocol across 8 hidden black-box functions.

Project Overview (Non-Technical Summary)

This project optimises eight hidden, complex systems without knowing their underlying mathematical formulas. These black-box systems range from maximising recipe scores to fine-tuning complicated software configurations. By deploying an advanced strategy called Bayesian Optimisation, we built a digital assistant that acts as a highly efficient explorer. This explorer systematically analyses previous results, balances the risks of testing unknown territories against polishing known successful setups, and recommends the next best settings to try. Over thirteen weeks, this approach successfully avoided deceptive dead ends and navigated vast spaces, discovering peak performance configurations across all eight unique tasks.


## Project Documentation Links
* **[Datasheet for Project Dataset](DATASHEET.md)** - Full breakdown of motivation, dataset composition, and collection history.
* **[Model Card for SCB-CR Approach](MODEL_CARD.md)** - Operational details, strategy evolution, and performance benchmarks.

## Final Performance Benchmarks


| Function | Dimensions | Optimization Task | Peak Output ($Y_{max}$) |
| :--- | :---: | :--- | :---: |
| Function 1 | 2D | Radiation Source Localization | 1.043e-7 |
| Function 2 | 2D | Noisy ML Model Hyperparameters | 0.7373 |
| Function 3 | 3D | Drug Adverse Reaction Minimization | -0.0236 |
| Function 4 | 4D | Warehouse Placement Optimization | -3.8206 |
| Function 5 | 4D | Chemical Yield Maximization | 1258.72 |
| Function 6 | 5D | Cake Recipe Score Maximization | -0.6360 |
| Function 7 | 6D | High-Dim ML Accuracy Tuning | 1.3649 |
| Function 8 | 8D | High-Dim Black-Box Optimization | 9.5984 |

