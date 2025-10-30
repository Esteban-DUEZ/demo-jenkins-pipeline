pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                script {
                    docker.image('maven:3.8.6-openjdk-17').inside('-v $HOME/.m2:/root/.m2') {
                        sh 'mvn clean compile'
                    }
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    docker.image('maven:3.8.6-openjdk-17').inside('-v $HOME/.m2:/root/.m2') {
                        sh 'mvn test'
                    }
                }
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
    }
    post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
