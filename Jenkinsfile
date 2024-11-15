pipeline {
    agent any
    stages {
        stage ('setup') {
            steps {
                sh '''
                    sudo python3 -m venv venv
                    sudo venv/bin/pip install -r lambda-app/requirements.txt
                '''
            }
        }
        stage ('test') {
            steps {
                sh '''
                    pytest
                '''
            }
        }
        stage ('build') {
            steps {
                sh '''
                    sam build -t lambda-app/template.yaml
                '''
            }
        }
        stage ('deploy') {
            environment {
                AWS_ACCESS_KEY_ID = credentials('aws_access_key')
                AWS_ACCESS_SECRET_KEY = credentials('aws_access_secret_key')
                AWS_DEFAULT_REGION = credentials('aws_region')
            }
            steps {
                sh '''
                    sam deploy -t lambda-app/template.yaml --no-confirm-changeset --no-fail-on-empty-changeset
                '''
            }
        }
    }
}
