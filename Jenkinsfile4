pipeline {
    agent any
 
    stages {
       stage('checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/jordanehou/geo-july.git'
            }
        }
 
        stage('Create Zip File') {
            steps {
                script {
                    // Create a zip file with all the code
                    sh 'zip -r code.zip .'
                }
            }
        }

        stage('Store in JFrog Artifactory') {
             steps{
               sh 'curl -u admin:password -T target/bio*.jar "http://ec2-51-20-132-82.eu-north-1.compute.amazonaws.com:8081/artifactory/geoapp/code.zip"'
            }
        }
 
        stage('Deploy to Ansible Server') {
            steps {
                script {
                    // Assuming your Ansible playbook is named 'deploy.yml'
                    sh 'ansible-playbook -i inventory/ansible-server deploy.yml'
                    
                }
            }
        }
    }
 
    post {
        success {
            echo 'Deployment successful'
        }
        failure {
            echo 'Deployment failed'
        }
    }
}
