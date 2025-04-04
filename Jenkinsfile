pipeline {
    agent any
    environment {
        AZURE_CREDENTIALS_ID = 'python-azure-principle'
        RESOURCE_GROUP = 'yashp_resource1'
        APP_SERVICE_NAME = 'myPythonAppyashp1'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/yash8209/python_basic_api_azure_test.git'
            }
        }

        stage('Setup Python') {
            steps {
                bat 'python --version'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'pip install fastapi'
                bat 'pip install uvicorn'
                bat 'pip install pytest'
            }
        }

       

        stage('Package Application') {
            steps {
                bat 'powershell Compress-Archive -Path * -DestinationPath app.zip -Force'
            }
        }

        stage('Deploy to Azure') {
            steps {
                withCredentials([azureServicePrincipal(credentialsId: AZURE_CREDENTIALS_ID)]) {
                    bat '''
                        az login --service-principal -u %AZURE_CLIENT_ID% -p %AZURE_CLIENT_SECRET% --tenant %AZURE_TENANT_ID%
                        az webapp deploy --resource-group %RESOURCE_GROUP% --name %APP_SERVICE_NAME% --src-path app.zip --type zip
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo ' Deployment Failed!'
        }
    }
}
