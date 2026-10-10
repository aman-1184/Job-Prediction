# JobShield AI — Explainable AI-Based Fraud Risk Detection for Online Job Postings

An AI-powered web application that analyzes online job postings and predicts whether they may be legitimate or potentially fraudulent using machine learning and text analysis.

## Overview

Online job scams can expose job seekers to financial loss, identity theft, and misleading employment opportunities. JobShield AI aims to help identify potentially fraudulent job postings using Natural Language Processing (NLP) and machine learning.

The project uses the **Employment Scam Aegean Dataset (EMSCAD)** to train and evaluate classification models. The selected model is a Linear Support Vector Machine (Linear SVM), using TF-IDF features to analyze job-posting text.

The application combines a user-friendly frontend with a Python backend to provide predictions from the trained model.

> **Disclaimer:** JobShield AI is an educational project. Its predictions are not guarantees of legitimacy or fraud. Users should independently verify employers and job offers before sharing personal information or making payments.

## Key Features

- **Job-posting analysis:** Submit job-posting information for classification.
- **Machine learning classification:** Uses a trained Linear SVM model.
- **Natural Language Processing:** Uses TF-IDF text feature extraction.
- **Explainable AI:** Designed to present interpretable indicators contributing to predictions, subject to backend implementation.
- **Interactive dashboard:** Displays project and dataset statistics.
- **Frontend and backend integration:** Connects the web interface to a Python prediction function through an API.
- **Fraud awareness:** Helps users recognize suspicious patterns in job advertisements.

## Dataset

This project uses the Employment Scam Aegean Dataset (EMSCAD).

| Dataset statistic | Count |
|---|---:|
| Total job postings | 17,880 |
| Legitimate postings | 17,014 |
| Fraudulent postings | 866 |
| Legitimate postings | 95.16% |
| Fraudulent postings | 4.84% |

The dataset is imbalanced because fraudulent postings represent a relatively small proportion of the total records. The selected Linear SVM uses class weighting to help account for this imbalance.

## Machine Learning Methodology

The project follows these main steps:

1. **Data preprocessing:** Clean and prepare job-posting text using the preprocessing pipeline developed in Google Colab.
2. **Text feature extraction:** Convert textual data into numerical features using TF-IDF.
3. **Model training:** Train and compare Logistic Regression, Multinomial Naive Bayes, and Linear SVM.
4. **Model selection:** Select Linear SVM based on the evaluation results.
5. **Prediction:** Use the fitted TF-IDF vectorizer and trained model to classify submitted job postings.
6. **Web integration:** Connect the prediction function to the frontend through an HTTP API.

### Models evaluated

- Logistic Regression
- Multinomial Naive Bayes
- Linear Support Vector Machine (Linear SVM)

### Selected model

The final model uses the following configuration:

```python
LinearSVC(
    C=1.0,
    class_weight="balanced",
    random_state=42,
    max_iter=10000
)
```

The model was trained and evaluated in Google Colab. The application should use the existing fitted model and vectorizer rather than retraining the model for each prediction.

## Experimental Results

The following results were obtained on the test set.

| Model | Accuracy | Precision | Recall | F1-score |
|---|---:|---:|---:|---:|
| Logistic Regression | 96.67% | 60.80% | 87.86% | 71.87% |
| Multinomial Naive Bayes | 97.01% | 100.00% | 38.15% | 55.23% |
| **Linear SVM** | **98.35%** | **81.67%** | **84.97%** | **83.29%** |

Linear SVM achieved the highest accuracy and F1-score among the three evaluated models. Its recall indicates that it identified a substantial proportion of fraudulent postings in the test set.

### Linear SVM confusion matrix

The confusion matrix uses legitimate postings as class 0 and fraudulent postings as class 1.

| | Predicted legitimate | Predicted fraudulent |
|---|---:|---:|
| Actual legitimate | 3,370 | 33 |
| Actual fraudulent | 26 | 147 |

These results reflect the evaluated test set and should not be interpreted as a guarantee of performance on future job postings.

## System Architecture

The application follows a frontend-backend architecture.

```text
User
  |
  v
JobShield AI Frontend
  |
  v
HTTP API (FastAPI)
  |
  v
Existing Python Prediction Function
  |
  v
Text Preprocessing
  |
  v
Fitted TF-IDF Vectorizer
  |
  v
Trained Linear SVM
  |
  v
Prediction Response (JSON)
  |
  v
Frontend Results Dashboard
```

The frontend collects job-posting details and sends them to the backend. The backend applies the appropriate preprocessing, transforms the text using the fitted vectorizer, and generates a prediction using the trained Linear SVM.

The backend returns the result to the frontend for display.

**Deployment note:** If the backend runs in Google Colab, the runtime must remain active and an accessible API endpoint must be available. A temporary tunnel URL may change when the session restarts.

