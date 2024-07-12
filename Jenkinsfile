pipeline {
    agent {label 'windows' }
    tools {
        maven "maven-3.9.6"
    }

    stages {
        
        
        stage('Build Docker') {
            agent { label 'windows' }
            steps {
                bat 'docker build -t anakdevops:$DOCKER_TAG .'
            }
        }
        stage('Build tag and push') {
            agent { label 'windows' }
            steps {
                bat 'docker tag anakdevops:$DOCKER_TAG anakdevops/springboot-desktop:$DOCKER_TAG'
                bat 'docker push anakdevops/springboot-desktop:$DOCKER_TAG'
                bat 'docker stop anakdevops || true' 
                bat 'docker rm anakdevops || true'   
                bat 'docker run -d --name anakdevops -p 8333:8080 anakdevops/springboot-desktop:$DOCKER_TAG'
            }
        }
        stage('Cleanup') {
             steps {
                cleanWs()  // Cleans workspace after all previous stages
             }
            }
        stage('Deploy') {
            agent { label 'windows' }
            steps {
               script {
                  bat 'docker stop anakdevops || true' 
                  bat 'docker rm anakdevops || true'   
                  bat 'docker run -d --name anakdevops -p 8444:8080 anakdevops/springboot-desktop:$DOCKER_TAG'
             }
           }
        }
    }    
}
