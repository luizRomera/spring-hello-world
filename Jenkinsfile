pipeline {
    agent {
        label 'slave'
    }
    
    stages {

        stage('Build') {
            steps {
                script {
                    sh 'mvn clean install'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    sh 'mvn test'
                }
            }
        }

        stage('Package') {
            steps {
                script {
                    sh 'mvn package'
                }
            }
        }

        stage('Publish Artifact') {
            steps {
                script {
                    archiveArtifacts "target/*.jar"
                }
            }
        }

    }

    post {
        success {
            echo 'Build successful! Artifact generated and published.'
        }

        failure {
            echo 'Build failed. Check the logs for more information.'
            echo 'Cleaning the environment.'
            sh 'rm -rf ~/.m2'
            sh 'rm -rf *'
        }
    }
}