# House Price Prediction Model Deployment


## Overview

This project implements an end-to-end machine learning pipeline for house price prediction, featuring automated model training, continuous deployment, and efficient inference capabilities. The system is designed to automatically retrain and deploy models when code changes are detected, ensuring that the best-performing model is always in production.

## Features

- **Automated Model Training**: Pipeline automatically trains models using the latest data and code
- **Continuous Deployment**: Models are automatically redeployed when code changes are detected
- **Efficient Inference**: System handles both batch and real-time prediction requests
- **MLflow Integration**: Comprehensive experiment tracking and model version management
- **Scalable Architecture**: Prediction service capable of handling multiple concurrent requests
- **Interactive UI**: Integration with Streamlit for user-friendly interfaces

## System Architecture

```
House-Price-Prediction/
│
├── pipelines/
│   ├── training_pipeline.py     # Handles model training workflow
│   ├── deployment_pipeline.py   # Manages continuous deployment
│   └── inference_pipeline.py    # Processes batch inference requests
│
├── models/
│   ├── model.py                 # Core model definition
│   └── preprocessing.py         # Data preprocessing utilities
│
├── config/
│   └── config.yaml              # Configuration parameters
│
├── utils/
│   ├── data_loader.py           # Handles data ingestion
│   └── dynamic_importer.py      # Dynamically imports data for predictions
│
├── app/
│   └── streamlit_app.py         # User interface for model interaction
│
└── tests/
    └── test_model.py            # Unit tests for model components
```

## Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/house-price-prediction.git
cd house-price-prediction

# Create and activate virtual environment
python -m venv venv
source venv/bin/activate  # On Windows use: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Set up MLflow
mlflow server --backend-store-uri sqlite:///mlflow.db --default-artifact-root ./mlruns
```

## Usage

### Training Pipeline

The training pipeline automatically trains models using the latest data and code:

```bash
python pipelines/training_pipeline.py
```

### Deployment Pipeline

The deployment pipeline checks for code changes and deploys the best-performing model:

```bash
python pipelines/deployment_pipeline.py
```

### Making Predictions

#### Batch Predictions

```python
from pipelines.inference_pipeline import InferencePipeline

# Initialize the inference pipeline
inference = InferencePipeline(model_uri="models:/house_price_model/Production")

# Load batch data and make predictions
predictions = inference.predict(batch_data_path="path/to/data.csv")
```

#### API Integration

```python
import requests
import json

# Prepare data for prediction
data = {"features": [features_list]}

# Send prediction request
response = requests.post(
    "http://localhost:5000/invocations",
    data=json.dumps(data),
    headers={"Content-Type": "application/json"}
)

# Get predictions
predictions = response.json()
```

### Streamlit Application

To run the interactive UI:

```bash
streamlit run app/streamlit_app.py
```

## MLflow Integration

This project leverages MLflow for experiment tracking and model management:

- **Experiment Tracking**: All training runs are logged with parameters, metrics, and artifacts
- **Model Registry**: Versioned models are stored in the MLflow Model Registry
- **Model Serving**: Models are served using MLflow's prediction service

Access the MLflow UI at `http://localhost:5000` after starting the MLflow server.

## Continuous Deployment Workflow

1. Changes to model code or data are detected by the deployment pipeline
2. The pipeline triggers a new training run with the updated code
3. Model performance is evaluated against predefined metrics
4. If the new model outperforms the current production model, it is promoted to production
5. The production model is automatically deployed for serving predictions

## Future Enhancements

- Implement A/B testing framework for model comparison
- Add support for more advanced feature engineering techniques
- Integrate with cloud platforms for scalable deployment
- Develop more sophisticated prediction monitoring and alerting
- Experiment with different modeling approaches to improve accuracy

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Acknowledgments

- [MLflow](https://mlflow.org/) for experiment tracking and model serving
- [Scikit-learn](https://scikit-learn.org/) for machine learning algorithms
- [Streamlit](https://streamlit.io/) for interactive UI components
