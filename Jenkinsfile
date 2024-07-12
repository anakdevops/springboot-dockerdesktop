pipeline {
    agent { label 'windows'}

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git branch: 'main', url: 'https://github.com/anakdevops/springboot-dockerdesktop.git'

               
                 bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
        stage('Build Docker') {
            steps {
                bat "docker image rm nakdevops:v1"
                bat "docker image rm anakdevops/spring-dockerdesktop:v1"
                bat "docker build -t anakdevops:v1 ."
                bat "docker tag anakdevops:v1 anakdevops/spring-dockerdesktop:v1"
                bat "docker push anakdevops/spring-dockerdesktop:v1"
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                    bat "kubectl apply -f deploy.yaml"
                    bat "kubectl apply -f services.yaml"
                }
            }
        stage('Curl') {
            steps {
                    bat "curl localhost:30007"
                }
            }
    }
}
