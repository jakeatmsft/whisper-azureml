# whisper-azureml

## Introduction
Whisper is a state-of-the-art speech to text model developed by OpenAI and recently released to the public.  It is easy to use and offers very accurate results for Speech-To-Text processing in a variety of environments.  This library serves to enable deployment of the Whisper models in AzureML using Managed Endpoints to provide easy, scalable, and performant inferencing of the models.


## Pre-requisites
- Azure Subscription
  -  [https://azure.microsoft.com/en-us/free/]
- Azure Machine Learning Workspace
  - [https://learn.microsoft.com/en-us/azure/machine-learning/quickstart-create-resources]
- Azure CLI
  - [https://learn.microsoft.com/en-us/dotnet/azure/install-azure-cli]
- AzureML CLIv2
  - [https://learn.microsoft.com/en-us/azure/machine-learning/how-to-configure-cli]


## Steps
- Pull Down Repository
- Deploy Environment Setup
  - Navigate to the "notebooks" folder and begin by opening the CreateEnvironment
  - Run the first cell to load the Whisper model and save it in your local environment.
  - Next run cells to connect to AzureML Workspace and deploy the model as well as the inferencing environment.
- Deploy Managed Endpoint
  - Navigate to the "deploy" folder and run the "deploy.sh" bash script to deploy the model:
    - The script will first deploy a "Managed Endpoint" that will serve as an access point for inferencing.
    - Then the script will create a "Deployment" to the "Managed Endpoint" that contains the actual model to be used.
      - The "Deployment" definition includes the following:
        - Model - The model to be used for scoring requests.
        - Code Configuration - The script to be used to initialize and run the model
        - Environment: The containerized environment to be used for execution of the model
        - Instance Type: Type of server (CPU/GPU, Memory) to be used to scoring.
        - Instance Count: The number of servers required to accommodate requests.
- Test the model by opening the "test-whisper-endpoint.ipynb":
  - Fill in the endpoint details from the deployment.
  - Run notebook to encode the "sample1.flac" audio and submit the audio file for Speech-To-Text recognition.
  - Analyze the result of the recognition in the response to validate that the model is working as expected. 



## References
- Whisper - [https://github.com/openai/whisper]
- AzureML - [https://learn.microsoft.com/en-us/azure/machine-learning/overview-what-is-azure-machine-learning]
- AzureML - Managed Endpoint [https://learn.microsoft.com/en-us/azure/machine-learning/how-to-deploy-managed-online-endpoints?tabs=azure-cli]
        