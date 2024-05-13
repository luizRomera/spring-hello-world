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

        stage('Sonar') {
            steps {
                script {
                    sh """
                        mvn clean verify sonar:sonar -Dsonar.host.url=http://192.168.0.212:9000 -Dsonar.projectKey=spring-hello-world -Dsonar.token=sqp_74b6816538009f2b57a9ac7a678a79a1c9c2d950
                    """
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
