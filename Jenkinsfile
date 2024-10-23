pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/kydipin/financeme.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn compile'
            }
        }
        stage('test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('package') {
            steps {
                sh 'mvn package'
            }
        }
        stage('Dockerize App FinaceMe') {
            steps {
                sh 'docker build -t financeme:latest .'
            }
        }
        stage ('Docker Image Tagging') {
            steps {
                sh 'docker tag financeme:latest kydipin/financeme:latest'
            }
        }
        stage ('Docker Push') {
            steps {
                sh 'docker push kydipin/financeme:latest'
            }
        }
        stage('FinanceMe Server Launching with Terraform') {
            steps {
                sh 'sudo terraform -chdir=terraform init'
                sh 'sudo terraform -chdir=terraform apply -auto-approve'
            }
        }
        stage ('Configure FinanceMe Server with Ansible Playbook') {
            steps {
                sh 'ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i ansible/inventory_aws_ec2.yml -u ubuntu --private-key /var/lib/jenkins/dark.pem ansible/FinanceMe.yml'
            }
        }
    }
}