pipeline {
    agent {label 'linux' }
    tools {
        maven "maven-3.9.6"
    }

    stages {
        stage('Clone & compile') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/anakdevops/springboot-dockerdesktop.git'
                sh "ls -lat"
                sh "mvn clean package"    
            }
        }
        
        stage('Build Docker') {
            steps {
                sh 'docker build -t anakdevops:$DOCKER_TAG .'
            }
        }
        stage('Build tag and push') {
            steps {
                sh 'docker tag anakdevops:$DOCKER_TAG anakdevops/java-pipeline:$DOCKER_TAG'
                sh 'docker push anakdevops/java-pipeline:$DOCKER_TAG'
                sh 'docker stop anakdevops || true' 
                sh 'docker rm anakdevops || true'   
                sh 'docker run -d --name anakdevops -p 8333:8080 anakdevops/java-pipeline:$DOCKER_TAG'
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
                  sh 'docker stop anakdevops || true' 
                  sh 'docker rm anakdevops || true'   
                  sh 'docker run -d --name anakdevops -p 8444:8080 anakdevops/java-pipeline:$DOCKER_TAG'
             }
           }
        }
    }    
}
