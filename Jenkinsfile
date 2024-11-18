pipeline {
    agent any
    stages {
        stage ('setup') {
            steps {
                sh '''
                    python3 -m venv venv
                    source venv/bin/activate
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
                    source venv/bin/activate
                    venv/bin/sam build -t template.yaml
                '''
            }
        }
        stage ('deploy') {
            environment {
                AWS_ACCESS_KEY_ID = credentials('aws_access_key')
                AWS_SECRET_ACCESS_KEY = credentials('aws_access_secret_key')
                AWS_DEFAULT_REGION = credentials('aws_region')
            }
            steps {
                sh '''
                    source venv/bin/activate
                    venv/bin/sam deploy -t template.yaml --stack-name helloWorld --no-confirm-changeset --no-fail-on-empty-changeset
                '''
            }
        }
    }
}
