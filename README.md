# Real-Time-Stock-Price-Forecasting-with-regular-Model-Updates

This repository features a real-time stock price forecasting system using Django, Docker, Jenkins, and OpenShift. It collects stock data, trains forecasting models, and shows forecasts on a user-friendly web interface.


## Structure of the Project

#### Data Collection and Preprocessing

The data directory contains scripts for gathering stock data from the Yahoo Finance API. The collected data is then preprocessed to format and organize it for further use. This involves dividing the data into historical data (older than 30 days) and current data (within the last 30 days). The processed data is stored in a designated folder.

#### Training and Updating the Model

The models section is a central part of the project. It includes scripts and files related to model training and updating. The process starts with the model_training script, which trains an initial forecasting model using historical data. Next, the model_update script refines the model using historical and current data. The model_evaluation script assesses both models against newly collected real-time data. If the updated model performs better than the initial model, it replaces the initial one. This updating process occurs hourly after data retrieval.

#### Web Application

The stock_predict_webapp directory contains a Django application that serves as the system's front end. The web application allows users to input a desired time and view forecasted stock prices for the top 10 stocks using the trained models. This web app integration with the forecasting models demonstrates the system's practical utility.

#### Dockerization

Docker is used to encapsulate various components of the project. Three distinct Dockerfiles are provided for different parts of the project:

- data: This Dockerfile handles data collection and preprocessing. It runs a script that gathers data from the Yahoo Finance API and performs preprocessing steps before storing the prepared data in a shared volume.
- model: This Dockerfile manages the training, evaluation, and updating of forecasting models. The script within this container uses data from the shared volume to train and evaluate the models and then stores the trained models back in the shared volume.
- webapp: This Dockerfile is for the web application and handles deploying the front-end interface. It interacts with the stored models in the shared volume to generate forecasts based on user input.

#### OpenShift Deployment

OpenShift is used for streamlined deployment. The OpenShift directory includes configuration files necessary for deploying the project's components on an OpenShift cluster. There are two deployment files:

- deployment-model: This configuration file orchestrates the execution of the data processing and model training Docker images. It manages networking, and the shared volume, and ensures at least one replica is always running.
- django-app-deployment: This deployment configuration ensures optimal load balancing by maintaining multiple replicas, providing consistent availability of the web application.

#### Jenkins Integration

Jenkins is crucial for continuous integration and deployment in this project. The jenkins directory contains pipeline configurations stored in the Jenkinsfile. These configurations automate data processing, model training, and deployment processes. The pipelines can be triggered by various events, such as specific time intervals or codebase changes. Integrating Jenkins highlights the project's practical use of continuous integration in a real-world scenario.



## The Workflow

#### Data Collection
- Used the `yfinance` library to gather stock data from Yahoo Finance for the top 10 popular stocks.
- Segregated data into historical and current sets.
- Implemented preprocessing steps to ensure data quality and consistency.

#### Model Training
- Used `HistGradientBoostingRegressor` for predictive modelling.
- Saved trained models in `models/saved_models`.
- Divided the process into three scripts:
  - `model_training`: Trains the initial model with historical data and saves it.
  - `model_update`: Updates the model using both historical and current data, then saves the updated model.
  - `model_evaluation`: Evaluates models against real-time data and replaces the initial model with the updated one if it performs better. This process runs hourly after new data retrieval.

#### Web Application
- Developed a user-friendly web app using the Django framework.
- Enabled users to input a desired time and view predicted stock prices for the top 10 stocks.
- Integrated trained models to provide real-time predictions.

#### Dockerization
- Created Dockerfiles for:
  - Data collection and processing.
  - Model training, evaluation, and updating.
  - Web application deployment.
- Each Dockerfile encapsulates specific functionality, ensuring modularity and portability.

#### OpenShift Cluster
- Set up a local OpenShift cluster using CodeReady Containers (CRC).
- Implemented two deployment files:
  - `deployment-model`: Manages sequential execution of data processing and model training, networking, and shared volumes, and ensures minimum replica availability.
  - `django-app-deployment`: Manages web server deployment, load balancing with multiple replicas, and ensures continuous application availability.

#### Jenkins Integration
- Configured a Jenkins instance on an Azure VM.
- Installed plugins, including the OpenShift plugin, for seamless integration.
- Defined a Jenkinsfile to automate the `deployment-model` pipeline, running it hourly to showcase continuous integration and deployment.


## Getting Started

To get the project up and running, follow these steps:

1. Clone the repository to your local machine.

2. Create and activate a Python virtual environment:
   ```sh
   python -m venv venv
   source venv/bin/activate
   ```
   
3. Install the required dependencies:
   ```sh
   pip install -r requirements.txt
   ```
   
4. Build the Docker images using the provided Dockerfiles.

5. Deploy the components on an OpenShift cluster using the configuration files in the `openshift` directory.

6. Configure Jenkins by installing necessary plugins and setting up pipeline jobs using the provided `Jenkinsfile`.
