pipeline {
    agent any
    environment {
    VAR = sh(script: 'sudo cat /tmp/replace.txt', returnStdout: true).trim()
    }


    stages {
        stage('Ansible Version') {
            steps {
                sh 'echo ${VAR}'
                sh 'sudo ansible --version'
            }
        }

        stage('Ansible Script to install Nginx on EC2 Instance') {
            steps {
              script {
                catchError(buildResult: 'Success', stageResult: 'Failure') {              
                sh 'sudo cp -rf /home/ubuntu/terraform/vpc/ansible/Terraform_Role ./'               
                sh 'sudo cp /home/ubuntu/terraform/vpc/terraform/Terraform_AWS_Keypair.pem ./'
                sh 'sudo chown jenkins ./Terraform_AWS_Keypair.pem'
                sh 'sudo chmod 400 Terraform_AWS_Keypair.pem'
                sh 'sudo cat ./Terraform_Role/tests/inventory'
                sh 'echo ${VAR}'
                sh """sudo sed -i '2s/.*/${VAR}/g' ./Terraform_Role/tests/inventory"""
                sh 'sudo cat ./Terraform_Role/tests/inventory'
                sh 'ansible-playbook -i ./Terraform_Role/tests/inventory ./Terraform_Role/tests/test.yml --key-file ./Terraform_AWS_Keypair.pem'

                echo "Succefully install nginx on EC2: ${VAR} using GIT->JENKINS->ANSIBLE->TERRAFORM."
                }
            }
        }
    }
  }
}
