pipeline {
    agent {
        label params.AGENT == "any" ? "" : params.AGENT 
    }

    parameters {
        choice(name: "AGENT", choices: ["any", "docker", "windows", "linux"]) 
    }

    environment {
        AWS_ACCESS_KEY_ID     = credentials('aws-secret-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
    }

    stages {
        stage("Build") {
            steps {
                sh '''
                    docker build -t vietlt215/Angular-app .
                '''
            }
        }
        stage('Init Provider') {
            steps {
                sh 'terraform init'
            }
        }
        stage('Plan Resources') {
            steps {
                sh 'terraform plan'
            }
        }
        stage('Apply Resources') {
            input {
                message "Do you want to proceed for production deployment?"
            }
            steps {
                sh 'terraform apply -auto-approve'
            }
        }
    }
}