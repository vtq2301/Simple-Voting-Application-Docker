pipeline {
    agent any

    environment {
        dockerRegistry = "quentindev97/practice"
        registryCredential = "dockercreds"
    }

    stages {
        stage("GIT checkout") {
            steps {
                git branch: 'main', url: 'https://github.com/vtq2301/Simple-Voting-Application-Docker.git'
            }
        }

        stage("Build Images") {
            steps {
                sh 'docker build -t voting-app vote/'
                sh 'docker build -t result-app result/'
                sh 'docker build -t worker-app worker/'
            }
        }

        stage("Tag Images") {
            steps {
                sh """
                docker tag voting-app ${dockerRegistry}:voting-app
                docker tag result-app ${dockerRegistry}:result-app
                docker tag worker-app ${dockerRegistry}:worker-app
                """
            }
        }

        stage("Push Images") {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        sh """
                        docker push ${dockerRegistry}:voting-app
                        docker push ${dockerRegistry}:result-app
                        docker push ${dockerRegistry}:worker-app
                        """
                    }
                }
            }
        }

        stage("Test") {
            steps {
                sh "echo 'Running Test...'"
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