## Technology Stack

| Component | Technology |
|---|---|
| Programming language | Python |
| Notebook environment | Google Colab |
| Machine learning | scikit-learn |
| Text processing | NLP and TF-IDF |
| Classification algorithm | Linear SVM |
| API backend | FastAPI (for API integration) |
| Frontend | Claude Artifact (React/TypeScript, if used) |
| Dataset | EMSCAD |

The frontend and API technologies describe the intended integration architecture; adapt this section if your final implementation uses different technologies.

## Getting Started

### Prerequisites

- A Google account to access Google Colab.
- Python and the required machine learning libraries.
- The trained model and fitted TF-IDF vectorizer.
- The frontend Artifact.
- FastAPI and an ASGI server if using the API integration described above.

### 1. Set up the model in Google Colab

Open your existing notebook containing the trained model and prediction function.

Verify that:
- The trained Linear SVM is available.
- The fitted TF-IDF vectorizer is available.
- The original preprocessing steps are preserved.
- The prediction function accepts the expected job-posting inputs.

Do not retrain the model merely to run the frontend.

### 2. Install API dependencies

If you are using FastAPI, run the following in a Colab cell:

```python
!pip install fastapi uvicorn
```

Additional dependencies may be needed depending on how you expose the API.

### 3. Expose the prediction function

Create a FastAPI endpoint that calls your existing prediction function and returns the result as JSON.

The endpoint must use the same text fields and preprocessing pipeline as the original training process. Configure CORS for the frontend origin when required.

The API URL and request schema depend on your actual implementation and should be documented once finalized.

### 4. Connect the frontend

Configure the frontend to send the job-posting fields to the API endpoint.

The frontend should:
1. Validate the submitted form.
2. Send the request to the backend.
3. Display a loading indicator.
4. Show the actual prediction returned by the model.
5. Display an appropriate error message if the backend is unavailable.

### 5. Test the application

Test the application using both legitimate and suspicious sample job postings. Confirm that the frontend displays the actual backend predictions rather than hardcoded or randomly generated results.

## Explainable AI

Explainability is an important objective of this project. A possible approach for a fitted linear text classifier is to inspect the model coefficients and the TF-IDF features of a particular posting.

For a binary Linear SVM, these coefficients can help identify which text features contribute toward either class. A backend can calculate and return influential terms to support an explanation of a prediction.

Explanations must be generated from the actual fitted model and submitted text. They should not be invented or presented as proof that a posting is fraudulent.

The standard Linear SVM configuration used in this project does not automatically produce calibrated fraud probabilities. Therefore, an SVM decision score should not be displayed as a fraud probability without an appropriate calibration process.

## Project Structure

A possible repository structure is:

```text
JobShield-AI/
├── README.md
├── frontend/
│   └── (frontend source, if exported)
├── backend/
│   └── (FastAPI source, if exported)
├── notebooks/
│   └── (Google Colab notebook, if shared)
└── requirements.txt
```

Adapt this structure to match the files actually included in your repository. Do not upload large datasets, model files, or credentials unless appropriate and permitted.

## Limitations

- Model predictions may contain false positives and false negatives.
- The dataset may not represent all current job markets, industries, or scam techniques.
- The dataset is imbalanced, which affects how evaluation metrics should be interpreted.
- Text-based classification cannot independently verify an employer's identity.
- Performance on new job postings may differ from test-set performance.
- Live predictions depend on the backend being available and using the correct preprocessing pipeline.

## Future Improvements

- Improve the explanation interface with model-based feature contributions.
- Evaluate performance on additional and more recent job-posting data.
- Explore model calibration if meaningful probability estimates are needed.
- Add robust input validation and API error handling.
- Deploy the backend to a persistent hosting service for reliable availability.
- Improve the dashboard's accessibility and usability.
- Evaluate model performance across relevant job categories and text fields.

## Contributing

Contributions and suggestions are welcome.

1. Fork the repository.
2. Create a branch for your change.
3. Make and test your changes.
4. Submit a pull request describing the improvement.

Please avoid committing credentials, API keys, personal data, or other sensitive information.

## License

No license has been specified yet. Add a `LICENSE` file before describing this repository as open source. Choose a license appropriate for your project and ensure that the dataset's terms permit your intended use.

## Acknowledgements

- Employment Scam Aegean Dataset (EMSCAD) for the job-posting data used in this project.
- The scikit-learn project for machine learning and text feature extraction tools.
- Google Colab for the notebook environment.
- Claude Artifact for assistance with the frontend development workflow.

## Author

Developed as a college mini-project on explainable AI and fraudulent online job-posting detection.

For questions, suggestions, or issues, please open an issue in this GitHub repository.
