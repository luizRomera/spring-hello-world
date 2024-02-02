pipeline {
    agent {
        label 'slave'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    catchError(buildResult: 'FAILURE', message: 'Failed to checkout code') {
                        withCredentials([sshUserPrivateKey(credentialsId: 'e317e956-79e1-42eb-a61e-0364bb55e74b', keyFileVariable: 'GIT_SSH_KEY', passphraseVariable: '', usernameVariable: 'git')]) {
                            checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: '']], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'e317e956-79e1-42eb-a61e-0364bb55e74b', url: 'git@github.com:luizRomera/spring-hello-world.git']]])
                        }
                    }
                }
            }
        }

        stage('check mvn') {
            steps {
                catchError(buildResult: 'FAILURE', message: 'Failed to build') {
                    script {
                        sh 'mvn -v'
                        sh 'java -version'
                    }
                }
            }
        }

        stage('Build') {
            steps {
                catchError(buildResult: 'FAILURE', message: 'Failed to build') {
                    script {
                        sh 'mvn clean install'
                    }
                }
            }
        }

        stage('Test') {
            steps {
                catchError(buildResult: 'FAILURE', message: 'Test failures detected') {
                    script {
                        sh 'mvn test'
                    }
                }
            }
        }

        stage('Package') {
            steps {
                catchError(buildResult: 'FAILURE', message: 'Failed to package') {
                    script {
                        sh 'mvn package'
                    }
                }
            }
        }

        stage('Publish Artifact') {
            steps {
                catchError(buildResult: 'FAILURE', message: 'Failed to publish artifact') {
                    script {
                        sh 'cp target/your-project.jar /path/to/output/directory/'
                    }
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