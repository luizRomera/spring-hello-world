def jarFileName = "target/demo-0.0.1-SNAPSHOT.jar"

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
                    sh 'pkill -f "java -jar ${jarFileName}" || true'
                }
            }
        }

        
        stage('Run Application') {
            steps {
                script {
                    sh "java -Dspring.backgroundpreinitializer.ignore=true -Dserver.port=8083 -jar ${jarFileName} > /dev/null 2>&1 &"
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
            sh 'rm -rf ~/.m2'
            sh 'rm -rf *'
        }
    }
}