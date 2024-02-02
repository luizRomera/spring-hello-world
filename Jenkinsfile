pipeline {
    agent {
        label 'slave'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    withCredentials([sshUserPrivateKey(credentialsId: 'e317e956-79e1-42eb-a61e-0364bb55e74b', keyFileVariable: 'GIT_SSH_KEY', passphraseVariable: '', usernameVariable: 'git')]) {
                        checkout([$class: 'GitSCM', branches: [
                            [name: '*/main']
                        ], doGenerateSubmoduleConfigurations: false, extensions: [
                            [$class: 'RelativeTargetDirectory', relativeTargetDir: '']
                        ], submoduleCfg: [], userRemoteConfigs: [
                            [credentialsId: 'e317e956-79e1-42eb-a61e-0364bb55e74b', url: 'git@github.com:luizRomera/spring-hello-world.git']
                        ]])
                    }
                }
            }
        }

        stage('check mvn') {
            steps {
                script {
                    sh 'mvn -v'
                    sh 'java -version'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    sh 'rm -rf ~/.m2'
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
                    sh 'cp target/hello-world.jar /tmp'
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