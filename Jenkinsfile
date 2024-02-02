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

        
        stage('Stop Previous Process') {
            steps {
                script {
                    sh 'pkill -f "java -jar demo-0.0.1-SNAPSHOT.jar" || true'
                }
            }
        }

        
        stage('Run Application') {
            steps {
                script {
                    def jarFileName = 'java -jar demo-0.0.1-SNAPSHOT.jar'
                    sh "java -Dspring.backgroundpreinitializer.ignore=true -jar ${jarFileName}"
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
        }
    }
}