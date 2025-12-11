# Machine Learning-Based Pultrusion Process Modeling and Optimization

[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)

This repository contains the complete implementation of machine learning surrogate models for pultrusion process prediction and optimization, as described in:

> [Razie Izadi et al.], "Machine Learning-Enhanced Modelling and Experimental Analysis of Foam-Core Thermoplastic Pultrusion" [Journal Name], [Year]. DOI: [to be added]

## Overview

Pultrusion process modeling traditionally relies on computationally expensive finite element simulations (7+ minutes per case). This work demonstrates that machine learning surrogate models can achieve:
- **17,000× computational speedup** (25 ms vs. 7 minutes)
- **>99.98% prediction accuracy** (R² > 0.9998)
- **Real-time process optimization** (44 seconds for complete multi-objective optimization)

## Repository Structure
```
├── data/
│   ├── cure_data_examples.txt          # Example cure degree profiles
│   ├── temperature_data_examples.txt   # Example temperature profiles
│   └── README.txt                      # Data format description
├── scripts/
│   ├── 01_data_processing.py           # Data organization and preprocessing
│   ├── 02_model_training.py            # ML model training and evaluation
│   ├── 03_prediction_tool.py           # Fast prediction interface
│   └── 04_optimization_tool.py         # Process parameter optimization
├── models/
│   ├── models_cure.pkl                 # Trained models (generated after training)
│   └── models_temp.pkl                 # Trained models (generated after training)
├── requirements.txt                     # Python dependencies
└── README.md                            # This file
```

## Features

### 1. Data Processing (`01_data_processing.py`)
- Organizes raw simulation data from parametric studies
- Creates structured datasets with proper headers
- Exports organized data to Excel format
- Generates design charts (cure degree and temperature maps)

**Input:** Raw text files with cure and temperature data  
**Output:** Organized Excel file, publication-quality design charts (600 DPI)

### 2. Model Training (`02_model_training.py`)
- Trains three ML algorithms: Neural Networks, Random Forest, Gradient Boosting
- Performs comprehensive model evaluation (R², MSE, MAE)
- Conducts feature importance analysis
- Generates model comparison visualizations
- Saves trained models for deployment

**Output:** 
- Trained model files (.pkl)
- Performance comparison charts
- Feature importance analysis
- Excel file with detailed metrics

### 3. Fast Prediction Tool (`03_prediction_tool.py`)
- Predicts complete cure and temperature profiles in ~25 milliseconds
- Generates publication-quality visualizations
- Exports results to Excel for further analysis
- Provides computational performance metrics

**Input:** Velocity (m/min), Die Temperature (°C)  
**Output:** Complete spatial profiles, key metrics, plots (PNG/PDF), Excel data

### 4. Optimization Tool (`04_optimization_tool.py`)
- Multi-objective optimization (thermal management, production rate, energy efficiency)
- Hybrid optimization strategy (Differential Evolution + SLSQP)
- Design space exploration and visualization
- Constraint handling and feasibility analysis

**Input:** Target cure degree, constraints, optimization objective  
**Output:** Optimal process parameters, design space maps, trade-off analysis

## Installation

### Requirements
- Python 3.8 or higher
- Google Colab (recommended) or local Python environment

### Setup

1. Clone the repository:
```bash
git clone https://github.com/yourusername/pultrusion-ml.git
cd pultrusion-ml
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. For Google Colab users:
   - Upload the scripts to your Colab environment
   - Run the installation cell in each notebook

## Quick Start

### Step 1: Process Your Data
```python
# Run in Google Colab or local environment
python scripts/01_data_processing.py

# Follow prompts to:
# - Enter velocity range (e.g., 0.11 to 0.21 m/min, step 0.01)
# - Enter temperature range (e.g., 110 to 140°C, step 10)
# - Upload cure and temperature data files
```

### Step 2: Train ML Models
```python
python scripts/02_model_training.py

# This will:
# - Train Neural Network, Random Forest, and Gradient Boosting models
# - Generate performance metrics and visualizations
# - Save trained models to models/ directory
```

### Step 3: Make Predictions
```python
python scripts/03_prediction_tool.py

# Upload trained models (models_cure.pkl, models_temp.pkl)
# Enter process parameters:
# - Velocity: 0.15 m/min
# - Die Temperature: 120°C
# 
# Output: Complete profiles in ~25 milliseconds
```

### Step 4: Optimize Process Parameters
```python
python scripts/04_optimization_tool.py

# Upload trained models
# Define optimization scenario:
# - Target cure degree: 0.99
# - Constraints: velocity, temperature ranges
# - Objective: minimize temperature / maximize velocity / minimize energy
#
# Output: Optimal parameters in ~44 seconds
```

## Example Data

The `data/` directory contains example datasets from parametric finite element simulations:

- **cure_data_examples.csv**: Cure degree profiles for selected velocity-temperature combinations
- **temperature_data_examples.csv**: Temperature profiles for the same conditions

Each file contains axial distance (0-1000 mm) versus output variable for multiple process conditions.

### Data Format
```
Axial_Distance  Cure_Degree  (or Temperature)
0.0             0.0001
5.0             0.0015
10.0            0.0042
...
1000.0          0.9935
```

Multiple process conditions are stacked sequentially in the order: velocity increases, then temperature increases.

## Computational Performance

| Task | Simulation Time | ML Time | Speedup |
|------|----------------|---------|---------|
| Single Prediction | 435 seconds | 0.025 seconds | 17,237× |
| Optimization | ~88 days | 44 seconds | 150,000× |

## Model Performance

| Model | Cure R² | Cure MAE | Temp R² | Temp MAE |
|-------|---------|----------|---------|----------|
| Neural Network | 0.999797 | 0.00386 | 0.998980 | 0.707°C |
| Random Forest | 0.998563 | 0.00563 | 0.999455 | 0.398°C |
| Gradient Boosting | 0.999480 | 0.00358 | **0.999832** | **0.238°C** |

**Best Models Used:**
- Cure Prediction: Neural Network (R² = 0.999797)
- Temperature Prediction: Gradient Boosting (R² = 0.999832)

## Citation

If you use this code in your research, please cite:
```bibtex
@article{izadi2025Machine,
  title={Machine Learning-Enhanced Modelling and Experimental Analysis of Foam-Core Thermoplastic Pultrusion},
  author={Razie Izadi et al},
  journal={Journal Name},
  year={2025},
  doi={to be added}
}
```

## Contact

For questions or collaboration opportunities:
- Email: razieizadi@gmail.com

## Requirements File Content
```
numpy>=1.21.0
pandas>=1.3.0
matplotlib>=3.4.0
scikit-learn>=1.0.0
scipy>=1.7.0
openpyxl>=3.0.0
```

## Troubleshooting

### Common Issues

**Issue:** "Models not found" error in prediction/optimization scripts  
**Solution:** Ensure you've run the training script first and the .pkl files are in the models/ directory

**Issue:** Font warnings in visualizations  
**Solution:** The scripts automatically handle font installation for Google Colab. For local use, install msttcorefonts

**Issue:** Memory errors during training  
**Solution:** Reduce the number of data points or use Google Colab Pro for more RAM

## Future Development

Potential extensions of this work:
- [ ] Physics-informed neural networks (PINNs) for improved extrapolation
- [ ] Transfer learning for new material systems
- [ ] Real-time process control integration
- [ ] Multi-objective Pareto frontier optimization
- [ ] Uncertainty quantification with Bayesian models

## Version History

- **v1.0.0** (2025): Initial release with trained models and optimization tools

---

