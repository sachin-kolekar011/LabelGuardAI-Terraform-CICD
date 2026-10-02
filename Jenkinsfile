pipeline {
    agent any

    environment {
        AWS_PROFILE = 'terraform-learning'
    }

    options {
        timestamps()
        disableConcurrentBuilds()
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('AWS Identity') {
            steps {
                sh 'aws sts get-caller-identity'
            }
        }

        stage('Terraform Version') {
            steps {
                sh 'terraform version'
            }
        }

        stage('Terraform Format Check') {
            steps {
                sh 'terraform fmt -check -recursive'
            }
        }

        stage('Terraform Init') {
            steps {
                sh 'terraform init -input=false'
            }
        }

        stage('Terraform Validate') {
            steps {
                sh 'terraform validate'
            }
        }

        stage('Terraform Plan') {
            steps {
                sh 'terraform plan -input=false -out=tfplan'
            }
        }

        stage('Approval') {
            steps {
                input message: 'Terraform plan is ready. Apply these changes to AWS?',
                    ok: 'Apply'
            }
        }

        stage('Terraform Apply') {
            steps {
                sh 'terraform apply -input=false tfplan'
            }
        }
    }

    post {
        success {
            echo 'Terraform validation and plan completed successfully.'
        }

        failure {
            echo 'Terraform pipeline failed. Check the Jenkins console output.'
        }
    }
}