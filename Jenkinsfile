pipeline {
    agent any

    stages {
        stage('Build & Tag Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred' {
                        sh "docker build -t prasanna1808/emailservice:latest ."
                    }
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred' {
                        sh "docker push prasanna1808/emailservice:latest "
                    }
                }
            }
        }
    }
}
