pipeline {
    agent any
    stages {
        stage ('setup') {
            steps {
                sh '''
                    python3 -m venv venv
                    venv/bin/pip install -r tests/requirements.txt
                '''
            }
        }
        stage ('test') {
            steps {
                sh '''
                    venv/bin/pytest
                '''
            }
        }
        stage ('build') {
            steps {
                sh '''
                    venv/bin/python3 -m ensurepip --upgrade
                    venv/bin/sam build -t template.yaml
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
                    venv/bin/sam deploy -t lambda-app/template.yaml --no-confirm-changeset --no-fail-on-empty-changeset
                '''
            }
        }
    }
}
