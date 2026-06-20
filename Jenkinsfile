pipeline {
    agent any

    parameters {
        choice(name: 'ACTION', choices: ['apply', 'destroy'])
    }

    environment {
        ARM_CLIENT_ID       = credentials('ARM_CLIENT_ID')
        ARM_CLIENT_SECRET   = credentials('ARM_CLIENT_SECRET')
        ARM_SUBSCRIPTION_ID = credentials('ARM_SUBSCRIPTION_ID')
        ARM_TENANT_ID       = credentials('ARM_TENANT_ID')
    }

    stages {
        stage('Clean Workspace') {
            steps {
                    deleteDir()
            }
        }
        
        stage('Checkout') {
            steps {
                git branch: 'main',
                url: 'https://github.com/dharidost/terraform-azure-vm.git'
            }
        }
        
        stage('Terraform Init') {
            steps {
                dir('terraform') {
                    sh 'terraform init'
                }
            }
        }

        stage('Terraform Plan') {
            steps {
                dir('terraform') {
                    sh 'terraform plan'
                }
            }
        }

        stage('Apply/Destroy') {
            steps {
                dir('terraform') {
                    script {
                        if (params.ACTION == 'destroy') {
                            sh 'terraform destroy -auto-approve'
                        } else {
                            sh 'terraform apply -auto-approve'
                        }
                    }
                }
            }
        }
    }
}
