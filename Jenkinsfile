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
                script {
                    def pythonHome = tool name: 'Python3', type: 'hudson.tasks.Maven$MavenInstallation'
                    env.PATH = "${pythonHome}/bin:${env.PATH}"
                }
                sh 'python --version'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'pytest tests/'
            }
        }

        stage('Package Application') {
            steps {
                sh 'zip -r app.zip *'
            }
        }

        stage('Deploy to Azure') {
            steps {
                withCredentials([azureServicePrincipal(credentialsId: AZURE_CREDENTIALS_ID)]) {
                    sh '''
                        az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET --tenant $AZURE_TENANT_ID
                        az webapp deploy --resource-group $RESOURCE_GROUP --name $APP_SERVICE_NAME --src-path app.zip --type zip
                    '''
                }
            }
        }
    }

    post {
        success {
            echo ' Deployment Successful!'
        }
        failure {
            echo ' Deployment Failed!'
        }
    }
}
