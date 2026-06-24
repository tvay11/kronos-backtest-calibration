# Kronos Backtest Calibration Sweep

## Project Overview

This repository contains a production-grade walk-forward backtest calibration sweep for the Kronos forecasting model. The primary objective is to systematically evaluate which model temperature setting produces honest probability bands, targeting an 80% empirical coverage rate within the p10/p90 forecast intervals. The sweep executes 100 Monte Carlo samples across three distinct temperature configurations (0.6, 0.9, 1.2) for highly liquid financial assets including NVDA, AAPL, and SPY. This calibration process is essential for ensuring that the model's probabilistic forecasts are well-calibrated and reliable for downstream quantitative decision-making.

## Core Workflow

The backtesting framework implements a rigorous walk-forward simulation that processes historical stock data through a rolling window methodology. At each step, the model is trained on a fixed historical window and generates out-of-sample forecasts for multiple horizons. The workflow systematically iterates over temperature parameters, generating ensembles of forecast paths to construct empirical probability distributions. For each temperature setting, the framework computes the empirical coverage of the p10/p90 bands and compares it against the theoretical 80% target. The calibration process identifies the temperature that minimizes the coverage error, thereby producing the most honest uncertainty estimates.

## Key Features

- **Walk-Forward Backtesting Simulation**: Implements a rolling window approach with custom forecast horizons of 5, 10, 20, and 60 trading days, ensuring temporal consistency and avoiding look-ahead bias.

- **Temperature Calibration Sweep**: Evaluates model temperatures at 0.6, 0.9, and 1.2 to systematically measure and adjust forecast uncertainty. Each temperature configuration is tested with 100 ensemble samples to ensure statistical robustness.

- **Coverage Percentage Metric**: Computes the empirical coverage rate of the p10/p90 forecast bands, with a target of 80%. This metric directly quantifies the calibration quality of the probabilistic forecasts.

- **Point Forecasting Error Evaluation**: Calculates Mean Absolute Percentage Error (MAPE) for point forecasts across all horizons and temperature settings, providing a complementary accuracy metric alongside calibration quality.

- **Multi-Platform Compatibility**: The codebase is designed to run seamlessly across three environments: local development machines, Kaggle notebooks with T4 x2 GPU acceleration, and Google Colab. Each platform-specific notebook handles environment setup and resource allocation automatically.

## Technology Stack

- **Python**: Core programming language for all data processing, modeling, and analysis.
- **Pandas**: Data manipulation and time series handling for historical stock data.
- **NumPy**: Numerical computing for array operations and statistical calculations.
- **Matplotlib / Seaborn**: Visualization libraries for generating calibration plots, coverage charts, and error analysis figures.
- **IPython**: Interactive computing environment for notebook execution and result inspection.
- **Jupyter Notebooks**: Primary development and execution environment for all three platform-specific implementations.

## Installation and Running

### Local Environment

1. Clone the repository:
   ```
   git clone https://github.com/yourusername/kronos-backtest-calibration.git
   cd kronos-backtest-calibration
   ```

2. Create and activate a virtual environment:
   ```
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. Install required dependencies:
   ```
   pip install pandas numpy matplotlib seaborn jupyter ipython
   ```

4. Configure data access: Set up environment variables or a `.env` file with any required API tokens or data source credentials for retrieving historical stock data.

5. Launch Jupyter and run the local notebook:
   ```
   jupyter notebook Kronos_Backtest.ipynb
   ```

### Kaggle Platform

1. Upload the `Kronos_Backtest_Kaggle.ipynb` notebook to your Kaggle account.
2. Attach a GPU accelerator (T4 x2 recommended) in the notebook settings.
3. Ensure the Kaggle dataset containing historical stock data is accessible via the Kaggle API or uploaded as a dataset.
4. Run all cells sequentially. The notebook will automatically handle GPU-accelerated tensor operations for the ensemble sampling.

### Google Colab

1. Open `Kronos_Colab.ipynb` in Google Colab by uploading the file or opening from Google Drive.
2. Enable GPU runtime: Runtime -> Change runtime type -> Hardware accelerator -> GPU.
3. Mount Google Drive if storing or retrieving data from Drive:
   ```
   from google.colab import drive
   drive.mount('/content/drive')
   ```
4. Execute the notebook cells in order. The Colab notebook includes automatic dependency installation and environment configuration.

## Repository Structure

```
kronos-backtest-calibration/
|-- Kronos_Backtest.ipynb          # Local walk-forward backtest re-runs
|-- Kronos_Backtest_Kaggle.ipynb   # Kaggle GPU-accelerated calibration sweep
|-- Kronos_Colab.ipynb             # Google Colab backtesting sweep
|-- README.md                      # This documentation file
```

## Results and Interpretation

Upon completion of the calibration sweep, the notebooks output a comprehensive analysis including:

- Coverage rate tables for each temperature and asset combination.
- Calibration curves showing empirical vs. theoretical coverage.
- MAPE error metrics for point forecast evaluation.
- Recommended temperature settings for each asset based on coverage accuracy.

The optimal temperature is selected as the one that minimizes the absolute deviation from the 80% coverage target across all forecast horizons. This calibrated temperature is then used for production forecasting with the Kronos model.