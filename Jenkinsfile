pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                deleteDir()
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'chmod +x mvnw'
                sh './mvnw clean compile'
            }
        }

        stage('Test') {
            steps {
                sh './mvnw test'
            }
        }

        stage('Package') {
            steps {
                sh './mvnw package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t petclinic .'
            }
        }

        stage('Deploy Container') {
            steps {
                sh 'docker stop petclinic || true'
                sh 'docker rm petclinic || true'
                sh 'docker run -d -p 8081:8080 --name petclinic petclinic'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
    }
}
