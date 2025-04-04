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

      

 stage('Set Up Python Environment') {
            steps {
                bat 'python -m venv venv'
                bat '.\\venv\\Scripts\\activate && pip install --upgrade pip'
                bat '.\\venv\\Scripts\\activate && pip install -r requirements.txt'
                bat '.\\venv\\Scripts\\activate && pip install pytest'
            }
        }

        stage('Run Tests') {
            steps {
                bat '.\\venv\\Scripts\\activate && pytest'
            }
        }

        stage('Prepare for Deployment') {
            steps {
                bat 'if exist publish (rmdir /s /q publish)'
                bat 'mkdir publish'
                bat 'xcopy /s /y *.py publish\\'
                bat 'if exist app\\ (xcopy /s /y app\\* publish\\app\\)'
                bat 'if exist main.py copy /y main.py publish\\'
                bat 'if exist requirements.txt copy /y requirements.txt publish\\'
            }
        }

        stage('Deploy to Azure') {
            steps {
                withCredentials([azureServicePrincipal(credentialsId: AZURE_CREDENTIALS_ID)]) {
                    bat 'az login --service-principal -u %AZURE_CLIENT_ID% -p %AZURE_CLIENT_SECRET% --tenant %AZURE_TENANT_ID%'
                    bat 'powershell Compress-Archive -Path publish\\* -DestinationPath publish.zip -Force'
                    bat 'az webapp deploy --resource-group %RESOURCE_GROUP% --name %APP_SERVICE_NAME% --src-path publish.zip --type zip'
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
