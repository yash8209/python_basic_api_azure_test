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
                git branch: 'main', url: 'https://github.com/yash8209/python_basic_api_azure_test.git'
            }
        }

        stage('Setup Python') {
    steps {
        bat '''
            echo %PATH%
            where python
            python --version
        '''
    }
}


        stage('Install Dependencies') {
            steps {
                bat 'pip install -r requirements.txt'
            }
        }

        stage('Package Application') {
            steps {
                bat '''
                    powershell Remove-Item -Path app.zip -ErrorAction Ignore
                    powershell Compress-Archive -Path * -DestinationPath app.zip
                '''
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
