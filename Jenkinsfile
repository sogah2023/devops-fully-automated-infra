def COLOR_MAP = [
    'SUCCESS': 'good', 
    'FAILURE': 'danger',
]

pipeline {
    agent any

    stages {
        stage('Git checkout') {
            steps {
                echo 'Cloning project codebase...'
                git branch: 'main', url: 'https://github.com/cvamsikrishna11/devops-fully-automated-infra.git'
                sh 'ls'
            }
        }
        
         stage('Verify Terraform Version') {
            steps {
                echo 'verifying the terrform version...'
                sh 'terraform --version'
               
            }
        }
        
        stage('Terraform init') {
            steps {
                echo 'Initiliazing terraform project...'
                sh 'terraform init'
               
            }
        }
        
        
        stage('Terraform validate') {
            steps {
                echo 'Code syntax checking...'
                sh 'terraform validate'
               
            }
        }
        
        
        stage('Terraform plan') {
            steps {
                echo 'Terraform plan for the dry run...'
                sh 'terraform plan'
               
            }
        }
        
        
        
        
        stage('Checkov scan') {
            steps {
                
                sh """
                sudo pip3 install checkov
                checkov -d .
                #checkov -d . --skip-check CKV_AWS_23,CKV_AWS_24,CKV_AWS_126,CKV_AWS_135,CKV_AWS_8,CKV_AWS_23,CKV_AWS_24
                #checkov -d . --skip-check CKV_AWS*
                """
               
            }
        }
        

        
        
         stage('Terraform apply') {
            steps {
                echo 'Terraform apply...'
                sh 'terraform apply --auto-approve'
               
               
            }
        }
        
        
    }
    
     post { 
        always { 
            echo 'I will always say Hello again!'
            slackSend channel: '#team-devops', color: COLOR_MAP[currentBuild.currentResult], message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} \n More info at: ${env.BUILD_URL}"
        }
    }
    
    
    
}
