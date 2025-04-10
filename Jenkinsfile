pipeline {
    agent any

    environment {
        AZURE_CREDENTIALS_ID = 'python-azure-principle'
        RESOURCE_GROUP = 'myPythonAppyashp1_group'
        APP_SERVICE_NAME = 'myPythonAppyashp1'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/yash8209/python_basic_api_azure_test.git'
            }
        }

        stage('Set Up Python Environment') {
            steps {
                bat 'python -m venv venv'
                bat 'pip install fastapi'
                bat 'pip install uvicorn'
                bat 'pip install pytest'
            }
        }

        stage('Package Code') {
            steps {
                bat 'if exist app.zip del app.zip'
                bat 'powershell Compress-Archive -Path * -DestinationPath app.zip -Force'
            }
        }

        stage('Deploy to Azure') {
            steps {
                withCredentials([azureServicePrincipal(credentialsId: AZURE_CREDENTIALS_ID)]) {
                    bat 'az login --service-principal -u %AZURE_CLIENT_ID% -p %AZURE_CLIENT_SECRET% --tenant %AZURE_TENANT_ID%'
                    bat 'az webapp deploy --resource-group %RESOURCE_GROUP% --name %APP_SERVICE_NAME% --src-path app.zip --type zip'

                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment Successful!'
        }
        failure {
            echo '❌ Deployment Failed!'
        }
    }
}

